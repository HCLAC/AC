---
title: mysql常识&命令&高级用法
date: 2020-11-25 17:19:53
tags: mysql
description: mysql学习笔记
---

### 常用命令
- 命令行登录服务`mysql -h 127.0.0.1:3306 -u root -p`回车后，输入密码
- 命令行操作别忘记最后的分号;
- 跳转到data01数据库：`use data01`
- 创建用户&授权用户
    - 创建本地用户:user01,密码:xxx:`create user user01@'%' identified by 'xxx';`
    - 创建远程用户：`create user ‘user02’@'%' identified by 'xxx';`
    - 授权data01数据库全部权限给user01`grant all privileges on data01.* to user01;`
    - 授权data01数据库只读权限给user02`GRANT SELECT ON data01.* TO 'user01';`
    - 有可能修改my.conf，去掉`bind-address=127.0.0.1`
- 显示所有用户:`SELECT DISTINCT User FROM mysql.user;`
- 显示所有数据库：`show databases;`
- 显示所有表：`show tables;`
- 删除数据库：`drop data001;`
- 创建新库data01：`create database data01; create schema data01;`
- table01表在col01添加col02字段，类型：varchar(255)，默认值：xxx`alter table table01 add column col02 varchar(255) DEFAULT NULL after col01;`
- table01表修改col01字段属性：`alter table table01 modify col01 varchar(500) default '' not null comment '图片地址';`
- 插入数据：`INSERT INTO table01(col01,col02) VALUES(xx,xx);`
- 修改table01表的col01字段数据，添加字符串前缀，Update后面可以加where：`UPDATE table01 SET col01= CONCAT('user_',col01);`
- 删除数据：`delete from table01 where ...`
- [索引]()操作：
    - 删除索引：drop index idx_index001 on table01;
    - 创建索引：create index idx_index001 on table01 (col01, col02);
- 性能优化命令：[explain](https://segmentfault.com/a/1190000008131735)
- [联合索引和单列索引](https://blog.csdn.net/Abysscarry/article/details/80792876)，主演最左前缀原则
- 删除所有记录：`delete from tb;` or `truncate （table） tb;`，[区别](https://www.cnblogs.com/fcc-123/p/10672604.html)
- 导入sql语句文件: `source filename`, 支持相对地址

### 小知识点
- text不能设置默认值

### 高级用法
- [乐观锁](https://www.cnblogs.com/laoyeye/p/8097684.html) 使用版本号实现乐观锁，在数据库中表中有一个version字段，每次修改判断version的值，
来判断是否有其他地方在修改同一个记录是否可以修改。
- [软删除](https://www.jianshu.com/p/889365078e24) 添加delete_at字段，一般的ORM库都支持. [弊端](https://ruby-china.org/topics/34540)