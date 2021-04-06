---
title: "Kotlin Conventions"
date: 2021-03-16T19:58:54+09:00
categories: ["Programming"]
tags: ["kotlin", "kotlin-for-java-developer"]
---

## operator overloading
```kotlin
a + b 
a.plus(b)

operator fun Point.plus(other: Point): Point {
  return Point(x + other.x, y + other.y)
}
```

| expression | function name |
| ---------- | ------------- |
| a + b      | plus          |
| a - b      | minus         |
| a * b      | times         |
| a / b      | div           |
| a % b      | mod           |


## Unary operations

```kotlin
-a
a.unaryMinus()
```

| expression | function name |
| ---------- | ------------- |
| +a         | unaryPlus     |
| -a         | unaryMinus    |
| !a         | not           |
| ++a, a++   | inc           |
| --a, a--   | dec           |
|            |               |

```kotlin
val lsit = listOf(1,2,3)
val newList = list + 2

val mutableList = mutableListOf(1,2,3)
mutableList += 4 // plusAssign
```

* on immutable, create new list
* on mutable, calls `plusAssign`

## Conventions
| symbol | translated to                  |
| ------ | ------------------------------ |
| a > b  | a.comparedTo(b) > 0            |
| a >= b | a.comparedTo(b) >= 0           |
| a == b | a.equals(b) // also check null |

### Accessing elements by index: `[]`

```kotlin
class Board {}

board[1, 2] = 'x'

operator fun Board.get(x: Int, y: Int): Char { .. }
operator fun Board.set(x: Int, y: Int, value: Char) { .. }
```
 

## The `in` convention

```kotling
a in c 
c.contains(a)
```
 

## `rangeTo` convention

```kotlin
start..end 
start.rangeTo(end)
```
 

### The `interator` convention

```kotlin
operator fun CharSequence.iterator(): CharInterator
for (c in "abc") {}
```
 

## Destructuring declarations
```kotlin
val (first, second) = pair
```
 

![](/Conventions/6A278B21-25F7-4B44-9F80-A1EBF849E262.jpeg)
Whenever the expression has the right number of operator competent and functions, it can be used as the right hand side for this destructuring in initialization, also in for loop or inside the lambda.
 

![](/Conventions/7B3B0952-117E-46E7-9200-D0282FF2A74E.jpeg)
You're always surrounded with a parenthesis of the variables which are the result of the destructuring.
 

![](/Conventions/20EAA89B-13A4-42BA-98A1-2D8CC4F64E05.jpeg)
Then, it takes part in destructuring, but the variable is not created.  
**For extension, use name conventions rather than interface**
