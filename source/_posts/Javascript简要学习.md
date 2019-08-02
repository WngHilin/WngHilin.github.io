---
title: JavaScript简要学习
date: 2019-08-01 10:47:11
tags: 
- Javaweb
- 学习
- Javascript
categories: JavaWeb
---

## 简介

JavaScript：

* 是**基于对象**和**事件驱动**的语言，应用于**客户端**
  * 基于对象：
    * 提供了很多对象，可直接使用
  * 事件驱动：
    * 可以实现动态效果
  * 客户端：指浏览器
* 特点：
  * 交互性
    * 信息的动态交互
  * 安全性：
    * js不能访问本地磁盘的文件
  * 跨平台性：
    * 通过浏览器实现
* 组成：
  * （1）**ECMAScript**
    * ECMA ：欧洲计算机协会
    * 由ECMA组织指定的js语法和语句
  * （2）**BOM**
    * browser object model 浏览器对象模型
  * （3）**DOM**
    * document object model 文档对象模型

***

## 使用

### JS和HTML的结合方式

* （1）使用script标签

  * ```html
    <script type="text/javascript"></script>
    ```

* （2）引入外部的js文件

  * ```html
    <script type="text/javascript" srt="*.js"></script>
    ```

* script标签放置：可以放在任何位置，但html是从上到下解析，所以最好放在</body>后面，否则JavaScript可能获取不到input等标签里面的值。

### 原始类型和声明变量

* js是弱类型语言

* 定义变量 使用关键字var

* 原始类型：

  * string：字符串

  ```javascript
  var str = "abc";
  ```

  * number：数字类型

  ```javascript
  var num = 123;
  ```

  * boolean：true or false
  * null：获取对象的引用，null表示对象引用为空

  ```javascript
  var date = new Date();
  ```

  * undefined：定义一个变量，没有赋值

  ```javascript
  var aa;
  ```

* typeof()：可以查看变量的类型

### 语句

* 种类：
  * if判断

  * switch语句：js所有类型都支持

  * 循环 for while do-while

    ```javascript
    for(var i = 0; i < 5; i++)
    {
            
    }
    ```

    

* 与java类似

### 运算符

* 与java中不同的：
  * js中不区分整数和小数，123 / 1000 == 0.123
  * 字符串相加和相减：
    * "456" + 1 == "4561"（相加做字符串连接）
    * "456" - 1 == "455"（相减进行真正的相减）
    * "abc" - 1会提示NaN表示不是一个数字
  * boolean操作：
    * true + 1 == 2
    * false + 1 == 1
    * 即true是1，false是0
  * “ === ”和“ == ”：
    * ==：值是否相等
    * ===：值和类型是否相等
  * 补充：document.write()：
    * 直接向页面写入内容，可以写入变量，固定值，html代码

<a name="Array" href></a>

### 数组

* 定义：

  * （1）var[] arr = {1, 2, 3}; //数组可以存放不同的数据类型

  * （2）使用内置Array对象

    * ```javascript
      var arr1 = new Array(5); //建立有5个元素的数组
      ```

    * ``` javascript
      var arr2 = new Array(3, 4, 5); //创建数组{3, 4, 5}
      ```

* 属性：
  
  * （1）length：arr.length;
  * <a id="gotoMoreArray" href="#LearnMoreArray">深入学习数组</a>

### 函数

* 定义函数：

  * （1）使用function关键字

  ```javascript
  function 方法名(参数列表){
      方法体;
      （返回值）;
  }
  //参数列表直接写参数名称，不用写var
  ```

  * （2）匿名函数

  ```javascript
  var 方法名 = function(参数列表){
      方法体和返回值;
  }
  ```

  * （3）使用内置对象Function

  ```javascript
  var add = new Function("参数列表", "方法体和返回值");
  //使用较少
  ```

### 全局变量和局部变量

* **全局变量**：在script标签里定义一个变量，其在**整个页面**的js部分都可使用
  * 方法内外，在另一个script标签使用
* **局部变量**：在方法内部定义一个变量，只能在方法内部使用

<a name="Reload" href></a>

### 重载

* 定义：函数名相同，函数参数列表不同(参数个数和参数类型)，根据参数不同去执行不同操作，但在js中，同一个作用域，出现两个名字一样的函数，后面的会覆盖前面的。故**JavaScript没有真正意义上的重载**。


***

## 深入

### String对象

* **创建**：

```javascript
var str = "abc";
```

* **属性**：

  * **length**：字符串的长度

* **方法**：

  * （1）与HTML相关：

    - **bold**：实现加粗

    ```javascript
    document.write(str.bold());
    ```

    - **fontcolor**：修改字符串的颜色

    ```javascript
    str2.fontcolor("red");
    ```

    - **fontsize**：修改字体大小

    ```javascript
    str3.fontsize(5);
    ```

    - **link**：将字符串显示为超链接

    ```javascript
    str4.link("wnghilin.top");
    ```

    - **sub**、**sup**：将字符串显示为上下标

  * （2）与Java类似：

    * **concat**：连接两个字符串

    ```javascript
    var str1 = "abc";
    var str2 = "gds";
    document.write(str1.concat(str2));
    
    输出结果：
    abcgds
    ```

    * **charAt**：返回指定位置的字符

    ```javascript
    var str3 = "abcdefg";
    str3.charAt(0);//返回a
    str3.charAt(20);//返回空字符串
    ```

    * **indexOf**：返回字符串位置，若不存在，返回-1
    * **split**：切分字符串，分为数组

    ```javascript
    var str4 = "a-b-c-d";
    var arr1 = str4.split("-");
    ```

    * **replace**：替换字符串

    ```javascript
    str6.replace("a", "6");
    ```

    * **substr** 和 **substring**：截取子字符串

    ```javascript
    var str5 = "abcdefghuiop";
    str5.substr(5, 3); //fgh 从5开始向后开始截取3个字符
    str5.substring(3, 5); //de 从3位开始，第5位结束但不包含第5位
    ```

<a name="LearnMoreArray" href></a>

### Array对象

* 创建：<a id="goArray" href="#Array">链接</a>

* 属性：

  * length：数组的长度

* 方法：

  * **concat**：数组拼接，用法类似字符串
  * **join**：根据指定字符分隔数组

  ```javascript
  var arr = new Array("a", "b", "c");
  document.write(arr); //输出：a, b, c
  document.write(arr.join("-")); //输出：a-b-c
  ```

  * **push**：向数组末尾添加一个或多个新的元素，并返回数组新的长度

  **<注>**如果向末尾添加的是数组，会将整个数组看作一个元素将其加入到原有数组中

  * **pop**：删除并返回最后一个元素
  * **reverse**：反转数组

### Date对象

* 获取当前时间

```javascript
var date = new Date();
date; //Fri Aug 02 2019 15:51:13 GMT+0800 (中国标准时间)
date.toLocaleString();// 2019/8/2 下午3:53:45
```

* 获取当前年月日和星期

```javascript
date.getFullYear(); //2019
date.getMonth(); //7，因为返回结果是0-11月，需要再加一返回真实月数
date.getDate(); //2
date.getDay(); //5，返回0-6，星期日是0
```

* 获取当前时分秒

```javascript
date.getHours(); //16
date.getMinutes(); //5
date.getSeconds(); //36
```

* 获取毫秒数：

```javascript
date.getTime(); //1564733221660，1970年1月1日至今的毫秒数
```



### Math对象

都是静态方法，只能通过**类名**+**方法**调用

* 方法：
  * **cell**：上舍入
  * **floor**：下舍入
  * **round**：四舍五入
  * **random**：产生0.0到1.0之间的随机数

* 属性
  * **PI**：圆周率
  * **E**：自然对数的底数

### 全局函数

* 不属于任何对象，致谢写名称使用

  * **eval**：如果字符串是js代码，使用该方法直接执行

  ```javascript
  var str = "alert(1234)";
  eval(str); //执行alert(1234)
  ```

  * **encodeURI** 和 **decodeURI**：对字符进行编码和解码
    * 将中文先编码再解码可以有效防止乱码
  * **encodeURIComponent** 和 **decodeURIComponent**：
    * 与上面只有编码字符多少的差别
  * **isNaN**：判断当前字符串是否是数字，若是，返回false
  * **parseInt**：解析字符串并返回一个整数

### 重载

有没有重载？<a id="goReload" href="#Reload">链接</a>

* 重载可模拟实现
  * 使用arguments对象实现，这个方法的缺点是麻烦。

```javascript
function overload(){
    if(arguments.length === 1)
        console.log('一个参数');
    else if(arguments.length === 2)
        console.log('两个参数');
}
//参数通过arguments[0]...来使用
```



### BOM对象

* **BOM**：浏览器对象模型

* 有哪些对象：

  * navigator：可以货期客户机的信息（浏览器的信息）

    * navigator.appName; //显示浏览器的名称

  * screen：可返回屏幕信息

    * screen.width; //返回屏幕的宽
    * screen.height; //返回屏幕的高

  * location：请求url地址

    * href属性

      * 获取请求的url地址，即浏览器地址栏的地址

      * 设置url地址

        * 可以设置页面按钮的事件，将其跳转到另外一个页面

        ```html
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <title>hello</title>
        </head>
        <body>
            <input type="button" value="跳转" onclick="href1()"></input>
        </body>
        <script>
            function href1() {
                location.href="http://www.baidu.com";
            }
        </script>
        </html>
        ```

  * history：请求url的历史记录

    * 可以通过其实现浏览器前进后退功能
      * history.back(); //到上一个页面
      * history.forward(); //到下一个页面
      * history.go(1); //到下一个页面
      * history.go(-1); //到下一个页面

  * **window**

#### window对象

* 窗口对象

* **顶层对象**（所用的bom对象都是在window对象的里面操作的）

  

* 方法：

  * window.alert()：弹出一个消息提示框

    * 简写alert();

  * confirm()：确认提示框，参数为提示内容

    * 返回值：若点击确认，则返回true，否则返回false

  * prompt(text, defaultText)：输入对话框

    * text为提示输入，defaultText为默认输入内容

  * **open(URL,name,features,replace)**：打开一个新窗口，并返回窗口对象

    * **URL**： 一个可选的字符串，声明了要在新窗口中显示的文档的 URL。如果省略了这个参数，或者它的值是空字符串，那么新窗口就不会显示任何文档。

    * **name**： 一个可选的字符串，该字符串是一个由逗号分隔的特征列表，其中包括数字、字母和下划线，该字符声明了新窗口的名称。这个名称可以用作标记 <a> 和 <form> 的属性 

    * **target **的值。如果该参数指定了一个已经存在的窗口，那么 open() 方法就不再创建一个新窗口，而只是返回对指定窗口的引用。在这种情况下，features 将被忽略。

    * **features**： 一个可选的字符串，声明了新窗口要显示的标准浏览器的特征。如果省略该参数，新窗口将具有所有标准特征。在窗口特征这个表格中，我们对该字符串的格式进行了详细的说明。

    * **replace**： 一个可选的布尔值。规定了装载到窗口的 URL 是在窗口的浏览历史中创建一个新条目，还是替换浏览历史中的当前条目。支持下面的值：
                   **true - URL**： 替换浏览历史中的当前条目。

      ​             **false - URL：** 在浏览历史中创建新的条目。

  * close()：关闭窗口，浏览器兼容性较差

  * **做定时器**：

    * setInterval(code, millisec)：每millisec毫秒执行一次code代码
    * 