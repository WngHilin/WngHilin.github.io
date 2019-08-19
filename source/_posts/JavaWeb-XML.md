---
title: XML入门
date: 2019-08-18 15:11:35
tags: 
- Javaweb
- 学习
- xml
categories: JavaWeb
---

## XML简介

1. 概念：Extensible Markup Language 可扩展标记语言
   * 可扩展：标签都是自定义的
2. 功能：
   * 存储数据：
     1. 配置文件
     2. 在网络中传输
3. **xml**与**html**的区别
   * xml标签都是自定义，html标签时预定义
   * xml语法严格，html语法松散
   * xml是存储数据的，html是展示数据的

* w3c：万维网联盟

## 语法

* 基本语法：

  1. 文档后缀名 .xml
  2. xml第一行必须定义为文档声明
  3. xml文档中有且仅有一个根标签
  4. 属性值必须使用引号引起来（单双都可）
  5. 标签必须正确关闭
  6. xml标签名称区分大小写

* 组成部分：

  1. **文档声明**

     1. 格式：&lt;?xml$ 属性列表 ?&gt;
     2. 属性列表：
        * version：版本号，必需的属性
        * encoding：编码方式，告知解析引擎当前文档使用的字符集，默认值：ISO-8859-1
        * standalone：是否独立
          * 取值
            * yes：不依赖其他文件
            * no：依赖其他文件

  2. 指令（了解）：结合css

     * ```xml
       <? xml-stylesheet type="text/css" href="a.css" ?>
       ```

  3. **标签**：标签名称自定义

     - 名称可以含字母、数字以及其他的字符
     - 名称不能以数字或者标点符号开始
     - 名称不能以字符 “xml”（或者 XML、Xml）开始
     - 名称不能包含空格

  4. **属性**：

     * id属性值唯一

  5. **文本**

     * CDATA区：该区域内的数据会被原样展示
       * 格式：&lt;![CDATA[数据]]&gt;

## 约束

* 概念：规定xml文档的书写规则
  * 在xml中引入约束文档
  * 读懂约束文档
* 分类：
  1. DTD：一种简单的约束技术
  2. Schema：一种复杂的约束技术

### DTD

* 引入dtd文档到xml文档中：

  * 内部dtd：将约束规则定义在xml文档中（较少使用）

  ```xml
  <!DOCTYPE students [
  		<!ELEMENT students (student*) >
  		<!ELEMENT student (name,age,sex)>
  		<!ELEMENT name (#PCDATA)>
  		<!ELEMENT age (#PCDATA)>
  		<!ELEMENT sex (#PCDATA)>
  		<!ATTLIST student number ID #REQUIRED>
  		]>
  ```

  * 外部dtd：将约束的规则定义在外部的dtd文件中
    * 本地：&lt;!DOCTYPE 根标签名 SYSTEM "dtd文件的位置"&gt;
    * 网络：&lt;!DOCTYPE 根标签名 PUBLIC "dtd文件名字" "dtd文件的位置URL"&gt;

### Schema

* 引入过程

  1. 填写xml文档的根元素

  2. 引入xsi前缀.  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

  3. 引入xsd文件命名空间.  xsi:schemaLocation="命名空间url  文件名.xsd"

  4. 为每一个xsd约束声明一个前缀,作为标识  xmlns:前缀="命名空间"



## 解析

* 概念：操作xml文档，将文档中的数据读取到内存中
  * 操作xml文档
    1. **解析**(读取)：将文档中的数据读取当内存中
    2. 写入：将内存中的数据保存到xml文档中
  * 解析xml方式：
    1. **DOM**：将标记语言文档一次性加载进内存，形成一棵DOM树
       * 优点：操作方便，可以对文件进行CRUD所有操作
       * 缺点：占内存
    2. **SAX**：逐行读取，基于事件驱动
       * 优点：不占内存
       * 缺点：只能**读取**，不能增删改
* xml常见解析器：
  1. JAXP：Sun公司提供的解析器，支持dom和sax两种思想
  2. DOM4J：一款非常优秀的解析器
  3. Jsoup：jsoup 是一款Java 的HTML解析器，可直接解析某个URL地址、HTML文本内容。它提供了一套非常省力的API，可通过DOM，CSS以及类似于jQuery的操作方法来取出和操作数据。
  4. PULL：Android内置的解析器，sax方式

### Jsoup解析器

* 步骤：
  1. 导入jar包
  2. 获取Document对象
  3. 获取对应的标签Element对象
  4. 获取数据

<center>示例</center>

```java
public class jsoupDemo1 {
    public static void main(String[] args) throws IOException {
        //2.获取Document对象，根据xml文档获取
        //2.1获取student.xml的path
        String path = jsoupDemo1.class.getClassLoader().getResource("student.xml").getPath();
        //2.2解析xml文档，加载进内训，获取dom树
        Document document = Jsoup.parse(new File(path), "utf-8");
        //3.获取元素对象 Element对象
        Elements elements = document.getElementsByTag("name");

        //3.1获取第一个name的Element对象
        Element element = elements.get(0);
        //3.2获取数据
        String name = element.text();
        System.out.println(name);
    }
}
```



* 对象的使用：

  1. **Jsoup**：工具类，可以解析html或xml文档，返回Document

     * parse：解析html或xml文档，返回Document

       * **parse**(File in, String charsetName)：解析xml或html文件
       * **parse**(String html)：解析xml或html字符串，参数为html或xml代码
       * **parse**(URL url, int timeoutMillis)：通过网络路径获取指定的文档对象

       ```java
       URL url = new URL("https://网址");
       ```

       

  2. **Document**：文档对象。代表内存中的dom树
     * 获取Element对象
       * getElementById(String id)：根据id属性值获取唯一的元素对象
       * **getElementsByTag**(**String** **TagName**)：根据标签名称获取元素对象集合
       * getElementsByAttribute(String key)：根据属性名称获取元素对象集合
       * getElementsByAttributeValue(String key, String value)：根据对应的属性名和属性值来获取元素对象

  3. **Elements**：元素Element对象的a't'h集合。可以当作ArrayList&lt;Elements&gt;来使用
  4. **Element**：元素对象
     1. 获取子元素对象
        - getElementById(String id)：根据id属性值获取唯一的元素对象
        - **getElementsByTag**(**String** **TagName**)：根据标签名称获取元素对象集合
        - getElementsByAttribute(String key)：根据属性名称获取元素对象集合
        - getElementsByAttributeValue(String key, String value)：根据对应的属性名和属性值来获取元素对象
     2. 获取属性值
        * String attr(String key)：根据属性名称获取属性值
     3. 获取文本内容
        * String text()：获取文本内容
        * String html()：获取标签体的所有内容（包括子标签的字符串内容）
  5. **Node**：节点对象
     * 是Document和Element的父类



* 快捷查询方式：

  1. selector：选择器

     * 方法：Elements select(String cssQuery)
       * 语法：参考Selector类中定义的语法

  2. XPath：**XPath**即为**XML路径语言**（XML Path Language），它是一种用来确定XML文档中某部分位置的语言。

     * 使用Jsoup的Xpath需要额外导入jar包

     ```java
         //3.根据document对象，创建JXDocument对象
             JXDocument jxDocument = new JXDocument(document);
     
             //4.结合xpath语法查询（具体查看文档）
             List<JXNode> jxNodes = jxDocument.selN("//student");
     ```

     