---
aliases:
- /mysql/2012/04/12/sql-note/
categories: mysql
date: "2012-04-12T00:00:00Z"
tags:
- mysql
- sql
title: sql笔记
---

一般数据库会根据使用的情况来设计, 为了保证性能一般链接的表不应该超过2个,

但是在数据统计汇总运算的时候就麻烦了, 往往需要查好多张表才能得到需要的数据.

如果查询很复杂的时候一次join好多张表会导致查询速度巨慢.

当然用php等动态语言把数据取出来进行运算也不错,就是需要写代码.

 

能在数据库内完成的运算 当然想要直接在库内完成

使用临时表存储计算过程中的中间数据时隔不错的选择


使用临时表进行数据统计， 避免出现超过3个以上的join操作

--以防万一先删除表

`drop table if exists tmp_table;`


--创建临时表的时候加上temporary 关键字就行了

`create temporary table tmp_table(uid bigint not null primary key, cityid int(10) unsigned, prov varchar(50), city varchar(50), group_id tinyint );`

 

--把多个表中需要的数据组合在一起插入临时表中, 暂时没有的数据插入空字符串

`insert into tmp_table select a.uid as uid, a.cityid as cityid,'','',0 from my_city a inner join my_op b on a.uid=b.uid where a.reg_date='2012-04-10' and b.ctime>0;`


--联合多表更新

`update tmp_table a, my_city b  set a.prov=b.prov, a.city=b.area where a.cityid=b.id;`


--还是联合多表更新

`update tmp_table a, my_city_auth b  set a.group_id=b.group_id where a.prov=b.prov;`

 

--剩下的就只是把算好的数据读出来了.

而且临时表是session级别的, 只有当前用户可见, 而且断开链接后就自动清理了.


