---
title: Greedy Algorithm
date: 2021-07-24
tags: ["algorithm"]
---

# Greedy Algorithm

# Main Ingredients 
- Safe Move
- Prove safety
- Solve subproblem 
- Estimate running time 

## Safe Move 
- A greedy choice is a safe move if there is an optimal solution consistent with this first move.

## Optimization
- Assume everything is somehow sorted 
- Which sort order is convenient?
- Greedy move can be faster after sorting 

## General Strategy
1. Make a greedy choice 
2. Prove that it is a safe move
3. Reduce to a subproblem
4. Solve the subproblem 

## Problem
### Maximum Salary
This is probably the most important problem in this course :). As the last question of an interview, your future boss gives you a few pieces of paper with a single number written on each of them and asks you to compose a largest number from these numbers. The resulting number is going to be your salary, so you are very motivated to solve this problem!

``` python
from itertools import permutations
from functools import cmp_to_key


def largest_number_naive(numbers):
    numbers = list(map(str, numbers))

    largest = 0

    for permutation in permutations(numbers):
        largest = max(largest, int("".join(permutation)))

    return largest


def compare(left: int, right: int):
    left_first = int(str(left) + str(right))
    right_first = int(str(right) + str(left))
    if left_first > right_first:
        return -1
    elif left_first < right_first:
        return 1
    else:
        return 0


def largest_number(numbers):
    numbers.sort(key=cmp_to_key(compare))
    return int("".join(map(str, numbers)))
```
- sort with single key has some limitation 
- Remember python `cmp_to_key`  function 

 