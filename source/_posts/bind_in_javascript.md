---
title: bind_in_javascript
layout: post
date: 2015-10-23 00:52:38
categories: ['javascript']
tags: ['bind']
---

这个是在 es5 中加进来的方法。将一个函数 f 绑定到一个对象 o 上,并返回新的函数。
之后调用新函数就会像调用绑定对象 o 的方法 f 一样 .下面是一个为 ECMAScript 准备的
bind()

```javascript

if (!Function.prototype.bind) {
  Function.prototype.bind = function (o/*,args*/) {
    //Save this and arguments values into variables so we can
    //ues them in the nested function below.
    var self = this, boundArgs = arguments;
    //The return value of bind() is a function
    return function () {
      //Build up an argument list,starting with any args passed to bind after the first one,
      // and follow those with all args passed to this function
      var args = [], i;
      for (var j = 1; j < boundArgs.length; j++) args.push(boundArgs[j]);
      for (i = 0; i < arguments.length; i++) args.push(arguments[i]);
      //Now invoke self as a method of o ,with those arguments
      return self.apply(o, args);
    };
  };
}

```
