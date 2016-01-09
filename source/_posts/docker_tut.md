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
                                                   # -d,--detach-false 让启动的docker后台运行，并且打印其pid。
                                                   # 若不带-d参数，那么由于-it参数，将直接进入docker容器内
docker start 容器id或名
docker start -ai                                   # 启动并连接到容器内
                                                   # -a,--attach=false 连接到容器内，绑定输出和错误输出，这不能往容器内输入命令
                                                   # -i，--interactive=false 链接到容器，并绑定容器的标准输入，
                                                   # 一般用-i就够了
docker stop 容器id或名
docker attach named_you                            # 通过指定名字，连接到docker内部,也可以通过制定id
ctrl-p ctrl-q                                      # 跳出本次会话，而不中止docker容器的运行
```

状态为 exited，stop,created 的都是已经停止运行的容器，需要start之后才能连接
