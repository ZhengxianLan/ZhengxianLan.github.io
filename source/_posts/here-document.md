---
layout: post
title: Here Document
categories: [linux]
tags: [bash]
date: 2015-01-19 22:49
description: here document
---


## Here Document 是什么

Here Document 是在 Linux Shell 中的一种特殊的重定向方式，它的基本的形式如下

cat << delimiter
  Here Document Content
delimiter


不过需要注意的是，如果你希望使用 cat <<- 的形式，记得注释掉 .vimrc 内的 set expandtab, 因为 tab 展开成 space,
那么 <<- 就不能将内容前的空格去掉，与预期不符。


## Mannual
[manual-to-here-document](https://gist.githubusercontent.com/ZhengxianLan/957a7a4fccf84fbc2d86/raw/f19e045d7b1ed775b730242f87e26dadbca98f16/here-document-manual)
参考:[linux shell 的 here document 用法 (cat << EOF)](http://my.oschina.net/u/1032146/blog/146941)
