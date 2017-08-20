---
title: mysql定时任务
date: 2017/7/3
tags: 数据库
---

要想保证能够执行event事件，就必须保证定时器是开启状态，默认为关闭状态

```
set GLOBAL event_scheduler = 1;
or
set GLOBAL event_scheduler = ON; 
```
要查看当前是否已开启事件调度器，可执行如下SQL：

```
SHOW VARIABLES LIKE 'event_scheduler'
or
SELECT @@event_scheduler;
```

查看事件
```
select * from mysql.event
```

如果原来存在该名字的任务计划则先删除  

```
drop event upload_to_sdmp;
```
<!-- more -->
新建一个定时任务

```
--从20170704开始每天执行
CREATE EVENT e_test
ON SCHEDULE EVERY 1 DAY STARTS TIMESTAMP '2017-07-04 00:00:00'
DO DELETE FROM `table` WHERE `time`< now();


--五天后执行
CREATE EVENT e_test
ON SCHEDULE AT CURRENT_TIMESTAMP + INTERVAL 5 DAY
DO TRUNCATETABLE test.aaa;


--20170704开始执行
CREATE EVENT e_test
ON SCHEDULE AT TIMESTAMP '2017-07-04 00:00:00'
DO TRUNCATETABLE test.aaa;


--每天执行五天后结束
CREATE EVENT e_test
ON SCHEDULE EVERY 1 DAY
ENDS CURRENT_TIMESTAMP+ INTERVAL 5 DAY
DO TRUNCATETABLE test.aaa;


--每天执行，但执行一次就结束-ON COMPLETION NOT PRESERVE
CREATE EVENT e_test
ON SCHEDULE EVERY 1 DAY
ON COMPLETION NOT PRESERVE
DO TRUNCATETABLE test.aaa;
```

关闭\打开事件：

```
--关闭事件：  
ALTER EVENT upload_to_sdmp DISABLE; 

--开启事件：  
ALTER EVENT upload_to_sdmp ENABLE; 
```
