---
title: oracle database
layout: post
date: 2015-10-31 20:34:20
categories: ['db','oracle']
tags: ['sql']
---

### 数据词典
1. 查询某用户下所有表
```sql
select table_name from all_tables where owner='SCOTT';
```
2. 查询emp表中所有字段
```sql
select*from all_tab_columns where table_name='EMP';
```
3. 列出表达索引列
```sql
select*from sys.all_ind_columns where table_name='EMP';
```
4. 列出表中的约束
```sql
select*from all_constraints where table_name='EMP';
```
5.在oracle中描述数据字典视图
```sql
select table_name,comments from dictionary where table_name like '%TABLE%';
```

- 不想插入null怎么办
若插入了确定的值，就算插入的值为null也不会使用默认值地，所以可以在己判断
sql=INSERT INTO T_sys_user (nm_sid,st_name,nm_status) VALUES(seq.nextval,?,?);
preparedstatement.setstring(1,'值')
preparedstatement.setint(2,('值'==null)? '1' : '值'); //null时设置成缺省值

### 开窗函数
over();
sum(sal) over(order by ename)                     --> 出现order by 就是连续求和
sum(sal) over(partition by deptno order by ename) --> 按分区连续求和
sum(sal) over(partition by deptno)                --> 求每个分区的薪资总和


rank() over()求每个部门的最高工资
```sql
  SELECT * FROM (
    SELECT RANK() OVER(pARTITion BY deptno ORDER BY sal DESC) r,deptno,sal max FROM emp
  ) WHERe r=1;
```
  --用rank()会出现并列第一
```
  SELECT * FROM (
  SELECT ROW_NUMBER() OVER(PARTITION BY deptno ORDER BY sal desc) r,deptno,sal max FROM emp
) WHERE r=1;
```
  --用row_number()，若有并列，只是取一个第一,row_number()没有排名功能，和rownum差不多


--计算出各个部门工资最高的员工，其工资占部门总薪资的比重
```sql
SELECT * FROM (
  SELECT ROW_NUMBER() OVER (PARTITION BY deptno ORDER BY percent DESC) r, percent, deptno, ename, sal
  FROM (
    SELECT sal, ROUND(sal / (SUM(sal) OVER (PARTITION BY deptno)), 2) percent, deptno, ename
    FROM emp
  )
) WHERE r = 1
;
```


--各工种人数
```sql
(SELECT
   SUM(DECODE(job, 'CLERK', 1, 0    )) CLERK,
   SUM(DECODE(JOB, 'SALESMAN', 1, 0 )) SALESMAN,
   SUM(DECODE(JOB, 'MANAGER', 1, 0  )) MANAGER,
   SUM(DECODE(JOB,'ANALYST',1,0     )) ANALYST,
   SUM(DECODE(JOB,'PRESIDENT',1,0   )) PRESIDENT
--    , deptno
   ,COUNT(*) total
 FROM emp
--  GROUP BY deptno
)
;
```

