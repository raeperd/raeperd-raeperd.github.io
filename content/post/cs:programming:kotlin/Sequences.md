---
title: "Sequence"
date: 2021-03-18T19:58:54+09:00
categories: ["Programming"]
tags: ["kotlin", "kotlin-for-java-developer"]
---

# Collections vs Sequences 
## Operation on collections
- lambdas are inlined (no performance overhead)
- but: intermediate collections are created for chained calls 
- Operations on collections are inlined. That works great for simple cases, but creates a significant performance overhead for chained calls, because intermediate collections are created
- Sequences solved this problem by storing operations to be performed, and evaluating them in a lazy manner. Operations on sequences mainly duplicate operations on collections. You can convert your chained calls from collections to sequences by adding a sequence at the beginning

## Sequences
- `Stream` in java
- **Collection vs Sequences implies eager vs lazy evaluation**
- `asSequence()` 


# More about Sequences 
![](/Sequences/download.jpg)

![](/Sequences/download%202.jpg)

![](/Sequences/download%203.jpg)


# Create Sequences 
``` kotlin
interface Sequence<out T> {
	operator fun interator(): Iterator<T>
}
```

![](/Sequences/download%204.jpg)
- Two ways to use iterator, eager and lazy

## `generateSequence`
``` kotlin
gerneateSequence { Random.nextInt() }
gerneateSequence { Random.nextInt(5).takeIf { it > 0} }
```


### Generating an infinite sequence
``` kotlin
val numbers = generateSequence(0) { it + 1 }
numbers.take(5).toList()

// to pervent integer overflow 
val numbers = generateSequence(BigInteger.ZERO) {
	it + BigInteger.ONE
}
```


## `yield`
- Not a language feature but a library function

``` kotlin
val numbers = sequence {
	var x = 0
	while (true) {
		yield(x++)
	}
}
number.take(5).toList() // [0, 1, 2, 3, 4]

sequence {
	yield(value)
	doSomething()
	yieldAll(list)
	doSomethingElse()
	yieldAll(sequence)
}
```

``` kotlin
fun fibonacci(): Sequence<Int> = sequence {
    var beforePrevious = 0
    var previous = 1
    yield(beforePrevious)
    yield(previous)
    while (true) {
		var nextValue = beforePrevious + previous
    	yield(nextValue)
		beforePrevious = previous
      previous = nextValue
    }
}

fun main(args: Array<String>) {
    fibonacci().take(4).toList().toString() eq
            "[0, 1, 1, 2]"

    fibonacci().take(10).toList().toString() eq
            "[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]"
}
```

# Library function
Consider,
- `count`
- `sortedByDescending`
- `mapNotNull`
- `getOrPut`

## `groupBy` vs `groupingBy`
 - `groupBy` return `Map` straightaway
- `groupingBy`  create a Grouping to be used later 
	- `eachCount`


