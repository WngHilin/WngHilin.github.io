---
title: JavaScript简要学习
date: 2019-08-01 10:47:11
tags: 
- Javaweb
- 学习
- Javascript
categories: JavaWeb
---

### ## 简介

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
  * 补充：document.write();
  * 直接向页面写入内容，可以写入变量，固定值，html代码