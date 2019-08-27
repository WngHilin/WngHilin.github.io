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

## Servlet概述

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
                   1.第一次被访问时，创建，默认值是-1
                       <load-on-startup>的值为负数
                   2.服务器启动时，创建
                       <load-on-startup>的值为0或正整数
               -->
               <load-on-startup>1</load-on-startup>
           </servlet>
       ```
     
       * Servlet的init方法，只执行一次，说明一个Servlet在内存中只存在一个对象，Servlet是单例的。
       * 多个用户同时访问时，可能存在线程安全问题
         * 解决：尽量不要在Servlet中定义成员变量。即使定义了成员变量，也不要对其修改值
  
  2. 提供服务：执行service方法，执行多次
  
     * 每次访问Servlet时，Servlet方法都会被调用一次 
  
  3. 被销毁：执行destroy方法，只执行一次
  
     * Servlet被销毁时执行。服务器关闭时，Servlet被销毁。
     * 只有服务器正常关闭时，才会执行destroy方法
     * destroy方法在Servlet被销毁之前执行，一般用于释放资源



### 使用Servlet3.0

* 好处：

  * 支持注解配置。可以不用web.xml

* 步骤：

  1. 创建JavaEE项目，选择Servlet的版本3.0以上，可不创建web.xml

  2. 定义一个类，实现Servlet接口

  3. 复写方法

  4. 在类上使用@WebServlet注解，进行配置

     * ```java
       @WebServlet("资源路径")
       ```

     * ```java
       //WebServlet定义
       package javax.servlet.annotation;
       
       @java.lang.annotation.Target({java.lang.annotation.ElementType.TYPE})
       @java.lang.annotation.Retention(java.lang.annotation.RetentionPolicy.RUNTIME)
       @java.lang.annotation.Documented
       public @interface WebServlet {
           java.lang.String name() default "";
       
           java.lang.String[] value() default {};
       
           java.lang.String[] urlPatterns() default {};
       
           int loadOnStartup() default -1;
       
           javax.servlet.annotation.WebInitParam[] initParams() default {};
       
           boolean asyncSupported() default false;
       
           java.lang.String smallIcon() default "";
       
           java.lang.String largeIcon() default "";
       
           java.lang.String description() default "";
       
           java.lang.String displayName() default "";
       }
       ```

* IDEA会为每一个tomcat部署的项目单独简历一份配置文件
  
* 查看控制台的log：Using CATALINA_BASE:   "C:\Users\Nier\.IntelliJIdea2019.2\system\tomcat\Tomcat_8_5_31_Tomcat_2"
  
* 工作空间项目 和 tomcat部署的web项目
  * tomcat真正访问的是“tomcat部署的web项目”，“tomcat部署的web项目”对应着“工作空间项目”的文本目录下的所有资源  
  * WEB-INF目录下的资源不能被浏览器直接访问



## Servlet详解

* Servlet的体系结构

  Servlet -- 接口

  ​	  |

  GenericServlet -- 抽象类

  ​	  |

  HttpServlet -- 抽象类
  * GenericServlet: 将Servlet接口中其他的方法做了默认空实现，只有service方法作为抽象
    * 定义Servlet类是，可以继承GenericServlet，实现service()方法即可
  * HttpServlet：对HTTP协议的一种封装，简化操作
    * 定义类继承HttpServlet
    * 复写doGet/doPost

* Servlet相关配置：
  1. urlpartten：Servlet访问路径
     * 一个Servlet可以定义多个访问路径
     * 路径定义规则
       1. /xxx：路径匹配
       2. /xxx/xxx：多层路径，目录结构
       3. *.do：扩展名匹配

* 请求消息数据格式
	1. 请求行
		请求方式 请求url 请求协议/版本
		GET /login.html HTTP/1.1
* 请求方式：
			* HTTP协议有7中请求方式，常用的有2种
				* GET：
					1. 请求参数在请求行中，在url后。
					2. 请求的url长度有限制的
					3. 不太安全
				* POST：
					1. 请求参数在请求体中
					2. 请求的url长度没有限制的
					3. 相对安全
	2. 请求头：客户端浏览器告诉服务器一些信息
		请求头名称: 请求头值
		* 常见的请求头：
			1. User-Agent：浏览器告诉服务器，我访问你使用的浏览器版本信息
				* 可以在服务器端获取该头的信息，解决浏览器的兼容性问题
	
		2. Referer：http://localhost/login.html
				* 告诉服务器，当前请求从哪里来
					* 作用：
						1. 防盗链：
						2. 统计工作：
	3. 请求空行
		空行，就是用于分割POST请求的请求头，和请求体的。
	4. 请求体(正文)：
		* 封装POST请求消息的请求参数的
	
* 字符串格式：
			POST /login.html	HTTP/1.1
			Host: localhost
			User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:60.0) Gecko/2010010       Firefox/60.0
		    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
		    Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
			Accept-Encoding: gzip, deflate
			Referer: http://localhost/login.html
			Connection: keep-alive
			Upgrade-Insecure-Requests: 1
		
		​	username=zhangsan



### request和response对象

* 原理：
  1. request和response对象是由服务器创建的，我们来使用它们
  2. request对象用来获取请求消息，response对象用来设置响应消息



* request对象继承体系结构：

  ServletRequest		--	接口
  	|	继承
  HttpServletRequest	--   接口
  	|	实现
  org.apache.catalina.connector.RequestFacade 类(tomcat)



* **request**：

  1. 获取请求消息数据

     1. 获取请求行数据

        * GET /servletdemo/demo1/?name=zhangsan HTTP/1.1

        * 方法：
          1. 获取请求方式：GET
             * String getMethod()
          2. （*）获取虚拟目录：/servletdemo
             * String getContextPath()
          3. 获取Servlet路径：/demo1
             * String getServletPath()
          4. 获取get方式请求参数：name=zhangsan
             * String getQuery()
          5. （*）获取请求URI：/servletdemo/demo1
             * String getRequestURI()：返回/servletdemo/demo1
             * StringBuffer getRequestUrl()：返回http://localhost/servletdemo/demo1
             * URL：统一资源定位符
             * URI：统一资源标识符
          6. 获取协议及版本：HTTP/1.1
             * String getProtocol()
          7. 获取客户机的IP地址
             * String getRemoteAddr()

     2. 获取请求头数据

        * 方法：
          * （*）String getHeader(String name)：通过请求头的名称获取请求头的值
          * Enumeration&lt;String&gt; getHeaderNames()：获取所有的请求头名称

     3. 获取请求体数据

        * 只有POST请求方式，才有请求体，在请求体中封装了POST请求的请求参数

        * 步骤：

          1. 获取流对象

             1. BufferedReader getReader()：获取字符输入流，只能操作字符数据
             2. ServletInputStream getInputStream()：获取字节数据输入流，可以操作所有类型数据

          2. 再从流对象中拿数据

             ```java
             @WebServlet("/requestDemo5")
             public class RequestDemo5 extends HttpServlet {
                 protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                     //获取请求消息体--请求参数
             
                     //1.获取字符流
                     BufferedReader br = request.getReader();
                     //2.读取数据
                     String line = null;
                     while((line = br.readLine()) != null){
                         System.out.println(line);
                     }
             
                 }
             
                 protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                 }
             }
             ```

  2. 其他功能：

     1. 获取请求参数通用方式：get和珀斯特请求方式都可以使用下列方式来获取请求参数

        1. String getParameter(String name)：根据参数名称获取参数值
        2. String[] getParameterValues(String name)：根据参数名称获取参数值的数组（多用于复选框）
        3. Enumeration&lt;String> getParameterNames()：获取所有请求的参数名称
        4. Map&lt;String, String[]> getParameterMap()：获取所有参数的map集合

        **中文乱码问题**：

        * get方式：tomcat8已经将乱码问题解决
        * post方式：会乱码
          * 解决：在获取参数前，设置request的编码
          * request.setCharacterEncoding("utr-8");

     2. 请求转发：一种在服务器内部的资源跳转方式

        1. 步骤：
           1. 通过request对象获取请求转发器对象：RequestDispatcher getRequestDispatcher(String path)
           2. 使用该对象进行转发: forward(ServletRequest request, ServletResponse response)
        2. 特点：
           1. 浏览器地址栏路径不发生变化
           2. 只能转发到当前服务器内部资源中
           3. 转发是一次请求
        3. 共享数据：
           * 域对象：一个有作用范围的对象，可以在范围内共享数据
           * request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
           * 方法：
             1. setAttribute(String name, Object obj)：存储数据
             2. getAttribute(String name)：通过键获取值
             3. removeAttribute(String name)：通过键移除值对
        4. 获取ServletContext对象：
           * ServletContext getServletContext()





## BeanUtils工具类

* 用于封装JavaBean

  1. JavaBean：标准的Java类
     * 要求：
       1. 类必须被public修饰
       2. 必须提供空参的构造器
       3. 成员变量必须使用private修饰
       4. 提供公共setter和getter方法
     * 功能：封装数据

  2. 概念：**属性**：setter和getter后截取的产物

     如成员变量为user，setter为setUser，则**U**ser为属性（若setter为setHehe，则Hehe为属性，与user无关，只与setter有关）

  3. 方法：

     1. setProperty()：操作的是属性，不是成员变量
     2. getProperty()
     3. **populate**(Object obj，Map map)：将map集合的键值对信息，封装到对应的JavaBean对象中