---
layout: blog
comments: true
istop: false
code: true
title: "JUC principle at a glance (1)"
background-image: https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/db1601e0-5f67-4d76-a6d1-3d5e5f2ca2d7/File/0d6e6e95bd1dab36ae3e9a8ba1493af0/completablefuture_in_java_8_.png
date:  2021-01-01 23:30:00
category: code
tags:
- java
- concurrent
- volatile
- Atomic
---

## 1. package structure

```sh
concurrent
	- atomic
	- locks
	- ... (several classes)
```

## 2. atomic

Atomic*(AtomicBoolean、AtomicInteger...) has a common declaration schema.

Here, take AtomicReference as the base example.

```java
public class AtomicReference<V> {
  
	// setup to use Unsafe.compareAndSwapInt for updates
  private static final Unsafe unsafe = Unsafe.getUnsafe();
  private static final long valueOffset;
  
 	static {
    try {
     	// Unsafe.objectFieldOffset() gets the offset of the declared field in the class. This is class-level information. It has nothing to do with the instance values of that field.
			// The offset is just used to determine which memory location to address when updating the value field of AtomicInteger instances.
      valueOffset = unsafe.objectFieldOffset
        (AtomicBoolean.class.getDeclaredField("value"));
    } catch (Exception ex) { throw new Error(ex); }
  }

  private volatile V value;
}
```



When doing get(), it directly returns the value field

```java
/**
  * Gets the current value.
  *
  * @return the current value
  */
public final V get() {
  return value;
}
```

as the value is declared as `volatile`. The Java `volatile` keyword[[1]](###Appendix) is used to mark a Java variable as "being stored in main memory". More precisely that means, that every read of a volatile variable will be read from the computer's main memory, and not from the CPU cache, and that every write to a volatile variable will be written to main memory, and not just to the CPU cache.



For a normal pattern, we use Atomic* like below:

```java
private AtomicInteger ai = new AtomicInteger(0);

public void add(int val) {
  Integer a;
  do {
    a = ai.get();
  } while(!ai.compareAndSet(a, a + val));
}

```

or directly use 

```java
private AtomicInteger ai = new AtomicInteger(0);
public void add(int val) {
	ai.addAndGet(val);
}
```





### Appendix

[1]. `volatile` keyword (visibility garantee)

![image-20210125112921442](https://i.loli.net/2021/01/25/2hB4E7J5Fc3umAi.png)

For a variable not declared as volatile, java has no garantee that the updated value of objects in one thread to be writen back from CPU registors back to the main memory or the CPU registors of another thread.

For volatile fields, value setting will be directly written into main memory, and the reading will be directly processed into main memory.

![](https://i.loli.net/2021/01/25/RGYwV7XzLNh5lkH.png)

1. Whenever write to a volatile variable, then **the volatile varible and all variables visible to that thread** will be written directly into main memory (ex: both object and hasNewObject in the figure above). 
2. Whenever a thread reads a volatile variable,  **the volatile varible and all variables visible to that thread** has to be read(refreshed) from the main memory. 

![](https://i.loli.net/2021/01/25/G83SXaMIxzeH7NU.png)

Instruction reordering could break the java volatile visibility garantee.

The orderering like above will garantee all three values are well written and read.

![](https://i.loli.net/2021/01/25/VJajr36siOfDML8.png)

like in the above figure, the value1 is not written into the main memory, and read out as an older value.

The **volatile** keyword ensures '**happen before**' principle, which means all statements before a written to a volatile variable will always remain before the volatile writing statements. All readings of a volatile variable will remain before the reading of other volatile/non-volatile variables. Thus ensuring system will not reorder statements to cause a wrong assignment in a code block including manipulations to volatile variables.



Java volatile is not always enough

```java
private volatile int count = 0;

public boolean inc() {
  if (this.count == 10) {
    return false;
  }
  this.count++;
  return true;
}
```

For the code above, multiple threads can still simultaneously go to the `this.count++` part with value 9, then both threads will write to the main memory with value 9. Thus, **CAS/synchronize** is needed to be with **volatile** to ensure thread-safety.



### Reference

[1]. https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html

[2]. http://tutorials.jenkov.com/java-concurrency/volatile.html

[3]. https://www.youtube.com/watch?v=nhYIEqt-jvY