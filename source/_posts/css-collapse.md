---
title: css-margin-collapse
layout: post
date: 2016-01-03 11:15:57
categories: ['css']
tags: ['margin']
---

css margin 重叠有可能带给开发理解上的模糊
一般有
 - 水平边距永远不会重合
 - 垂直边距
   - 父子元素的 margin-top,margin-bottom
   - 相邻元素的 margin-bottom vs margin-top
   * 常规流向中两个或多个块框相邻的垂直边距会重合。结果的边距宽度是相邻边距宽度中较大的值。如果出现负边距，则在最大的正边距中减去绝对值最大的负边距。如果没有正边距，则从零中减去绝对值最大的负边距。`

对于 父子垂直边距重合，和可参考 [bootstrap的clearfix](http://getbootstrap.com/css/#helper-classes-clearfix)

- Thanks to
  - [clearfix](http://haoduoshipin.com/v/166)
  - [css-margin的相关属性，问题及应用](http://www.zhangxinxu.com/wordpress/2009/08/css-margin%E7%9A%84%E7%9B%B8%E5%85%B3%E5%B1%9E%E6%80%A7%EF%BC%8C%E9%97%AE%E9%A2%98%E5%8F%8A%E5%BA%94%E7%94%A8/)
