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



***



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

* 请求消息：客户端发送给服务器端的数据
	
	* 数据格式：
	
	1. 请求行
		请求方式 请求url 请求协议/版本
		GET /login.html HTTP/1.1
		
		1. 请求方式：
		
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
		
	
			username=zhangsan





* 响应消息：服务器端发送给客户端的数据

  * 数据格式

    1. 响应行

       1. 组成：协议/版本 响应状态码 状态码描述

       2. 响应状态码：服务器告诉客户端浏览器本次请求和响应的一个状态

          1. 状态码都是3位数字

          2. 分类：
             1. 1xx：服务器接收客户端消息，但没有接收完成，等待一段时间后，发送1xx状态码
             2. 2xx：成功。代表：200
             3. 3xx：重定向。代表：302（重定向），304（访问缓存）
             4. 4xx：客户端错误。
                * 代表：
                  * 404：请求路径没有对应的资源
                  * 405：请求方式没有对应的doXXX方法
             5. 5xx：服务器端错误，代表：500（服务器内部出现异常）

    2. 响应头

       1. 格式：头名称: 值
       2. 常见的响应头：
          1. Content-Type：服务器告诉客户端本次响应体数据格式以及编码格式
          2. Content-disposition：服务器告诉客户端以什么格式打开响应体数据
             * 值：
               * in-line：默认值，在当前页面打开
               * attachment; filename=xxx：以附件形式打开响应体。文件下载

    3. 响应空行

    4. 响应体：传输的数据

  * 响应字符串格式：

    ```
    HTTP/1.1 200 OK
    Content-Type: text/html;charset=UTF-8
    Content-Length: 101
    Date: Wed, 06 Jun 2018 07:08:42 GMT
    
    <html>
    	<head>
        	<title>response</title>
        </head>
        <body>
    		hello, response
    	</body>
    </html>
    ```

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



#### request

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
      2. 特点：<a name="Forward" href></a>
         1. 浏览器地址栏路径不发生变化
         2. 只能转发到当前服务器内部资源中
         3. 转发是一次请求，**可以使用request对象共享数据**
         4. 对比：<a href="#Redirect">重定向的特点</a>
      3. 共享数据：
         * 域对象：一个有作用范围的对象，可以在范围内共享数据
         * request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
         * 方法：
           1. setAttribute(String name, Object obj)：存储数据
           2. getAttribute(String name)：通过键获取值
           3. removeAttribute(String name)：通过键移除值对
      4. 获取ServletContext对象：
         * ServletContext getServletContext()

#### response

* 功能：设置响应消息
  1. 设置响应行
     1. 格式：HTTP/1.1 200 OK
     2. 设置状态码：setStatus(int sc)
  2. 设置响应头：setHeader(String name, String value)
  3. 设置响应体
     * 使用步骤：
       1. 获取输出流
          * 字符输出流：PrintWriter getWriter()
          * 字节输出流：ServletOutputStream getOutputStream()
       2. 使用输出流，将数据输出到客户端浏览器



* 案例：

  1. 完成重定向

     ```java
     /**
      * 重定向
      */
     @WebServlet("/responseDemo1")
     public class ResponseDemo1 extends HttpServlet {
         protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
             System.out.println("demo1...");
     
             //访问这个资源会自动跳转到/responseDemo2资源
             //1. 设置状态码为302
             response.setStatus(302);
             //2. 设置响应头location
             response.setHeader("location", "/Response/responseDemo2");
             
             //简单的重定向方法
             response.sendRedirect("/Response/responseDemo2");
         }
     
         protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
             this.doPost(request, response);
         }
     }
     ```

     * 重定向的特点：<a name="Redirect" href></a>
       1. 地址栏发生变化
       2. 重定向可以访问其他站点（服务器）的资源
       3. 重定向是两次请求，**不能用request对象共享数据**
     * 对比：<a href="#Forward">转发的特点</a>
     * 路径的写法：
       * 相对路径：通过相对路径不可以确定唯一资源
         * 如：./index.html（ ./ 可以省略）
         * 不以 / 开头，以 . 开头
         * 规则：确定访问当前资源和目标资源之间的相对位置关系
           * ./：当前目录
           * ../：后退一级目录
       * 绝对路径：通过绝对路径可以确定唯一资源
         * 如：http://locahost/day15/responseDemo2        /day15/responseDemo2
         * 以 / 开头的路径
         * 规则：判断定义路径是给谁用的
           * 给客户端浏览器使用：需要加虚拟目录（项目的访问路径）
             * **最好动态获取虚拟目录**：request.getContextPath
             * &lt;a>, &lt;form>, 重定向
           * 给服务器使用：不需要加虚拟目录 
             * 转发路径

  2. 服务器输出字符数据到浏览器：注意编码

     * 步骤

       * 获取字符输出流

       * 输出数据

         ```java
             protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                 
                 //获取流对象之前，设置流的默认编码，ISO-8859-1设置为：GBK
                 response.setCharacterEncoding("utf-8");
                 
                 //告诉浏览器，服务器发送的消息体数据的编码，建议浏览器使用该编码解码
                 response.setHeader("content-type", "text/html; charset=utf-8");
                 
                 //简单形式设置编码
                 response.setContentType("text/html;charset=utf-8");
                 //1.获取字符输出流
                 PrintWriter pw = response.getWriter();
                 //2.输出数据
                 pw.write("<h1>Hello response</h1>");
             }
         ```

  3. 服务器输出字节数据到浏览器

     * 步骤：
       * 获取字节输出流
       * 输出数据

  4. 验证码

     * 本质：图片

     * 目的：防止恶意表单注册

       ```java
       @WebServlet("/checkCodeServlet")
       public class CheckCodeServlet extends HttpServlet {
           protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       
       
               int width = 100;
               int height = 50;
       
               //1.创建一对象，在内存中图片(验证码图片对象)
               BufferedImage image = new BufferedImage(width,height,BufferedImage.TYPE_INT_RGB);
       
       
               //2.美化图片
               //2.1 填充背景色
               Graphics g = image.getGraphics();//画笔对象
               g.setColor(Color.PINK);//设置画笔颜色
               g.fillRect(0,0,width,height);
       
               //2.2画边框
               g.setColor(Color.BLUE);
               g.drawRect(0,0,width - 1,height - 1);
       
               String str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghigklmnopqrstuvwxyz0123456789";
               //生成随机角标
               Random ran = new Random();
               for (int i = 1; i <= 4; i++) {
                   int index = ran.nextInt(str.length());
                   //获取字符
                   char ch = str.charAt(index);//随机字符
                   //2.3写验证码
                   g.drawString(ch+"",width/5*i,height/2);
               }
               //2.4画干扰线
               g.setColor(Color.GREEN);
       
               //随机生成坐标点
               for (int i = 0; i < 10; i++) {
                   int x1 = ran.nextInt(width);
                   int x2 = ran.nextInt(width);
       
                   int y1 = ran.nextInt(height);
                   int y2 = ran.nextInt(height);
                   g.drawLine(x1,y1,x2,y2);
               }
       
               //3.将图片输出到页面展示
               ImageIO.write(image,"jpg",response.getOutputStream());
           }
           protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
               this.doPost(request,response);
           }
       }
       ```

       ```html
       <!DOCTYPE html>
       <html lang="en">
       <head>
           <meta charset="UTF-8">
           <title>Title</title>
           <script>
               /*
                   分析：
                       点击超链接或者图片，换一张图片
                       1.给超链接和图片绑定单击事件
                       2.重新设置图片的src属性值
                */
           window.onload = function(){
               //1.获取图片对象
               var img = document.getElementById("checkCode");
               //2.绑定单击事件
               img.onclick = function(){
                   //加时间戳
                   var date = new Date().getTime();
                   img.src = "/day15/checkCodeServlet?"+date;
               }
           }
           </script>
       </head>
       <body>
           <img id="checkCode" src="/day15/checkCodeServlet" />
       
           <a id="change" href="">看不清换一张？</a>
       </body>
       </html>
       ```



* 案例：

  * 文件下载：
    1. 页面显示超链接
    2. 点击超链接后弹出下载提示框
    3. 完成图片文件下载
  * 分析：
    1. 超链接指向的资源如果能够被浏览器解析，则在浏览器中展示，如果不能解析，则弹出下载提示框。不满足需求
    2. 任何资源都必须弹出下载提示框
    3. 使用响应头设置资源的打开方式
       * content-disposition：attachment;filename=xxx
  * 步骤：
    1. 定义页面，编辑超链接href属性，指向Servlet，传递资源名称filename
    2. 定义Servlet
       1. 获取文件名称
       2. 使用字节输入流加载文件进内存
       3. 指定response的响应头：content-disposition: attachment;filename=xxx
       4. 将数据写出到response输出流

* html页面

  ```html
      <a href="/Response/img/1.png">图片1</a>
      <hr>
      <a href="/Response//downloadServlet?filename=1.png">图片1</a>
  ```

* Servlet代码

  ```java
  @WebServlet("/downloadServlet")
  public class DownloadServlet extends HttpServlet {
      protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          //1.获取请求餐宿和，文件名称
          String filename = request.getParameter("filename");
          //2.使用字节输入流加载文件进内存
          //2.1找到文件的服务器路径
          ServletContext servletContext = this.getServletContext();
          String realPath = servletContext.getRealPath("/img/" + filename);
          //2.2用字节流关联
          FileInputStream fis = new FileInputStream(realPath);
  
          //设置response的响应头
          //3.1设置响应头类型 content-type
          //获取文件MIME类型
          String mimeType = servletContext.getMimeType(filename);
          response.setHeader("content-type", mimeType);
          //3.2设置响应头打开方式 content-disposition
          response.setHeader("content-disposition","attachment;filename="+filename);
  
          //4.将输入流的数据写出到输出流中
          ServletOutputStream outputStream = response.getOutputStream();
          byte[] buff = new byte[1024 * 8];
          int len = 0;
          while((len = fis.read(buff)) != -1){
              outputStream.write(buff,0,len);
          }
  
          fis.close();
      }
  
      protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          this.doPost(request, response);
      }
  }
  ```



* **中文文件名问题**：

  * 解决思路：
    1. 获取客户端使用的浏览器版本信息
    2. 根据不同的浏览器信息进行编码处理，工具类可以从网上下载
  * 工具类示例

  ```java
  import sun.misc.BASE64Encoder;
  import java.io.UnsupportedEncodingException;
  import java.net.URLEncoder;
  
  
  public class DownLoadUtils {
  
      public static String getFileName(String agent, String filename) throws UnsupportedEncodingException {
          if (agent.contains("MSIE")) {
              // IE浏览器
              filename = URLEncoder.encode(filename, "utf-8");
              filename = filename.replace("+", " ");
          } else if (agent.contains("Firefox")) {
              // 火狐浏览器
              BASE64Encoder base64Encoder = new BASE64Encoder();
              filename = "=?utf-8?B?" + base64Encoder.encode(filename.getBytes("utf-8")) + "?=";
          } else {
              // 其它浏览器
              filename = URLEncoder.encode(filename, "utf-8");
          }
          return filename;
      }
  }
  
  ```

  

***

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

***



## ServletContext对象

1. 概念：代表整个web应用，可以和程序的容器（服务器）来通信

2. 获取：

   1. 通过request对象获取

      request.getServletContext();

   2. 通过HttpServlet获取

      this.getServletContext();

3. 功能：

   1. 获取MIME类型

      * MIME类型：在互联网通信过程中顶一顶一种文件数据类型
        * 格式：大类型/小类型		text/html		image/jpeg
      * 获取：String getMimeType(String file)

   2. 域对象：共享数据

      1. setAttribute(String name, Object value)
      2. getAttribute(String name)
      3. removeAttribute(String name)

      * ServletContext对象范围：所有用户所有请求的数据

   1. 获取文件的真实（服务器）路径

      1. 方法：String getRealPath(String path)

      ```java
              // 通过HttpServlet获取
              ServletContext context = this.getServletContext();
      
      
              // 获取文件的服务器路径
              String b = context.getRealPath("/b.txt");//web目录下资源访问
              System.out.println(b);
             // File file = new File(realPath);
      
              String c = context.getRealPath("/WEB-INF/c.txt");//WEB-INF目录下的资源访问
              System.out.println(c);
      
              String a = context.getRealPath("/WEB-INF/classes/a.txt");//src目录下的资源访问
              System.out.println(a);
      ```

      