---
title: "Type in Kotlin"
date: 2021-04-06T19:58:54+09:00
tags: ["kotlin", "kotlin-for-java-developer"]
categories: ["Programming"]
---

# Basic Type
- kotlin doesn’t have primitive type 
- `Int` or `Int?`

## `Int` in bytecode
``` kotlin
fun foo(): Int = 1
```

``` java
public static final int foo() {
	return 1;
}
```

## `Int?` in bytecode
``` kotlin
fun bar(): Int? = 1
```

``` java
public static final Integer foo() {
	return 1;
}
```

## Primitive & wrapper types
![](/Type/download.jpg)
![](/Type/download%202.jpg)

## `String`
- Kotlin `String` hides some confusing methods

## `Any`
- `Any` in Kotlin is a super type for all non-nullable types. 

![](/Type/download%203.jpg)
- Unlike `Object`, `Any` is not only super type for reference types, but it’s also super type for types like `Int` corresponding to primitives. 

``` kotlin
log(2017)

// auto boxing happend
fun log(any: Any) {
	println("Value: $any")
}

// no auto boxing now
fun log(i: Int) {
	println("Value: $i")
}
```


## Function 
![](/Type/download%204.jpg)

## `Array`
``` java
int[] ints1 = {1, 2,};
int[] ints2 = {1, 2,};

ints1.equals(ints2); // false
Arrays.equals(ints1, ints2) // true
```

``` kotlin
val ints1 = intArrayOf(1, 2)
val ints2 = intArrayOf(1, 2)

println(ints1 == ints2) // false
println(ints1.contentEquals(ints2) // true
```
- Prefer Lists to Arrays
	- only Kotlin, there is no reason for arrays.
	- `MutableList` is Java util `ArrayList` under the hood. `ArrayList` is very close to array in terms of performance. So, prefer `List` to `Array` by default and avoid the necessity to remember the right way to compare arrays.

# Kotlin type hierachy
![](/Type/download%205.jpg)

![](/Type/download%206.jpg)

## `Unit`
- A type that allows only ::one value:: and thus can hold no information
- the function completes successfully
- `Unit` instead of `void` 

## `Nothing`
- A type that has ::no values::
- The function never completes
- This function never returns, only throws exceptions

## Why `Nothing` needs in kotlin 
![](/Type/download%207.jpg)
![](/Type/download%208.jpg)
- answer should be super type of both execution result

# Nullable types
![](/Type/download%209.jpg)
![](/Type/download%2010.jpg)
- Platform type `Type!` can be only seen in compiler error message
- With Java code without annotation, kotlin compiler cannot infer any nullability

## How to still prevent NPEs?
- Annotate your Java types
	- Supports many kinds of annotations from different packages
	- Can specify `@NotNull` as default, and annotate only `@Nullable` types 
	- JSR-305, `@ParametersAreNonnullByDefault`
- Specify types explicitly

``` java
public class Session {
	public String getDescription() {
	return null;
	}
}
```

``` kotlin
val session = Session()
val description: String? = session.description
println(description?.lengh) // null 
```


## Collection types
### `List` & `MutableList`
- Two interfaces declared in `kotlin.collections` package
- `MutableList` extends `List`

### Read-only != immutable 
- Read-only interface just lacks mutating methods
- The actual list can be changed by another reference

![](/Type/download%2011.jpg)

### Read-only interfaces improve API
``` kotlin
object Shop {
	private val customers = mutableListOf<Customer>()
	fun getCustomers(): List<Customer> = customers
}

val customers = getCustomers()
customers.add() // error ! 
```

