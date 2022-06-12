---
title: SQL 基础 - GROUP BY, WHERE, HAVING 用法及顺序
tags: Sql
---

昨天学习SQL，碰到了个小问题：既然where和having都可以表示“有条件地筛选出想要的内容”，那么，它们的区别是什么呢？

<!--more-->

肯定是有区别的，否则的话SQL语言体系当中没必要出现两个作用和适用场合一样的关键子句，那样太啰嗦了，绝不是一种有逻辑、自成体系的计算机语言会出现的情况~~

## 总结

1、select子句及其顺序

```roomsql
select from where group by having order by limit
```

1）在SQL语句中，where子句并不是必须出现的

2）where子句是对检索记录中每一行记录的过滤。

3）having子句出现在group by子句后面。

where子句对检索结果中每一条记录第一次过滤后，group by对每条记录进行分组，having对各个组中的记录进行再次过滤。因此是先通过where子句过滤、再通过group by子句分组、最后通过having子句对组中记录再次进行过滤。

4）如果在一句SQL语句中，where子句和group by……having子句同时都有的话，必须where子句在前，group by……having子句在后。

即group by子句必须出现在where子句之后，having子句必须在group by子句之后。

（where先执行，再groupby分组；groupby先分组，having再执行）

5）group by子句是对检索记录的分组，后面跟随的字段名是分组的依据。根据语法，在select子句中，除聚合函数语句外，SELECT子句中的每个列名称都必须在GROUP BY子句中作为分组的依据。

比如说按照姓名分组的正确示范：

查出没门课程都大于80分的学生姓名，select name from table group by name having min(grade) > 80

（注意，where没有出现哦~~）

比如带着聚合函数的正确示范：

select vend_id, count(*) from products group by vend_id;

比如错误的示范：

select vend_id, count(*) , sales from products group by vend_id;

这个语句，其中sales字段，在group by中没有，所以查询的结果，sales的值是错误的！

把它改正确的方法是将sales字段添加到group by子句中，语法倒是正确了，但是不是有业务上的实际含义，这个就另说了。

## 问题

> select和having哪个先执行？select name from student group by name having min(score)>80可以执行，但是在select后面没有这个列是为什么？

having 先于select执行，select后面没有score列，但是student表里有score列啊。先执行的having,此时score是student表里的字段，所以能取到。

这段sql的整体执行顺序是：

1.from student表;

2.group by name按学生分组;

3.having min(score>80)找到每门成绩都不低于80分的学生;

4.select name取出名字.
