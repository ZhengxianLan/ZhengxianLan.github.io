---
layout: post
title: Sublime im bug on linux
categories: linux
tags: [editor]
description: sublime im bug on linux
---

---
linux 上的 sublime 对 CJK 支持并不好，官方似乎也并不在意

以至于 sublime 论坛上用户 **[cjacker](https://www.sublimetext.com/forum/memberlist.php?mode=viewprofile&u=9780&sid=5af5517cb058abf0bdb389d51730852c)** 自己写了个补丁以解决这个问题 [cjk-for-sublime](https://www.sublimetext.com/forum/viewtopic.php?f=3&t=7006&p=63513)

---
解压 sublime_text2 的安装包后，到其执行目录下
```bash
sudo apt-get install gtk+-2.0
gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
LD_PRELOAD=./libsublime-imfix.so ./sublime_text
```
这样，我们就启动了一个支持 fcitx 输入法的 sublime_text2

但是呢，这样我们只能从命令行启动才可以使用带拼音支持的 sublime，不爽呀。

把 sublime.desktop 改为下面这个样子就 ok 啦
<pre>
    [Desktop Entry]
    Encoding=UTF-8
    Name=sublime
    Comment=sublime
    Exec=bash -c 'LD_PRELOAD=~/program/st2/libsublime-imfix.so /home/lan/program/st2/sublime_text' %F
    Exec=bash -c 'LD_PRELOAD=~/program/st2/libsublime-imfix.so /home/lan/program/st2/sublime_text' --new-window
    Terminal=false
    Type=Application
    Icon=~/program/st2/Icon/256x256/sublime_text.png
    StartupNotify=false
</pre>

```bash
sed "s,\~,$(cd ~;pwd),g" ~/app/sublime_text_3/custom.desktop
```

[sublime_imfix.c for sublime-text-3 的 c 源代码](https://gist.githubusercontent.com/ZhengxianLan/84f66a9ec5ccee72898d/raw/ba3db961dfe9b9abee75f0a35cbcd261bb83de99/sublime-inux-fix-im)

发现直接更改自带的 sublime_text.desktop 效果更好
 - 可以直接在 launcher 正确显示图标和输入中文
```
[Desktop Entry]
Version=1.0
Type=Application
Name=Sublime
GenericName=Text Editor
Comment=Sophisticated text editor for code, markup and prose
Exec=bash -c "export LD_PRELOAD='/home/lan/app/sublime_text_3/libsublime-imfix.so'; /home/lan/app/sublime_text_3/sublime_text %F"
Terminal=false
MimeType=text/plain;
Icon=/home/lan/app/sublime_text_3/Icon/256x256/sublime-text.png
Categories=TextEditor;Development;
StartupNotify=true
Actions=Window;Document;

[Desktop Action Window]
Name=New Window
Exec=bash -c "export LD_PRELOAD='/home/lan/app/sublime_text_3/libsublime-imfix.so'; /home/lan/app/sublime_text_3/sublime_text %F"
OnlyShowIn=Unity;

[Desktop Action Document]
Name=New File
Exec=bash -c "export LD_PRELOAD='/home/lan/app/sublime_text_3/libsublime-imfix.so'; /home/lan/app/sublime_text_3/sublime_text %F"
OnlyShowIn=Unity;
```
