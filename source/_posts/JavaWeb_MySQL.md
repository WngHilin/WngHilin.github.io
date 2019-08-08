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

1. 安装目录
   * 配置文件：my.ini
2. 数据目录
   * 数据库：文件夹
   * 表：文件
   * 数据：文件中储存的数据

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
         INEINSERT INTO stu VALUES(2,'秀吉',18,99.2,'2000-08-23',NULL,"秀吉");
         ```

     * 出了数字类型，其他类型需要' '或" "引起来

2. 删除数据

   * 语法：

     * delete from 表名 [where 条件];

     * ```sql
       DELETE FROM stu WHERE id=1;
       ```

   * **注意**：

     * 如果不加条件，则删除表中所有记录
     * truncate table 表名; -- 删除表，再创建一个一模一样的空表，建议用这种方式删除全部记录
     * delete from 表名; -- 执行次数为记录条数，效率较低

3. 修改数据

   * 语法：
     * update 表名 set 列名1 = 值1, 列名2 = 值2, ... [where 条件];
   * **注意**
     * 如果不加条件，则修改表中所有数据



### DQL：查询表中的记录

1. 语法：

```sql
select 
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段
having
	分组之后的条件
order by
	排序
limit
	分页限定
```

2. 基础查询

   1. 多个字段的查询

      * select 字段名1, 字段名2... from 表名;
      * 可用*来替代所有字段

   2. 去除重复

      * ```sql
        -- 去除重复的结果集
        SELECT DISTINCT address FROM student;
        ```

   3. 计算列

      * ```sql
        -- 计算math和english两列的和
        -- 如果有null参加运算，则结果仍为null
        SELECT NAME, math, english, math + english FROM student3;
        -- 将NULL替换为0
        SELECT NAME, math, IFNULL(english), math + english FROM student3;
        ```

   4. 起别名

      * ```sql
        SELECT NAME, math, english, math + english AS 总分 FROM student3;
        -- 简化形式
        SELECT NAME, math 数学, english 英语, math + english 总分 FROM student3;
        ```

3. 条件查询

   1. where后跟条件

   2. 运算符

      * ">", "<", "<=", ">=", "=", "<>"
      * BETWEEN...AND
      * IN（集合）
      * LIKE
        * 占位符：
          * _：单个任意字符
          * %：多个任意字符
      * IS NULL
      * and 或 &&
      * or 或 ||
      * not 或 !

      ```sql
      -- 查询年龄在20到30之间的（包含20和30）
      SELECT * FROM student3 WHERE age BETWEEN 20 AND 30;
      -- 查询英语成绩为空
      SELECT * FROM student3 WHERE english IS NULL;
      -- 查询英语成绩不为空
      SELECT * FROM student3 WHERE english IS NOT NULL;
      ```

4. 模糊查询

   * ```sql
     -- 查询所有姓王的同学
     SELECT * FROM student3 WHERE name LIKE '王%';
     -- 查询名字有三个字的人
     SELECT * FROM student3 WHERE NAME LIKE '___';
     -- 查询姓名中有“国”字的人
     SELECT * FROM student3 WHERE NAME LIKE '%国%';
     ```

   * 

* 排序查询

  * 语法：

    * order by 排序字段1 排序方式1, 排序字段2 排序方式2, ...;
    * 先按字段1排序，如有相同，按字段2排序，以此类推。

  * 排序方式

    * ASC：升序，默认

    * DESC：降序

    * ```sql
      SELECT * FROM stu ORDER BY math ASC, english DESC;
      ```

* 聚合函数：将一列数据作为一个整体，进行纵向的计算。

  1. count：计算个数
  2. max：计算最大值
  3. min：计算最小值
  4. sum：计算和
  5. avg：计算平均值
     * **注**：所有聚合函数会排除为NULL的数据

  ```sql
  SELECT AVG(math) FROM student;
  ```

  排除NULL的解决方案

  ```sql
  SELECT COUNT(IFNULL(english,0)) FROM student;-- 把NULL替换成0，以便统计人数等（不会修改原数据）
  -- 或选择非空的列进行计算
  ```

* 分组查询：

  * 语法：group by 分组字段;

  ```sql
  -- 按照性别分组，并分别求数学的平均分
  SELECT sex, AVG(math) FROM student GROUP BY sex;
  -- 加入分组限定条件
  SELECT sex, AVG(math), COUNT(id) FROM student WHERE math > 60 GROUP BY sex;
  SELECT sex, AVG(math), COUNT(id) FROM student WHERE math > 60 GROUP BY sex HAVING COUNT(id)>1;
  ```

  * **注意**：
    1. 分组之后查询的字段：分组字段，聚合函数
    2. where 和 having的区别
       1. where在分组前限定，如不满足条件，则不参与分组；having在分组后进行限定，如果不满足结果，则不会被查询出来。
       2. where后不可以跟聚合函数，having可以

* 分页查询：

  * 语法：limit 开始的索引, 每页查询的条数;
  * 开始的索引 = （当前的页码 - 1） *  每页显示的条数

  ```sql
  -- 每一页显示3条记录
  SELECT * FROM student LIMIT 0,3; -- 第一页
  SELECT * FROM student LIMIT 3,3; -- 第二页
  ```

  * 分页操作是一个MySQL“ 方言 ” 

### 约束

* **概念**：对标重的数据进行限定，保证数据的正确性、有效性和完整性。

* **分类**:

  1. 主键约束：primary key
  2. 非空约束：not null
  3. 唯一约束：unique
  4. 外键约束：foreign key

* **非空约束 not null**：

  * 创建表时添加约束：

  ```sql
  CREATE TABLE stu(
  	id INT,
  	name VARCHAR(32) NOT NULL -- name 为非空约束
  );
  
  ```

  * 创建表后添加约束：

  ```sql
  ALTER TABLE stu MODIFY name VARCHAR(32) NOT NULL;
  ```

  * 删除非空约束 <a name="DeleteNotNULL" href></a>

  ```sql
  ALTER TABLE stu MODIFY name VARCHAR(32);
  ```

* **唯一约束：unique**

  * ```sql
    CREATE TABLE stu(
    	id INT, 
    	phone_number VARCHAR(20) UNIQUE
    );
    
    ALTER TABLE stu MODIFY phone_number VARCHAR(20) UNIQUE;
    -- 若已有重复数据，则无法添加唯一约束
    -- mysql中 唯一约束限定的列的值可以有多个null
    ```

  * 删除

    * ```sql
      ALTER TABLE stu DROP INDEX phone_number;
      -- 注意，与非空约束方法不同
      ```

* **主键约束：primary key**

  * 基本概念：

    1. 含义：非空且唯一
    2. 一张表只有一个字段为主键
    3. 主键就是表中记录的唯一标识

  * ```sql
    CREATE TABLE stu(
    	id INT PRIMARY KEY, 
    	name VARCHAR(20) 
    );
    ALTER TABLE stu MODIFY id VARCHAR(20) PRIMARY KEY;
    -- 有重复或空则无法添加主键约束
    ```

  * 删除

    * ```sql
      ALTER TABLE stu DROP PRIMARY KEY;
      ```

  * 自动增长：

    * 概念：如果某一列是数值类型，使用 auto_increment 可以完成值的自动增长
    * 创建表时，添加主键乐数，并完成主键自增长

    ```sql
    CREATE TABLE stu(
    	id INT PRIMARY KEY AUTO_INCREMENT, 
    	NAME VARCHAR(20) 
    );
    
    -- 之后不用输入id也可实现id的自增长，但也可手动输入
    INSERT INTO stu VALUES(NULL, 'ccc');
    ```

    * 删除：与非空约束方法相同，<a id="gotoMoreDeleteNotNULL" href="#DeleteNotNULL">非空约束的删除</a>

* **外键约束：foreign key**，让表与表产生关系，从而保证数据的正确性

  1. 在创建表时可以添加外键

     * 语法：

       ```sql
       CREATE TABLE 表名(
           ......
           外键列,-- 注意这个逗号
           constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列的名称)
       );
       ```

       ```sql
       CREATE TABLE employee(
       	id INT PRIMARY KEY AUTO_INCREMENT,
       	NAME VARCHAR(20),
       	age INT,
       	dep_id INT, -- 外键对应主表的主键
       	CONSTRAINT emp_dept_fk FOREIGN KEY (dep_id) REFERENCES department(id)
       )
       ```

  2. 删除外键

     ```sql
     ALTER TABLE employee DROP FOREIGN KEY emp_dept_fk;
     ```

  3. 创建表后创建外键

     ```sql
     ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键列名称) REFERENCES 主表名称(主表列的名称);
     ```

  4. 级联操作：

     * 设置级联更新

       ```sql
       ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键列名称) REFERENCES 主表名称(主表列的名称) ON UPDATE CASCADE;
       ```

     * 设置级联删除

       ```sql
       -- 二者可以同时存在
       ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键列名称) REFERENCES 主表名称(主表列的名称) ON DELETE CASCADE;
       ```

***

## 数据库的设计

### 多表之间的关系

1. 一对一
2. 一对多（多对一）
3. 多对多

