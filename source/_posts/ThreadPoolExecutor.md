---
title: ThreadPoolExecutor
layout: post
date: 2015-10-21 13:48:57
categories: ['java','thread']
tags: ['concurrency']
---

ThreadPoolExecutor 不是并发工具, 而是 限制并发 的工具...

例如只起一个的话, 网站有 1000 并发请求, 但是调用到这个 ThreadPoolExcecutor 就卡成 8 并发了...

如果改成多线程部署环境每个请求起一个 ThreadPoolExecutor, 那线程数就爆炸了...

ThreadPoolExecutor 主要有两种应用场景: 一是某种需求的资源支持不了高并发, 要限制并发; 二是服务器本身面对的并发数不高, 吃不满 CPU. 你可以 wrk 或者 ab 用高并发的情况测一下会不会爆炸...

至于开多少个线程, 如果每个 worker 都是纯 CPU 计算并且 有足够的 CPU (例如网站请求量很低), 就开核心数目个, 这时它是作为一个并行工具而不是并发工具.

如果 worker 是 IO 密集型的就得多开, 开多少应该看实际情况而不是 "凭经验", 但开的太多 ThreadPool 就变成虚设了.

来自吕戈

看了他的回答，对线程池更了解了


