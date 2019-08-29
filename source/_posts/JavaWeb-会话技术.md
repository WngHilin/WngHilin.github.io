---
title: 会话技术——Cookie&Session
date: 2019-08-28 19:25:32
tags: 
- Javaweb
- 学习
categories: JavaWeb
---

## 会话技术

1. 会话：一次会话中包含多次请求和响应
   * 一次会话：浏览器第一次给服务器资源发送请求，会话建立，知道有一方断开为止
2. 功能：在一次会话的范围内的多次请求间，共享数据
3. 方式：
   1. 客户端会话技术：Cookie
   2. 服务器端会话技术：Session



## Cookie

1. 概念：客户端会话技术，将数据保存到客户端的技术

2. 使用步骤：

   1. 创建Cookie对象，绑定数据
      * new Cookie(String name, String Value)
   2. 放松Cookie对象
      * response.addCookie(Cookie cookie);
   3. 获取Cookie，拿到数据
      * Cookie[] request.getCookies()

3. 实现原理

   基于响应头set-cookie和请求头cookie实现

4. cookie的**细节**：

   1. 一次可以发送**多个**cookie

      * 可以创建多个Cookie对象，使用多次response.addCookie来发送cookie即可

   2. cookie在浏览器中保存时间

      * 默认情况下，当浏览器关闭后，Cookie数据被清理
      * 持久化存储：
        1. setMaxAge(int seconds)
           1. 正数：将Cookie数据写到硬盘的文件中。持久化存储。seconds代表cookie存活时间
           2. 负数：默认值
           3. 零：删除Cookie信息

   3. cookie保存中文

      * tomcat 8 之前，不能直接存储中文
        * 需要将中文数据转码 --- 一般采用url编码
      * tomcat 8 之后，cookie支持中文数据，对特殊字符还是不支持，建议使用url编码

   4. cookie获取范围

      * 默认情况下，一个tomcat服务器中部署了多个Web项目，这些项目中cookie不能共享

        * setPath(String path)：设置Cookie的获取范围。默认情况下，设置当前的虚拟目录
          * 如果要共享，可以将path设置为"/"

      * 不同的tomcat服务器间cookie的共享问题

        * setDomain(String path)：如果设置**一级域名**相同，那么多个服务器之间cookie可以共享

          如cookie.setDomain(".baidu.com")，则news.baidu.com和tieba.baidu.com之间的cookie可以共享
   
5. cookie的**特点**：

   1. cookie存储在客户端浏览器
   2. 浏览器对于单个cookie的大小有限制(4kb左右)，以及对同一个域名下的总cookie数量也有限制(20个)

   * 作用：
     1. cookie一般用于存储少量的不太敏感的数据
     2. 在不登录的情况下，完成服务器对客户端的身份识别



* **案例**：记住上一次的访问时间
* 分析：
  1. 可以采用Cookie完成
  2. 在服务器中的Servlet判断是否有一个名为lastTime的cookie
     1. 有：不是第一次访问
        1. 响应数据：欢迎回来，您上次访问时间为：显示时间字符串
        2. 写回cookie
     2. 没有：是第一次访问
        1. 响应数据：您好，欢迎您首次访问
        2. 写回cookie：lastTime=时间

```java
@WebServlet("/cookieTest")
public class CookieTest extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //设置响应的消息体的数据格式以及编码
        response.setContentType("text/html;charset=utf-8");

        //1.获取所有Cookie
        Cookie[] cookies = request.getCookies();
        boolean flag = false;//没有cookie为lastTime
        //2.遍历cookie数组
        if(cookies != null && cookies.length > 0){
            for (Cookie cookie : cookies) {
                //3.获取cookie的名称
                String name = cookie.getName();
                //4.判断名称是否是：lastTime
                if("lastTime".equals(name)){
                    //有该Cookie，不是第一次访问

                    flag = true;//有lastTime的cookie
                    
                    //响应数据
                    //获取Cookie的value，时间
                    String value = cookie.getValue();
                    System.out.println("解码前："+value);
                    //URL解码：
                    value = URLDecoder.decode(value,"utf-8");
                    System.out.println("解码后："+value);
                    response.getWriter().write("<h1>欢迎回来，您上次访问时间为:"+value+"</h1>");

                    //设置Cookie的value
                    //获取当前时间的字符串，重新设置Cookie的值，重新发送cookie
                    Date date  = new Date();
                    SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
                    String str_date = sdf.format(date);
                    System.out.println("编码前："+str_date);
                    //URL编码
                    str_date = URLEncoder.encode(str_date,"utf-8");
                    System.out.println("编码后："+str_date);
                    cookie.setValue(str_date);
                    //设置cookie的存活时间
                    cookie.setMaxAge(60 * 60 * 24 * 30);//一个月
                    response.addCookie(cookie);
         
                    break;
                }
            }
        }


        if(cookies == null || cookies.length == 0 || flag == false){
            //没有，第一次访问

            //设置Cookie的value
            //获取当前时间的字符串，重新设置Cookie的值，重新发送cookie
            Date date  = new Date();
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
            String str_date = sdf.format(date);
            System.out.println("编码前："+str_date);
            //URL编码
            str_date = URLEncoder.encode(str_date,"utf-8");
            System.out.println("编码后："+str_date);

            Cookie cookie = new Cookie("lastTime",str_date);
            //设置cookie的存活时间
            cookie.setMaxAge(60 * 60 * 24 * 30);//一个月
            response.addCookie(cookie);

            response.getWriter().write("<h1>您好，欢迎您首次访问</h1>");
        }


    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```



## Session

1. 概念：服务器端会话技术，再一次会话的多次请求间共享数据，将数据保存在服务器端

2. 入门：

   1. 获取Session对象：

      HttpSession session = request.getSession()

   2. HttpSession对象：

      Object getAttribute(String name)

      void setAttribute(String name, Object value)

      void removeAttribute(String name)

3. 原理：

   * Session的实现是依赖于**Cookie**的

4. 细节：

   1. 当客户端关闭后，服务器不关闭，两次获取的Session默认情况下**不是**同一个
      * 如果需要相同，则可以创建Cookie，键为**JSESSIONID**，值设置为**session.getId()**，设置最大存活时间，可以长久化存储
   2. 客户端不关闭，服务器关闭，两次获取的Session一般**不是**同一个
      * 要确保数据不丢失
        * Session的钝化：
          * 在服务器正常关闭之前，将Session对象系列化到硬盘上
        * Session的火花：
          * 在服务器启动后，将Session文件转化为内存中的Session对象即可
   3. Session销毁时间
      1. 服务器关闭
      2. session对象调用invaliddate()
      3. session默认失效时间  30分钟（可以在tomcat的conf/web.xml中配置session-config）

5. **特点**：

   1. session用于储存一次会话的多次请求的数据，存在服务器端
   2. session可以存储任意类型，任意大小的数据



* session与Cookie的区别：
  1. session存储数据在服务器端，Cookie在客户端
  2. session没有数据大小限制，Cookie有
  3. session数据安全，Cookie相对不安全



### 案例：验证码

1. 需求：
   1. 访问带有验证码的登录界面login.jsp
   2. 用户输入用户名，密码以及验证码
      * 如果用户名和密码输入有误，跳转登录界面，提示：用户名或密码错误
      * 如果验证码输入有误，跳转登录页面，提示：验证码错误
      * 如果全部输入正确，则跳转到主页success.jsp，显示：用户名，欢迎您

