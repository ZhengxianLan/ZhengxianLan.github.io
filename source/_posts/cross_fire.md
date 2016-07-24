---
title: cross the great fire wall
layout: post
date: 2016-06-10 21:12:08
categories: ['gfw']
tags: ['pptp']
---

参考
- [digitalocean](https://www.digitalocean.com/community/tutorials/how-to-setup-your-own-vpn-with-pptp)
- [boweihe 中文](https://boweihe.me/?p=189)

> 只需要注意 vps 网卡不是 eth0，通常是 venet0 就好了

1. Install pptpd
```bash
rpm -i http://poptop.sourceforge.net/yum/stable/rhel6/pptp-release-current.noarch.rpm
yum -y install pptpd
```
2. Edit  /etc/pptpd.conf
add these lines:
```
localip 10.0.0.1
remoteip 10.0.0.100-200
```
3. Add your `username password allowhost`
such as
`myname mypassword *`
4. Add DNS servers to a file,which can be found in `/etc/pptpd.conf `
```
# TAG: option
#       Specifies the location of the PPP options file.
#       By default PPP looks in '/etc/ppp/options'
#
option /etc/ppp/options.pptpd
```
  Note: you should edit the file which **option** point to.
5. Restart your pptpd.
`service pptpd restart`
6. Verify that is running and accepting connections: `netstat -tlnp|grep 1723`
7. Setup Forwarding.  Simply edit /etc/sysctl.conf and add the following line if it doesn’t exist there already:
`net.ipv4.ip_forward = 1 `
To make changes active, run **sysctl -p**
And if there's some error information like below:
```
# Do not accept source routing
error: "net.bridge.bridge-nf-call-ip6tables" is an unknown key
error: "net.bridge.bridge-nf-call-iptables" is an unknown key
error: "net.bridge.bridge-nf-call-arptables" is an unknown key
```
  This may happens on openvz vps,just comment these 3 lines.And then run `sysctl -p`
8. Create a NAT rule for iptables
`iptables -t nat -A POSTROUTING -o venet0 -j MASQUERADE && iptables-save`
Noteworthy: Many tutorial alway use eth0.But it is always venet0 on a vps.
9. Ok,that's all.[Setting on ios](http://strongvpn.com/setup_ios_9_pptp.html)
