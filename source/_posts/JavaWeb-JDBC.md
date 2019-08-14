---
title: JDBC-在Java中操作数据库
date: 2019-08-14 17:02:19
tags: 
- Javaweb
- 学习
- 数据库
- Java
categories: JavaWeb
---

## JDBC基础

* 概念：Java Database Connectivity  Java数据库连接

  * 本质：官方定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商实现这套接口，提供数据库驱动 jar 包，可以使用（JDBC）编程，真正执行的代码是驱动jar包中的实现类。

* 快速入门：

  * 步骤：

    1. 导入驱动jar包 mysql-connector-java-版本号-bin.jar
       1. 复制到mysql-connector-java-版本号-bin.jar到项目的libs目录下
       2. 右键 --> Add As Libary
    2. 注册驱动
    3. 获取数据库的连接对象 Connection
    4. 定义sql
    5. 获取执行sql语句的对象 Statement
    6. 执行sql，接受返回结果
    7. 处理结果
    8. 释放资源

    ```sql
    public class JDBCdemo1 {
        public static void main(String[] args) throws ClassNotFoundException, SQLException {
            //1.导入驱动jar包
            //2.注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            //3.获取数据库连接对象 Connection
            Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db2", "用户名", "密码");
            //4.定义sql语句
            String sql = "sql语句";
            //5.获取执行sql的对象 Statement
            Statement stmt = conn.createStatement();
            //6.执行sql
            int count = stmt.executeUpdate(sql);
            //7.处理结果
            System.out.println(count);
            //8.释放资源
            stmt.close();
            conn.close();
        }
    }
    ```

    

## 详解各个对象

### DriverManager：驱动管理对象

* 功能：

  1. 注册驱动：该执行哪一个jar包

     1. static void registerDriver(Driver driver) :注册与给定的驱动程序 DriverManager 。 

     2. 写代码使用：  Class.forName("com.mysql.jdbc.Driver");

        * 在com.mysql.jdbc.Driver类中存在静态代码块，该代码的内容为调用registerDriver方法，所以方法2较为方便。

        **注**：mysql5之后的驱动jar包可以省略注册驱动的步骤

  2. 获取数据库连接

     * 方法：static Connection getConnection(String url, String user, String password) 
     * 参数：
       * url：指定连接的路径
         * 语法：jdbc:mysql://ip地址(域名):端口号/数据库名称
         * 如果连接的是本机的mysql服务器，并且mysql服务器默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称
       *  user：用户名
       * password：密码



