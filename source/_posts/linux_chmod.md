---
title: linux 4位权限含义
layout: post
date: 2015-11-01 12:58:18
categories: ['linux']
tags: ['base']
---

日常使用chmod多是修改3位数，虽然4位数不常见，但是，不搞清楚，心里总是好像有疙瘩。
先建一个测试文件
 `: > touch`
修改权限为755
 `chmod 755 test`
查看文件权限
 `ls -l test`
  看到
  -rwxr-xr-x 1 lan lan 0 Nov  1 12:59 test
其实我们还可以在前面添加一位数字的，这就是setuid,setgid,sticky,分别对应的数字也是4，2,1

- setuid: 设置使文件在执行阶段具有文件所有者的权限. 典型的文件是 /usr/bin/passwd. 如果一般用户执行该文件, 则在执行过程中, 该文件可以获得root权限, 从而可以更改用户的密码.
- setgid: 该权限只对目录有效. 目录被设置该位后, 任何用户在此目录下创建的文件都具有和该目录所属的组相同的组.

- sticky bit: 该位可以理解为防删除位. 一个文件是否可以被某用户删除, 主要取决于该文件所属的组是否对该用户具有写权限.
如果没有写权限, 则这个目录下的所有文件都不能被删除, 同时也不能添加新的文件. 如果希望用户能够添加文件但同时不能删除文件,
  则可以对文件使用sticky bit位. 设置该位后, 就算用户对目录具有写权限, 也不能删除该文件.

  比如，我们让学生上传作业到ftp，但不允许他们删除上面的文件，就可以设置setgid让新增的文件都不具有读的权限，防止抄袭。同时，设置sticky 位，防止恶意删除。

### 如何操作
操作这些标志与操作文件权限的命令是一样的, 都是 chmod. 有两种方法来操作,

- 使用字符分别对用户，组，其他修改第一标志位

chmod u+s temp -- 为temp文件加上setuid标志. (setuid 只对文件有效)

chmod g+s tempdir -- 为tempdir目录加上setgid标志 (setgid 只对目录有效)

chmod o+t temp -- 为temp文件加上sticky标志 (sticky只对文件有效)

- 采用八进制方式. 对一般文件通过三组八进制数字来置标志, 如 666, 777, 644等. 如果设置这些特殊标志, 则在这组数字之外外加一组八进制数字. 如 4666, 2777等. 这一组八进制数字三位的意义如下,

abc

a - setuid位, 如果该位为1, 则表示设置setuid 4---

b - setgid位, 如果该位为1, 则表示设置setgid 2---

c - sticky位, 如果该位为1, 则表示设置sticky 1---

设置完这些标志后, 可以用 ls -l 来查看. 如果有这些标志, 则会在原来的执行标志位置上显示. 如

rwsrw-r-- 表示有setuid标志

rwxrwsrw- 表示有setgid标志

rwxrw-rwt 表示有sticky标志

那么原来的执行标志x到哪里去了呢? 系统是这样规定的, 如果本来在该位上有x, 则这些特殊标志显示为小写字母 (s, s, t). 否则, 显示为大写字母 (S, S, T)

注意：setuid和setgid会面临风险，所以尽可能的少用

demo
```bash
chmod -x test                              # 除去test的执行权限x
 l test
  -rw-r--r-- 1 lan lan 0 Nov  1 12:59 test
chmod 7644 test                            # 增加 suid,sgid,sticky
l test                                     # 由于之前x文件并没有执行权限，因此ugo的末位都为大写SST
  -rwSr-Sr-T 1 lan lan 0 Nov  1 12:59 test
chmod 0555 test                            # 去除所与suid，sgid，sticky，并为test的ugo都增加执行权限x
l test
  -r-xr-xr-x 1 lan lan 0 Nov  1 12:59 test
chmod 7555 test                            # 保留test的ugo执行权限，并设置suid,sgid,sticky位都为1,即得到首位的7
l test                                     # 由于之前test的ugo都具备执行权限x，因而增加suid，sgid，sticky后，各个x位变为相应的sst
  -r-sr-sr-t 1 lan lan 0 Nov  1 12:59 test
```

ref:
  [Linux文件权限4位数字含义  ](http://blog.163.com/wang_ly2442/blog/static/94943407201482195732291/)
  [linux权限补充：rwt rwT rws rwS 特殊权限](http://www.cnblogs.com/qlwy/archive/2011/06/26/2121919.html)
  [更多参考IBM](http://www.ibm.com/developerworks/cn/linux/l-lpic1-v3-104-5/)
