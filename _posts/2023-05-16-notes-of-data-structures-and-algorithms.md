---
layout: post
title: "Notes of Data Structures and Algorithms in Python"
date: "2023-05-16 01:09:00 +0800"
description: "Data Structures and Algorithms in Python" # (optional)
img: "2023-05-06-cover-image-notes-of-chatgpt-prompt-engineering-for-developers.jpg" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['Data Structure', 'Algorithm', 'Python']
categories: ['Python']
---

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


