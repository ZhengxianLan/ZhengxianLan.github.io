---
title: iptables
layout: post
date: 2016-07-10 08:54:54
categories: ['linux']
tags: ['iptables']
---

### iptables 基础
iptables 底层由 netfilter 模块实现

- netfilter 可以对数据进行允许，丢弃，修改操作。
- netfilter 支持通过一下方式对数据包进行分类
  - 源 IP 地址
  - 目标 IP 地址
  - 使用接口 (网卡)
  - 使用协议 (TCP,UDP,ICMP 等)
  - 端口号
  - 连接状态 (new,ESTABLISHED,RELATED,INVALID)

#### Netfilter

![iptables规则](http://i4.piimg.com/4851/c53954d437866e56.png)

- filter 用于对数据进行过滤
- nat 用于对数据包的源、目标地址进行修改
- mangle 用于对数据包进行高级修改 (qos 参数修改)

- filter point with common ACTION
 PREROUTING  => DNAT
 POSTROUTING => SNAT,MASQUERADE
 INPUT       => ACCEPT,REJECT,DROP

#### 常用功能
- 作为服务器使用
  过滤到本机的流量     => INPUT 链、filter 表
  过滤由本机发出的流量 => OUTPUT 链、filter 表
- 作为路由器使用
  过滤转发的流量                                  => FORWARD 链、filter 表
  对转发数据的源、目标 IP 地址进行修改 (NAT 功能) => PREROUTING/POSTROUTING 链、nat 表

#### 规则
- iptables 通过规则对数据进行访问控制
- 一个规则使用一行配置
- 规则按顺序排列
- 当收到、发出、转发数据包时，使用规则对数据包进行匹配，按规则顺序进行逐条匹配
- 数据包按照第一个匹配上的规则执行相关动作：丢弃、放行、修改
- 没有匹配规则，则使用默认动作 (每个 chain 拥有各自的默认动作)
- 通过 iptables 命令创建一条规则，一条规则包含以下几个部分
  `iptables -t filter -A INPUT -s 192.168.1.1 -j DROP`
  - 表： 规定使用的表 (filter,nat,mangle, 不同表有不同功能)
  - 链： 规定过滤点
  - 匹配属性 (可写多个)： 规定匹配数据包的特征
  - 匹配后的动作： 过滤放行，丢弃，拒绝

#### 基本操作
- 列出现有 iptables 规则
  `iptables -L`
- 插入一条规则
  `iptables -I INPUT 3 -p tcp --dport 22 -j ACCEPT`
  向 INPUT 链插入一条规则，顺序排第三
- 删除一条规则
  `iptables -D INPUT 3`
  `iptables -D INPUT -s 192.168.1.2 -j DROP`
- 删除所有规则
  `iptables -F`
- 对 iptables 规则的增删改只在本次会话中生效，重启后失效。若需要永久生效，需要保存到文件

#### 匹配参数
- 基于 IP 地址
```
 -s 192.168.1.1
 -d 10.0.0.0/8
```
- 基于接口
```
 -i eth0
 -o eth1
```
- 排除参数
` -s '!' 192.168.1.0/24
- 基于协议及端口
```
 -p tcp --dport 23
 -p udp --sport 53
 -p icmp
```

##### INPUT、OUTPUT

- 控制到本机的网络流量:
```bash
  iptables -A INPUT -s 192.168.1.100 -j DROP
  iptables -A INPUT -p tcp --dport 80 -j DROP
  iptables -A INPUT -s 192.168.1.100-p tcp --dport 80 -j DROP
  iptables -A INPUT -i eth0 -j ACCEPT
  #Connections from a range of IP addresses
  iptables -A INPUT -s 10.10.10.0/24 -j DROP <=> iptables -A INPUT -s 10.10.10.0/255.255.255.0 -j DROP
```
- Connection States
As we mentioned earlier, a lot of protocols are going to require two-way communication. For example, if you want to allow SSH connections to your system, the input and output chains are going to need a rule added to them. But, what if you only want SSH coming into your system to be allowed? Won’t adding a rule to the output chain also allow outgoing SSH attempts?

That’s where connection states come in, which give you the capability you’d need to allow two way communication but only allow one way connections to be established. Take a look at this example, where SSH connections FROM 10.10.10.10 are permitted, but SSH connections TO 10.10.10.10 are not. However, the system is permitted to send back information over SSH as long as the session has already been established, which makes SSH communication possible between these two hosts.
```bash
iptables -A INPUT -p tcp --dport ssh -s 10.10.10.10 -m state --state NEW,ESTABLISHED -j ACCEPT

iptables -A OUTPUT -p tcp --sport 22 -d 10.10.10.10 -m state --state ESTABLISHED -j ACCEPT
```


##### FORWARD
- 当使用 linux 作为路由 (进行数据转发) 设备使用的时候，可以通过定义 forward 规则来进行转发控制
如：
  禁止所有 192.168.1.0/24 到 10.1.1.0/24 的流量
  `iptables -A FORWARD -s 192.192.168.1.0/24 -d 10.1.1.0/24 -j DROP`

##### NAT 功能
- NAT (Network Address Translation) 网络地址转换是用来对数据包的 IP 地址进行修改的机制，NAT 分为两种：
  - SNAT 源地址转换，通常用于伪装内部地址 (一般意义上的 NAT)
  - DNAT 目标地址转换，通常用于跳转
- iptables 中实现 NAT 功能的是 NAT 表

###### 常用 NAT
- 通过 NAT 进行跳转,DNAT 只能用于 PREROUTING 链
`iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-dest 192.168.1.10`
- 通过 NAT 对出向数据进行跳转:
`iptables -t nat -A OUTPUT -p tcp --dport 80 -j DNAT --to-dest 192.168.1.100:8080`
- 通过 NAT 对数据流进行伪装
(一般意义上的 NAT，将内部的地址全部伪装为一个外部公网 IP 地址)
`iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE`
- 通过 NAT 隐藏源 IP 地址
`iptables -t nat -A POSTROUTING -j SNAT --to-source 1.2.3.4`
- nat
  SNAT 源地址转换   => POSTROUTING
  DNAT 目标地址转换 => PREROUTING
### 保存规则
- 保存到文件 : `service iptables save` => 写入到 /etc/sysconfig/iptables [centos]
```
sudo /sbin/iptables-save    # ubuntu
/sbin/service iptables save # redhead
/etc/init.d/iptables save   # redhead
```
- 远程管理必须允许来自客户端主机的 SSH 流量确保这是第一条 iptables 规则
，否则可能会将自己锁在外面。


### Refs
specially thanks to
 - linuxcasts
 - [howtogeek](http://www.howtogeek.com/177621/the-beginners-guide-to-iptables-the-linux-firewall/)
