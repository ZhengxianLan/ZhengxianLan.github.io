---
title: ThreadPoolExecutor
layout: post
date: 2015-10-21 13:48:57
categories: ['java','thread']
tags: ['concurrency']
---
> ThreadPoolExecutor 不是并发工具, 而是 限制并发 的工具...
  例如只起一个的话, 网站有 1000 并发请求, 但是调用到这个 ThreadPoolExcecutor 就卡成 8 并发了...
  如果改成多线程部署环境每个请求起一个 ThreadPoolExecutor, 那线程数就爆炸了...
  ThreadPoolExecutor 主要有两种应用场景: 一是某种需求的资源支持不了高并发, 要限制并发; 二是服务器本身面对的并发数不高, 吃不满 CPU. 你可以 wrk 或者 ab 用高并发的情况测一下会不会爆炸...
  至于开多少个线程, 如果每个 worker 都是纯 CPU 计算并且 有足够的 CPU (例如网站请求量很低), 就开核心数目个, 这时它是作为一个并行工具而不是并发工具.
  如果 worker 是 IO 密集型的就得多开, 开多少应该看实际情况而不是 "凭经验", 但开的太多 ThreadPool 就变成虚设了.

来自吕戈

看了他的回答，对线程池更了解了


1. 对于删除目录文件，例如删除 svn 目录，这种操作是 io 时间长，而 cpu 使用率低的操作，可以使用尽量大的并发操作，耗时比单线程省很多。
2. 而对于编译型的操作，例如使用 google closure 编译压缩 js 文件，这种为消耗 cpu 的操作，如果并发量太大，会使电脑挂掉，此时 corePoolSize 设置为大于或等于 cpu 核心数即可。
```java
ThreadPoolExecutor pool = new ThreadPoolExecutor(4, 800, 1, TimeUnit.SECONDS,
        // 无界队列，无限大小，线程数量不会超过 corePoolSize,
        // 此时 maxPoolSize 和 TimeUnit,RejectedExecutionHandler 都无用了
        new LinkedBlockingDeque<Runnable>(),
        new ThreadPoolExecutor.AbortPolicy());
```
