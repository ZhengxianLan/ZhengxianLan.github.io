---
layout: post
title: Mp3 encodes convertation
categories: [linux]
tags: [mp3]
date: 2013-10-12 22:46
description: mp3 encodes convertation
---

## 解决 banshee 音乐播放器歌曲乱码问题

用 Python 写的 “Mutagen”，目前最新版本 1.11，Ubuntu 7.04 源里也带有 1.10 版本的 Mutagen，可以用这个命令来安装：
sudo apt-get install python-mutagen

ps: 安装 Quod Libet 和 Listen 都必须这个

### 如何使用：

```
mid3iconv -e gbk *.mp3
```

如果想转换当前目录下的所有 mp3 (包括子目录)：

```
find . -iname "*.mp3" -execdir mid3iconv -e gbk {} \;
```
