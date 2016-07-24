---
title: mysql-master-slave-conf
layout: post
date: 2015-11-01 18:18:53
categories: ['mysql','master']
tags: ['master']
---
转自 [Mysql 主从配置](http://www.pengzihe.com/?p=186)

系统环境：Centos6.5

Mysql:5.5.30

Master: 10.128.129.159

Slave: 10.128.128.12

- 修改 mysql 配置文件
 - 在 master 服务器上，编辑 /etc/my.cnf 文件中的”[mysqld]”段添加如下内容：
```
server-id=1

log-bin=mysql-bin

relay-log=mysql-relay-bin
```
 - 在 slave 服务器上，编辑 /etc/my.cnf 文件中的”[mysqld]”段添加如下内容：
```
server-id=2   #server-id 一定不能和主的 server-id 相同，否则 replication 功能会报错。

log-bin=mysql-bin #启动二进制日志功能

relay-log=mysql-relay-bin   #启动中继日志功能，如果是主服务器的话可以不启用
```
- 创建复制用户并授权
 在 master 服务器上创建复制用户：
```sql
grant replication slave on *.* to ‘pzh’@’10.128.128.12’ identified by ‘123456’;
```

- 手动同步数据库
 - 如果 master 上已经有 mysql 数据，那么在执行主从备份之前，需要将 master 的和 slave 上的两个 mysql 的数据库保持同步，首先在 master 上执行全局读锁定，然后再备份数据库：
```mysql
mysql>FLUSH TABLES WITH READ LOCK;   #全局读锁定，不能写入数据
```
 - 执行上面语句之后不要退出这个终端，否则这个锁就失效了，退出这个终端数据库默认会执行 unlock tables 语句。打开另外一个终端，使用 mysqldump 工具备份数据库，我这里的 master 数据库里面有一个 pzh 的数据库，所以使用 mysqldump 工具先把这个数据库备份出来：
```bash
#mysqldump –uroot –p pzh > pzh.sql ## 备份 pzh 数据库
```

 - 在从数据库中首先导入 pzh 数据库：
```sql
mysql>create database pzh;  ## 创建数据库
```
```bash
#mysql –uroot –p pzh < pzh.sql   ## 导入数据库
```

- Slave 数据库同步 master 数据库
 - mysql>show master status;
 - 在 slave 中将 master 设置为主服务器
```sql
mysql>change master to master_host=’10.128.129.159′, master_user=’pzh’,   master_password=’123456′,master_log_file=’mysql-bin.000006′, master_log_pos=360;
```
## 这里需要注意的是 master_log_file 和 master_log_pos 这两个选项，这两个选项的值实际上对应的就是 master 数据库上通过 sql 语句”show master status;”查询到的结果。

 - 在从服务器上启动 salve 服务，执行如下命令：
```mysql
mysql>start slave;    ### 启动 slave 服务

mysql>show slave status\G;   ## 查看 slave 的运行状态
```
至此,mysql 主从配置搭建完成。
