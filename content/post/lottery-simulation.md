---
aliases:
- /php/2011/11/23/lottery-simulation/
categories: php
date: "2011-11-23T00:00:00Z"
tags:
- php
- lottery
title: 使用类似于奖池方式的抽奖模拟
---

一般来说要做一个抽奖程序会立即想到用随机数. 但是通常情况下随机数也不是那么的随机, 数量比较大的时候才能体现出比较平均, 数量较小的时候中奖概率的可控性将会很差. 一不小心多被抽中几个麻烦就大了.

```php


<?php
class T_prize{
    var $item;
    var $table;
 
/**
 * 抽奖主程序 外部调用入口
 * @param unknown_type $db 数据库链接
 * @param unknown_type $id 抽奖类型
 */
     function prize($db, $id){
         global $prize_set;
        //
         if(!isset($prize_set[$id])){
             return false;
         }
         $this->item = $prize_set[$id];
         $table = 'my_prize_store_table_' . $id;
         //抽奖并发较大时可以进行分表, 多生成几个奖池
         //$table = 'my_prize_store_table_' . $id . '_' . rand(0, 4);
         $this->table = $table;
         //防止多个用户抽到同一个条目, 进行锁表
         $db->query("lock table " . $table . " write");
         $pid = $this->get_one($db, $id);
         //奖池中没有奖品时重新初始化奖池
         if($pid == '-1'){
             $this->create_pool($db, $id);
             $pid = $this->get_one($db, $id);
         }
         //释放锁 返回抽奖结果
         $db->query("unlock table");
         return $pid;
      }
 
/**
  * 抽奖一次
  * @param unknown_type $db
  * @param unknown_type $id
  */
     function get_one($db, $id){
         $table = $this->table;
         $sql = "SELECT * FROM ${table} WHERE enable = '1' LIMIT 1";
         $row = $db->fetch($db->query($sql));
         $success = false;
         $re = array();
         if($row){
             $id = $row['id'];
             $pid = $row['pid'];
             $update_data = array(
                 'enable' => 0,
             );
             $db->update($table, $update_data, "id='$id'");
             $result = $db->getAffectedRows();
             if($result > 0){
                 $success = true;
                 $re = $row;
             }else{
                 my_log('抽奖池条目状态修改发生错误 id:' . $id);
             }
         }else{
             return '-1';
         }
         if($success){
             return $pid;
         }else{
             return 0;
         }
     }
 
/**
  * 奖品池初始化
  * @param unknown_type $db
  * @param unknown_type $id
  */
     function create_pool($db, $id){
         $table = $this->table;
         my_log('init prize pool id:' . $id);
         $item = $this->item;
         $total_num = $item['total'];
         $list = $item['list'];
         $tprize_list = array();
         foreach ($list as $key=>$one){
             $tnum = intval($total_num * $one['probability']);
             if($tnum < 1){
                 my_log('create prize pool create alert: total_num:' . $total_num . ' item_name:' . $item['name'] . ' probability:' . $item['probability']);
                 continue;
             }
             for ($i = 0; $i < $tnum; $i ++){
                 do{
                     $id = rand(1, $total_num);
                 }while(isset($tprize_list[$id]));
                 $tprize_list[$id] = $key;
             }
         }
         my_log('start');
         $db->query('TRUNCATE ' . $table);
         $insert_str_arr = array();
         for($i=1; $i<=$total_num; $i++){
             $this_pid = 0;
             $inset_data = array('pid'=>0);
              if(isset($tprize_list[$i])){
                  $this_pid = $tprize_list[$i];
              }
              //每100条记录插入一次可以大幅减小数据库插入时间时间
              $insert_str_arr[] = "($this_pid)";
              if($i % 100 == 0){
                  $insert_str = implode(',', $insert_str_arr);
                  $insert_str = 'insert into ' . $table . '(pid) values' . $insert_str;
                  $db->query($insert_str);
                  $insert_str_arr = array();
             }
         }
         my_log('end');
     }
 
  }
 
  //自定义日志
  function my_log($msg){
    //write logs here
  }

```


使用奖池的概念可以将中奖概率严格限制一下,  生成一定数量的格子, 按照概率算出有奖品的格子的数量, 然后再随机填充到格子里去, 执行抽奖的时候一次去打开格子. 要么中奖,要么不中, 当所有格子都被开启过之后,  再次执行初始化重新填充所有的格子.
设置一下需要使用的奖品数据和概率

```php

<?php
$prize_set = array();
//抽奖1
$prize_set[0] = array(
   'total' => 1000,
   'list' => array(
        1 => array('name'=> '奖品1', 'probability'=> '0.01', 'product_id' => 3),
        2 => array('name'=> '奖品2', 'probability'=> '0.05', 'product_id' => 7),
        3 => array('name'=> '奖品3', 'probability'=> '0.1', 'product_id' => 9),
    ),
 );
 
 //抽奖2
 $prize_set[1] = array(
     'total' => 1000,
     'list' => array(
        1 => array('name'=> '奖品1', 'probability'=> '0.001', 'product_id' => 3),
        2 => array('name'=> '奖品2', 'probability'=> '0.005', 'product_id' => 7),
        3 => array('name'=> '奖品3', 'probability'=> '0.01', 'product_id' => 9),
        4 => array('name'=> '奖品4', 'probability'=> '0.02', 'product_id' => 1),
        5 => array('name'=> '奖品5', 'probability'=> '0.05', 'product_id' => 2),
        6 => array('name'=> '奖品6', 'probability'=> '0.1', 'product_id' => 4),
    ),
);

```

我有两组抽奖, 所以定义了两组.  注意, 每组的总数 乘以 这组中最小的一个概率一定要大于1,  不然这个奖品就永远也没可能抽中了.

这种方式也有缺点:

1.概率的调整只能在下次奖池初始化时才能生效,

2.使用数据库表锁来保证同一个格子只能被一个人抽中, 会导致性能降低, (我们使用的是myisam, 只能使用表级锁)无法支持大量的并发, 有个很笨的但最简单的解决方案就是对同一个抽奖使用多个奖池, 将请求分散到各个表去.


后记:表在lock 状态中实际上是不支持truncate 操作的,  先delete from table 删除所有数据, 再 alter table xxx set auto_increment=1  就可以解决这个问题...

