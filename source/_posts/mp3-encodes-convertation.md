---
layout: post
title: Mp3 encodes convertation
categories: [linux]
tags: [mp3]
description: mp3 encodes convertation
---

 ConvertingMP3Tags

 MP3 Tag Encoding Problem

 If you found your MP3 tags (title, album, etc) are not well displayed in players or applications like Rhythmbox, which is especially true for Asian users, you may want to convert their tags to UTF-8 such that they should be properly displayed almost everywhere.

 EasyTAG

 EasyTag supports IDv2.4, which allows UTF-8, as of version 3.1.1. The first release of Ubuntu to include this is 7.10 (Gutsy). notice though that easytag doesn't support converting tags that already exist.

 You can install easytag from the universe repository.

```
sudo apt-get install easytag
```

 Using python-mutagen

 Python-mutagen is a tool which can do mass conversion of mp3 files. Unfortunately the current version (1.0) in Ubuntu 6.06 (Dapper Drake) does not support the mass converting function. Starting in Ubuntu 6.10 python-mutagen does support mass conversion.

 Installing python-mutagen

 You can install python-mutagen from the Universe Repository.

```
sudo apt-get install python-mutagen
```

 Convert tags of MP3 files using python-mutagen

 Go to the directory where you put your MP3 files, or go to the home directory, using the following command,

```
 find . -iname "*.mp3" -execdir mid3iconv -e <encoding> {} \;
 ```

 where <encoding> is the original encoding of the tags (e.g. "GBK" for simplified Chinese or "windows-1255" for hebrew). python-mutagen will then search for all .mp3 files in current directory (recursively) and convert their tags to proper form.


## 解决 banshee 音乐播放器歌曲乱码问题

用 Python 写的 “Mutagen”，目前最新版本 1.11，Ubuntu 7.04 源里也带有 1.10 版本的 Mutagen，可以用这个命令来安装：
sudo apt-get install python-mutagen

ps: 安装 Quod Libet 和 Listen 都必须这个

### 使用方法：
```
mid3iconv -e gbk *.mp3
```

如果想转换当前目录下的所有 mp3 (包括子目录)：

```
find . -iname "*.mp3" -execdir mid3iconv -e gbk {} \;
```
