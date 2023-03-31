---
layout: post
title: "The Factory Method Pattern in Python"
date: "2023-03-31 01:09:00 +0800"
description: "The Factory Method Pattern in Python" # (optional)
img: "2023-03-31-cover-image-the-factory-method-design-pattern-in-python.webp" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['python', 'programming language']
categories: ['python', 'programming language']
---

# defination

**Factory Method is a creational design pattern used to create concrete implementations of a common interface.** It separates the process of creating an object from the code that depends on the interface of the object. 

Instead of using a complex if/elif/else conditional structure to determine the concrete implementation, the application delegates that decision to a separate component that creates the concrete object. With this approach, the application code is simplified, making it more reusable and easier to maintain.



![Advanced Python Tutorial]({{site.baseurl}}/assets/img/2023-03-30/bc-10.jpg)



#### 源自[从阮晓寰到“编程随想”：一个普通公民和“极客”如何成了“国家的敌人”？](https://ngocn2.org/article/2023-03-29-program-think-enemy-of-the-state/) 发布于 2023-03-29