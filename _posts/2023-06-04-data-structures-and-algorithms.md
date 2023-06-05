---
layout: post
title: "Data Structures and Algorithms"
date: "2023-06-04 01:09:00 +0800"
description: "Data Structures and Algorithms" # (optional)
img: "2023-06-04-cover-image-data-structures-and-algorithms.png" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['Data Structure', 'Algorithm']
categories: ['DSA']
---

## What are Data Structures?

Data structure is a storage that is used to store and organize data. It is a way of arranging data on a computer so that it can be accessed and updated efficiently.

## Types of Data Structure

- Linear data structure
    1. Array Data Structure
    2. Stack Data Structure: LIFO (**Last In First Out**) principle
    3. Queue Data Structure: FIFO (**First In First Out**) principle
        - Simple Queue
        - Circular Queue
        - Priority Queue
        - Double Ended Queue (Deque)
    4. Linked List Data Structure
- Non-linear data structure
    1. Graph Data Structure
        - Spanning Tree and Minimum Spanning Tree
        - Strongly Connected Components
        - Adjacency Matrix
        - Adjacency List
    2. Trees Data Structure
        - Binary Tree
        - Binary Search Tree
        - AVL Tree
        - B-Tree
        - B+ Tree
        - Red-Black Tree

### Linear Vs Non-linear Data Structures

| Linear Data Structures    | Non Linear Data Structures |
|   :-----  |   :-----  |
| The data items are arranged in sequential order, one after the other.  | The data items are arranged in non-sequential order (hierarchical manner). |
| All the items are present on the single layer. | The data items are present at different layers. |
| It can be traversed on a single run. That is, if we start from the first element, we can traverse all the elements sequentially in a single pass. | It requires multiple runs. That is, if we start from the first element it might not be possible to traverse all the elements in a single pass. |
| The memory utilization is not efficient. | Different structures utilize memory in different efficient ways depending on the need. |
| The time complexity increase with the data size. | Time complexity remains the same.
| Example: Arrays, Stack, Queue | Example: Tree, Graph, Map |


## Master Theorem

The master method is a formula for solving recurrence relations of the form:

```
T(n) = aT(n/b) + f(n),
where,
n = size of input
a = number of subproblems in the recursion
n/b = size of each subproblem. All subproblems are assumed to have the same size.
f(n) = cost of the work done outside the recursive call, 
      which includes the cost of dividing the problem and
      cost of merging the solutions

Here, a ≥ 1 and b > 1 are constants, and f(n) is an asymptotically positive function.
```

### Master Theorem

If a ≥ 1 and b > 1 are constants and f(n) is an asymptotically positive function, then the time complexity of a recursive relation is given by

```
T(n) = aT(n/b) + f(n)

where, T(n) has the following asymptotic bounds:

    1. If f(n) = O(nlogb^(a-ϵ)), then T(n) = Θ(nlogb^a).

    2. If f(n) = Θ(nlogb^a), then T(n) = Θ(nlogb^a * log n).

    3. If f(n) = Ω(nlogb^(a+ϵ)), then T(n) = Θ(f(n)).

ϵ > 0 is a constant.
```

## Divide and Conquer Algorithm

- Divide: Divide the given problem into sub-problems using recursion.
- Conquer: Solve the smaller sub-problems recursively. If the subproblem is small enough, then solve it directly.
- Combine: Combine the solutions of the sub-problems that are part of the recursive process to solve the actual problem.

Applications:

- Binary Search
- Merge Sort
- Quick Sort
- Strassen's Matrix multiplication
- Karatsuba Algorithm

Advantages:

- The complexity for the multiplication of two matrices using the naive method is O(n3), whereas using the divide and conquer approach (i.e. Strassen's matrix multiplication) is O(n^2.8074). This approach also simplifies other problems, such as the Tower of Hanoi.
- This approach is **suitable for multiprocessing systems**.
- It makes efficient use of memory caches.

## 