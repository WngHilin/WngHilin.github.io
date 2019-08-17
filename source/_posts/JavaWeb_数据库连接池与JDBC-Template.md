---
title: 数据库连接池与JDBC Template
date: 2019-08-15 15:32:58
tags: 
- Javaweb
- 学习
- 数据库
- Java
- Spring
categories: JavaWeb
---

## 数据库连接池

* 概念：一个容器（集合），用来存放数据库连接。当系统初始化好后，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之后，会将连接对象归还给容器
* 优势:
  1. 节约资源
  2. 用户访问高效
* 实现：
  1. 标准接口：DataSource    javax.sql包下的
     1. 方法：
        * 获取连接：getConnection()
        * 归还连接：如果连接对象Conection是从连接池中获取，调用Connection.close()，则不会再关闭连接，而是归还连接
  2. 由数据库厂商来实现
     1. C3P0：数据库连接池技术
     2. Druid：数据库连接池实现技术，由阿里巴巴提供

### C3P0

* 步骤：

  1. 导入jar包：c3p0-0.9.5.2.jar和mchange-commons-java-0.2.12.jar（不能忘记导入数据库驱动jar包）
  2. 定义配置文件
     * 名称：c3p0.properties 或 c3p0-config.xml
     * 路径：直接将文件放在src目录下即可
  3. 创建核心对象   数据库连接池对象  ComboPooledDataSource
  4. 获取连接：getConnection

  ```java
  //1.创建数据库连接池
  DataSource ds = new ComboPooledDataSource();
  //2.获取连接对象s
  Connection conn = ds.getConnection();
  ```



### Druid

* 步骤：
  1. 导入jar包：druid-1.0.9.jar(和数据库驱动jar包)
  2. 定义配置文件
     1. properties形式
     2. 可以叫任意名称，可以放在任意目录下
  3. 加载配置文件.properties
  4. 获取数据库连接池对象：通过工厂类来获取  DruidDataSourceFactory
  5. 获取连接：getConnection

<center>示例</center>
```java
import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.Connection;
import java.util.Properties;
/**
 * Druid演示
 */
public class DruidDemo1 {
    public static void main(String[] args) throws Exception {
        //1.导入jar包
        //2.配置文件
        //3.加载配置文件
        Properties pro = new Properties();
        InputStream is = DruidDemo1.class.getClassLoader().getResourceAsStream("druid.properties");
        pro.load(is);
        //4.获取连接池对象
        DataSource ds = DruidDataSourceFactory.createDataSource(pro);

        //5.获取连接
        Connection conn = ds.getConnection();
        System.out.println(conn);
    }
}
```



* 定义工具类
  1. 定义一个类 JDBCUtils
  2. 提供方法：
     1. 获取连接方法：通过数据库连接池获取连接
     2. 提供静态代码块加载配置文件，初始化连接池对象
     3. 提供方法
        1. 获取连接方法：通过数据库连接池获取连接
        2. 释放资源
        3. 获取连接池的方法

<center>示例</center>
```java
/**
 * Druid连接池的工具类
 */
public class JDBCUtils {
    //定义成员变量Datasource
    private static DataSource ds;


    static{
        try {
            //加载配置文件
            Properties pro = new Properties();
            pro.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
            //获取Datasource
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取连接
     */
    public static Connection getConnection() throws SQLException{
        return ds.getConnection();
    }

    /**
     * 释放资源，此处close为归还连接池
     */
    public static void close(Statement stmt, Connection conn){略，同上}
    
    /**
     * 释放资源
     */
    public static void close(ResultSet rs, DataStatement stmt, Connection conn){略，同上}
    
    /**
     * 获取连接池
     */
    public static DataSource getDataSourse(){
        return ds;
    }
}
```



 ## Spring JDBC

* Spring框架对JDBC的简单封装。提供JDBCTemplate对象简化JDBC的开发
* 步骤
  1. 导入jar包
  2. 创建JdbcTemplate对象。依赖于数据源的DataSource
     * JdbcTemplate template = new JdbcTemplate(ds);
  3. 调用JdbcTemplate的方法完成CRUD操作
     * update()：执行DML语句
     * queryForMap()：查询结果，将结果封装为map集合
       * 查询的结果集长度只能是1，即只能查找一条结果
     * queryForList()：查询结果，将结果封装为list集合
       * 将每一条记录封装成map集合，再将这些map封装成list集合
     * query()：查询结果，将结果封装为JavaBean对象
       * 参数：RowMapper
         * 一般使用BeanPropertyRowMapper，见下方实例
     * queryForObject()：查询结果，将结果封装为对象

update方法示例：

```java
public class JDBCTemplateDemo1 {
    public static void main(String[] args) {
        //创建JdbcTemplate对象
        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSourse());
        //调用方法
        String sql = "update account set balance = 5000 where id = ?";
        int count = template.update(sql, 3);//将?的值设置为3
        System.out.println(count);
    }
}
```

query方法示例：

```java
public class JDBCTemplateDemo2 {

    JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSourse());
    /**
     * 测试query()方法,自写RowMapper实现类
     */
    @Test
    public void test1(){
        String sql = "select * from account";
        List<Account> accounts = template.query(sql, new RowMapper<Account>() {

            @Override
            public Account mapRow(ResultSet rs, int i) throws SQLException {
                Account ac = new Account();
                int id = rs.getInt("id");
                int balance = rs.getInt("balance");
                String name = rs.getString("name");

                ac.setBalance(balance);
                ac.setName(name);
                ac.setId(id);


                return ac;
            }
        });

        System.out.println(accounts);
    }

    /**
     * 调用写好的BeanProperRowMapper类
     */
    @Test
    public void test2(){
        String sql = "select * from account";

        List<Account> accounts = template.query(sql, new BeanPropertyRowMapper<Account>(Account.class));

        System.out.println(accounts);
    }
}
```



