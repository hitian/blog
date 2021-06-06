---
aliases:
- /mysql/2011/12/01/mysql-table-lock/
categories: mysql
date: "2011-12-01T00:00:00Z"
tags:
- mysql
- table
- lock
title: mysql表锁定中不支持清空操作?
---
偶然发现这个问题

```mysql

mysql> lock table test_my write;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from test_my;
Empty set (0.00 sec)

mysql> insert into test_my values(null, 1, 1);
Query OK, 1 row affected (0.00 sec)

mysql> select * from test_my;
+—-+——+——–+
| id | pid  | enable |
+—-+——+——–+
|  1 |    1 |      1 |
+—-+——+——–+
1 row in set (0.00 sec)

mysql> truncate table test_my;
ERROR 1192 (HY000): Can't execute the given command because you have active lock
ed tables or an active transaction

```

`TRUNCATE TABLE was not allowed under LOCK TABLES.`

表锁中不可以使用清空操作, 这个让我很郁闷.


```mysql

mysql> delete from test_my;
Query OK, 1 row affected (0.00 sec)
 

mysql> alter table test_my auto_increment=1;
Query OK, 0 rows affected (0.05 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> select * from test_my;
Empty set (0.00 sec)

mysql> insert into test_my values(null, 99, 1);
Query OK, 1 row affected (0.00 sec)

mysql> select * from test_my;
+—-+——+——–+
| id | pid  | enable |
+—-+——+——–+
|  1 |   99 |      1 |
+—-+——+——–+
1 row in set (0.00 sec)

mysql>

```


分布操作: 先删除所有数据, 然后重新设置自增长的起始点却可以. 


