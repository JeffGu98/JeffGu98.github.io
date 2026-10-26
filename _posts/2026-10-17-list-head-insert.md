---
title: "列表头上插入为什么慢"
date: 2026-10-17
categories:
  - runtime
tags:
  - "python"
  - "java"
excerpt: "连续数组在末尾追加便宜，在开头插入要搬动后面所有元素。"
---

## 问题

用 list.insert(0, x) 累积数据，数据一大就明显变慢。

## 结论

Python 的 list 和 Java 的 ArrayList 都是连续数组。末尾追加大多是摊还常数时间，空间不够才整体搬一次。在下标 0 插入，后面每个元素都要往后挪，是线性时间。如果两端都要增删，用 deque 或 LinkedList 这种为端点准备的结构；如果主要按下标随机访问，继续用数组，只从末尾操作。

## 依据

Python 里 collections.deque 的两端 append 和 pop 是常数时间，按下标访问则是线性。可以分别计时 insert(0) 和 append，数量从一千加到十万，时间差会很明显。

## 还不确定

CPython 的 list 过度分配策略会变，常数因子以实测为准。复杂度的方向不变：头插相对末尾追加是线性的。
