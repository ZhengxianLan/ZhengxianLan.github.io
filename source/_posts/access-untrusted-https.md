---
title: access_untrusted_https
date: 2015-07-27 15:26:08
categories: java
tags: [https]
---

有时候，需要调用对方的接口。http 很简单。
但是，对方使用了 https, 好嘛，更 secure。
但是，是个自签发的证书...
这样，我们要么导入证书，要么忽略验证 (比较危险的方式)。
有这么牛的网站吗？
- [12306](https://kyfw.12306.cn/otn/regist/init)
- [广发银行](https://ebanks.gdb.com.cn/sperbank/perbankLogin.jsp)

## 只是请求信息，不 post 数据

- java [code](https://gist.github.com/ZhengxianLan/7fea50b85481d94628a3)
- ruby [code](https://gist.github.com/ZhengxianLan/d94acb9c47dd8ffff0f3)

## 不仅要 get 数据，还要 post 给对方

- java
[使用到的 jar 包](http://pan.baidu.com/s/1nt5TFol)
[demo](https://gist.github.com/ZhengxianLan/0d5fc92562812a49aed7)

- ruby
[code](https://gist.github.com/ZhengxianLan/d96316db0f55d58c8db7)
[ref](http://www.rubyinside.com/nethttp-cheat-sheet-2940.html)
