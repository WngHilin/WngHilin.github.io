---
layout: hexo
title: AJAX&JSON
date: 2019-08-31 09:43:43
tags: 
- Javaweb
- 学习
categories: JavaWeb
---

## AJAX

1. 概念：ASynchronous JavaScript and XML     异步的JavaScript 和 XML

   > AJAX是一种无需刷新整个页面而更新部分页面的技术

   1. 异步和同步：客户端和服务器端相互通信的基础上

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/1.同步和异步.bmp)

2. 实现方式：

   1. 原生的JS实现方式

      ```javascript
      var xmlhttp;
      if (window.XMLHttpRequest)
      {// code for IE7+, Firefox, Chrome, Opera, Safari
        	xmlhttp=new XMLHttpRequest();
      }
      else
      {// code for IE6, IE5
        	xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
      }
      
      //2.建立连接
      /*参数：
      	1.请求方式
      		get方式，请求参数在URL后面拼接，send方法为空参
      		post方式，请求参数在send方法中定义
      	2.请求URL路径
      	3.同步或异步请求
      */
      xmlhttp.open("GET", "test1.txt", "true");
      
      //3.发送请求
      xmlhttp.send();
      
      //4.接收并处理来自服务器的响应结果
      //获取方式：xmlhttp.responseText
      //当服务器响应成功后获取
      
      //当xmlhttp对象的就绪状态改变时，出发时间onreadystatechange
      xmlhttp.onreadystatechange=function(){
          if(xmlhttp.readyState==4 && xmlhttp.statu==200)
          {
              //获取服务器的相应结果
              xmlhttp.responseText;
          }
      }
      ```

      

   2. JQuery实现方式

      1. $.ajax()
      
         * 语法：$.ajax({键值对});
      
           ```javascript
               <script>
                   function  fun() {
                       //使用$.ajax()发送异步请求
                       $.ajax({
                           url:"ajaxServlet",
                           type:"post",
                           //data:"username=jack",//请求参数
                           data:{"username":"jack"},
                           success:function (data) {
                               alert(data);
                           },//响应成功后的回掉函数
                           error:function () {
                               //出错时执行的函数
                           },
                           dataType:"text"//设置接收到响应数据的格式
                       });
                   }
               </script>
           ```
      
           
      
      2. $.get()：发送get请求
      
         * 语法：$.get(url, [data], [callback], [type])
           * 参数：
             * url：请求路径
             * data：请求参数
             * callback：回调函数
             * type：响应结果的类型
      
      3. $.post()：发送post请求
      
         * 语法：$.post(url, [data], [callback], [type])
           - 参数：
             - url：请求路径
             - data：请求参数
             - callback：回调函数
             - type：响应结果的类型





## JSON

1. 概念：JavaScript Object Notation JavaScript对象表示法

   > JSON现在多用于存储和交换文本信息，进行数据的传输

   ```javascript
   var p = {"name":"张三", "age":23, "gender":"男"};
   ```

2. JSON比XML更小、更快、更容易解析



3. 语法：

   1. 基本规则

      * 数据在名称/值对中：json数据是由**键值对**构成的
        * 键用引号引起来（单双都可），也可以不使用引号
        * **取值类型**
          1. 数字（整数或浮点数）
          2. 字符串（双引号中）
          3. 逻辑值（true或false）
          4. 数组（方括号中）
          5. 对象（花括号中）{"address":{"province":".." .......}}
          6. null
      * 数据由**逗号**分隔：多个键值对由逗号分隔
      * **花括号**保存对象：使用{ }定义json格式
      * **方括号**保存数组：[]

   2. 获取数据

      1. json对象.键名
      2. json对象["键名"]
      3. 数组对象[索引]

   3. 遍历：

      1. for in循环

         ```javascript
         for(var key in json对象){
             //key可以获取键名
             //获取键值只能使用方法2，因为key是一个字符串
             person[key];
         }
         ```



### JSON数据和Java对象之间的转换

* JSON解析器
  * 常见：Jsonlib，Gson，fastjson，jackson

1. JSON转为Java对象

   1. 步骤
      1. 导入Jackson的jar包
      2. 创建Jackson核心对象  ObjectMapper
      3. 调用ObjectMapper的相关方法进行转换
         1. readValue(json数据, Class)

2. Java对象转为JSON

   1. 步骤：
      1. 导入Jackson的jar包
      2. 创建Jackson核心对象  ObjectMapper
      3. 调用ObjectMapper的相关方法进行转换

   ```java
   public class JacksonTest {
   
       //Java对象转为JSON字符串
       @Test
       public void test1() throws JsonProcessingException {
           Person p = new Person();
           p.setName("张三");
           p.setAge(23);
           p.setGender("男");
   
           //创建Jackson的核心对象
           ObjectMapper mapper = new ObjectMapper();
           //3. 转换
           /*
               转换方法
                   writeValue(参数1, obj)
                       参数1：
                           File：将obj对象转换位JSON字符串，并保存到指定文件中
                           Writer：将obj对象转换位JSON字符串，并填充到字符输出流中
                           OutputStream：将obj对象转换位JSON字符串，并填充到字符输出流中
                   writeValueAsString(obj)：将对象转为json字符串
           */
           String json = mapper.writeValueAsString(p);
           System.out.println(json);
       }
   }
   ```

   2. 注解：
      1. @JsonIgnore：排除属性
      2. @JsonFormat：属性值的格式化
   3. 复杂的java对象转换
      1. LIst集合：数组
      2. Map集合：对象格式一致



**注意**：服务器响应的数据，在客户使用时，要想当作json格式使用

1. $.get(type)：将最后一个参数指定为"json"

2. 在服务器端设置MIME类型：

   response.setContentType("application/json;charset=utf-8")