---
aliases:
- /mysql/2012/04/12/latin1/
categories: mysql
date: "2012-04-12T00:00:00Z"
tags:
- mysql
- latin1
title: 用latin1来存储汉字真抓狂
---

mysql 是从4.0 升级上来的， 直接使用了latin1_swedish_ci 存储汉字，没想到带来了好多麻烦。


首先是查询的问题

```mysql
mysql> select * from my_city where  city=’温州’;
+—-+——+
| id | city |
+—-+——+
|  2 | 温州 |
|  3 | 梧州 |
+—-+——+
2 rows in set (0.00 sec)
```
 
当然, 如果在city上建了唯一索引, 就会倒大霉...
 
 
然后是排序的问题
 
```mysql
mysql> select * from my_city order by city asc;
+—-+——+
| id | city |
+—-+——+
|  6 | 上海 |
|  2 | 温州 |
|  3 | 梧州 |
|  5 | 西安 |
|  7 | 重庆 |
|  4 | 安徽 |
|  8 | 北京 |
+—-+——+
7 rows in set (0.00 sec)
```


这个顺序看起来更让人郁闷...
 

临时解决方案:

```ALTER TABLE  `my_city` CHANGE  `city`  `city` VARCHAR( 25 ) CHARACTER SET latin1 COLLATE latin1_bin NULL DEFAULT NULL```
 
修改字段的collate 为 latin1_bin 

然后再来看
 
查询正常

```mysql
mysql> select * from my_city where  city=’温州’;
+—-+——+
| id | city |
+—-+——+
|  2 | 温州 |
+—-+——+
1 row in set (0.00 sec)
```
 
排序部分正常
 
```mysql
mysql> select * from my_city order by city asc;
+—-+——+
| id | city |
+—-+——+
|  4 | 安徽 |
|  8 | 北京 |
|  6 | 上海 |
|  2 | 温州 |
|  3 | 梧州 |
|  5 | 西安 |
|  7 | 重庆 |
+—-+——+
7 rows in set (0.00 sec)
```
 
要彻底解决还是要整个转换为gbk ,  当然utf-8 会更好一点.
