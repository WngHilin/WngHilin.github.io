---
title: 前端开发框架——Bootstrap
date: 2019-08-17 15:59:03
tags: 
- Javaweb
- 学习
- 前端
- Bootstrap
categories: JavaWeb
---

> Bootstrap是美国[Twitter](https://baike.baidu.com/item/Twitter/2443267)公司的设计师Mark Otto和Jacob Thornton合作基于HTML、CSS、[JavaScript](https://baike.baidu.com/item/JavaScript/321142) 开发的简洁、直观、强悍的[前端](https://baike.baidu.com/item/前端/5956545)开发框架，使得 Web 开发更加快捷。

## Bootstrap简介

1. 好处

   1. Bootstrap定义了很多css样式和js插件，可以提供极为丰富的页面效果
   2. 响应式布局。
      * 同一套页面可以兼容不同分辨率的设备

2. 使用

   1. 下载Bootstra [https://v3.bootcss.com/](https://v3.bootcss.com/)

   2. 将3个文件夹复制进项目

   3. 创建html页面，引入必要的资源文件，以下为基本模板（去掉了兼容ie8的设置）

      ```html
      <!DOCTYPE html>
      <html lang="zh-CN">
      <head>
          <meta charset="utf-8">
          <meta http-equiv="X-UA-Compatible" content="IE=edge">
          <meta name="viewport" content="width=device-width, initial-scale=1">
          <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
          <title>Bootstrap HelloWorld</title>
      
          <!-- Bootstrap -->
          <link href="css/bootstrap.min.css" rel="stylesheet">
      
          <!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
          <script src="js/jquery-3.2.1.min.js"></script>
          <!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
          <script src="js/bootstrap.min.js"></script>
      
      </head>
      <body>
      <h1>你好，世界！</h1>
      </body>
      </html>
      ```



## 响应式布局

* 同一套页面可以建中不同分辨率的设备

* 实现：依赖于**栅格系统**

  * 栅格系统：将一行平均分为12个格子，可以指定元素占几个格子

* 步骤：

  1. 定义**容器**。相当于table

     1. 分类：
        1. **container**：两边留白
        2. **container-fluid**：每种设备都占满100%宽度

  2. 定义**行**。相当于tr      样式：**row**

  3. 定义**元素**。指定该元素在不同的设备上所占的格子的数目    样式：**col-设备代号-格子数目**

     * 设备代号：

       1. **xs**：超小屏幕  手机( <768px )
       2. **sm**：小屏幕  平板( ≥768px )
       3. **md**：中等屏幕  桌面显示器( ≥1200px )
       4. **lg**：大屏幕  大桌面显示器( 大于等于1200px )

       ```html
       <!-- 1.定义容器 -->
           <div class="container-fluid">
               <!-- 2.定义行 -->
               <div class="row">
                   <!-- 3.定义元素
                       大显示器一行12个格子
                       pad上一行6个格子
                   -->
                   <div class="col-lg-1 col-sm-2">栅格</div>
                   <div class="col-lg-1 col-sm-2">栅格</div>
                   <div class="col-lg-1 col-sm-2">栅格</div>
                   <div class="col-lg-1 col-sm-2">栅格</div>
                   <div class="col-lg-1 col-sm-2">栅格</div>
                   <div class="col-lg-1 col-sm-2">栅格</div>
                   <div class="col-lg-1 col-sm-2">栅格</div>
                   <div class="col-lg-1 col-sm-2">栅格</div>
                   <div class="col-lg-1 col-sm-2">栅格</div>
                   <div class="col-lg-1 col-sm-2">栅格</div>
                   <div class="col-lg-1 col-sm-2">栅格</div>
                   <div class="col-lg-1 col-sm-2">栅格</div>
               </div>
           </div>
       ```

     * **注**：
       1. 一行中格子数目超过12，超出部分自动换行
       2. 栅格类属性可以向上兼容（适用于与屏幕宽度大于或等于分界点大小的设备）
       3. 如果设备宽度小于设置栅格类属性设备代码的最小值，会一个元素占满一整行

## CSS样式和JS插件

### CSS样式

* 全局CSS样式

  * 按钮：[https://v3.bootcss.com/css/#buttons](https://v3.bootcss.com/css/#buttons)

    * 设置默认按钮：[https://v3.bootcss.com/css/#buttons-tags](https://v3.bootcss.com/css/#buttons-tags)

      ```html
      <a class="btn btn-default" href="#" role="button">Link</a>
      <button class="btn btn-default" type="submit">Button</button>
      <input class="btn btn-default" type="button" value="Input">
      <input class="btn btn-default" type="submit" value="Submit">
      ```

    * 设置按钮样式: [https://v3.bootcss.com/css/#buttons-options](https://v3.bootcss.com/css/#buttons-options)

      ```html
      <button type="button" class="btn btn-default">（默认样式）Default</button>
      <button type="button" class="btn btn-primary">（首选项）Primary</button>
      <button type="button" class="btn btn-success">（成功）Success</button>
      <button type="button" class="btn btn-info">（一般信息）Info</button>
      <button type="button" class="btn btn-warning">（警告）Warning</button>
      <button type="button" class="btn btn-danger">（危险）Danger</button>
      <button type="button" class="btn btn-link">（链接）Link</button>
      ```

    * 设置按钮尺寸：[https://v3.bootcss.com/css/#buttons-sizes](https://v3.bootcss.com/css/#buttons-sizes)

      ```html
      <button type="button" class="btn btn-primary btn-lg">（大按钮）Large button</button>
      <button type="button" class="btn btn-default btn-lg">（大按钮）Large button</button>
      <button type="button" class="btn btn-primary">（默认尺寸）Default button</button>
      <button type="button" class="btn btn-default">（默认尺寸）Default button</button>
      <button type="button" class="btn btn-primary btn-sm">（小按钮）Small button</button>
      <button type="button" class="btn btn-default btn-sm">（小按钮）Small button</button>
      <button type="button" class="btn btn-primary btn-xs">（超小尺寸）Extra small button</button>
      <button type="button" class="btn btn-default btn-xs">（超小尺寸）Extra small button</button>
      ```

  * 图片

  * 表格

  * 表单

* 组件：

  * 导航条
  * 分页条

### JS插件

* 轮播图