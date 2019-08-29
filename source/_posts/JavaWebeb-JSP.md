---
title: JSP——Java服务器端页面
date: 2019-08-29 09:48:19
tags: 
- Javaweb
- 学习
categories: JavaWeb
---

## 简介

1. 概念：
   * Java Server Pages：java服务器端页面
     * 可以理解为：一个特殊的页面，其中既可以指定定义html标签，又可以定义java代码
     * 用于简化书写

2. 原理
   * JSP本质上就是一个Servlet
3. JSP的脚本：JSP定义Java代码的方式
   1. &lt;%   代码 %&gt;：定义的Java代码，在service方法中。service方法中可以定义什么，该脚本中就可以定义什么
   2. &lt;%!  代码 % &gt;：定义的Java代码，在jsp转换后的Java类的成员位置
   3. &lt;%= 代码 % &gt;：定义的Java代码，会输出到页面上，输出语句可以定义什么，该脚本就可以定义什么
4. JSP的内置对象
   * 在JSP页面中不需要获取和创建，可以直接使用的对象
   * JSP一共有9个内置对象
   * 其中3个
     * request
     * response
     * out：字符输出流对象。可以将数据输出到页面上。和response.getWriter()类似
       * response.getWriter().write和out.write()的区别
         * 在tomcat服务器真正给客户端做出响应之前，会先找response缓冲区数据，再找out缓冲区数据
         * response.getWriter()数据输出永远再out.write之前



## 深入

### 指令

* 作用：用于配置JSP页面，导入资源文件

* 格式：

  <%@ 指令名称 属性名1=属性值1 属性名2=属性值2 ......>

* 分类：

  1. page：配置JSP页面的
     * contentType：等同于response.setContentType()
       1. 设置响应体的MIME类型和字符集
       2. 设置当前jsp页面的代码（只能是高级的IDE才能生效，如果使用低级工具，则需要设置pageEncoding）
     * import：导包
     * errorPage：当前页面发生异常后，会自动跳转到制定的错误页面
     * isErrorPage：指定当前页面是否是异常页面
       * true：可以使用内置对象exception
       * false：默认值，不可以使用该内置对象
  2. include：页面包含的。导入页面的资源文件
     * <%@include file="top.jsp" %&gt;
  3. taglib：导入资源
     * 导入标签库：<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jst1/core" %>
       * prefix：自定义的前缀

### 注释

1. html注释：
   &lt;!-- -- >：只能注释html代码
2. jsp注释：推荐使用
   &lt;%-- --%>：可以注释所有

### 内置对象

* 在jsp页面中不需要创建，直接使用的对象
* 一共有9个：

|     变量名      |      真实类型       |              作用               |
| :-------------: | :-----------------: | :-----------------------------: |
| **pageContext** |     PageContext     |        当前页面共享数据         |
|   **request**   | HttpServletRequest  | 一次请求访问的多个资源（转发）  |
|   **session**   |     HttpSession     |      一次会话的多个请求间       |
| **application** |   ServletContext    |       所有用户间共享数据        |
|  **response**   | HttpServletResponse |            响应对象             |
|    **page**     |       Object        | 当前页面（Servlet）的对象  this |
|     **out**     |      JspWriter      |   输出对象，数据输出到页面上    |
|   **config**    |    ServletConfig    |        Servlet的配置对象        |
|  **exception**  |      Throwable      |            异常对象             |



***

## MVC：开发模式

* 如果过度使用jsp，在jsp中既写大量Java代码，又写html，造成难以维护，难以分工协作





* MVC：

  1. M：Model，模型
     * 完成具体业务操作，如：查询数据库，封装对象
  2. V：View，视图
     * 展示数据
  3. C：Controller，控制器
     * 获取用户的输入
     * 调用模型
     * 将数据交给视图展示

  ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/MVC开发模式.bmp)

  * 优缺点：
    1. 优点：
       1. 耦合性低，方便维护，利于分工协作
       2. 重用性高
    2. 缺点：
       1. 使得项目架构变得复杂，对开发人员要求高