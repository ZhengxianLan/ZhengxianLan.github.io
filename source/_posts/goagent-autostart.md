---
title: GoAgent-Autostart
date: 2015-07-14 09:31:35
categories: gfw
tags: goagent
---

##goagent 自启动

- 添加到启动目录 /etc/init.d
	``` bash
	sudo ln -sf /home/lan/app/gogent/local/proxy.sh /etc/init.d/goagent
	```
- update-rc.d 添加到运行级别
	``` bash
	sudo update-rc.d goagent defaults
	```

- 若欲删除之
	``` bash
	sudo update-rc.d goagent remove -f
	```
### 凡事总有个但是

次日，ubuntu 不能登录鸟。有点《恐怖油轮》的感觉 => 在登录界面输入密码后又返回登录窗口。
- ctrl + alt + F1 进入命令行，登录正常。但是，`ls -l ~/.Xauthority` 显示为 root 所有。这样，普通用户没权限进入 x.
- chown .Xautority 将其纳入旗下。但重新登录，仍无法进入 x.
- 删除之前所做的 goagent 自启动，这下终于可以进入熟悉的桌面了。

### 还是想 goagent 自启动

- 转移 goagent

	``` bash
	sudo mv /path/to/goagent /opt
	sudo ln -sf /opt/goagent/local/proxy.sh /etc/init.d/goagent
	sudo update-rc.d goagent/defaults
	``` 

### 运行级别

- Ubuntu 中的运行级别
	- 0（关闭系统）
	- 1（单用户模式，只允许 root 用户对系统进行维护）
	- 2 到 5（多用户模式，其中 3 为字符界面，5 为图形界面）
	- 6（重启系统）

对应目录 rc0.d,rc1.d......rc6.d。每个目录下都是到 init.d 目录的一部分一些链接，真正干活的 init.d 里的脚本。链接名中 S 表示开启（Start），K 表示停止（Kill）。数字代表开启或者停止的顺序。

<pre>
$ sudo update-rc.d goagent defaults
Adding system startup for /etc/init.d/goagent ...
   /etc/rc0.d/K20goagent -> ../init.d/goagent
   /etc/rc1.d/K20goagent -> ../init.d/goagent
   /etc/rc6.d/K20goagent -> ../init.d/goagent
   /etc/rc2.d/S20goagent -> ../init.d/goagent
   /etc/rc3.d/S20goagent -> ../init.d/goagent
   /etc/rc4.d/S20goagent -> ../init.d/goagent
   /etc/rc5.d/S20goagent -> ../init.d/goagent

</pre>
