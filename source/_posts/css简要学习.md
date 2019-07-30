---
title: css简要学习
date: 2019-07-30 21:31:49
tags: 
- Javaweb
- 学习
categories: JavaWeb
---

## CSS简介

* css：层叠样式表
  * 样式表：有很多属性和属性值
  * css可以使得页面显示效果更好，可以提高后期样式代码的可维护性

***

## CSS使用

### CSS和HTML的四种结合方式

* （1）style属性：

  * ```html
    <div style = "background-color:red;color:green">内容</div>
    ```

  <center>style属性示例</center>

* （2） style标签：

  * ```html
    <!---  此处可以设置<div>标签的样式  --->
    <style type="text/css">
        div{
            css代码;
        }
    </style>
    ```

  <center>style标签示例</center>

* （3）在style标签 使用语句：

  * 创建css文件（.css）
  * 在style标签中使用@import url(css文件路径)；

* （4）使用头标签引入外部css文件

  * 创建css文件

  * ```html
    <link rel="stylesheet" type="text/css" href="css路径">
    ```

* **注**：第三种方式在某些浏览器下不起作用，一般用第四种

### CSS优先级

优先级：即最终以哪一个样式为准

* 从上到下，从内到外，优先级从低到高。（一般情况）
  * 即html中，以在下方的和内部的样式为准
  * 格式：选择器名称{属性名: 属性值; ······}

***

### CSS基本选择器

#### 种类

* （1）标签选择器：用标签的名称作为选择器

* （2）class选择器：

  * 每个html标签都有个属性 class

    ```html
    <!---  此处可以用class1作为选择器 --->
    <div class="class1"></div>
    <p class="class1"></p>
    ```

  * ```css
    div.class1{
        css代码;
    }
    /*此处修改所有class属性为class1的样式*/
    .class1{
        css代码；
    }
    ```

* （3）id选择器：

  * ```html
    <div id="id1"></div>
    ```

  * ```css
    div#id1{
    	css代码;
    }
    /*此处修改所有id属性为id1的样式*/
    #id1{
        css代码;
    }
    ```

#### 优先级

* id选择器 > class选择器 > 标签选择器

### CSS扩展选择器

* （1）关联选择器

  * ```html
    <div><p>
        内容
        </p></div>
    ```

  * 设置嵌套标签里的样式：

    ```css
    /*div里的p标签*/
    div p{
        css代码;
    }
    ```

    

* （2）组合选择器

  ```html
  <div>
      <p>
          内容
      </p>
  </div>
  ```

  * 将div和p设置为相同样式

  ```css
  div,p{
      css代码;
  }
  ```

* （3）伪元素选择器

  * css提供的定义好的样式，可以直接选用

  * 比如超链接：

    * 状态：原始 悬停 点击 点击后
    * 原始： :link
    * 悬停： :hover
    * 点击： :active
    * 点击后： :visited

    ```css
    a:hover{
        css代码;
    }
    ```

### CSS盒子模型

在布局前需要把数据装到一块一块的区域内，这个区域叫做盒子。

![css盒子模型](https://www.runoob.com/images/box-model.gif)

- **Margin(外边距)** - 清除边框外的区域，外边距是透明的。

  - 与边框类似

- **Border(边框)** - 围绕在内边距和内容外的边框。

  - **上**：border-top

  - **下**：border-bottom

  - **左**：border-left

  - **右**：border-right

  - ```css
    /*统一设置*/
    div{
        border: 2px solid blue;
    }
    /*分别设置*/
    div#id1{
        border-left: 2px dashed yellow;
    }
    ```

- **Padding(内边距)** - 清除内容周围的区域，内边距是透明的。

  - 与边框类似

- **Content(内容)** - 盒子的内容，显示文本和图像。







***

学习资料：[(28天完整版)JavaWeb视频教程](https://www.bilibili.com/video/av37452727/?p=28)

资料、图片来源：[CSS 盒子模型](https://www.runoob.com/css/css-boxmodel.html)

