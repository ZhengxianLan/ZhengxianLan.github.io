---
title: rbenv-install-local-ruby.tar
date: 2015-10-07 16:02:03
categories: ['ruby] 
tags: ['rbenv'] 
---

```
$ wget http://ftp.ruby-lang.org/pub/ruby/2.1/ruby-2.1.2.tar.gz

$ tar xvfz ruby-2.1.2.tar.gz
$ cd ruby-2.1.2
$ ./configure --prefix=$HOME/.rbenv/versions/2.1.2
$ make
$ make instalb

$ rbenv global 2.1.2
$ rbenv rehash
$ gem update --system
$ gem install bundler
$ rbenv rehash
