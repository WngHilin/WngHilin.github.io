---
title: 数据库学习
date: 2019-08-04 16:16:10
tags: 
- Javaweb
- 学习
- 数据库
- MySQL
categories: JavaWeb
---

## 数据库的基本概念

---

* **数据库**(database)：
  * 用于存储和管理数据的仓库
  * 特点：
    1. 持久化存储数据（数据库就是一个文件系统）
    2. 方便存储和管理数据
    3. 使用同意的方式操作数据库  --- SQL
  * 数据库软件：Oracle、MySQL、Microsoft SQL Server、DB2

***

## MySQL

### 配置

- 服务启动：
  - net start mysql：需要管理员权限
  - services.msc
- 登录：
  1. mysql -uroot -p**自己的密码**(mysql -uroot -p也可)
  2. mysql -h**ip** -uroot -p**链接目标的密码**（ip替换为目标的ip地址）
  3. mysql --host=**ip** --user=root --password=root
- 退出：
  1. exit
  2. quit



### 目录结构

 	1. 安装目录：
     * 配置文件 my.ini
 	2. 数据目录
     * 数据库：文件夹
     * 表：文件
     * 数据：文件中存储的数据

***

## SQL

### 基本概念

* Struct Query Language：结构化查询语言
* 操作所有关系型数据库的规则
* 每一种数据库操作方式存在不一样的地方，称为“方言”

### 通用语法

1. SQL语句可以单行或多行书写，以分号结尾
2. 使用空格和缩进增强语句可读性
3. MySQL的SQL语句不区分大小写，但关键字建议用大写
4. 3种注释：
   * 单行注释：**-- 注释内容**或 **# 注释内容**（注意空格）
   * 多行注释：**/* 注释 */**

### 分类

1. **DDL(Data Definition Language)数据定义语言**
   	* 用来定义数据库对象：数据库，表，列等。关键字：create, drop,alter 等
2. **DML(Data Manipulation Language)数据操作语言**
   	* 用来对数据库中表的数据进行增删改。关键字：insert, delete, update 等
3. **DQL(Data Query Language)数据查询语言**
   	* 用来查询数据库中表的记录(数据)。关键字：select, where 等
4. **DCL(Data Control Language)数据控制语言**(了解)
   	* 用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT， REVOKE 等

### DDL：操作数据库、表

1. 操作数据库：CRUD

   1. C(**Create**)：创建

      * 创建数据库
        * create database 数据库名;

        * create database if not exists 数据库名;-- 如果数据库不存在，则创建数据库
        * create database 数据库名 character set 字符集名;-- 手动指定字符集

      ```sql
      -- 创建db4并且将其字符集设置为gbk
      create if not exists db4 character set gbk;
      ```

   2. R(**Retrieve**)：查询

      * 查询所有数据库的名称：
        * show databases;
      * 查看某个数据库的字符集，查询某个数据库的创建语句
        * show create database 数据库名称;

   3. U(**Update**)：修改

   4. D(**Delete**)：删除

   5. 使用数据库

2. 操作表

