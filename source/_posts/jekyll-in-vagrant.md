---
layout: post
title: Jekyll in vagrant
categories: [linux]
tags: [jekyll]
description: jekyll vagrant polling fresh
---



### ** 不能编辑 css js**

在 vagrant 中启动本地 Jekyll
通过 Windows 上的 sublime 来编辑 css 的时候总是出错
不管输入什么都会在_site 里面编译成 NUL

几番尝试后

- 在 window 上别处编辑好之后再拷贝到 jekyll 目录就不会出现 NUL
- 把 Jekyll 文件夹转移到非 virtualbox 共享目录，即 linux 文件系统上编辑不会出错

对于一般的新建 post 没什么问题
只是修改 js，css 的时候就麻烦一些

### ** 不能自动更新 **
- 发现在 Windows 上编辑 vagrant 上的 jekyll 并不会自动生成，应该是没有通知到系统吧。而在 vagrant 内用你 vim 编辑是会自动生成新的网页的。
原来需要使用 `jekyll serve  --watch --force_polling`
- 还是改用 ubuntu 了，windows 太费心了

