---
title: "JVM 什么时候会做 Full GC"
date: 2026-10-26
categories:
  - runtime
tags:
  - "java"
  - "jvm"
  - "gc"
excerpt: "Full GC 通常是常规回收失败后的整堆停顿，不是每次老年代一满就发生。"
---

## 问题

日志里突然出现一次很长的 Full GC，接口超时。想知道是内存真的不够，还是收集器走了失败回退。

## 结论

Full GC 是整堆、通常要停顿应用线程的回收。G1、ZGC 这类收集器平时用自己的周期清垃圾；走到 Full GC，多半是正常周期没能腾出足够空间。常见原因是老年代放不下要晋升的对象、元空间满了、超大对象占满了 region，或者代码调用了 System.gc() 且没有关掉显式 GC。

## 依据

在 GC 日志里找 Full 或 Pause Full。同时看老年代占用、Metaspace、humongous allocation。对象比 G1 一个 region 的一半还大，就会按超大对象分配。用 jcmd GC.heap_info 或 JVM 的 GC 日志，不要只看一次接口耗时。

## 还不确定

不同收集器和 JDK 版本的日志格式不一样。ZGC、Shenandoah 的失败回退不能和 Parallel GC 的 Full GC 逐字对照。
