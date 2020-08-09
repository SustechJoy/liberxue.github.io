---
layout: blog
comments: true
code: true
title:  "Optional usage in java to save chain-invocation"
tags:
- java 8
- optional
- chain-invocation

background-image: https://cdn.dribbble.com/users/984227/screenshots/4647279/echarts.png
date:   2020-08-09 21:18:00
category: code

---

Code optimization: chain-invokation saver - Optional

Chain-invokation presents normally in our code, which looks like 

```java
company.getLeaderDepartment().getLeader().getName() // unsafe
```

codes like above may easy to cause NPE (Null Pointer Exception)

To solve this problem:

```java
if (company != null) {
	if (compnay.getLeaderDepartment() != null) {
		if (company.getLeaderDepartment().getLeader() != null) {
			String name = company.getLeaderDepartment().getLeader().getName(); // safe now
		}
	}
}
```

But how to make the code more elegant?

Use Library Optional

Source Code:

Optional has an empty object:

```java
private static final Optional<?> EMPTY = new Optional<>();
```

Optional has two methods:

```java
of: // must not be null, or throws exception
public static <T> Optional<T> of(T value) {
	return new Optional<>(value);
}
ofNullable: // can be null
public static <T> Optional<T> ofNullable(T value) {
	return value == null ? empty() : of(value);
}
```

using provided map api, we can try to convert one type to another type

```java
public<U> Optional<U> map(Function<? super T, ? extends U> mapper) {
  Objects.requireNonNull(mapper);
  if (!isPresent())
    return empty();
  else {
    return Optional.ofNullable(mapper.apply(value));
  }
}
```

we also have two methods to get value

```java
public T orElse(T other) {
  return value != null ? value : other; // get a default value
}
public T orElseGet(Supplier<? extends T> other) {
  return value != null ? value : other.get(); // invoke a passing functional supplier interface[1]
}
```

finally, our code should look like

```java
Optional.ofNullable(company)
  .map(Company::getLeaderDepartment)
  .map(Department::getLeader)
  .map(People::getName)
  .orElse(StringUtils.EMPTY);
```



[1]. 

functional supplier interface:

```
() -> return T;
```

functional consumer interface:

```
(T t) -> void();
```

