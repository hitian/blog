---
aliases:
- /database/2011/07/12/simple-mysql-table-level-locks/
categories:
- database
category_backup: database
date: "2011-07-12T00:00:00Z"
description: ""
tags:
- mysql
- php
title: 简单的mysql表级锁
---
由于业务需要, 一个表不允许两个及以上用户同时操作.由于是mysql4.1 事务等高级特性就不用想了, 存储引擎统一使用的MyISAM.其实转换成InnoDB 就可以使用行级锁了, 但是由于是线上正在使用的数据库, 数据量比较大,转换怕出现意外. 要实现的功能又是在后台使用. 所以就直接使用表级锁完全可以了.在mysql中对表加锁和解锁其实挺简单的

```mysql
LOCK TABLE table_name;
UNLOCK TABLE;
```

demo

```php
<?php

$db = new Mysql();
$db->open();
$db->query("lock table " . $table);
$sql = "SELECT * FROM ${table} WHERE gid='${gid}' AND sid IS NULL LIMIT 1";
$row = $db->fetch($db->query($sql));
$success = false;
$re = array();
if($row){
	$id = $row['id'];
	$update_data = array(
		'udate' => date('Y-m-d H:i:s'),
		'tuid' => $uid,
		'operator' => $_COOKIE['admin_name'],
		'sid' => $sid,
		);
	$db->update($this->table, $update_data, "id='$id'");
	$result = $db->getAffectedRows();
	if($result > 0){
		$success = true;
		$re = $row;
	}else{
		$msg = '发生错误: 物品库存修改失败!';
	}
}else{
	$msg = '库存不足';
}
$db->query("unlock table");
$db->close();
//return array($success, $msg, $re);

```
