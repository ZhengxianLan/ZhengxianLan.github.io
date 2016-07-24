---
title: js-base
layout: post
date: 2016-01-03 19:18:56
categories: ['js']
tags: ['base']
---

```javascript
var done;
function hi(arg) {return done=true,"hello" +arg;}
console.log(hi("中国"));
console.log("done=>"+done)

//hello 中国
//done=>true
```
