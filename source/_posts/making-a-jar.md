---
layout: post
title: Making a jar
categories: java
tags: jar
description: make jar
---

1. 闲来无事,man jar, 发现原来直接使用 jar 的命令行是一件很容易的事情。

    ```bash
    #这就是可以用于生成 taskomc 的一行 jar 命令。。。
    jar -cvmf META-INF/MANIFEST.MF taskomc.jar com
    ```

	可以指定 Manifest, 和要压缩到 jar 内的文件，并且，自动辨识目标文件是否为目录，若目录自动递归。

2. 指定了 manifest 要注意，必须以一个换行结束，否则 jar 不予解析.
<pre>
An existing manifest file must end with a new line character.  
jar does not parse the last line of a manifest file if it does not end with a new line character.
</pre>
3. 查看 jar 内所有的文件 (不包括目录)： jar -tf taskomc.jar |grep -v '/$'|nl


[jar 的 mannual](https://gist.githubusercontent.com/ZhengxianLan/80b10d46db78d788cca7/raw/fcb1e4d88ab52e2d09bd9c369582d20d3eb96420/man-jar)
