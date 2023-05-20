---
layout: post
title: "Notes of Data Structures and Algorithms in Python"
date: "2023-05-16 01:09:00 +0800"
description: "Data Structures and Algorithms in Python" # (optional)
img: "2023-05-06-cover-image-notes-of-python-data-structures-and-algorithms.png" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['Data Structure', 'Algorithm', 'Python']
categories: ['Python']
---

## Recursion

Recursive functions typically follow this pattern:

- There are one or more base cases that are directly solvable without the need for further recursion. <!--more-->
- Each recursive call moves the solution progressively closer to a base case.

```python
def factorial(n):
    return 1 if n <= 1 else n * factorial(n - 1)
```

```python
import statistics

# Args: numbers is a list
def quicksort(numbers: list = []):
    if len(numbers) <= 1:
        return numbers
    else:
        pivot = statistics.median(
            [
                numbers[0],
                numbers[len(numbers) // 2],
                numbers[-1]
            ]
        )
        items_less, pivot_items, items_greater = (
            [n for n in numbers if n < pivot],
            [n for n in numbers if n == pivot],
            [n for n in numbers if n > pivot]
        )

        return (
            quicksort(items_less) +
            pivot_items +
            quicksort(items_greater)
        )
```

![Binary Search Tree (BST)]({{site.baseurl}}/assets/img/2023-05-16/quicksort-recurrsion.webp)

## Linear Search VS Binary Search

**In the case of linear search:**

1. The _time complexity_ of the algorithm is **cN** for some fixed <!--more-->constant **c** that depends on the number of operations we perform in each iteration and the time taken to execute a statement. Time complexity is sometimes also called the **running time** of the algorithm.

1. The **space complexity** is some constant **c'** (independent of **N**), since we just need a single variable _position_ to iterate through the array, and it occupies a constant spance in the computer's memory(RAM).

> **Big O Notation**: Worst-case complexity is often expressed using the **Big O Notation**, in the Big O, we drop fixed constants and lower powers of variables to capture the trend of relationship between the size of the input and the complexity of the algorithm i.e. if the complexity of the algorithm is cN^3 + dN^2 + eN + f, in the Big O notation it is expressed as **O(N^3)**

Thus, the time complexity of linear search is **O(N)** and its space complexity is **O(1)**

**In the case of binary search:**

**k = log N**

Where log refers to log to the base 2. Therefore, our algorithm has the time complexity **O(log N)**. This fact is often stated as: binary search **runs** in logarithmic time. You can veify that the space complexity of binary search is **O(1)**.

The real difference be tween the complexities: **O(N**) and **O(log N)**


## Binary Search Tree

A _binary search tree (BST)_ is a node-based binary tree data structure that has the following properties. 

- The left subtree of a node contains only nodes with keys less than the node’s key.
- The right subtree of a node contains only nodes with keys greater than the node’s key.
- Both the left and right subtrees must also be binary search trees.
- Each node (item in the tree) has a **distinct key**.

**Binary Search Tree (BST) Traversals – Inorder, Preorder, Post Order**

![Binary Search Tree (BST)]({{site.baseurl}}/assets/img/2023-05-16/Screenshot20220730at30104AM-660x431.png)

Output: 

- Inorder Traversal: 10 20 30 100 150 200 300
- Preorder Traversal: 100 20 10 30 200 150 300
- Postorder Traversal: 10 30 20 150 300 200 100

**Inorder Traversal:**

At first traverse left subtree then visit the root and then traverse the right subtree.

- Traverse left subtree
- Visit the root and print the data.
- Traverse the right subtree

```python
# Python3 code to implement the approach
 
# Class describing a node of tree
class Node:
    def __init__(self, v):
        self.left = None
        self.right = None
        self.data = v
 
# Inorder Traversal
def printInorder(root):
    if root:
        # Traverse left subtree
        printInorder(root.left)
         
        # Visit node <== Main different code
        print(root.data, end=" ")
         
        # Traverse right subtree
        printInorder(root.right)
```

Time complexity: O(N), Where N is the number of nodes.
Auxiliary Space: O(h), Where h is the height of tree

**Preorder Traversal:**

At first visit the root then traverse left subtree and then traverse the right subtree.

- Visit the root and print the data.
- Traverse left subtree
- Traverse the right subtree

```python
class Node:
    def __init__(self, v):
        self.data = v
        self.left = None
        self.right = None
 
# Preorder Traversal
def printPreOrder(node):
    if node is None:
        return
    # Visit Node <== Main different code
    print(node.data, end=" ")
 
    # Traverse left subtree
    printPreOrder(node.left)
 
    # Traverse right subtree
    printPreOrder(node.right)
```

Time complexity: O(N), Where N is the number of nodes.
Auxiliary Space: O(H), Where H is the height of the tree

**Postorder Traversal:**

At first traverse left subtree then traverse the right subtree and then visit the root.

- Traverse left subtree
- Traverse the right subtree
- Visit the root and print the data.

```python
class Node:
    def __init__(self, v):
        self.data = v
        self.left = None
        self.right = None
 
# Preorder Traversal
def printPostOrder(node):
    if node is None:
        return
 
    # Traverse left subtree
    printPostOrder(node.left)
 
    # Traverse right subtree
    printPostOrder(node.right)
     
    # Visit Node <== Main different code
    print(node.data, end = " ")
```

Time complexity: O(N), Where N is the number of nodes.
Auxiliary Space: O(H), Where H is the height of the tree

### Convert a normal BST to Balanced BST

- Traverse given BST in inorder and store result in an array. This step takes O(n) time. Note that this array would be sorted as inorder traversal of BST always produces sorted sequence.
- Build a balanced BST from the above created sorted array using the recursive approach discussed here. This step also takes O(n) time as we traverse every element exactly once and processing an element takes O(1) time.

### check if a Binary Tree is BST or not

**Naive Approach:**

The idea is to for each node, check if max value in left subtree is smaller than the node and min value in right subtree greater than the node.

**Approach (Efficient):**

The idea is to write a utility helper function isBSTUtil(struct node* node, int min, int max) that traverses down the tree keeping track of the narrowing min and max allowed values as it goes, looking at each node only once. The initial values for min and max should be INT_MIN and INT_MAX — they narrow from there. 

Note: This method is not applicable if there are duplicate elements with the value INT_MIN or INT_MAX.

```python
# Python program to check if a binary tree is bst or not
 
INT_MAX = 4294967296
INT_MIN = -4294967296
 
# A binary tree node
class Node:
 
    # Constructor to create a new node
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None
 
 
# Returns true if the given tree is a binary search tree
# (efficient version)
def isBST(node):
    return (isBSTUtil(node, INT_MIN, INT_MAX))
 
# Returns true if the given tree is a BST and its values
# >= min and <= max
def isBSTUtil(node, mini, maxi):
 
    # An empty tree is BST
    if node is None:
        return True
 
    # False if this node violates min/max constraint
    if node.data < mini or node.data > maxi:
        return False
 
    # Otherwise check the subtrees recursively
    # tightening the min or max constraint
    return (isBSTUtil(node.left, mini, node.data - 1) and
            isBSTUtil(node.right, node.data+1, maxi))
```

**Check whether the binary tree is BST or not using inorder traversal:**

The idea is to use Inorder traversal of a binary search tree generates output, sorted in ascending order. So generate inorder traversal of the  given binary tree and check if the values are sorted or not

- Do In-Order Traversal of the given tree and store the result in a temp array. 
- This method assumes that there are no duplicate values in the tree
- Check if the temp array is sorted in ascending order, if it is, then the tree is BST.

```python
# Python3 program to check
# if a given tree is BST.
import math
 
# A binary tree node has data,
# pointer to left child and
# a pointer to right child
class Node:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None
 
 
def isBSTUtil(root, prev):
    # traverse the tree in inorder fashion
    # and keep track of prev node
    if (root != None):
        if (isBSTUtil(root.left, prev) == True):
            return False
 
        # Allows only distinct valued nodes
        if (prev != None and
                root.data <= prev.data):
            return False
 
        prev = root
        return isBSTUtil(root.right, prev)
 
    return True
 
 
def isBST(root):
    prev = None
    return isBSTUtil(root, prev)
```

## Self-Balanceing Binary Trees and AVL Trees

A self-balancing binary tree remains balanced after every insertion or deletion. Several decades of research has gone into creating self-balancing binary trees, and many approaches have been devised e.g. B-trees, Red Black Trees and AVL (Adelson-Velsky Landis) Trees.

Self-balancing in AVL trees is achieved by tracking the **_balance factor_** (difference between the height of the left subtree and the right subtree) for each node and _rotating_ unbalanced subtrees along the path of insertion/deletion to balance them.

Dictonary in Python is **not** Binary Search Tree structure. It uses **Hash Tables**.
