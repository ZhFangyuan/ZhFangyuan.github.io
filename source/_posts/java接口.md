---
title: java接口
date: 2021-11-11 23:04:49
categories:
- java基础
tags:
---

## 1、修饰符

接口前加上public，则可以被所有类继承；若不显示添加public修饰符，则只能被同一个包中的类继承。

## 2、抽象方法

抽象方法可以继承接口，但是可以不用像普通类一样实现这些接口，即可以只实现接口中的部分方法。

## 3、接口方法

接口中的方法都是public abstract类型的，因此对于一个接口可以不用显示地写出public和abstract两个修饰符。

## 4、常量

接口中不能拥有变量，但是可以拥有常量，且这些常量前地修饰符为public、final、static，这几个修饰符默认不用写。

## 5、接口回调

接口回调是指让接口调用实现类的方法。举个例子。

```java
public interface Hello {
    String sayHello(String str);
}

public class HelloImpl(String str) implements Hello {
    @Override
    public String sayHello(String str) {
        System.out.println(str);
    }
}

Hello hello = new HelloImpl();
hello.sayHello("hello world!");
```

回调指的是Hello hello = new HelloImpl();这一行代码，我们在开发的过程中经常会用到接口回调，我们可以把hello看作一个指针，指向了实例对象HelloImpl，接着就可以操作HelloImpl对象的各个方法了。