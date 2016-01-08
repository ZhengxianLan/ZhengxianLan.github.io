---
title: git stash
layout: post
date: 2016-01-08 21:00:43
categories: ['git']
tags: ['stash']
---

使用git stash 暂存未提交的修改
待优先级高的需求开发并提交之后
我们还需要把之前的工作现场还原
就需要使用 git stash pop
但是，如果有文件冲突，就会提示pop fail
需要手工合并冲突,再 git stash drop

git stash
git stash pop
git stash list
git stash show
git stash show -p
