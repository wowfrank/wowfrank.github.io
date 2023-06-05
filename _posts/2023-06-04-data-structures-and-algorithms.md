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
            1. Full Binary Tree: A full Binary tree is a special type of binary tree in which every parent node/internal node has either two or no children.
            1. Perfect Binary Tree: A perfect binary tree is a type of binary tree in which every internal node has exactly two child nodes and all the leaf nodes are at the same level.
            1. Complete Binary Tree: A complete binary tree is just like a full binary tree, but with two major differences
            1. Degenerate or Pathological Binary Tree: A degenerate or pathological tree is the tree having a single child either left or right.
            1. Skewed Binary Tree: A skewed binary tree is a pathological/degenerate tree in which the tree is either dominated by the left nodes or the right nodes. 
            1. Balanced Binary Tree: It is a type of binary tree in which the difference between the height of the left and the right subtree for each node is either 0 or 1.
        - Binary Search Tree
            
            The properties that separate a binary search tree from a regular binary tree is:

            1. All nodes of left subtree are less than the root node
            1. All nodes of right subtree are more than the root node
            1. Both subtrees of each node are also BSTs i.e. they have the above two properties

            _In the graph below, The binary tree on the right isn't a binary search tree because the right subtree of the node "3" contains a value smaller than it._

            ![Binary Search Tree]({{site.baseurl}}/assets/img/2023-06-04/bst-vs-not-bst.webp)

        - AVL Tree
            
            AVL tree is a self-balancing **binary search tree** in which each node maintains extra information called a **balance factor** whose value is either -1, 0 or +1.
            
            Balance Factor = (Height of Left Subtree - Height of Right Subtree) or (Height of Right Subtree - Height of Left Subtree)

            AVL tree got its name after its inventor Georgy Adelson-Velsky and Landis.

            An example of a balanced avl tree is

            ![AVL Tree]({{site.baseurl}}/assets/img/2023-06-04/avl-tree-final-tree-1_0_2.webp)

            Left-Right and Right-Left Rotate:

            ![AVL Tree Left Rotation]({{site.baseurl}}/assets/img/2023-06-04/avl-tree-leftright-rotate-1.png)

            ![AVL Tree Right Rotation]({{site.baseurl}}/assets/img/2023-06-04/avl-tree-leftright-rotate-2.png)

        - B-Tree
            
            B-tree is a special type of self-balancing search tree in which each node can contain more than one key and can have more than two children. It is also known as a height-balanced m-way tree.

            ![B Tree]({{site.baseurl}}/assets/img/2023-06-04/b-tree.webp)

            **B-tree Properties**:
            
            1. For each node x, the keys are stored in increasing order.
            1. In each node, there is a boolean value x.leaf which is true if x is a leaf.
            1. If **n** is the order of the tree, each internal node can contain at most **n - 1** keys along with a pointer to each child.
            1. Each node except root can have **at most n** children and **at least n/2** children.
            1. All leaves have the same depth (i.e. height-h of the tree).
            1. The root has at least **2** children and contains a minimum of **1** key.
            1. If **n ≥ 1**, then for any **n-key** B-tree of height h and minimum degree t ≥ 2, h ≥ logt^(n+1)/2.

        - B+ Tree

            An important concept to be understood before learning B+ tree is multilevel indexing. In multilevel indexing, the index of indices is created as in figure below.

            ![B+ Tree]({{site.baseurl}}/assets/img/2023-06-04/multilevel-indexing.webp)

            ![B+ Search Tree]({{site.baseurl}}/assets/img/2023-06-04/search-tree.webp)

            **Properties of a B+ Tree**:

            1. All leaves are at the same level.
            1. The root has **at least 2** children.
            1. Each node except root can have a maximum of **m** children and **at least m/2** children.
            1. Each node can contain a maximum of **m - 1** keys and a minimum of **⌈m/2⌉ - 1** keys.

        - Red-Black Tree

## Linear Vs Non-linear Data Structures

| Linear Data Structures    | Non Linear Data Structures |
|   :-----  |   :-----  |
| The data items are arranged in sequential order, one after the other.  | The data items are arranged in non-sequential order (hierarchical manner). |
| All the items are present on the single layer. | The data items are present at different layers. |
| It can be traversed on a single run. That is, if we start from the first element, we can traverse all the elements sequentially in a single pass. | It requires multiple runs. That is, if we start from the first element it might not be possible to traverse all the elements in a single pass. |
| The memory utilization is not efficient. | Different structures utilize memory in different efficient ways depending on the need. |
| The time complexity increase with the data size. | Time complexity remains the same.
| Example: Arrays, Stack, Queue | Example: Tree, Graph, Map |


## Comparison between a B-tree and a B+ Tree

![B+ Tree]({{site.baseurl}}/assets/img/2023-06-04/B-tree-compare.webp)

![B+ Tree]({{site.baseurl}}/assets/img/2023-06-04/B+tree-compare.webp)

The data pointers are present only at the leaf nodes on a B+ tree whereas the data pointers are present in the internal, leaf or root nodes on a B-tree.

The leaves are not connected with each other on a B-tree whereas they are connected on a B+ tree.

Operations on a B+ tree are **faster** than on a B-tree.

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

**Master Theorem**

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



#### 源自[Data Structure and Algorithms](https://www.programiz.com/dsa/algorithm)