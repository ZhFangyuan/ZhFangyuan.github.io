---
title: Integer.valueOf和Integer.parseInt
date: 2021-11-06 16:37:17
categories: 
- java基础
tags:
---

## 1、Integer.valueOf

Integer.valueOf有三个构造方法，分别是Integer.valueOf(int i)、Integer.valueOf(String s)和Integer.valueOf(String s,int radix)。

<!--more-->

需要注意的有两点：

1） Integer.valueOf返回的是包装类Integer，即返回的是一个数字对象，其内部实际上调用了Integer.parseInt，这个很多博客中都有说到。

2） Integer.valueOf(int i)这个方法不仅可以传入int类型的数字，还可以传入char类型的字符，当传入的是char时，返回的是char的ascii码值，例如：

```java
System.out.println(Integer.valueOf('a'));
// 输出结果为a的ascii码值97
```

## 2、Integer.parseInt

Integer.parseInt有两个构造方法，分别是Integer.parseInt(String s)和Integer.parseInt(String,int radix)，用于将字符串转换为数字。