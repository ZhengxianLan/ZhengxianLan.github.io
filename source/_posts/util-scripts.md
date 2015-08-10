---
layout: post
title: util scripts
categories: [linux]
tags: [util]
description: linux 2mini script
date: 2014-07-16
---

###提取差异文件

- 在开发 webapps 时，不可避免的需要添加某个功能或者是修改 bug。而这些改动又需要向前方提交增量补丁。
	对 Java 而言，就是一些 class 之类的。这样手动拣选这些藏匿于不同目录而层次又很深的目录中比较费时间。
	曾经就因为不小心漏了一个 icon。于是写了个小脚本。方便提取补丁。
- 脚本在 [这里](https://github.com/ZhengxianLan/notes/blob/master/utils/x)

```bash
`x i /path/to/webapp` # 在项目改动之前先执行
`x g`                 # 修改完成了后执行
                      # 到当前目录下 temp 找个 patch 就好了
```
如果在 windows 中，可以使用 vagrant。把发布路径改到共享文件夹就好了

---

###改名小脚本

在 windows 上有时候需要批量修改文件名，可以装 bulk
但是，那个还不如敲个命令快呢
- [一睹为快](https://github.com/blockme/notes/blob/master/utils/rename.sh)
- 依赖 **git-bash**
#### 使用
- 在 windows 上安装 git-bash 之后就可以使用了，把该脚本放在 `%path%` 中
```bash
rename 's/\.txt$//'  *    # 消除后缀
rename 's/\(\.txt\)\+$//' * # 消除后缀
rename 's/$/\.txt/' *   # 增加后缀
find -iname '*[!.]???' -type f -exec rename 's/$/\.txt/' '{}' \; #增加后缀
```
---

###统计一个目录下的各种文件数量及比重

[summary](https://github.com/ZhengxianLan/notes/blob/master/utils/summary)

###查看文件夹大小

在 linux 下，想查看目录下每个文件或文件夹的大小
默认给出的单位都是统一的
就写了个 [脚本](https://github.com/ZhengxianLan/notes/blob/master/utils/dus)
