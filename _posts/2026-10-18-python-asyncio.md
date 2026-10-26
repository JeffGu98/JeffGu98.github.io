---
title: "asyncio 适合什么样的活"
date: 2026-10-18
categories:
  - runtime
tags:
  - "python"
excerpt: "它用一个线程交错等待很多 IO，不靠它把计算变快。"
---

## 问题

把计算函数改成 async 之后，并没有变快，还把调用链都染成了 async。

## 结论

asyncio 在一个线程里调度协程。协程遇到 await 的 IO 就让出，事件循环去跑别的协程。适合同时等很多网络或磁盘响应。纯计算不让出，会堵住整个循环，别的协程都停。CPU 工作放到进程池，阻塞的老库放到线程池，不要在协程里直接调用它们。

## 依据

一个协程里 time.sleep 会卡住所有任务；asyncio.sleep 才会让出。requests 是阻塞的，在 async 函数里直接调用等于堵住循环。要并发 HTTP，用异步客户端，或把阻塞调用放进 run_in_executor。

## 还不确定

async 函数的取消、超时和异常组需要单独设计。这里只区分它擅长等待，不擅长计算。
