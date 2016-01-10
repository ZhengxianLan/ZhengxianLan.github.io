---
title: docker_tutorial
layout: post
date: 2016-01-10 01:35:19
categories: ['docker']
tags: ['tutorial']
---
```bash
docker ps -a                                       # 查看所有容器状态,如果没有创建容器，没数据了
docker run [image_name]                            # 指定一个镜像，docker会基于该容器创建一个容器，默认会退出,
                                                   # 实际上，docker run= docker create + docker start
docker run -dit [image_name] --name [named_you]    # 通过增加 -dit，使docker不立即退出
                                                   # -i,--interactive =false Keep STDIN open even if not attached
                                                   # -t,--false Allocate a pseudo-TTY
                                                   # -d,--detach=false 让启动的docker后台运行，并且打印其pid。
                                                   # 若不带-d参数，那么由于-it参数，将直接进入docker容器内
---
docker start 容器id或名字                          # 启动容器，并打印其pid。容器后台运行
docker start -ai                                   # 启动并连接到容器内
                                                   # -a,--attach=false 连接到容器内，绑定输出和错误输出，这不能往容器内输入命令
                                                   # -i，--interactive=false 链接到容器，并绑定容器的标准输入，
                                                   # 一般用-i就够了
---
docker stop 容器id或名
docker attach named_you                            # 通过指定名字，连接到docker内部,也可以通过制定id
ctrl-p ctrl-q                                      # 跳出本次会话，而不中止docker容器的运行
```

状态为 exited，stop,created 的都是已经停止运行的容器，需要start之后才能连接

## build your own image
### create a docker image from an existing container
####docker commit
- run container --> modify it --> stop it --> create image from it

```bash
# docker run -i -t --name guest oraclelinux:6.6 /bin/bash
# yum install httpd
# docker stop guest
# docker commit -m "add httpd" -a "lanzhx"  `docker ps -l -q` mymod/httpd:v1
# docker images # lists images
# docker rm guest
# docker run -d --name newguest -p 8080:80 mymod/httpd:v1 /usr/sbin/httpd -D FOREGROUND
# docker ps
# docker port newguest 80
# docker run -d --name newguest -p 127.0.0.1:8080:80 -p 192.168.1.2:8080:80 \
           mymod/httpd:v1 /usr/sbin/httpd -D FOREGROUND   # using multiple -p options to bind restrict IP addr
# docker rmi mymod/httpd:v1   # remove an image
```
[Create a docker image from an existing container](https://docs.oracle.com/cd/E52668_01/E54669/html/section_c5q_n2z_fp.html)
### Building an image from a Dockerfile
```bash
$ mkdir sinatra
$ cd sinatra
$ touch Dockerfile

```
Dockerfile
<pre>
# This is a comment
FROM ubuntu:14.04
MAINTAINER Kate Smith <ksmith@example.com>
RUN apt-get update && apt-get install -y ruby ruby-dev
RUN gem install sinatra
</pre>

cd to directory which contain Dockerfile
```bash
$ docker build -t ouruser/sinatra:v2 .
```
instruction:
- You’ve specified our docker build command and used the -t flag to identify our new image as belonging to the user ouruser, the repository name sinatra and given it the tag v2.

- You’ve also specified the location of our Dockerfile using the . to indicate a Dockerfile in the current directory.
  Note: You can also specify a path to a Dockerfile.
```bash
$ docker run -t -i ouruser/sinatra:v2 /bin/bash
```
[Build your own images](https://docs.docker.com/engine/userguide/dockerimages/)
