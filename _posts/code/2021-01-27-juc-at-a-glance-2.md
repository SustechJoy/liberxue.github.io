---
layout: blog
comments: true
istop: false
code: true
title: "JUC principle at a glance (2)"
background-image: https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/db1601e0-5f67-4d76-a6d1-3d5e5f2ca2d7/File/0d6e6e95bd1dab36ae3e9a8ba1493af0/completablefuture_in_java_8_.png
date:  2021-01-27 23:30:00
category: code
tags:
- java
- concurrent
- LongAdder
---

## AtomicIntegerArray

```java
private static final int base = unsafe.arrayBaseOffset(int[].class);
private final int[] array;

static {
  int scale = unsafe.arrayIndexScale(int[].class);
  if ((scale & (scale - 1)) != 0)
    throw new Error("data type scale not a power of two");
  shift = 31 - Integer.numberOfLeadingZeros(scale);
}
```

Comes from java official doc, we know the following:

> Report the offset of the first element in the storage allocation of a given array class.  If [#arrayIndexScale](http://www.docjar.com/docs/api/sun/misc/Unsafe.html#arrayIndexScale)  returns a non-zero value for the same class, you may use that scale factor, together with this base offset, to form new offsets to access elements of arrays of the given class.

```java
System.out.println(int[].class);

output:
class [I
```

Copy contructor:

```java
/**
 * Creates a new AtomicIntegerArray with the same length as, and
 * all elements copied from, the given array.
 *
 * @param array the array to copy elements from
 * @throws NullPointerException if array is null
 */
public AtomicIntegerArray(int[] array) {
  // Visibility guaranteed by final field guarantees
  this.array = array.clone();
}
```

Getter:

```java
/**
 * Gets the current value at position {@code i}.
 *
 * @param i the index
 * @return the current value
 */
public final int get(int i) {
  return getRaw(checkedByteOffset(i));
}

private int getRaw(long offset) {
  return unsafe.getIntVolatile(array, offset);
}
```



The getIntVolatile() is a method provided by unsafe, Unsafe supports all primitive values and can even write values without hitting thread-local caches by using the volatile forms of the methods.

> getXXX(Object target, long offset): Will read a value of type XXX from target's address at the specified offset.
>
> getXXXVolatile(Object target, long offset): Will read a value of type XXX from target's address at the specified offset and not hit any thread local caches.
>
> putXXX(Object target, long offset, XXX value): Will place value at target's address at the specified offset.
>
> putXXXVolatile(Object target, long offset, XXX value): Will place value at target's address at the specified offset and not hit any thread local caches



GetAndSet:

```java
/**
 * Atomically sets the element at position {@code i} to the given
 * value and returns the old value.
 *
 * @param i the index
 * @param newValue the new value
 * @return the previous value
 */
public final int getAndSet(int i, int newValue) {
  return unsafe.getAndSetInt(array, checkedByteOffset(i), newValue);
}

> Unsafe class (CAS algorithm and getIntVolatile() to make thread-safe)
public final int getAndSetInt(Object var1, long var2, int var4) {
  int var5;
  do {
    var5 = this.getIntVolatile(var1, var2);
  } while(!this.compareAndSwapInt(var1, var2, var5, var4));

  return var5;
} 
```

 

## AtomicIntegerFieldUpdater

Comes from the comment in the class:

```java
* Note that the guarantees of the {@code compareAndSet}
* method in this class are weaker than in other atomic classes.
* Because this class cannot ensure that all uses of the field
* are appropriate for purposes of atomic access, it can
* guarantee atomicity only with respect to other invocations of
* {@code compareAndSet} and {@code set} on the same updater.
```

Usage Exmaple:

```java
class Person {
    volatile int id;
    ...
}

public static void main(String[] args) {
  // initialize the AtomicIntegerFieldUpdater, with parameter as the target class name and field name
  AtomicIntegerFieldUpdater<Person> mAtoLong = AtomicIntegerFieldUpdater.newUpdater(Person.class, "id");
  Person person = new Person(12345);
  mAtoLong.compareAndSet(person, 12345, 1000);
  System.out.println("id="+person.getId());
}
```



## LongAdder

The LongAdder comes with Striped64 class.

The implementation **keeps an array of counters that can grow on demand**. When more threads are calling *increment()*, the array will be longer. Each record in the array can be updated separately – reducing the contention. Due to that fact, the *LongAdder* is a very efficient way to increment a counter from multiple threads.

```java
/**
     * Adds the given value.
     *
     * @param x the value to add
     */
public void add(long x) {
  Cell[] as; long b, v; int m; Cell a;
  if ((as = cells) != null || !casBase(b = base, b + x)) {
    boolean uncontended = true;
    if (as == null || (m = as.length - 1) < 0 ||
        (a = as[getProbe() & m]) == null ||
        !(uncontended = a.cas(v = a.value, v + x)))
      longAccumulate(x, null, uncontended);
  }
}
```

The `getProbe()` method returns the `UNSAFE.getInt(Thread.currentThread(), PROBE);`, which is a value stored in the Thread class, the value is initialized in ThreadLocalRandom class as follows.

```java
/**
 * Initialize Thread fields for the current thread.  Called only
 * when Thread.threadLocalRandomProbe is zero, indicating that a
 * thread local seed value needs to be generated. Note that even
 * though the initialization is purely thread-local, we need to
 * rely on (static) atomic generators to initialize the values.
 */
static final void localInit() {
  int p = probeGenerator.addAndGet(PROBE_INCREMENT);
  int probe = (p == 0) ? 1 : p; // skip 0
  long seed = mix64(seeder.getAndAdd(SEEDER_INCREMENT));
  Thread t = Thread.currentThread();
  UNSAFE.putLong(t, SEED, seed);
  UNSAFE.putInt(t, PROBE, probe);
}
```

Core method is show below:

```java
final void longAccumulate(long x, LongBinaryOperator fn,
                          boolean wasUncontended) {
  int h; // magic value to (& with array size - 1) to get accessed cell index
  if ((h = getProbe()) == 0) {
    ThreadLocalRandom.current(); // force initialization
    h = getProbe();
    wasUncontended = true;
  }
  boolean collide = false;                // True if last slot nonempty
  for (;;) {
    Cell[] as; Cell a; int n; long v;
    if ((as = cells) != null && (n = as.length) > 0) {
      if ((a = as[(n - 1) & h]) == null) {
        if (cellsBusy == 0) {       // Try to attach new Cell
          Cell r = new Cell(x);   // Optimistically create
          if (cellsBusy == 0 && casCellsBusy()) {
            boolean created = false;
            try {               // Recheck under lock
              Cell[] rs; int m, j;
              if ((rs = cells) != null &&
                  (m = rs.length) > 0 &&
                  rs[j = (m - 1) & h] == null) {
                rs[j] = r;
                created = true;
              }
            } finally {
              cellsBusy = 0;
            }
            if (created)
              break;
            continue;           // Slot is now non-empty
          }
        }
        collide = false;
      }
      else if (!wasUncontended)       // CAS already known to fail
        wasUncontended = true;      // Continue after rehash
      else if (a.cas(v = a.value, ((fn == null) ? v + x :
                                   fn.applyAsLong(v, x)))) // CAS specific cell
        break;
      else if (n >= NCPU || cells != as)
        collide = false;            // At max size or stale
      else if (!collide)
        collide = true;
      else if (cellsBusy == 0 && casCellsBusy()) {
        try {
          if (cells == as) {      // Expand table unless stale
            Cell[] rs = new Cell[n << 1];
            for (int i = 0; i < n; ++i)
              rs[i] = as[i];
            cells = rs;
          }
        } finally {
          cellsBusy = 0;
        }
        collide = false;
        continue;                   // Retry with expanded table
      }
      h = advanceProbe(h);
    }
    else if (cellsBusy == 0 && cells == as && casCellsBusy()) {
      boolean init = false;
      try {                           // Initialize table
        if (cells == as) {
          Cell[] rs = new Cell[2];
          rs[h & 1] = new Cell(x);
          cells = rs;
          init = true;
        }
      } finally {
        cellsBusy = 0;
      }
      if (init)
        break;
    }
    else if (casBase(v = base, ((fn == null) ? v + x :
                                fn.applyAsLong(v, x))))
      break;                          // Fall back on using base
  }
}
```

The implementation extends `Striped64`, which is a data holder for 64-bit values. The values are held in cells, which are padded (or striped), hence the name. Each operation made upon the LongAdder will modify the collection of values present in the Striped64. When contention occurs, a new cell is created and modified, so the the old thread can finish concurrently with contending one. When you need the final value, the sums of each cell is simply added up.

```java
public long sum() {
  Cell[] as = cells; Cell a;
  long sum = base;
  if (as != null) {
    for (int i = 0; i < as.length; ++i) {
      if ((a = as[i]) != null)
        sum += a.value;
    }
  }
  return sum;
}
```

The `sum()` iterates all over the array and reads each volatile variables, which can be costly. Howover, having multiple such cells is a way of spreading out the contention and thus increasing throughput.

 

### Reference

[1]. https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html

[2]. https://stackoverflow.com/questions/30691083/how-longadder-performs-better-than-atomiclong