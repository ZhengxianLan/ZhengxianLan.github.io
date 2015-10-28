---
title: vim_browserlink_sync
layout: post
date: 2015-10-28 21:31:41
categories: ['vim']
tags: ['sync']
---

使用browserlink.vim，在vim中编辑，浏览器中同步刷新
- 添加 Plugin 到 ~/.vimrc
  ` Plugin 'jaxbot/browserlink.vim' `
  记得 vim +PluginInstall
- 添加用户自定义js到chrome
  创建 browserlink.user.js
```javascript
// ==UserScript==
// @name       Browserlink Embed
// @namespace  http://use.i.E.your.homepage/
// @version    0.1
// @description  enter something useful
// @match      http://localhost/*
// @match      file:///*
// @match      http://127.0.0.1/*
// @copyright  2012+, You
// ==/UserScript==

var src = document.createElement("script");
src.src = "http://127.0.0.1:9001/js/socket.js";
src.async = true;
document.head.appendChild(src);```
 打开chrome，转到chrome://extensions/
 将browser.user.js 托入 chrome中
- browser.user.js 不能识别如 file:///之类的本地文件模式
  因而需要一个简易服务器
  ```bash
  sudo npm install http-server -g
  echo "alias hs='http-server &>/dev/null &'" >~/.bash_aliases
  source ~/.zshrc
  ```
- 运行hs,打开html编辑发现已经可以即时刷新了
- 设置~/.vimrc,添加
```vi
nnoremap <C-g> :!google-chrome http://localhost:8080/%:t<CR> " browser preview with ctrl-p
```
  就可以直接在browser中打开当前文件了
