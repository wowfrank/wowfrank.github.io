---
layout: post
title: "线性代数极简入门"
date: "2023-04-12 01:09:00 +0800"
description: "线性代数极简入门" # (optional)
img: "2023-04-12-cover-image-linear-algebra-in-simple-description.jpg" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['math', 'linear algebra']
categories: ['math', 'linear algebra']
---

# 复习：向量

## 回忆高中数学

**向量**，就是有方向的量

![Linear Algebra]({{site.baseurl}}/assets/img/2023-04-12/v2-ae135e52fa1694d55e4b1ae36c32a447_720w.webp)

如上的二维向量在代数上可以用2个维度上的分量表示<!--more-->： **_(m,n)_**。同理，找**_n_**个数写到括号里，就构成了一**_n_**维向量

> 三维以内的向量可以用几何表示，可以直观地理解。  
> 现实中大部分问题其实是更高维度的向量，它们无法用几何表示，但性质是相同的。  
> 一言以蔽之，几何是低维的代数，代数是高维的几何。

**向量加法**：利用三角形法则

![Linear Algebra]({{site.baseurl}}/assets/img/2023-04-12/v2-e49d7c68389d111156fb706802c3bd6e_720w.webp)

在代数上表示，就是分量分别相加。这个性质通过简单的平移可以看出。

![Linear Algebra]({{site.baseurl}}/assets/img/2023-04-12/v2-a926127b273c1bf14af145ed6033cc4b_720w.png)

**向量点积**：

![Linear Algebra]({{site.baseurl}}/assets/img/2023-04-12/v2-38d52dd63b3e39415f70a32bc1c0cc6a_720w.webp)

点积的几何意义是投影后的模长乘积。它其实也能用矩阵解释。

**向量的长度**：

![Linear Algebra]({{site.baseurl}}/assets/img/2023-04-12/v2-f8e3eec4b37de9de4cf317e558f85ec7_720w.png)

**向量的夹角**：

![Linear Algebra]({{site.baseurl}}/assets/img/2023-04-12/v2-f62262ad85a6b007395f2c620fbbf16c_720w.png)

## 来点大学的

**向量空间**：

我们可能也发现了，向量是个有方向有长度的箭头，那么它肯定存在于某个空间中。直观来说，二维向量就在二维空间中，三维向量就在三维空间中……

![Linear Algebra]({{site.baseurl}}/assets/img/2023-04-12/v2-f62262ad85a6b007395f2c620fbbf16c_720w.png)

而其实我们研究的线性代数，它对向量空间的定义突出一个“线性”：

向量空间**_V_**是一个集合，其元素是向量。该集合对于其元素向量的加法及数乘两种运算封闭。即：

- 若$a \in V$, $b \in V$，则$a+b \in V$
- 若$a \in V$, $k \in R$，则$ka \in V$

> 你可能不知道，向量空间又叫线性空间。

向量空间并不一定是$R^n$，也可以是它们的子集。下面几个空间也是向量空间：

![Linear Algebra]({{site.baseurl}}/assets/img/2023-04-12/v2-4874d0bce36816a344721d32ad1ca59a_720w.webp)

向量组：就是 
 中的一组向量。显然，它们是同维度的。

向量组合：对 
 中的一组向量 
 ，指定一组实数 
, 那么向量
 称为该向量组的线性组合，或者说 
 能被这组向量线性表示。

举个例子：对于 
 这三个向量：

![Linear Algebra]({{site.baseurl}}/assets/img/2023-04-12/v2-17a4364803773016eabf7924def5aec8_720w.webp)

那么黄色就是 
 的线性组合：

![Linear Algebra]({{site.baseurl}}/assets/img/2023-04-12/v2-857cc37336ed6222982c77e01f0f1564_720w.webp)

线性相关与线性无关：向量组中的任一向量都不能被其它向量线性表示，就说向量组线性无关；否则就是线性相关。

再举个例子： 
 是线性无关的， 
黄
 是线性相关的。

张成空间：一个向量组 
 的所有线性组合构成的集合 
 （显然是个向量空间），称为该向量组的张成空间，记为 
 . 或称该向量组张成 
 。

![Linear Algebra]({{site.baseurl}}/assets/img/2023-04-12/v2-fe0437b8aa6016d0c4512dfbaec9fefa_720w.webp)

向量空间的基：如果一个线性无关的向量组 
 张成向量空间 
 ，则称向量组 
 是空间 
 的一个基。

基有这么个性质： 
 中的任何向量 
 都可被唯一地表示为：

![Linear Algebra]({{site.baseurl}}/assets/img/2023-04-12/v2-81583779a40d6308026e5089984d172b_720w.webp)

**向量空间的维度**：就是一组基的向量个数。


其中 
（
 就相当于 
 在这组基中的坐标。如果 
 是标准基（或自然基），其实就变成直角坐标系了。






#### 参考

  - [DESIGN PATTERNS in PYTHON](https://refactoring.guru/design-patterns/python)
  - 有具体代码例子 [Design Patterns](https://sourcemaking.com/design_patterns)
