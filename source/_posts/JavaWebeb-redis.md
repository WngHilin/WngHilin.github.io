---
title: redis——高性能的非关系型数据库
date: 2019-09-01 09:03:41
tags:
- 数据库
- 后端
- JavaWeb
categories: JavaWeb
---

## 基础

1. 概念：redis是一款高性能的NOSQL系列的非关系型数据库

   > 1.1.什么是NOSQL
   > 	NoSQL(NoSQL = Not Only SQL)，意即“不仅仅是SQL”，是一项全新的数据库理念，泛指非关系型的数据库。
   > 	随着互联网web2.0网站的兴起，传统的关系数据库在应付web2.0网站，特别是超大规模和高并发的SNS类型的web2.0纯动态网站已经显得力不从心，暴露了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题。

   

   > 1.1.1.	NOSQL和关系型数据库比较
   > 	优点：
   > 		1）成本：nosql数据库简单易部署，基本都是开源软件，不需要像使用oracle那样花费大量成本购买使用，相比关系型数据库价格便宜。
   > 		2）查询速度：nosql数据库将数据存储于缓存之中，关系型数据库将数据存储在硬盘中，自然查询速度远不及nosql数据库。
   > 		3）存储数据的格式：nosql的存储格式是key,value形式、文档形式、图片形式等等，所以可以存储基础类型以及对象或者是集合等各种格式，而数据库则只支持基础类型。
   > 		4）扩展性：关系型数据库有类似join这样的多表查询机制的限制导致扩展很艰难。

   > 缺点：
   > 	1）维护的工具和资料有限，因为nosql是属于新的技术，不能和关系型数据库10几年的技术同日而语。
   > 	2）不提供对sql的支持，如果不支持sql这样的工业标准，将产生一定用户的学习和使用成本。
   > 	3）不提供关系型数据库对事务的处理。

   > 1.1.2.	非关系型数据库的优势：
   > 	1）性能NOSQL是基于键值对的，可以想象成表中的主键和值的对应关系，而且不需要经过SQL层的解析，所以性能非常高。
   > 	2）可扩展性同样也是因为基于键值对，数据之间没有耦合性，所以非常容易水平扩展。

   > 1.1.3.	关系型数据库的优势：
   > 	1）复杂查询可以用SQL语句方便的在一个表以及多个表之间做非常复杂的数据查询。
   > 	2）事务支持使得对于安全性能很高的数据访问要求得以实现。对于这两类数据库，对方的优势就是自己的弱势，反之亦然。

   > 1.1.4.	总结
   > 	关系型数据库与NoSQL数据库并非对立而是互补的关系，即通常情况下使用关系型数据库，在适合使用NoSQL的时候使用NoSQL数据库，
   > 	让NoSQL数据库对关系型数据库的不足进行弥补。
   > 	一般会将数据存储在关系型数据库中，在nosql数据库中备份存储关系型数据库的数据

   > 1.2.主流的NOSQL产品
   > 	•	键值(Key-Value)存储数据库
   > 			相关产品： Tokyo Cabinet/Tyrant、Redis、Voldemort、Berkeley DB
   > 			典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 
   > 			数据模型： 一系列键值对
   > 			优势： 快速查询
   > 			劣势： 存储的数据缺少结构化
   > 	•	列存储数据库
   > 			相关产品：Cassandra, HBase, Riak
   > 			典型应用：分布式的文件系统
   > 			数据模型：以列簇式存储，将同一列数据存在一起
   > 			优势：查找速度快，可扩展性强，更容易进行分布式扩展
   > 			劣势：功能相对局限
   > 	•	文档型数据库
   > 			相关产品：CouchDB、MongoDB
   > 			典型应用：Web应用（与Key-Value类似，Value是结构化的）
   > 			数据模型： 一系列键值对
   > 			优势：数据结构要求不严格
   > 			劣势： 查询性能不高，而且缺乏统一的查询语法
   > 	•	图形(Graph)数据库
   > 			相关数据库：Neo4J、InfoGrid、Infinite Graph
   > 			典型应用：社交网络
   > 			数据模型：图结构
   > 			优势：利用图结构相关算法。
   > 			劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。
   >
   > 
   >
   > 1.3 什么是Redis
   > 	Redis是用C语言开发的一个开源的高性能键值对（key-value）数据库，官方提供测试数据，50个并发执行100000个请求,读的速度是110000次/s,写的速度是81000次/s ，且Redis通过提供多种键值数据类型来适应不同场景下的存储需求，目前为止Redis支持的键值数据类型如下：
   > 		1) 字符串类型 string
   > 		2) 哈希类型 hash
   > 		3) 列表类型 list
   > 		4) 集合类型 set
   > 		5) 有序集合类型 sortedset
   > 	1.3.1 redis的应用场景
   > 		•	缓存（数据查询、短连接、新闻内容、商品内容等等）
   > 		•	聊天室的在线好友列表
   > 		•	任务队列。（秒杀、抢购、12306等等）
   > 		•	应用排行榜
   > 		•	网站访问统计
   > 		•	数据过期处理（可以精确到毫秒
   > 		•	分布式集群架构中的session分离



2. 下载安装

   1. 官网：[https://redis.io](https://redis.io/)		中文网：[https://www.redis.net.cn](https://www.redis.net.cn/)

      windows版本：[https://github.com/MicrosoftArchive/redis/tags](https://github.com/MicrosoftArchive/redis/tags)

   ![1567301725808](C:\Users\Nier\AppData\Roaming\Typora\typora-user-images\1567301725808.png)

   2. 安装：解压即可使用
      * redis.windows.conf：配置文件
      * redis-cli.exe：redis的客户端
      * redis-server.exe：redis的服务器端

   



## 命令操作

1. redis的数据结构：

   * redis存储的是：key, value格式的数据，其中key都是字符串，value有5种不同的数据结构

     * value的数据结构

       1) 字符串类型 string

       2) 哈希类型 hash : map格式

       3) 列表类型 list

       4) 集合类型 set

       5) 有序集合类型 sortedset



2. 字符串类型 string

   1. 存储：set key value
   2. 获取：get key
   3. 删除：del key

   

3. 哈希类型 hash

   1. 存储：hset key field value
   2. 获取：
      1. 获取指定field对应的值：hget key field
      2. 获取所有的field和value：hgetall key
   3. 删除：hdel key field [field...] (可以删除多个)



4. 列表类型 list：可以添加一个元素到列表的头部(左边)或者尾部(右边)
   1. 添加：
      1. lpush key value：将元素加入列表左边
      2. rpush key value：将元素加入列表右边
   2. 获取：range key start end：范围获取
      1. 获取所有：range key 0 -1
   3. 删除：
      1. lpop key：删除列表最左边的元素，并将元素返回
      2. rpop key：删除列表最右边的元素，并将元素返回



5. 集合类型 set  不允许重复元素
   1. 存储：sadd key value
   2. 获取：smembers key：获取set集合中所有元素
   3. 删除：srem key value：产出set集合中的某个元素



6. 有序集合类行 sortedset：不允许重复元素，且元素有顺序
   1. 存储：zadd key score value：score为数据对应的分数，用分数对数据进行排序
   2. 获取：zrange key start end
   3. 删除：zrem key value



7. 通用命令：
   1. keys * ：查询所有的键
   2. type key ：获取键对应的值的类型
   3. del key：删除指定的key value



## 持久化

1. redis是一个内存数据库，当redis服务器重启，或者电脑重启，数据会丢失，我们可以将redis内存中的数据持久化保存到硬盘的文件中

2. redis持久化机制：

   1. RDB：默认机制，不需要进行配置，默认使用

      * 在一定的间隔时间中，检测key的变化情况，然后持久化数据

      1. 编辑redis.windows.conf文件

         ```
         # 900 s(15 min)后如果至少有1个key被改变，进行持久化
         save 900 1
         # 300 s(5 min)后如果至少有10个key被改变，进行持久化
         save 300 10
         # 60 s后如果至少有10000个key被改变，进行持久化
         save 60 10000
         ```

      2. 重新启动redis服务器，并指定配置文件名称

         ```
         # cmd中
         redis-server.exe redis.windows.conf
         ```

         

   2. AOF：日志记录的方式，可以记录每一条命令的操作。可以每一次命令操作后，持久化数据

      1. 编辑redis.windows.conf文件

         ```
         # 将该值设置为yes以开启AOF
         appendonly no
         # appendfsync always：每一次操作都进行持久化
         appendfsync everysec：每隔一秒进行一次持久化
         # appendfsync no：不进行持久化
         ```

      2. 重新启动redis服务器，并指定配置文件名称

         ```
         # cmd中
         redis-server.exe redis.windows.conf
         ```



***

## Jedis

* Jedis：一款java操作redis数据库的工具

* 使用步骤：

  1. 下载jedis的jar包

  2. 使用

     ```java
         @Test
         public void test1(){
             //1. 获取连接
             //如果使用空参，默认值就为"localhost", 6379
             Jedis jedis = new Jedis("localhost", 6379);
             //2. 操作
             jedis.set("username", "zhangsan");
             //3. 关闭连接
             jedis.close();
         }
     ```





* Jedis操作redis的数据结构

  1）字符串类型 string

  ​		set

  ​		get

  ​		使用setex方法，指定过期时间的key value

  ```java
  jedis.setex(key, seconds, value); //将key:value存入redis，并且seconds秒后自动删除
  ```

  2）哈希类型 hash : map

  ​		hset

  ​		hget

  ​		hgetAll：返回一个Map集合

  3）列表类型 list

  ​		lpush / rpush

  ​		lpop / rpop

  ​		lrange

  4）集合类型 set

  ​		sadd

  ​		smembers

  5）有序集合类型 sortedset

  ​		zadd



* Jedis连接池：JedisPool

  * 使用：

    1. 创建JedisPool连接池对象

    2. 调用方法getResource()方法来获取连接

       ```java
           @Test
           public void test2(){
               //0. 创建配置对象
               JedisPoolConfig config = new JedisPoolConfig();
               config.setMaxTotal(50);
               config.setMaxIdle(10);
               
               //1. 创建连接池对象
               JedisPool jedisPool = new JedisPool("localhost", 6379);
       
               //2. 获取连接
               Jedis jedis = jedisPool.getResource();
               //3. 使用
               jedis.set("1", "2");
       
               //关闭 归还到连接池中
               jedis.close();
       
           }
       ```

  * 工具类：

    ```java
    /**
     *JedisPool工具类
     */
    public class JedisPoolUtils {
        private static JedisPool jedisPool;
    
        static {
            //读取配置文件
            InputStream is = JedisPoolUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
            //创建Properties对象
            Properties pro = new Properties();
            try {
                pro.load(is);
            } catch (IOException e) {
                e.printStackTrace();
            }
    
            JedisPoolConfig config = new JedisPoolConfig();
            config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
            config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));
    
            jedisPool = new JedisPool(config, pro.getProperty("host"), Integer.parseInt(pro.getProperty("post")));
    
    
        }
        public static Jedis getJedis(){
            return jedisPool.getResource();
        }
    }
    ```

    

    