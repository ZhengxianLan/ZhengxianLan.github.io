---
title: only final variablies accessible in inner class
layout: post
date: 2015-10-29 18:17:49
categories: ['java','base']
tags: ['closure']
---

java 内部类使用外部方法提供的参数为何一定要使用 final 修饰？
让我们看看 js 里闭包的坑 [js closure](http://zhengxianlan.github.io/2015/10/15/javascript-garden/)

这样，在参数传到 closure 中，外部作用域仍然可以修改这个参数，会造成与预期不一致的结果。
一般来说，我们传入一个参数，就希望在使用闭包时，这个参数和传入时是一致的。
而 java 向来以防止程序员犯错而著称，尽量把能犯错误的路都给堵死，而不是在运行时出现 bug。

来自栈爆的解释 [why-are-only-final-variables-accessible-in-anonymous-class](http://stackoverflow.com/questions/4732544/why-are-only-final-variables-accessible-in-anonymous-class)




