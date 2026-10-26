---
title: "volatile 保证了什么"
date: 2026-10-22
categories:
  - runtime
tags:
  - "java"
  - "jvm"
excerpt: "它保证这个变量的可见性和一定的顺序，不保证 i++ 这种复合动作只发生一次。"
---

## 问题

一个线程改了标志，另一个线程一直看不到。加上 volatile 之后又担心计数还是错的。

## 结论

没有 volatile 或锁时，一个线程写入的普通字段，另一个线程可以一直把旧值放在寄存器或 CPU 缓存里。volatile 写会把之前的写入发布出去，volatile 读能看见那次写之前的结果。它不把读改写合成一个原子动作。volatile int 的 i++ 仍是读、加、写三步，两个线程会丢更新。

## 依据

可见性要用 happens-before 来记：对 volatile 字段的写，先于之后对同一字段的读。计数要用 AtomicInteger 或加锁。只用来发布一个“可以停了”的布尔标志时，volatile 是够的。

## 还不确定

更细的重排序规则、和 final 字段安全发布的差别，要以 JSR-133 和你所用的 JDK 为准。单靠背 volatile 不能替代锁的互斥。
