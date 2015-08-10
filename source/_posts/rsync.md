---
layout: post
title: Rsync
categories: linux
tags: cli
description: rsync
date: 2014-08-10
---

1. rsync 的 src 都是把路径分隔符 (即 '/') 后面的内容传送到目的地址。

	``` bash
	rsync -av /src/foo /dest
	rsync -av /src/foo/ /dest/foo
	```
	上面 2 行命令效果一样

2. rsync 仅传输变化的部分。但是本地文件默认传输整个文件.

	```
	rsync -avhzP --delete /a/b/c  /a1/b1/
	```
	** 注意 ** 目标要是文件夹的父级文件夹，若为 /a1/b1/c1, 那么会在 c1 下面存放整个 c 文件夹

4. 即使对于目录，使用 -avhz 也比 -rvhz 要好，因此 avhz 只会发送发生改变的文件，而 rvhz 总是会发送全部文件。
	即使对单个文件，avhz 也只在文件改变的时候发送，而 rvhz 总是发送文件。

5. 有了 rsync --delete 就可以实现文件同步。
	而对于 windows，也有 GUI 的同步软件，例如 FileSync，更安全。被删除的文件是移动到回收站，避免了由于路径设置错误造成的误删除。

6. 对于本地的 2 个文件，rsync 默认不会使用 delta-transfer，若要强制其使用应该增加 --no-W 选项。
	刚刚发现在 manual 内地 -no-option section 内有介绍，但是不详细

7. 可以使用 rsync 修补破损的下载文件。比如下载了一个破损的 fedora.iso。使用 rsync 可以只下载破损的部分

	```bash
    rsync -av rsync://www.muug.mb.ca/pub/fedora/linux/releases/9/Fedora/i386/os/images/boot.iso /home/seve/Download
    ```
    网址可以在 mirror list 中查找支持 rsync 的网站，通过 http 用浏览器浏览，找到需要的文件，拷贝其下载地址，然后修改 http 为 rsync 即可。
