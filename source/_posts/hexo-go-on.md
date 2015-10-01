---
title: hexo go on
date: 2015-09-15 16:26:34
categories: ['etc','hexo']
tags:
---

```bash
sudo apt-get install -y git nodejs-legacy npm
git clone -b source git@github.com:ZhengxianLan/ZhengxianLan.github.io.git blog  

cd blog
sudo npm install hexo-cli -g
npm install hexo hexo-server  hexo-deployer-git --save
hexo init ; npm install
rm source/_posts/hello-world.md themes/landscape/source/css/images/banner.jpg ;

git checkout .
hexo g
hexo s # start server locally for test

```
