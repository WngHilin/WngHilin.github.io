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



***

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



### Connection：数据库连接对象

* 功能：
  1. 获取执行sql的对象：
     * Statement createStatement()
     * PreparedStatement prepareStatement(String sql)
  2. 管理事务：
     * 开启事务：void setAutoCommit(boolean autoCommit)   
       * 调用该方法，设置参数为false，即开启事务
     * 提交事务：commit()
     * 回滚事务：rollback()



### Statement：执行sql的对象

* 执行sql
  * boolean execute(String sql)：可以执行任意sql语句（了解）
  * int executeUpdate(String sql)：执行DML语句、DDL语句
    * 返回值：影响的行数，通过影响的行数判断执行是否成功(若大于 0 则执行成功)，创建表返回值为0
  * ResultSet executeQuery(String sql)：执行DQL(select)语句

<center>示例</center>
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBCdemo2 {
    public static void main(String[] args) {
        Statement stmt = null;
        Connection conn = null;
        try {
            //1.注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2.定义SQL
            String sql = "insert into account values(null, 'wangwu', 3000)";
            //3.获取Connection对象
            conn = DriverManager.getConnection("jdbc:mysql:///db2", "root", "wnghilin");
            //4.获取Statement对象
            stmt = conn.createStatement();
            //5.执行Sql
            int count = stmt.executeUpdate(sql); //影响的行数
            //6.处理结果
            System.out.println(count);
            if(count > 0){
                System.out.println("添加成功");
            } else{
                System.out.println("添加失败");
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }finally{
            //7.释放资源
            //注意避免空指针异常
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



### ResultSet：结果集对象

* 功能：封装查找结果

  * next()：游标向下移动一行(游标最开始指向如图所示位置)

  ![](https://s2.ax1x.com/2019/08/15/mA4oKx.png)

  * getXxx(参数)：获取数据
    * Xxx：代表数据	如：int getInt(),   String getString()
    * 参数：
      * int：代表列的编号，如：getString(1)，获取第一列，不是从0开始
      * String：代表列的名称，如：getInt("id")

<center>示例</center>
```java
import java.sql.*;

public class JDBCdemo3 {
    public static void main(String[] args) {
        Statement stmt = null;
        Connection conn = null;
        ResultSet result = null;
        try {
            //1.注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2.定义SQL
            String sql = "select * from account";
            //3.获取Connection对象
            conn = DriverManager.getConnection("jdbc:mysql:///db2", "root", "wnghilin");
            //4.获取Statement对象
            stmt = conn.createStatement();
            //5.执行Sql
            result = stmt.executeQuery(sql);
            //6.处理结果
            //6.1让游标向下移动一行
            result.next();
            //6.2获取数据
            int id = result.getInt("id");
            String name = result.getString("name");
            double balance = result.getDouble(3);
            System.out.println(id + "---" + name + "---" + balance);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            //7.释放资源
            //注意避免空指针异常
            if (result != null) {
                try {
                    result.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
                if (stmt != null) {
                    try {
                        stmt.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
                }
                if (conn != null) {
                    try {
                        conn.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
}
```

### PreparedStatement：执行sql的对象

1. SQL注入问题：在拼接sql时，有一些sql的特殊关键字参与字符串的拼接，会造成安全问题

   1. 输入用户随便，输入密码a' or 'a' = 'a

   2. sql: select * from user where username = '随便' and password = 'a' or 'a' = 'a'

      一定返回true

2. 解决：使用PreparedStatement

3. 预编译的SQL：参数使用?作为占位符

4. 步骤

   1. 导入驱动jar包 mysql-connector-java-版本号-bin.jar
   2. 注册驱动
   3. 获取数据库的连接对象 Connection
   4. 定义sql
      * 注意：sql参数使用?作为占位符，如select * from user where username=? and password = ?;
   5. 获取执行sql语句的对象 PreparedStatement
      * Connection.prepareStatement(String sql)
   6. 给?赋值
      * 方法：setXxx(参数1, 参数2)
      * 参数1：?的位置，从1开始
      * 参数2：?的值
   7. 执行sql，接受返回结果，不需要再传递sql语句
   8. 处理结果
   9. 释放资源

5. **优势**：

   * 可以防止SQL注入
   * 效率更高



***



## 抽取JDBC工具类：JDBCUtils

* 目的：简化书写
* 分析：
  * 注册驱动抽取
  * 抽取方法获取连接对象
    * 不传递参数，但需要保证类的通用性
    * 解决方案：配置文件
  * 抽取方法释放资源

<center>示例</center>

```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;
import java.net.URL;
import java.sql.*;
import java.util.Properties;

/**
 * JDBC工具类
 */
public class JDBCUtils {
   private static String url;
   private static String user;
   private static String password;
   private static String driver;
    /**
     * 文件的读取，只需要读取一次就可以拿到这些值
     */
    static{
        //读取资源文件，获取值

        try {
            //1.创建Properties集合类
            Properties pro = new Properties();

            //获取src路径下的文件的方式:ClassLoader 类加载器
            ClassLoader classLoader = JDBCUtils.class.getClassLoader();
            URL res = classLoader.getResource("jdbc.properties");
            String path = res.getPath();
            //2.加载文件
            pro.load(new FileReader(path));

            //3.获取数据，赋值
            url = pro.getProperty("url");
            user = pro.getProperty("user");
            password = pro.getProperty("password");
            driver = pro.getProperty("driver");
            //4.注册驱动
            Class.forName(driver);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

    }

    /**
     * 获取连接
     * @return 连接对象
     */
    public static Connection getConnection(){

        return null;
    }

    /**
     * 释放资源
     * @param stmt
     * @param conn
     */
    public static void close(Statement stmt, Connection conn){
        if(stmt != null){
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if(conn != null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 释放资源
     * @param rs
     * @param stmt
     * @param conn
     */
    public static void close(ResultSet rs, Statement stmt, Connection conn){
        if(rs != null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if(stmt != null){
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if(conn != null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```







