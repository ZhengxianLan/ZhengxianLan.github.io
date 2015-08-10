---
layout: post
title: Update brightness on ubuntu
categories: linux
tags: ubuntu
description: update brightness on ubuntu
---

Acer 笔记本安装 ubuntu 后，若不能调整亮度
```bash
sudo sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX="acpi_osi=Linux acpi_backlight=video"/' /etc/default/grub
sudo sed -i '$i echo 50 >/sys/class/backlight/intel_backlight/brightness' /etc/rc.local
sudo update-grub
```