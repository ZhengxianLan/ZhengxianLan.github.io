---
title: use colon in bash
layout: post
date: 2015-10-31 02:10:24
categories: ['bash']
tags: ['colon']
---

今天刚发现 colon 不仅能用在 while : , 作出类似 true 的作用。
也能用做 then 中的占位符，类似于 Python 中 pass
在二进制操作中作占位符，-> ${username=`whoami`}
创建新文件，或者清空文件 -> : > target  # 与 `cat /dev/null > target` 相同，但是使用 : 不会产生新进程，因 `:` 是内建量。而且，`>target` 在 zsh 中会进入与用户交互的阻塞状态，卡住脚本执行。因而使用 `: >taget` 更具通用性。



