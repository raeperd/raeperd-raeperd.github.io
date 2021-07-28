---
title: Greatest Common Divisor
date: 2021-07-24
tags: ["algorithm"]
---


# Greatest Common Divisor

## Euclidean algorithm
![](https://i.ibb.co/MD1p9QN/download.jpg)

![](https://i.ibb.co/Cm43XZq/download-2.jpg)

``` kotlin
fun euclideanGCD(a, b) {
	if (b == 0)
		reutrn a
	return euclideanGCD(b, a % b)
}
```
- Each step reduces the size of numbers by about a factor of 2 
- Takes about log(ab) steps
- GCDs of 100 digit numbers takes about 600 steps
- Each step a single division

# Summary 
- Naive algorithm is too slow 
- The correct algorithm is much better
- Finding the correct algorithm **requires something** interesting about the problem

# Implementation 
## GCD
### naive 
``` kotlin
fun gcdNaive(a: Int, b: Int): Int {
    var currentGcd = 1
    var d = 2
    while (d <= a && d <= b) {
        if (a % d == 0 && b % d == 0 && currentGcd < d) {
            currentGcd = d
        }
        ++d
    }
    return currentGcd
} 
```

### correct
``` kotlin
 fun gcdEuclideanMethod(a: Int, b: Int): Int {
    if (b == 0) {
        return a
    }
    return gcdEuclideanMethod(b, a % b)
}
```
- Second parameter always less then equal to first parameter by definition of remainder

## LCM
### naive
``` kotlin
 fun lcmNaive(a: Int, b: Int): Long {
    for (l in 1..a.toLong() * b)
        if (l % a == 0L && l % b == 0L)
            return l

    return a.toLong() * b
}
```

### correct
``` kotlin
 fun lcmEuclideanMethod(a: Int, b: Int): Long {
    val gcd = gcdEuclideanMethod(a, b)
    return a.div(gcd).times(b.toLong())
}
```
