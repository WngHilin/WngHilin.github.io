---
title: JavaWeb_JQuery入门
date: 2019-08-30 19:00:01
tags: 
- Javaweb
- 学习
- JQuery
categories: JavaWeb
---

## 基础

1. 概念：一个JavaScript框架，简化JS开发

   > jQuery是一个快速、简洁的JavaScript框架，是继Prototype之后又一个优秀的JavaScript代码库（*或JavaScript框架*）。jQuery设计的宗旨是“write Less，Do More”，即倡导写更少的代码，做更多的事情。它封装JavaScript常用的功能代码，提供一种简便的JavaScript设计模式，优化HTML文档操作、事件处理、动画设计和Ajax交互。

2. 步骤：
   1. 下载JQuery
      1. jquery-xxx.js域jquery-xxx.min.js的区别
         1. jquery-xxx.js：用于阅读源码
         2. jquery-xxx.min.js：用于开发中使用
   2. 在项目中导入JQuery的js文件
   3. 使用



3. JQuery对象和JS对象的区别与转换
   1. JQuery对象在操作时，更加方便
   2. JQuery对象和js对象方法不通用
   3. 相互转换
      * jq → js：jq对象[索引] 或者 jq对象.get(索引)
      * js → jq：$(js对象)



4. 选择器：筛选具有相似特征的元素(标签)

   ```javascript
   
   	1. 基本操作学习：
   		1. 事件绑定
   			//1.获取b1按钮
               $("#b1").click(function(){
                   alert("abc");
               });
   		2. 入口函数
   			 $(function () {
   	           
      			 });
   			 window.onload  和 $(function) 区别
                    * window.onload 只能定义一次,如果定义多次，后边的会将前边的覆盖掉
                    * $(function)可以定义多次的。
   		3. 样式控制：css方法
   			 // $("#div1").css("background-color","red");
         		$("#div1").css("backgroundColor","pink");
   
   	2. 分类
   		1. 基本选择器
   			1. 标签选择器（元素选择器）
   				* 语法： $("html标签名") 获得所有匹配标签名称的元素
   			2. id选择器 
   				* 语法： $("#id的属性值") 获得与指定id属性值匹配的元素
   			3. 类选择器
   				* 语法： $(".class的属性值") 获得与指定的class属性值匹配的元素
   			4. 并集选择器：
   				* 语法： $("选择器1,选择器2....") 获取多个选择器选中的所有元素
   		2. 层级选择器
   			1. 后代选择器
   				* 语法： $("A B ") 选择A元素内部的所有B元素		
   			2. 子选择器
   				* 语法： $("A > B") 选择A元素内部的所有B子元素
   		3. 属性选择器
   			1. 属性名称选择器 
   				* 语法： $("A[属性名]") 包含指定属性的选择器
   			2. 属性选择器
   				* 语法： $("A[属性名='值']") 包含指定属性等于指定值的选择器
   			3. 复合属性选择器
   				* 语法： $("A[属性名='值'][]...") 包含多个属性条件的选择器
   		4. 过滤选择器
   			1. 首元素选择器 
   				* 语法： :first 获得选择的元素中的第一个元素
   			2. 尾元素选择器 
   				* 语法： :last 获得选择的元素中的最后一个元素
   			3. 非元素选择器
   				* 语法： :not(selector) 不包括指定内容的元素
   			4. 偶数选择器
   				* 语法： :even 偶数，从 0 开始计数
   			5. 奇数选择器
   				* 语法： :odd 奇数，从 0 开始计数
   			6. 等于索引选择器
   				* 语法： :eq(index) 指定索引元素
   			7. 大于索引选择器 
   				* 语法： :gt(index) 大于指定索引元素
   			8. 小于索引选择器 
   				* 语法： :lt(index) 小于指定索引元素
   			9. 标题选择器
   				* 语法： :header 获得标题（h1~h6）元素，固定写法
   		5. 表单过滤选择器
   			1. 可用元素选择器 
   				* 语法： :enabled 获得可用元素
   			2. 不可用元素选择器 
   				* 语法： :disabled 获得不可用元素
   			3. 选中选择器 
   				* 语法： :checked 获得单选/复选框选中的元素
   			4. 选中选择器 
   				* 语法： :selected 获得下拉框选中的元素
   ```

   