---
title: mysql-master-slave-conf
layout: post
date: 2015-11-01 18:18:53
categories: ['mysql','master']
tags: ['master']
---
转自 [Mysql主从配置](http://www.pengzihe.com/?p=186)

系统环境：Centos6.5

Mysql:5.5.30

Master: 10.128.129.159

Slave: 10.128.128.12

- 修改mysql配置文件
 - 在master服务器上，编辑/etc/my.cnf文件中的”[mysqld]”段添加如下内容：
```
server-id=1

log-bin=mysql-bin

relay-log=mysql-relay-bin
```
 - 在slave服务器上，编辑/etc/my.cnf文件中的”[mysqld]”段添加如下内容：
```
server-id=2   #server-id一定不能和主的server-id相同，否则replication功能会报错。

log-bin=mysql-bin #启动二进制日志功能

relay-log=mysql-relay-bin   #启动中继日志功能，如果是主服务器的话可以不启用
```
- 创建复制用户并授权
 在master服务器上创建复制用户：
```sql
grant replication slave on *.* to ‘pzh’@’10.128.128.12’ identified by ‘123456’;
```

- 手动同步数据库
 - 如果master上已经有mysql数据，那么在执行主从备份之前，需要将master的和slave上的两个mysql的数据库保持同步，首先在master上执行全局读锁定，然后再备份数据库：
```mysql
mysql>FLUSH TABLES WITH READ LOCK;   #全局读锁定，不能写入数据
```
 - 执行上面语句之后不要退出这个终端，否则这个锁就失效了，退出这个终端数据库默认会执行unlock tables语句。打开另外一个终端，使用mysqldump工具备份数据库，我这里的master数据库里面有一个pzh的数据库，所以使用mysqldump工具先把这个数据库备份出来：
```bash
#mysqldump –uroot –p pzh > pzh.sql ##备份pzh数据库
```

 - 在从数据库中首先导入pzh数据库：
```sql
mysql>create database pzh;  ##创建数据库
```
```bash
#mysql –uroot –p pzh < pzh.sql   ##导入数据库
```

- Slave数据库同步master数据库
 - mysql>show master status;
 - 在slave中将master设置为主服务器
```sql
mysql>change master to master_host=’10.128.129.159′, master_user=’pzh’,   master_password=’123456′,master_log_file=’mysql-bin.000006′, master_log_pos=360;
```
##这里需要注意的是master_log_file和master_log_pos这两个选项，这两个选项的值实际上对应的就是master数据库上通过sql语句”show master status;”查询到的结果。

 - 在从服务器上启动salve服务，执行如下命令：
```mysql
mysql>start slave;    ###启动slave服务

mysql>show slave status\G;   ##查看slave的运行状态
```
至此,mysql主从配置搭建完成。
