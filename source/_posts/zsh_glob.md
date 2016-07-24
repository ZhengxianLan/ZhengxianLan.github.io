---
title: glob in zsh
layout: post
date: 2016-07-05 09:42:48
categories: ['zsh']
tags: ['glob']
---

- 通配符操作符
```
# list text files that end in a number from 1 to 10
ls -l zsh_demo/**/*<1-10>.txt

# list text files that start with the letter a
ls -l zsh_demo/**/[a]*.txt

# list text files that start with either ab or bc
ls -l zsh_demo/**/(ab|bc)*.txt

# list text files that don't start with a lower or uppercase c
ls -l zsh_demo/**/[^cC]*.txt
```
- 筛选文件
```
# list every file directly below the zsh_demo folder
ls zsh_demo

# list every file in the folders directly below the zsh_demo folder
ls zsh_demo/*

# list every file in every folder two levels below the zsh_demo folder
ls zsh_demo/*/*

# list every file anywhere below the zsh_demo folder
ls zsh_demo/**/*

# list every file that ends in .txt in every folder at any level below the zsh_demo folder
ls zsh_demo/**/*.txt
```

[Thanks to reasoniamhere](http://reasoniamhere.com/2014/01/11/outrageously-useful-tips-to-master-your-z-shell/)
