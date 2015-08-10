---
title: fix-hexo-categories-no-next-page
date: 2015-08-11 00:54:41
categories: fix
tags: hexo
---

hexo默认的landscape主题已经11个月没更新了。
在一个categories下，post多得超过单页数量时，却没有显示下一页button。
虽说，可以修改浏览器地址，但是，总归不是正确的姿势。
打开 themes/landscape/layout/_partial/archive.ejs
添加一下到底部

```html
<% if (page.total > 1){ %>
    <nav id="page-nav">
      <%- paginator({
        prev_text: '&laquo; Prev',
        next_text: 'Next &raquo;'
      }) %>
    </nav>
  <% } %>
```
实际上，这些代码本来就存在该文件内，不知何时不能显示了。不知`(pagination == 2)`是为何意，能用就好了吧。