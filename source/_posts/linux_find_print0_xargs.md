---
title: find-print0-xargs0
layout: post
date: 2016-05-17 20:46:25
categories: ['linux']
tags: ['find','xargs']
---

- 在find命令中使用 -exec '{}'，通常能很好的控制对目标文件的动作。但有时候需要把结果以管道的形式传递给其他命令使用。
    `find "$target" $newer -regextype posix-egrep ! -regex "$exclude"  -type f -print0|xargs -0 md5sum >"$tmp"/"$1"`

比如，上面这句，就是把一系列的文件取到MD5摘要后添加到一个文本内。
- 而如果使用
    `find "$target" $newer -regextype posix-egrep ! -regex "$exclude"  -type f -exec  md5sum '{}' >"$tmp"/"$1" \;`
则MD5摘要仅能保留最后一个，因为使用的是重定向，前面的都被冲掉了。当然也可以在每次刷新摘要时先 `:>"$tmp"/"$1"`,并使用`>>`替换`>`

- 而使用 xargs 配合 find 时，需要注意，如果文件名包含空格时，那么需要 find -print0 以便将多个文件名以一行输出，再 xargs -0 配合起来，
即可应付这种情况。
 xargs -0 即把null值作为分隔符，而非空白。

> --null
> -0
> Input  items  are terminated by a null character instead of by whitespace, and the quotes and backslash are not special (every
> character is taken literally).  Disables the end of file string, which is treated like any other argument.  Useful when  input
> items  might  contain  white space, quote marks, or backslashes.  The GNU find -print0 option produces input suitable for this
> mode.
