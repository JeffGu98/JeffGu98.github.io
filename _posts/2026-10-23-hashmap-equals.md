---
title: "改了 equals 为什么必须改 hashCode"
date: 2026-10-23
categories:
  - runtime
tags:
  - "java"
excerpt: "相等的对象必须有相同的哈希，否则 HashMap 会把它们放进不同的桶。"
---

## 问题

两个按业务看是同一个键的对象，put 进去 get 不出来。

## 结论

HashMap 先用 hashCode 选桶，再用 equals 在桶里找。约定是：equals 为 true 的两个对象，hashCode 必须相同。只改 equals、沿用 Object 的哈希，两个内容相同的键会落到不同桶，get 就找不到。哈希还要稳定：作为键的字段如果会变，对象可能留在旧桶里，再也匹配不上。

## 依据

Java 的 Object 约定写在 equals 和 hashCode 的文档里。用记录类或只含不可变字段的键，让 IDE 按同一组字段生成这两个方法。检查时可以放两个内容相同、却不是同一个实例的键，断言 get 能取回。

## 还不确定

树化、扩容之后的查找仍然遵守这个约定，但并发用 HashMap 本身就会丢更新，那是另一件事，该用并发容器或把映射限制在一个线程里。
