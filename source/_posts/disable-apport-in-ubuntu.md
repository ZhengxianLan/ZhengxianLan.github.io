---
title: disable apport in ubuntu
date: 2015-10-17 11:23:37
categories: ['linux','ubuntu']
tags: ['miscellaneous']
---

apport 收集各种crash信息，并上传到ubuntu
但是，很多信息收集上去，bug依旧
而且，很占用cpu
开发版本收集也就算了，稳定版实在不用那么丧心病狂
遂禁之
```bash
sudo service apport stop
sudo vi /etc/default/apport
```
change
`enabled=1`
to
`enabled=0`

ref:
[diable-ubuntu-apport]http://askubuntu.com/questions/371524/how-to-stop-kill-usr-share-python-usr-share-apport-apport()
[why we should diable apport in stable release](http://askubuntu.com/questions/139424/is-it-safe-to-remove-apport)
