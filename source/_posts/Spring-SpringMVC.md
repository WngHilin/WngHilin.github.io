---
title: SpringMVC学习
date: 2019-10-08 16:38:35
tags: 
- JavaWeb
- 学习
- 框架
categories: Spring
---

## 初步理解

* 步骤
  1. 导入jar包
  
  2. 在web.xml中配置DispatcherServlet
  
     ```xml
     <servlet>
         <servlet-name>HelloWeb</servlet-name>
         <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
         <load-on-startup>1</load-on-startup>
     </servlet>
     
     <servlet-mapping>
         <servlet-name>HelloWeb</servlet-name>
         <url-pattern>/</url-pattern>
     </servlet-mapping>
     ```
  
  3. 加入SpringMVC配置文件
  
     * 默认为WEB-INF/[servlet-name]-servlet.xml, 也可以在servlet标签中使用init-param进行指定
  
  4. 编写处理请求的处理器，并将其标识为处理器
  
     ```java
     @Controller
     @RequestMapping("/hello")
     public class HelloWorld {
         @RequestMapping(method = RequestMethod.GET)
         public String printHello(ModelMap model){
             model.addAttribute("message", "Hello World");
             return "hello";
         }
     }
     ```
  
  5. 编写视图
  
     ```xml
     <context:component-scan base-package="top.wnghilin.HelloWorld"></context:component-scan>
     
     <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
         <property name="prefix" value="/WEB-INF/jsp/"></property>
         <property name="suffix" value=".jsp"></property>
     </bean>
     ```



## 使用@RequestMapping映射请求

除了修饰方法，也可以修饰类

1. 类定义处：提供初步的请求映射信息，相对于WEB应用的根目录
2. 方法处：提供进一步的细分映射信息（如类定义处为/hello，方法处为/hello1，访问该方法就为/hello/hello1）
3. 相对于类定义处的URL。若类定义为标注@RequestMapping，则方法处标记的URL相对于WEB应用的根目录



* 指定请求方式：

  * method属性：取值为RequestMethod.GET和RequestMethod.POST

* 指定请求参数和请求头（了解）

  * params：

    ```java
    params = {"username", "age!=10"}//指定参数中必须有username，且参数age不能等于10
    ```

  * headers：

    ```java
    headers={"Accept-Language=zh-CN,zh,q=0.8"}//指定该请求头中Accept-Language的值必须为zh-CN,zh,q=0.8，否则无法访问
    ```

* RequestMapping支持Ant风格的URL

  * ?：匹配文件名中的一个字符
  * *：匹配文件名中的任意字符
  * **：匹配多层路径
  * 如：/user/*/createUser匹配/user/aaa/createUser、/user/**/createUser匹配/user/createUser或/user/aaa/bbb/createUser等URL

* **@PathVarieble**：可以来映射URL中的占位符到目标方法的参数中

  ```java
  @RequestMapping("/testPathVarieble/{id}")
  public String testPathVarieble(@PathVariable("id") int id){
      System.out.println("test: " + id);
      return "hello";
  }
  ```

  URL中的id值可以传入testPathVarieble的id中

>HTTP协议中，四个表示操作方式的动词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET用来获取资源，POST用来新建资源，PUT用来更新资源，DELETE用来删除资源

* **REST**风格示例

  * /order/1 HTTP GET  得到id = 1的order
  * /order/1 HTTP DELETE  得删除d = 1的order
  * /order/1 HTTP PUT 更新id = 1的order
  * /order HTTP POST 新增order

* HiddenHttpMethodFilter：浏览器form表单值只支持GET与POST请求。Spring3.0中添加了一个过滤器，可轻易将这些请求转换为标准的http方法，使得其支持GET、POST、PUT与DELETE请求

  * 如何发送**PUT**请求和**DELETE**请求

    1. 需要配置HiddenHttpMethodFilter

    2. 需要发送POST 请求

    3. 需要在发送POST请求时携带一个name="_method"的隐藏域，值为PUT或DELETE

       **注意！！！**：tomcat8不支持DELETE和PUT操作，所以以上代码只能在tomcat7上运行，否则会出现状态码405

       ```xml
       <!--必须作用于Dispatcher之前--> 
       <!--再web.xml中配置HiddenHttpMethodFilter--> 
       <filter>
           <filter-name>HiddenHttpMethodFilter</filter-name>
           <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
       </filter>
       <!--对所有的请求进行过滤--> 
       <filter-mapping>
           <filter-name>HiddenHttpMethodFilter</filter-name>
           <servlet-name>HelloWeb</servlet-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
       ```

       ```html
       <form action="hello/testRest/1" method="post">
       ..
       </form>
       ```

       ```html
       <form action="hello/testRest/1" method="post">
             <input type="hidden" name="_method" value="DELETE">
             <input type="submit" value="test DELETE">
       </form>
       ```



* 使用@RequestMapping映射请求：
  * @RequestParam：
    * value值即为请求参数的参数名
    * required 该参数是否必须
    * defaultValue 请求参数的默认值
    * 放置在方法的参数前
  * @RequestHeader（映射请求头信息）：
    * value值即为请求头的名称，如 Accept-Encoding 
    * 用法与@RequestParam类似
  * @CookieValue：
    * value为cookie name，常用的为JSESSIONID

### 使用POJO对象绑定请求参数值

* SpringMVC 会按请求参数名和POJO属性名进行自动匹配，自动为该对象填充属性值，支持级联属性

* 示例

  ```html
  <form action="springmvc/testPOJO" method="post">
      username: <input type="text" name="username">
      <br>
      password: <input type="password" name="password">
      <br>
      age: <input type="text" name="age">
      <br>
      email: <input type="text" name="email">
      <br>
      province: <input type="text" name="address.province">
      <br>
      city: <input type="text" name="address.city">
      <br>
      <input type="submit" value="test POJO">
  </form>
  ```

  ```java
  public class Address {
      private String province;
      private String city;
  	//此处省略了setter和getter方法
  }
  
  public class User {
      private String username;
      private String password;
      private int age;
      private String email;
      private Address address;
  	//此处省略了setter和getter方法
  }
  ```

  ```java
  @RequestMapping("/testPOJO")
  public String testPOJO(User user){
      System.out.println("user: " + user.toString());
      return "success";
  }
  ```

  

### 使用Servlet原生API作为目标方法的参数

* 具体支持以下类型

  * Http'ServletRequest

  * HttpServletResponse

  * HttpSession

  * java.security.Principal

  * Locale 

  * InputStream

  * OutputStream

  * Reader

  * Writer

  * 示例

    ```java
    @RequestMapping("/testServletAPI")
    public String testServletAPI(HttpServletRequest request,
                                 HttpServletResponse response,
                                 Writer out
                                ) throws IOException {
        System.out.println("Test API: " + request + ", " + response);
        out.write("Hello SpringMVC");
        return "success";
    }
    ```





## 处理模型数据

* ModelAndView类型：

  * 其中可以包含视图和模型信息

  * SpringMVC会把ModelAndView的model中数据放入request域对象中

  * **注意！！！**：ModelAndView的包为org.springframework.web.servlet.ModelAndView，而不是org.springframework.web.portlet.ModelAndView，自动导包可能会导入第二个包，这样会出现404错误

  * 示例：

    ```java
    @RequestMapping("/testModelAndView")
    public ModelAndView testModelAndView(){
        String viewName = "success";
        ModelAndView view = new ModelAndView(viewName);
    
        view.addObject("time", new Date());
        return view;
    }
    ```

    ```jsp
    <!--使用request域对象中的值-->
    ${requestScope.time}
    ```

* Map或者Model类型的参数：

  * 这两种类型放在目标方法的参数中，返回值仍为String

  * 目标方法可以添加Map类型（也可以是Model类型或者ModelMap类型）的参数

  * 示例：

    ```java
    @RequestMapping("/testMap")
    public String testMap(Map<String, Object> map){
        System.out.println(map.getClass().getName());
        map.put("name", Arrays.asList("Tom", "Jerry", "Steve"));
        return "success";
    }
    ```

* @SessionAttribute注解

  * value属性：将指定的数据同时存放在request域和session域中

  * types属性：class类型的参数，指定哪些模型属性需要放到会话中

  * 示例：

    ```java
    @RequestMapping("/testSessionAttribute")
    public String testSessionAttribute(Map<String, Object> map){
        User user = new User("username", "password", 13, "888888@gmail.com");
        map.put("user", user);
        return "success";
    }
    ```
  
* @ModelAttribute注解

  * 从数据库中获取一个对象要修改它的属性值，但有些属性值不能修改，只修改能修改的部分，普通情况下，没被修改的部分会为空，如图

    ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20191017172513.png)

  * 在获取对象的方法添加@ModelAttribute注解，可以解决这个问题

    ```java
    @ModelAttribute
    public void getUser(@RequestParam(value = "id", required = false)Integer id,
                        Map<String, Object> map){
        if(id != null){
            //模拟从数据库获取对象
            User user = new User(1, "xxxxxx", "123456", 18, "xxxx@gmail.com");
            System.out.println("原数据: " + user);
            
            map.put("user", user);
        }
    }
    ```

    ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20191017172636.png)

  * **运行流程**：
  
    1. 执行@ModelAttribute注解修饰的方法：从数据库中去除对象，把对象放入Map中，键为user
    2. SpringMVC从Map中取出User对象，并把表单的请求参数赋给该User对象的对应属性
    3. SpringMVC把上述对象传入目标方法的参数



