---
title: zip- 乱码
date: 2015-07-26 22:48:36
categories: linux
tags: [zip,charset]
---

### zip
-m 压缩后删除系统上的源文件。如果源文件是多个，那么将所有源文件都压缩后才会删除源文件。当磁盘紧张时使用，如果使用搬瓦工,1.8GB 的磁盘...
-9 高压缩率
-r 递归压缩目录下的文件
对已存在的压缩包，zip 会添加 / 修改到压缩包。比如磁盘实在是太少了。你一次 -m 多个文件都不行，那么就先小批量的压缩, 即 zip -m9 out.zip input1 input2，最后 zip -m9 out.zip input*
-d 从 zip 中删除指定文件 : zip -d archive.zip file_to_delete
### unzip
- 查看 zip 包的文件:
		`unzip -l file.zip`
- 提取特定文件，而不是完全解压缩
		`unzip /media/music.zip "*/*.mp3" -d /home/myhome/tmp`
- -O 对于在 linux 乱码的情况，在国内，因为大多数是默认 GBK 的 windows 系统，因此 `unzip -O GBK f.zip`
   但是，若是文件内容是 GBK，在 linux 下 cat 一下还是乱码，此时需要 iconv
   `iconv -f GBK -t UTF-8 -o output_utf_file  input_gbk_file`
