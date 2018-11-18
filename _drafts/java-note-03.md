---
layout: post
title: Java 笔记 3 - 异常
date: 2017-10-05 14:14:20 +0300
author: 沉一叶
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: java.jpg # Add image post (optional)
tags: [Java]
categories: Java
---

对问题的描述，对其封装。问题是程序运行时出现的不正常情况。
 
体系：
java.lang.Throwable：
Throwable (共性：可抛性，可以被throw、throws操作)：
          |--Error
          |--Exception
无论错误还是异常，都有具体的子类，子类名以父类名为后缀。

处理方式：
1、抛出：
throw：抛出异常对象；用在函数内
throws：抛出异常类，可以多个；用在函数上
函数内容有throw，并没有处理，函数上一定要声明，否则编译失败
2、捕捉：
* try{
*          需要被检测的代码；
* }
* catch（异常类 变量）{
*          处理异常代码：（处理方式）
* }
* finally{
*          一定会执行的语句；
* }

处理原则：
多个catch，父类的放在下面
子父类覆盖时：
子类抛出的异常必须是父类异常的子类或者子集；父类或者接口没有抛出异常，子类覆盖只能try，不能抛出。

异常分两类：
checked exceptiom：Exception及其子类，编译时检测异常，抛出时函数必须声明（调用者处理）
unchecked exception：RuntimeException及其子类：运行时异常，编译时不检查，可以不声明（程序停止）

自定义异常
按照java的oop思想，对特有问题进行封装，定义类继承Exception或RuntimeException
可抛性；具备异常的共性方法。
异常信息要传递给父类的构造函数，在构造函数添加：super()