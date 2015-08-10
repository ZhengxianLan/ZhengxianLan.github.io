---
layout: post
title: A patch generator
categories: [linux]
tags: [utils,bash]
description: here document
date: 2014-07-16
---

- 在开发 webapps 时，不可避免的需要添加某个功能或者是修改 bug。而这些改动又需要向前方提交增量补丁。
	对 Java 而言，就是一些 class 之类的。这样手动拣选这些藏匿于不同目录而层次又很深的目录中比较费时间。
	曾经就因为不小心漏了一个 icon。于是写了个小脚本。方便提取补丁。

- 脚本在 [这里](https://github.com/ZhengxianLan/notes/blob/master/utils/x)

---

- 在项目改动之前先执行 `x.sh i /path/to/webapp`
- 修改完成了后执行 `x.sh g`
- 到当前目录下 temp 找个 patch 就好了

如果在 windows 中，可以使用 vagrant。把发布路径改到共享文件夹就好了


