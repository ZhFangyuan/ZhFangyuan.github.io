---
title: java注解
categories: 
- java基础
---

[TOC]

## 1、概念

> 定义：注解（Annotation），也叫元数据。一种代码级别的说明。它是JDK1.5及以后版本引入的一个特性，与类、接口、枚举是在同一个层次。它可以声明在包、类、字段、方法、局部变量、方法参数等的前面，用来对这些元素进行说明，注释。 

定义可能比较抽象，其实我们平时看到的@Override、@Deprecated 等都是注解，其中@Override表示这个方法是重写的方法，@Deprecated表示这个方法被弃用。

除了这些java本身提供的一些注解，我们自己也可以自定义一些注解，下面举个例子。
<!--more-->

## 2、举个例子

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface InvokeLog {
    String methodName () default "";
}
```

上面的代码的意思是，我们定义了一个叫做InvokeLog的注解，以后这个注解就可以用在需要的方法上面了。我们需要关注的地方主要有三个。首先，我们定义的是一个注解，所以必须要有@interface注解，其他的注解都是可以省略的，其他的注解省略了就会使用默认的值。然后再来分析下比较重要的两个注解：@Target和@Retention

@Target注解的作用是定义这个注解使用的范围，ElementType.METHOD表示这个注解使用在方法上面的，再比如ElementType.FIELD表示这个注解使用在成员变量上面的。

@Retention注解的作用是定义这个注解起作用的范围，RetentionPolicy.RUNTIME表示这个注解在程序运行期间起作用，再比如RetentionPolicy.SOURCE表示这个注解只在编译期间起作用，编译完成之后就没用了。

