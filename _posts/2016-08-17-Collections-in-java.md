---
layout: post
title:  "Java集合框架--List"
date:   2016-08-17 14:03:28 +0800
categories: Java
---
# 概述
`List`的常用子类主要有`ArrayList`,`LinkedList`这样的非线程安全类，还有`Stack`,`Vector`,`CopyOnWriteArrayList`这样的线程安全类。这里说一下每个类的数据结构及其实现方法。

# ArrayList
`ArrayList`从数据结构上来说是一个数组，支持动态扩张，但不支持动态收缩。
