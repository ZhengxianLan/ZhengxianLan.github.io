---
layout: post
title: Vps ssh alive
categories: linux
tags: vps
description: Vps ssh alive
---

## ssh 自动断开的问题

在连接远程 SSH 服务的时候，经常会发生长时间后的断线，或者无响应（无法再键盘输入）。

总体来说有两个方法：

###  依赖 ssh 客户端定时发送心跳。

putty、SecureCRT、XShell 都有这个功能，但是目测不太好用。

此外在 Linux 下：

- 打开 ssh_config
```bash
sudo vim /etc/ssh/ssh_config
```
- 添加
<pre>
ServerAliveInterval 20
ServerAliveCountMax 999
</pre>

即每隔 20 秒，向服务器发出一次心跳。若超过 999 次请求，都没有发送成功，则会主动断开与服务器端的连接。

### 更改服务器端，即在 ssh 远端。

- 打开
``` bash
	sudo vim /etc/ssh/sshd_config
```
- 添加
<pre>
ClientAliveInterval 30
ClientAliveCountMax 6
</pre>

ClientAliveInterval 表示每隔多少秒，服务器端向客户端发送心跳，是的，你没看错。

下面的 ClientAliveInterval 表示上述多少次心跳无响应之后，会认为 Client 已经断开。

所以，总共允许无响应的时间是 60*3=180 秒。

上述配置后，我做了个简单测试。连接米国的 vps，打开 ssh 后，不做任何操作，目前已经维持连接 3 天整，没有任何问题。中间还经历了几次短时间断网 (几十秒)，都自动恢复了。

``` bash
sudo service sshd restart
```

以上转自 [四号程序员](http://www.coder4.com/archives/3751)




