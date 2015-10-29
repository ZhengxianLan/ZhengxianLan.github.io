---
title: encodeURI encodeURIComponent
layout: post
date: 2015-10-23 23:36:15
categories: ['javascript','encode']
tags: ['web']
---

提交表单前，对于中文，我们通常会使用到 encodeURI() 和 encodeURIComponent()

escape 已经不被提倡就不讨论了。

- encodeURI
  获取一个可访问的 URL，比如
  `encodeURI("http://www.google.com/a file with spaces.html")`
  =>
  `http://www.google.com/a%20file%20with%20spaces.html`
  而如果使用 encodeURIComponent 加密整个 URL，那么将得到一个非法 URL
  `http%3A%2F%2Fwww.google.com%2Fa%20file%20with%20spaces.html`
- encodeURIComponent
  通常 encodeURIComponent 用于加密 URL 上的 parameter，而非完整 URL
  `param1 = encodeURIComponent("http://xyz.com/?a=12&b=55")`
  然后，我们这样使用 encodeURIComponent
  `http://www.domain.com/?param1=http%3A%2F%2Fxyz.com%2F%Ffa%3D12%26b%3D55&param2=99`

  值得注意的是，encodeURIComponent 并不会转义 `'`, 这会引起注入 bug。可以使用 `"` 来代替 `'` 包裹参数
  href='myURL' => href="myURL"


[ref](http://stackoverflow.com/questions/75980/best-practice-escape-or-encodeuri-encodeuricomponent?rq=1)
