---
title: mysql-join
date: 2015-08-10 22:36:49
categories: [db,mysql]
tags: [sql]
---

### 使用 join 优化子查询
每一条外表记录都要进行一次子查询，因而速度慢，在外表数据量大的时候尤为明显。可用 EXPLAIN/DESC sql 来查看查询计划以及查询类型
若 DESC 发现，有 SIMPLE + DEPENT SUBQUERY 2 个 SELECT_TYPE, 那么极有可能造成慢查询。使用 JOIN 发现会是 2 个 SIMPLE 类型的 SELECT_TYPE
  [mysql-optimization](https://dev.mysql.com/doc/refman/5.5/en/optimization.html)
  [join-vs-sub-query](http://stackoverflow.com/questions/2577174/join-vs-sub-query) ~~高票答案竟未被采纳~~1
  [子查询是规范，较早出现且可读性更好、易于理解，join是现代dbms加强后出现的](https://dev.mysql.com/doc/refman/5.5/en/rewriting-subqueries.html)

- VS
```sql
  SELECT a.user_name a.over,(SELECT over FROM user2 b WHERE a.user_name = b.user_name) AS over2 FROM user1 a;
  --join
  SELECT a.user_name,a.over,b.over AS over2 FROM user1 a LEFT JOIN user2 b on a.user_name = b.user_name;
```


