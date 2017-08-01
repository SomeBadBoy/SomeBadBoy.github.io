---
layout: post
title:  "Java集合框架--概述"
date:   2016-08-05 17:06:38 +0800
categories: Java
---
# 概述
`Java`中的集合框架有两种，一种是`Map`，一种是`Collection`，`Collection`是一种线性的数据结构，`Map`则是一种key-value形式的数据结构。`Java`中的集合框架十分常用，比如`HashMap`，`ArrayList`，`LinkedList`等，下面介绍一下集合框架的关系，后续再深入每个类的内部，分析其实现原理与应用场景。

# Collection
![](http://i1.piimg.com/567571/fb3a6966923d6f86.png)
`Collection`的派生类主要有三种，`List`，`Set`，`Queue`。

`List`在数据结构上可以称为链表，其特点是它记录了每个元素所在的位置，可以通过`get(int index)`方法获取对应位置上的元素，或者通过`set(int index, E value)`向指定位置插入元素，该位置之后的元素将依次后移一位。也可以通过`indexOf(E value)`获取该元素在List中对应的位置。典型应用是`ArrayList`,`LinkedList`,`Stack`。

`Set`在数据结构上可以称为集合，其特点是它所包含的元素互不相同，其实也就是数学意义上的`集合`。元素之间是否相同的判断，是调用`equals(Object o)`方法进行判断，当不实现`equals(Object o)`方法时，判断的是这两个元素在内存中的地址是否相同。典型应用是`HashSet`,`TreeSet`。

`Queue`在数据结构上可以称为队列，其特点是队列中的元素在一个方向上总是先进先出的，可以通过`offer(E value)`向队尾插入元素，也可以通过`peek(E value)`从队首获取一个元素。`Queue`的一个子类`Deque`是一个双向队列，也就是既可以向对尾插入元素，也可以向队首插入元素，既可以从队首获取元素，也可以从队尾获取元素。在一个方向上，其同样满足队列的先进先出特性。典型应用有`ArrayBlockingQueue`,`LinkedList`,`PriorityQueue`。

# Map
![](http://i1.piimg.com/567571/a7346c2ed2dc4cf0.png)
`Map`是`key`与`value`之间的映射关系，这种关系可以是一对一，一对多，多对一或多对多，根据不同的实现会有不同的映射关系与限制。常用的`Cache`其实也是一种`key`与`value`的映射关系。典型应用有`HashMap`,`TreeSet`,`LinkedHashMap`。

# 结语
第一篇我们总览了`Java`的集合框架，下面将逐一介绍`Collection`与`Map`的常用实现以及其实现原理与核心算法。
