---
title: Fibonacci numbers
date: 2021-07-24
tags: ["algorithm"]
---


# Fibonacci numbers
#cs/algorithm #algorithmic-toolbox

![](https://i.ibb.co/C1X8KGG/download.jpg)
- **Recursive call makes this algorithm too slow !** 


![](https://i.ibb.co/qDtFG1V/download-2.jpg)
- Computing same thing over and over again


![](https://i.ibb.co/Nm4YJy7/download-3.jpg)


# Implementation
## 1 Fibonacci
### naive
``` kotlin
fun calculateFibonacciNaive(n: Long): Long {
    return if (n <= 1) n else calculateFibonacciNaive(n - 1) + calculateFibonacciNaive(n - 2)
}
```

### correct
``` kotlin
fun calculateFibonacci(n: Long): Long {
    if (n < 2) {
        return n
    }
    val fibonacciArray = LongArray(n.toInt() + 1) { it.toLong() }
    for (index in 2..fibonacciArray.lastIndex) {
        fibonacciArray[index] = fibonacciArray[index - 1] + fibonacciArray[index - 2]
    }
    return fibonacciArray.last()
}
```

## 2 Last digit of Fibonacci number
### naive 
``` kotlin
fun getFibonacciLastDigitNaive(n: Int): Int {
    if (n <= 1)
        return n

    var previous = 0
    var current = 1

    for (i in 0 until n - 1) {
        val tmpPrevious = previous
        previous = current
        current = (current + tmpPrevious) % 10
    }

    return current % 10
} 
```

### correct
``` kotlin
fun calculateFibonacciLastDigit(n: Int): Int {
    if (n < 2) {
        return n
    }
    val fibonacciArray = IntArray(n + 1) { 0 }
    fibonacciArray[1] = 1
    for (index in 2..fibonacciArray.lastIndex) {
        fibonacciArray[index] = (fibonacciArray[index - 1] + fibonacciArray[index - 2]) % 10
    }
    return fibonacciArray.last()
}
```

## 5 Fibonacci number again
### Problem Introduction
In this problem, your goal is to compute 𝐹𝑛 modulo 𝑚, where 𝑛 may be really huge: up to 1014. For such values of 𝑛, an algorithm looping for 𝑛 iterations will not fit into one second for sure. Therefore we need to avoid such a loop.
To get an idea how to solve this problem without going through all 𝐹𝑖 for 𝑖 from 0 to 𝑛, let’s see what happens when 𝑚 is small — say, 𝑚 = 2 or 𝑚 = 3.
𝑖 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15
𝐹𝑖 0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610
𝐹𝑖 mod 2 0 1 1 0 1 1 0 1 1 0 1 1 0 1 1 0
𝐹𝑖 mod 3 0 1 1 2 0 2 2 1 0 1 1 2 0 2 2 1
Take a detailed look at this table. Do you see? Both these sequences are periodic! For 𝑚 = 2, the period is 011 and has length 3, while for 𝑚 = 3 the period is 01120221 and has length 8. Therefore, to compute, say, 𝐹2015 mod 3 we just need to find the remainder of 2015 when divided by 8. Since 2015 = 251 . 8 + 7, we conclude that 𝐹2015 mod 3 = 𝐹7 mod 3 = 1.
This is true in general: for any integer 𝑚 ≥ 2, the sequence 𝐹𝑛 mod 𝑚 is periodic. The period always starts with 01 and is known as Pisano period.

### naive
``` kotlin
 fun getFibonacciHugeNaive(n: Long, m: Long): Long {
    if (n <= 1) return n

    var previous: Long = 0
    var current: Long = 1

    for (i in 0 until n - 1) {
        val tmpPrevious = previous
        previous = current
        current += tmpPrevious
    }

    return current % m
}
```

### correct
``` kotlin
 fun getFibonacciHuge(n: Long, m: Long): Long {
    if (n < 2) {
        return n
    }
    if (m == 2L) {
        return n.rem(2).plus(1).rem(2)
    }
    val fibonacciArray = LongArray(m.times(m).toInt()) { it.toLong() }
    var cycleLength = 0
    for (index in 2..fibonacciArray.lastIndex) {
        fibonacciArray[index] = fibonacciArray[index - 1].plus(fibonacciArray[index - 2]).rem(m)
        if ((fibonacciArray[index - 1] == 1L).and(fibonacciArray[index] == 0L)) {
            cycleLength = index
            break
        }
    }
    return fibonacciArray[n.rem(cycleLength).toInt()]
}
```

## 6 Last digit of the sum of Fibonacci
### implementation
``` kotlin
 fun getFibonacciSumLastDigit(n: Long): Long {
    if (n < 2) {
        return n
    }
    val cycleLength = 60 // Pisano period of 10
    val targetIndex = n.rem(cycleLength).toInt()
    val fibonacciLastDigitArray = LongArray(targetIndex + 1) { it.toLong() }
    val fibonacciSumLastDigitArray = LongArray(targetIndex + 1) { it.toLong() }
    for (index in 2..targetIndex) {
        fibonacciLastDigitArray[index] = fibonacciLastDigitArray[index - 1].plus(fibonacciLastDigitArray[index - 2]).rem(10)
        fibonacciSumLastDigitArray[index] = fibonacciSumLastDigitArray[index - 1].plus(fibonacciLastDigitArray[index]).rem(10)
    }
    return fibonacciSumLastDigitArray.last()
}
```

## 7 last digit of the sum of the Fibonacci again
### implementation
``` kotlin
fun getFibonacciPartialSum(from: Long, to: Long): Long {
    val cycleLength = 60 // Pisano period of 10
    val fromIndex = from.rem(cycleLength).toInt()
    val toIndex = to.rem(cycleLength).toInt()
    val maxIndex = max(toIndex, fromIndex)

    val fibonacciLastDigitArray = LongArray(maxIndex + 1) { it.toLong() }
    val fibonacciSumLastDigitArray = LongArray(maxIndex + 1) { it.toLong() }
    for (index in 2..maxIndex) {
        fibonacciLastDigitArray[index] = fibonacciLastDigitArray[index - 1].plus(fibonacciLastDigitArray[index - 2]).rem(10)
        fibonacciSumLastDigitArray[index] = fibonacciSumLastDigitArray[index - 1].plus(fibonacciLastDigitArray[index]).rem(10)
    }
    if (from == 0L) {
        return fibonacciSumLastDigitArray[toIndex]
    }
    return fibonacciSumLastDigitArray[toIndex]
        .minus(fibonacciSumLastDigitArray[fromIndex - 1])
        .plus(10).rem(10)
} 
```

## 8 last digit of the sum of squares of Fibonacci numbers
### implementation
``` python
 def fibonacci_sum_squares(n):
    assert 0 <= n <= 10 ** 18

    if n < 2:
        return n

    cycle_length = 60
    if n % cycle_length == 0:
        return 0

    target_index = n % 60
    last_digits_of_fibonacci_numbers = [0] * (target_index + 1)
    last_digits_of_fibonacci_numbers[1] = 1
    last_digits_of_sum_of_square_of_fibonacci_numbers = last_digits_of_fibonacci_numbers.copy()
    for index in range(2, target_index + 1):
        last_digits_of_fibonacci_numbers[index] = (last_digits_of_fibonacci_numbers[index - 1]
                                                   + last_digits_of_fibonacci_numbers[index - 2]) % 10
        last_digits_of_sum_of_square_of_fibonacci_numbers[index] = \
            (last_digits_of_sum_of_square_of_fibonacci_numbers[index - 1] + last_digits_of_fibonacci_numbers[
                index] ** 2) \
            % 10
    return last_digits_of_sum_of_square_of_fibonacci_numbers.pop()
```


# Afterwards
- Read instruction more carefully
- Can find recursive pattern by finding first few pattern of recursive pattern
