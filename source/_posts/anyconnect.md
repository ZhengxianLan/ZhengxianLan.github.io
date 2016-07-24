---
title: anyconnect
layout: post
date: 2016-06-14 23:42:24
categories: ['gfw']
tags: ['vpn']
---
### 参考
- [熊孩子](http://xionghaizi.blogspot.com/2015/03/ubuntu1404anyconnect.html)
- [折腾笔记](https://bitinn.net/11084/)

- 这主要是参考熊孩子的
- 执行转换的时候，还是输入密码吧，否则，在 iPhone 上导入的时候，不输入密码过不了
- 启动时遇到错误: The option 'listen-clear-file' cannot be combined with 'auth=certificate'
  注释掉 `listen-clear-file = /var/run/ocserv-conn.socket`
- [锁屏断线](https://www.v2ex.com/t/152730):
  > 由于 iOS 系统在锁屏一段时间后会中断 VPN 连接，而屏幕解锁后 AnyConnect 会自动重新连接，如果中间连接中断的时间超过了 cookie-timeout 参数设置的数值，那么重新连接会失败。cookie-timeout = 86400 可以使 AnyConnect 在连接中断后的一天内自动重新连接成功。
