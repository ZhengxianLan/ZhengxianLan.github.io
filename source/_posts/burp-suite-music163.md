---
title: burp-suite-music163
date: 2015-07-29 15:17:51
categories: linux
tags: music
---

pc 在线 musci.163.com 并不能下载音乐
用 burp suite 获取 mp3Url 并转换为 curl command
在我的音乐界面，开启 burp, 一般会截获到类似 'http://music.163.com/weapi/playlist/detail' 的 url，通过 search mp3Url，
可以看到 mp3 文件是存储在 m1.music.126.net, 比如一首歌曲为 m1.music.126.net/JNCvf4SsNgWK7ybuYLj2JQ==/3108319371772876.mp3

```ruby
#!/usr/bin/env ruby
# coding: utf-8
require 'fileutils'

location='/tmp/163/'
FileUtils::mkdir_p(location) unless File.exist?(location)
Dir.chdir(location)
# use burp suite to get curl command content
curl="$curl"
`#{curl}|grep -o 'mp3Url":"[^"]*'|sed's/mp3Url":"//g' |wget -ci -`

Dir.glob("*.mp3").each do |f|
	fn= `mid3v2 "#{f}"|grep TIT2|sed s'/TIT2=//g'`
	File.rename("#{f}","#{fn.to_s.strip}.mp3")
end

```

如果只是下载当前播放的
