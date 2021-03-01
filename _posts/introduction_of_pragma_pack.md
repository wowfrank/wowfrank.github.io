---
layout: post
title: "Introduction of C++ Pragma Pack (7)"
date: 2021-02-28 00:07:00 +0800
description: "Introduction of C++ Pragma Pack (7)" # (optional)
img: 2021-02-28-cover-image-of-cpp-notes.jpg # Add image post (optional)
fig-caption: "Introduction of C++ Pragma Pack (7)" # Add figcaption (optional)
tags: ['Programming', 'C++']
categories: ['Programming', 'C++']
---

**\#pragma pack** instructs the compiler to pack structure members with particular alignment. Most compilers, when you declare a struct, will insert padding between members to ensure that they are aligned to appropriate addresses in memory (usually a multiple of the type's size). This avoids the performance penalty (or outright error) on some architectures associated with accessing variables that are not aligned properly. For example, given 4-byte integers and the following struct:

```cpp
struct Test
{
   char AA;
   int BB;
   char CC;
};
```

The compiler could choose to lay the struct out in memory like this:

\|   1   \|   2   \|   3   \|   4   \|  

\| AA(1) \| pad.................. \|
\| BB(1) \| BB(2) \| BB(3) \| BB(4) \| 
\| CC(1) \| pad.................. \|

and sizeof(Test) would be 4 × 3 = 12, even though it only contains 6 bytes of data. The most common use case for the #pragma (to my knowledge) is when working with hardware devices where you need to ensure that the compiler does not insert padding into the data and each member follows the previous one. With #pragma pack(1), the struct above would be laid out like this:

\|   1   \|

\| AA(1) \|
\| BB(1) \|
\| BB(2) \|
\| BB(3) \|
\| BB(4) \|
\| CC(1) \|

And sizeof(Test) would be 1 × 6 = 6.

With #pragma pack(2), the struct above would be laid out like this:

\|   1   \|   2   \| 

\| AA(1) \| pad.. \|
\| BB(1) \| BB(2) \|
\| BB(3) \| BB(4) \|
\| CC(1) \| pad.. \|

And sizeof(Test) would be 2 × 4 = 8.

Order of variables in struct is also important. With variables ordered like following:

```cpp
struct Test
{
   char AA;
   char CC;
   int BB;
};
```

and with #pragma pack(2), the struct would be laid out like this:

\|   1   \|   2   \| 

\| AA(1) \| CC(1) \|
\| BB(1) \| BB(2) \|
\| BB(3) \| BB(4) \|

and sizeOf(Test) would be 3 × 2 = 6.