---
title: 数据库MySQL学习
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

      * 修改数据库的字符集
        * alter database 数据库名称 character set 字符集名称;

   4. D(**Delete**)：删除

      * 删除数据库
        * drop database 数据库名称;
        * drop database if exisits 数据库名称; -- 判断数据库是否存在
        * **慎用**

   5. 使用数据库

      * 查询当前正在使用的数据库名称
        * select database();
      * 使用数据库
        * use 数据库名称;

   

2. 操作表：CRUD

   1. C(**Create**)：创建

      * 数据类型：
        - int：整数类型
          - age int;
        - double：小数类型
          - score double(5, 2); -- 一共5位，保留两位小数
        - date：日期，只包含年月日，yyyy-MM-dd
        - datetime：日期，包含年月日时分秒，yyyy-MM-dd HH:mm:ss
        - timestamp：事件错类型，包含年月日时分秒，yyyy-MM-dd HH:mm:ss
          - 如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间自动赋值
        - varchar：字符串类型
          - name varchar(20); -- 姓名最大20个字符
          - zhangsan 8个字符， 张三 2个字符

      - 创建表

        - ```sql
          create table 表名(
              列名1 数据类型1,
              列名2 数据类型2,
              ......
              列名n 数据类型n
          )
          -- 注意最后一列不用加逗号(,)
          ```

      - 复制表：create table 表名 like 被复制表名;

   2. R(**Retrieve**)：查询

      - 查询某个数据库中所有的表名称
        - show tables;
      - 查询表结构
        - desc 表名;

   3. U(**Update**)：修改

      - 修改表名
        - alter table 表名 rename to 新的表名;
      - 修改字符集
        - show create table 表名;-- 查看表的字符集
        - alter table 表名 character set 字符集;
      - 添加一列
        - alter table 表名 add 列名 数据类型;
      - 修改列名称 类型
        - alter table 表名 change 列名 新列名 新数据类型;
        - alter table 表名 modify 列名 新数据类型;
      - 删除列
        - alter table 表名 drop 列名;

   4. D(**Delete**)：删除

      - drop table if exists 表名;
      - drop table 表名;



### DML：增删改标中数据

1. 添加数据

   * 语法：

     * insert into 表名(列名1, 列名2, ...... 列名n) values(值1, 值2, ...... 值n);

   * 注意：

     * 列名和值应该一一对应

     * 如果表名后，不定义列名，则默认给所有列添加值

       * ```sql
         INSERT INTO stu VALUES(2,'秀吉',18,99.2,'2000-08-23',NULL,"秀吉");
         ```

     * 出了数字类型，其他类型需要' '或" "引起来

2. 删除数据

3. 修改数据



### DQL：查询表中的记录

* select * from 表名;