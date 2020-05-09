---
layout: article
aside:
  toc: true
title:  "北京面试历险记一"
tag: 面试
---
* content
{:toc}


七月份并不是一个面试的好季节，但我还是义无反顾的去面了。以下是一些面试经验总结，以及所经历的一些面试题，以此警醒自己，也希望能对后来者有所帮助。

## JAVA基础
### volatile关键字简述
并发有三个特性，原子性、可见性、有序性，`volatile`满足后两者，但不满足原子性（这里有个冷知识，对于Long以及Double这样的8字节数据，在32位机下，由于数据总线一次性只能读入4个字节而有可能无法保证原子性，JVM推荐实现原子性，但HotSpot似乎并没有这么做，因此需要对Long以及Double加`volatile`以保证写入与读取的原子性）

### synchronized 与 Lock 的区别
`synchronized`是`JAVA`的关键字之一，作用是可以保证线程同步，`Lock`是JDK中提供的一个接口，作用是可以显式的进行加锁与释放，并且可以通过不同的实现来完成不同的需求。至于`synchronized`与`Lock`的详细区别与介绍，请参考后续文章。


### 单例模式
常用的有三种方式

```java
public enum Singleton {
  INSTANCE;
  public void function() {
    // do work
  }
}
```
枚举模式，简单明了，JVM实现自带单例，JDK1.5以后建议使用，缺点是无法继承抽象类

```java
public class Singleton {
  private static final Singleton instance = new Singleton();
  private Singleton() {}
  public static Singleton getInstance() {
    return instance;
  }
}
```
用私有化构造方法，初始化时new出一个对象的方式，也能实现一个单例，缺点是在启动时该单例即初始化完成，如果初始化过程十分耗时，且很占资源，可能导致初始化过程较慢

```java
public class Singleton {
  private class Holder {
    static final Singleton instance = new Singleton();
  }
  private Singleton() {}
  public static Singleton getInstance() {
    return Holder.instance;
  }
}
```
与上面一个实现方式相比，优点是不会在启动时加载，而是在调用时加载，避免了启动的耗时，缺点就是第一次调用会比较耗时

### 生产者消费者
用`synchronized`与`Lock`实现生产者与消费者模式，着重考察`Lock`中`Condition`的用法及其实现原理。

### GC算法
GC算法的原理，包括堆的结构，young GC与full GC，常用GC算法说明。一般举例CMS，因为CMS对于系统的吞吐量影响较小，适用于IO密集型系统。GC算法的过程，可以参考[what-is-garbage-collection](https://plumbr.eu/handbook/what-is-garbage-collection)

### HashMap与ConcurrentHashMap
HashMap的实现原理，多线程情况下存在什么问题，什么场景会触发这些问题。使用ConcurrentHashMap能够解决什么问题，ConcurrentHashMap的实现原理，JDK1.7 与 JDK1.8 的实现上有什么区别，等等等等。

## MySQL

### 索引
索引的实现，B+树与B树的区别，为何不用B树做索引，联合索引的实现，最左前缀的原理。

### MVCC
MySQL的MVCC，或者说类MVCC是如何实现的，注意undo log 与 redo log，事务版本号。

## 框架与协议

### NIO的使用

### SpringMVC的使用
Spring的事务管理，Spring的分发机制等

### Zookeeper的使用
分布式协同一致性，主从的备份与同步，选主算法，Zookeeper在dubbo中的使用

### dubbo的使用
dubbo的一些实现原理，主要是看是否有过阅读源码的经历

## 其他
还会问一些与工作内容相关的问题，比如系统架构，系统的设计，数据库的设计等等，都是需要平时工作中关注的点，不能仅仅满足于实现需求，需要弄清楚系统的特性，怎么实现这个需求，为什么要这么实现这个需求，等等等等。
