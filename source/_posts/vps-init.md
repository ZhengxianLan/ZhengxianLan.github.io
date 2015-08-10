---
title: vps-init
date: 2015-07-16 19:40:00
categories: [linux,vps]
tags: [vps]
---


- 新增普通用户并为其提供管理权限
	```bash
passwd root # 修改 root 密码
adduser lan # 新增普通用户
visudo      # 修改 /etc/sudoers，为新用户提供管理权限
# 这里默认使用 nano，通过 sudo update-alternatives --config editor 选择 vim-nox
	```
  在 root   ALL=(ALL:ALL) ALL 下新增
<pre>
lan  ALL=(root) NOPASSWD: ALL
</pre>

	如果希望每次 sudo 都需要输入密码，则把 [NOPASSWD:] 去掉，注意连带冒号一起
	此时，可在本地机器将公钥上传至服务器
	```bash
	ssh-copy-id -p port_number new_user@server_ip
	```
- 修改 ssh 服务配置
 `sudo vi /etc/ssh/sshd_config`
 - 使服务器定时向客户端发送心跳, 添加 
<pre>
	ClientAliveInterval 30
	ClientAliveCountMax 6
</pre>
 	ps: 如果平时只用一台专用电脑作为客户端，而会使用多台服务器，那么配置客户端更好
 	即在本地电脑: `sudo vi /etc/ssh/ssh_config` 加入
```
ServerAliveInterval 20
ServerAliveCountMax 999
```
 - 禁用密码登录和禁止 root 登录，在此之前别忘记把自己的 id_rsa.pub 通过 `ssh-copy-id copy` 到服务器上, 修改
<pre>
    PasswordAuthentication yes  => PasswordAuthentication no
    PermitRootLogin yes => PermitRootLogin no
</pre>

 `sudo service ssh restart`
- shadowsocks
	1. 安装 ss
	```bash
	sudo apt-get update
	sudo apt-get install -y python-pip 
	sudo pip install shadowsocks
	```
	2. 添加 ss 配置
	`sudo vi /etc/shadowsocks.json, 样例如下 `
<pre>
	{
	    "server":"0.0.0.0",
	    "server_port":7325,
	    "local_port":1080,
	    "password":"my password",
	    "timeout":600,
	    "method":"aes-256-cfb"
	}
</pre>

	3. 后台启动 ss
```bash
	 mkdir ~/bin
	 echo 'sudo ssserver -c /etc/shadowsocks.json -d start' > ~/bin/ssup
	 sudo ln -sf ~/bin/ssup /etc/init.d/shadowsocks
	 sudo update-rc.d shadowsocks defaults
	 sudo ssserver -c /etc/shadowsocks.json -d start
```	 

