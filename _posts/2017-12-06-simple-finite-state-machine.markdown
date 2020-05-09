---
layout: article
aside:
  toc: true
title:  "状态机分享"
tag: JAVA
---
* content
{:toc}


# 1 概念描述

## 1.1 有限状态机

摘录一段wiki的介绍：

> 有限状态机（英语：finite-state machine，缩写：FSM）又称有限状态自动机，简称状态机，是表示有限个状态以及在这些状态之间的转移和动作等行为的数学模型。

按照使用场景分类，FSM可以分为接收器/识别器和转换器，简单来说，就是接收器会输出一个二元的结果，可以以“是”或“否”来表明是否接受当前输入，而转换器则没有确定的输出结果。

再引用一段wiki的表述，来说明一个FSM需要的五个要素：输入，状态集，初始状态，转移函数，最终状态

>![][inputSymbol] 是输入字母表（符号的非空有限集合）。
>
>![][initStateSet] 是状态的非空有限集合。
>
>![][initState] 是初始状态，它是 ![][initStateSet] 的元素。在非确定有限状态自动机中，![][initState] 是初始状态的集合。
>
>![][stateTransition] 是状态转移函数： ![][stateTransitionFormulator]。
>
>![][finalStateSet] 是最终状态的集合，![][initStateSet] 的（可能为空）子集。

在有限状态机中，可以进一步细分为确定 **有限状态机（DFA）** 以及 **非确定有限状态机（NFA）** ，简单来说，这两者之间的区别就是DFA对于每一个输入所转移的下一个状态是确定的，而NFA对于一个输入可能会有多个转移状态。需要说明的是NFA与DFA是等价的，可以相互转换，转换方式一般使用幂集构造方式。一般来说，交易系统中的状态机都是DFA，因此以下主要说明DFA相关的实现。

### 1.1.1 一个典型的NFA状态机

![一个典型的NFA状态机][NFAexample]

### 1.1.2 一个典型的DFA状态机

![一个典型的DFA状态机][DFAexample]

## 1.2 事件驱动

**事件驱动（Event-Driven programming）** 是一种通过事件来驱动程序流转的一种 **编程范式（Programming paradigm）**，一般的事件有：鼠标/键盘输入、消息等，在事件触发时，通过回调函数来处理相关事件。还有一些典型的编程范式比如 **单线程** 与 **多线程**。单线程（同步）其实就是将所有任务串行的在单个线程中依次执行，单线程最大的问题就是由于任务的串行执行导致的性能问题，以及多任务之间隐藏的耦合和依赖问题，一般单线程模型只在一些简单场景或者需要强一致性的场景下使用；多线程是将任务拆解之后，放到多个线程中并行的执行，这能解决单线程中性能问题以及多任务之间的耦合和依赖问题，但是多线程一个痛点就是callback hell，也就是线程切换带来的上下文传递的问题，还有一个就是多任务之间的调度问题，也是需要处理的一个点。事件驱动则解决了上述几个痛点

1. 性能低下的问题：由于是异步事件触发，而非串行执行，因此执行时间为最长任务的执行时间；
2. 隐含的耦合问题：通过事件触发把任务完全的解耦，每个任务之间不存在交集，不互相感知，只做份内的事；
3. callback hell：事件触发是异步的，但是执行每个任务可以是同步的，避免了上下文切换导致的callback hell。

关于事件驱动的编程模型，这里就不展开了，有兴趣都可以看一看[Martin fowler老爷子的博客](https://martinfowler.com/)，真·大神。

## 1.3 Questions

以下状态机属于DFA还是NFA呢？

# 2 开源框架 spring-statemachine

## 2.1 简介

[spring-statemachine](http://projects.spring.io/spring-statemachine/)是spring团队提供的一个状态机开源框架，与Spring结合得比较紧密，目前（2017-12）spring-statemachine距离发出第一版release接近两年，最新版本是1.2.7-RELEASE。

## 2.2 基础模型

spring-statemachine定义的模型比较全，有


# 4.前景规划

[inputSymbol]: /assets/images/finite-state-machine/inputSymbol.svg
[initStateSet]: /assets/images/finite-state-machine/initStateSet.svg
[initState]: /assets/images/finite-state-machine/initState.svg
[finalStateSet]: /assets/images/finite-state-machine/finalStateSet.svg
[stateTransition]: /assets/images/finite-state-machine/stateTransition.svg
[stateTransitionFormulator]: /assets/images/finite-state-machine/stateTransitionFormulator.svg
[NFAexample]: /assets/images/finite-state-machine/NFAexample.svg
[DFAexample]: /assets/images/finite-state-machine/DFAexample.svg
