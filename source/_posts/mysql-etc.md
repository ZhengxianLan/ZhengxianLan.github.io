---
title: mysql-etc
date: 2015-09-18 08:53:27
categories: [db,mysql]
tags: sql
---

一般不建议使用自增主键。因其在分布式，数据迁移，跨库都容易产生问题。
对于NOT NULL的列，插入NULL时，会变为0
having通常用于group之后的过滤。
having不走索引，故是不可替代where的

对以存在数据的表进行插入操作
insert ignore
replace into

- 建表
    1.  -- 表结构相同  [ create table ... lilke ] + [ insert into ... select * from ] 
        create table new_table1 like origin_table;
        insert into new_table1 select * from origin_table;
        describe new_table1;
    2. --表结构不同 	[ create table ... select * from ... ]
        create table new_table2 select * from origin_table where ...;
        describe new_table2;
- 删表 delete vs truncate
    1. 若表为自增型主键，自增主键设置从n开始:
        - 那么在delete数据之后，新插入数据会重新从n递增。
        - truncate table 会破坏表结构，新插入数据的主键值从n开始
    2. truncate 不可在事物内使用，而delete可以。
    3. 删除2个表的交集
        - delete t1,t2 from table1 as t1,table2 as t2 where t1.id=t2.id;

---
### analyze query        

1. `show variables like 'slow_query_log';`
  if off then => set global slow_query_log=on;
2. `show variables like 'log_queries_not_using_indexes';`
  if off then => set global log_queries_not_using_indexes=on;
3. `show variables like 'long_query_time';`   # a variable to tell whether a query is slow query.
  for test => set global long_query_time=0; # i.e. record every queries.
4. `show variables like 'slow_query_log_file';` # show where is slow_query_log_file ?
5. tool
  1. mysqldumpslow
  2. pt-query-digest
     -> output to file : pt-query-digest slow-log > slow_log.report
     -> output to database table:
        pt-query-digest slow.log -review h=127.0.0.1,D=test,p=root,P=3306,u=root,t=query_review \
        --create-reviewtable --revew-history t=hostname_slow
6. pt-query-digest /path/to/your/slow_query_log  (can be found in mysql: show variables like 'slow_query_log_file'; )
	typically,use pt-query-digest with root user,cause slow_query_log_file maybe out of user's home dir
