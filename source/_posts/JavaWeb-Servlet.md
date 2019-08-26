---
title: Servlet-运行在服务器端的小程序
date: 2019-08-21 15:18:08
tags: 
- Javaweb
- 学习
- 服务器
- Servlet
categories: JavaWeb
---

## Servlet

* 概念：运行在服务器端的小程序  server applet

  * Servlet就是一个接口，定义了Java类被浏览器访问到（tomcat识别）的规则

* 使用

  1. 创建JavaEE项目

     ```java
     public class ServletDemo1 implements Servlet
     ```

  2. 定义一个类，实现Servlet接口

  3. 实现接口中的抽象方法

  4. 配置在Servlet

     ```xml
     <!--配置Servlet-->
         <servlet>
             <servlet-name>demo1</servlet-name>
             <servlet-class>top.wnghilin.servlet.ServletDemo1</servlet-class>
         </servlet>
         
         <servlet-mapping>
             <servlet-name>demo1</servlet-name>
             <url-pattern>/demo1</url-pattern>
         </servlet-mapping>
     ```

* 执行原理：

  1. 当服务器接受到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径
  2. 查找web.xml文件，是否有对应的&lt;url-pattern&gt;标签体内容
  3. 如果有，则再找到对应的&lt;servlet-class&gt;全类名
  4. tomcat会将字节码文件加载进内存，并且创建其对象
  5. 调用其方法



* Servlet中的生命周期：

  1. 被创建：执行init方法，只执行一次

     * 默认情况下，第一次被访问时，Servlet被创建

     * 可以配置执行Servlet的创建时机

       ```xml
       <servlet>
               <servlet-name>demo2</servlet-name>
               <servlet-class>top.wnghilin.servlet.ServletDemo2</servlet-class>
               <!--指定Servlet创建时机
                   1.第一次被访问时，创建
                       <load-on-startup>的值为负数
                   2.服务器启动时，创建
                       <load-on-startup>的值为0或正整数
               -->
               <load-on-startup>1</load-on-startup>
           </servlet>
       ```

  2. 提供服务：执行service方法，执行多次

  3. 被销毁：执行destroy方法，只执行一次