---
title: Web案例_用户信息展示
date: 2019-08-30 18:33:00
tags: 
- Javaweb
- 实践
categories: JavaWeb实践
---

## 基本功能

1. 需求：用户信息的增删改查操作

2. 设计：

   1. 技术：Servlet + JSP + MySQL + JDBCTemplate + Druid + BeanUtils + Tomcat

   2. 数据库设计：

      ```sql
      create database user_information; -- 创建数据库
      use user_information;		      -- 使用数据库
      create table user(
      	id int primary key auto_increment,  -- 用户编号
      	name varchar(32) not null,			-- 用户名
      	gender varchar(5),					-- 用户性别
      	age int,							-- 用户年龄
      	address varchar(64),				-- 用户住址
      	QQnumber varchar(32),				-- 用户QQ号码
      	email varchar(64)					-- 用户邮箱
      )
      ```

3. 开发：

   1. 环境搭建
      1. 创建数据库环境
      2. 创建项目，导入需要的jar包
   2. **编码**

4. 测试

5. 部署运维