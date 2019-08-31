---
title: Filter和Listener
date: 2019-08-30 08:58:50
tags: 
- Javaweb
- 学习
categories: JavaWeb
---

## Filter：过滤器

* 概念

  * Web中的过滤器：当访问服务器的资源时，过滤器可以将请求拦截下来，完成一些特殊的功能
  * 过滤器的作用：
    * 一般用于完成通用的操作。如：登录验证、统一编码处理、敏感字符过滤...

* 步骤：

  1. 定义一个类，实现接口Filter
  2. 复写方法
  3. 配置拦截路径
     1. web.xml
     2. 注解

  ```java
  package top.wnghilin.web.filter;
  
  import javax.servlet.*;
  import javax.servlet.annotation.WebFilter; //注意导入的是Servlet包的Filter
  import java.io.IOException;
  @WebFilter("/*")//访问所有资源之前都会执行该过滤器
  public class FilterDemo1 implements Filter {
      @Override
      public void init(FilterConfig filterConfig) throws ServletException {
  
      }
  
      @Override
      public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
          System.out.println("FilterDemo1......");
  
          //放行
          filterChain.doFilter(servletRequest, servletResponse);
      }
  
      @Override
      public void destroy() {
  
      }
  }
  ```

  4. 细节：

     1. web.xml配置

        ```xml
            <filter>
                <filter-name>demo1</filter-name>
                <filter-class>top.wnghilin.web.filter.FilterDemo1</filter-class>
            </filter>
            <filter-mapping>
                <filter-name>demo1</filter-name>
                <!-- 拦截路径 -->
                <url-pattern>/*</url-pattern>
            </filter-mapping>
        ```

        

     2. 过滤器执行流程

        1. 执行过滤器
        2. 执行放行后的资源
        3. 回来执行过滤器放行代码下面的代码

     3. 过滤器的生命周期方法

        1. init：在服务器启动后，会创建Filter对象，然后调用init方法。只执行一次。用于加载资源
        2. doFilter：每一次请求被拦截资源时，会执行。执行多次
        3. destroy：在服务器关闭后，Filter对象被销毁。如果服务器时正常关闭，则会执行destroy方法。只执行一次，用于释放资源

     4. 过滤器配置：

        * 拦截路径配置
          1. 具体资源路径： /index.jsp   只有访问index.jsp资源时，过滤器才会被执行
          2. 拦截目录：/user/*    访问/user下的所有资源时，过滤器都会被执行
          3. 后缀名拦截：*.jsp     访问所有后缀名为jsp资源时，过滤器都会被执行
          4. 拦截所有资源：/*    访问所有资源时，过滤器都会被执行
        * 拦截方式配置：资源被访问的方式
          1. 注解配置
             * 设置：dispacherTypes属性
               * **REQUEST**：默认值。浏览器直接请求资源时，才会被执行
               * **FORWARD**：转发访问资源时，才会被执行
               * INCLUDE：包含访问资源
               * ERROR：错误跳转资源
               * ASYNC：异步访问资源
               * **注意**：用{ }包含可以使用多个值
          2. web.xml配置
             * 设置&lt;dispacher>&lt;/dispacher>

     5. 过滤器链(配置多个过滤器)

        * 执行顺序：如果有两个过滤器：过滤器1和过滤器2
          1. 过滤器1
          2. 过滤器2
          3. 资源执行
          4. 过滤器2
          5. 过滤器1
        * 过滤器先后顺序问题
          1. 注解配置：按照类名的字符串比较规则，值小的先执行
             * 如：AFilter和BFilter，AFilter先执行
          2. web.xml配置：&lt;filter-mapping>谁定义在上面，谁先执行



### 增强对象的功能

1. 设计模式：一些通用的解决固定问题的方式

   1. 装饰模式

   2. 代理模式

      1. 概念：

         1. 真实对象：被代理的对象
         2. 代理对象：
         3. 代理模式：代理对象代理真是对象，达到增强真实对象功能的目的

      2. 实现方式：

         1. 静态代理：有一个类文件描述代理模式

         2. 动态代理：在内存中形成代理类

            * 实现步骤

              1. 代理对象和真实对象实现相同的接口
              2. 代理对象 = Proxy.newProxyInstance()
              3. 使用代理对象调用方法
              4. 增强方法
                 1. 增强参数列表
                 2. 增强返回值类型
                 3. 增强方法体执行逻辑

              ```java
              import java.lang.reflect.InvocationHandler;
              import java.lang.reflect.Method;
              import java.lang.reflect.Proxy;
              
              public class ProxyTest {
              
                  public static void main(String[] args) {
                      //1.创建真实对象
                      Lenovo lenovo = new Lenovo();
                      
                      //2.动态代理增强lenovo对象
                      /*
                          三个参数：
                              1. 类加载器：真实对象.getClass().getClassLoader()
                              2. 接口数组：真实对象.getClass().getInterfaces()
                              3. 处理器：new InvocationHandler()
                       */
                      SaleComputer proxy_lenovo = (SaleComputer) Proxy.newProxyInstance(lenovo.getClass().getClassLoader(), lenovo.getClass().getInterfaces(), new InvocationHandler() {
              
                          /*
                              代理逻辑编写的方法：代理对象调用的所有方法都会触发该方法执行
                                  参数：
                                      1. proxy:代理对象
                                      2. method：代理对象调用的方法，被封装为的对象
                                      3. args:代理对象调用的方法时，传递的实际参数
                           */
                          @Override
                          public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                              /*System.out.println("该方法执行了....");
                              System.out.println(method.getName());
                              System.out.println(args[0]);
              			   
              			   */
                              //判断是否是sale方法
                              if(method.getName().equals("sale")){
                                  //1.增强参数
                                  double money = (double) args[0];
                                  money = money * 0.85;
                                  System.out.println("专车接你....");
                                  //使用真实对象调用该方法
                                  String obj = (String) method.invoke(lenovo, money);
                                  System.out.println("免费送货...");
                                  //2.增强返回值
                                  return obj+"_鼠标垫";
                              }else{
                                  Object obj = method.invoke(lenovo, args);
                                  return obj;
                              }
                          }
                      });
              
                      //3.调用方法
              
                     /* String computer = proxy_lenovo.sale(8000);
                      System.out.println(computer);*/
              
                      proxy_lenovo.show();
                  }
              }
              
              ```

              



## Listener：监听器

* 概念：web的三大组建之一

  * 事件监听机制：
    * 事件：一件事情
    * 事件源：事件发生的地方
    * 监听器：一个对象
    * 注册监听：将事件、事件源、监听器绑定在一起。当事件源上发生某个事件后，执行监听器代码

* ServletContexListener：

  * 方法

    * void contextDestroyed(ServletContextEvent sce)
    * void contextInitialized(ServletContextEvent sce)

  * 步骤：

    * 定义一个类，实现ServletContextListener接口

    * 复写方法

    * 配置

      1. web.xml

         ```xml
            <!--
               配置监听器
            -->
            <listener>
               <listener-class>top.wnghilin.web.listener.ContextLoaderListener</listener-class>
            </listener>
         <!-- 指定初始化参数 -->
            <context-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>/WEB-INF/classes/applicationContext.xml</param-value>
            </context-param>
         ```

         

      2. 注解：@WebListener

      ```java
      import javax.servlet.ServletContext;
      import javax.servlet.ServletContextEvent;
      import javax.servlet.ServletContextListener;
      import javax.servlet.annotation.WebListener;
      import java.io.FileInputStream;
      
      
      @WebListener
      public class ContextLoaderListener implements ServletContextListener {
      
          /**
           * 监听ServletContext对象创建的。ServletContext对象服务器启动后自动创建。
           *
           * 在服务器启动后自动调用
           * @param servletContextEvent
           */
          @Override
          public void contextInitialized(ServletContextEvent servletContextEvent) {
              //加载资源文件
              //1.获取ServletContext对象
              ServletContext servletContext = servletContextEvent.getServletContext();
      
              //2.加载资源文件
              String contextConfigLocation = servletContext.getInitParameter("contextConfigLocation");
      
              //3.获取真实路径
              String realPath = servletContext.getRealPath(contextConfigLocation);
      
              //4.加载进内存
              try{
                  FileInputStream fis = new FileInputStream(realPath);
                  System.out.println(fis);
              }catch (Exception e){
                  e.printStackTrace();
              }
              System.out.println("ServletContext对象被创建了。。。");
          }
      
          /**
           * 在服务器关闭后，ServletContext对象被销毁。当服务器正常关闭后该方法被调用
           * @param servletContextEvent
           */
          @Override
          public void contextDestroyed(ServletContextEvent servletContextEvent) {
              System.out.println("ServletContext对象被销毁了。。。");
          }
      }
      ```

      