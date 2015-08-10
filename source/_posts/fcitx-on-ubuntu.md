---
layout: post
title: Fcitx on ubuntu
categories: linux
tags: fcitx
description: fcitx on ubuntu
date: 2015-07-10
---


1. 曾经因为改了 fcitx 的默认配置文件。。。竟然演变到重装系统。

    安装 google-pinyin 之后，一般我们都习惯于在 google-pinyin 下输入时，按下左边的 shift 键就能输入英文，很是方便。
    这个是需要在 fcitx 的 => System tray icon[系统托盘图标上右键]=> config=> Global config => Extra key for trigger input method => L_SHIFT

2. 如果不能輸入簡體字

    右键 fcitx 图标 => configi=> addon=> 勾选 advance=> 取消选中 Simplified Chinese To Traditional Chinese

3. 重装系统后，安装 fcitx 不能切换的话，记得把系统的默认切换输入法禁用，否则，不能切换 fcitx 的输入法。

    system setting => keyboard => Typing => , 将 4 个 short cut 全部停用。

```bash
#sudo apt-get purge ibus, do not remove ibus on ubuntu.Some software integate with it
sudo apt-get install -y fcitx fcitx-googlepinyin im-switch fcitx-config-gtk fcitx-table-all
im-switch -s fcitx -z default
mkdir -p ~/.config/fcitx/data
cp ~/data/note/ubuntu/fcitx/QuickPhrase.mb ~/.config/fcitx/data/QuickPhrase.mb
mkdir -p ~/.config/fcitx/skin/
tar -xf ~/data/note/ubuntu/fcitx/Dunkel.tgz -C ~/.config/fcitx/skin/
```
