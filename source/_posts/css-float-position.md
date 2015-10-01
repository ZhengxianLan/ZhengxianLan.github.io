---
title: css-float-position
date: 2015-10-02 00:10:49
categories: ['fe','css']
tags: 'float'
---

 css 中 通过 float，position 都可以使元素跳出正常文档流
- float 通常结合 top，left 使用。
  如果已经设置过 top,left 而下次又想将元素移动至别的地方，需要使用 top:auto;left:auto; 来覆盖，否则仅仅设置 bottom,right 是无效的
- postion 通常是 position:absolute 和 postion:relative 结合使用
  通常需要移动位置的元素设置 position:absolute; 而它是相对于含有 postion:relative 最近的祖先元素进行浮动。
- 由于浮动造成元素从正常文档流脱离，通常还需要进行clearfix
