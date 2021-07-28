---
title: Divide and Conquer
date: 2021-07-24
tags: ["algorithm"]
---


# Binary Search

![So let's look at the problem statement for searching in a sorted array.](https://i.ibb.co/F3K1T3T/search-in-sorted-array.jpg)

- monotonic non-decreasing array



![But if we search for 70, we'll also return 7.](https://i.ibb.co/vzD3kCN/search-in-a-sorted-array.jpg)



``` python
def binary_search(array: List[int], low: int, high: int, key: int):
  if high < low:
    return low -1 
  mid = int(low + (high - low) / 2)
  if key == array[mid]:
    return mid
  if key < array[mid]:
    return binary_search(array, low, mid - 1, key)
  if key > array[mid]:
    return binary_search(array, mid + 1, high, key)
```



![runtime-of-binary-search](https://i.ibb.co/KjDTx4r/runtime-of-binary-search.jpg)



``` python
def binary_search(array: List[int], low: int, high: int, key: int):
  while low <= high:
    mid = int(low + (high - low) / 2)
    if key == array[mid]:
      return key
    if key < array[mid]:
      high = mid - 1
    else: # key > array[mid]
      low = mid + 1
  return low - 1
```



# Polynomial Multiplication

## Problem overview and Naive solution

### Problem Overview

![polynomail-multiplication](https://i.ibb.co/ZM6Wv1Q/polynomail-multiplication.jpg)



![multiplying-polynomials](https://i.ibb.co/B21CJyM/multiplying-polynomials.jpg)



### Naive Algorithm 

``` python
def multiply_polynomial(a: List[int], b: List[int], n: int) -> List[int]:
  result = 2 * n * [0]
  for i in range(n):
    for j in range(n):
      result[i + j] = result[i + j] + a[i] + b[j]
  return result
```



## Naive Divide and Conquer Algorithm

![multiplying-polynomials-with-naive-divide-and-conquer](https://i.ibb.co/vmLsJzC/multiplying-polynomials-with-naive-divide-and-conquer.jpg)



![multiplying-polynomials-with-naive-divide-and-conquer-example](https://i.ibb.co/d70x3CG/multiplying-polynomials-with-naive-divide-and-conquer-example.jpg)



Mult2 doesn't match the problem statement because it requires two additional parameters a_l*a**l* and b_l*b**l*. These parameters are used as part of the recursion to specify the lower bound of the beginning of the sub-polynomials that are being multiplied (Mult2 multiplies the polynomials specified by the coefficients A[a_l\ldots a_l + n - 1*a**l*…*a**l*+*n*−1] and B[b_l\ldots b_l + n - 1*b**l*…*b**l*+*n*−1]).

We can write a wrapper around Mult2 that hides these parameters:
