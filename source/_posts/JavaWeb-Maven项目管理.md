---
title: Maven项目管理
date: 2019-09-02 08:41:02
tags: 
- Javaweb
- 学习
- 项目管理
categories: JavaWeb
---

## 基础

* maven开发的项目，jar包不在项目中，项目保存的是jar包的坐标，jar包实际放在jar包仓库中。

* **依赖管理**：maven工程对jar包的管理过程



* **构建**项目：指的是从编译、测试、运行、打包、安装、部署整个过程都交给maven进行管理。



## 下载和安装

1. 下载地址：[http://mirror.bit.edu.cn/apache/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.zip](http://mirror.bit.edu.cn/apache/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.zip)

2. 安装：解压即可

3. /conf/settings.xml用来配置maven的文件

4. 配置环境变量

   1. 系统变量中新建值

      ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190902085316.png)

   2. path中新建值，输入%MAVEN_HOME%\bin(**注意**：必须确保环境中已经配置了JAVA_HOME)

      ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190902085640.png)

      ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190902085754.png)

   3. 确认是否配置完成：命令行中输入mvn -v

      ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190902085901.png)



## maven仓库

1. 打开./conf/settings.xml

2. 可以看到默认位置，maven会在系统盘找本地仓库，如果本地仓库没有，它在联网状态下会在中央仓库下载（若未联网，则会报错）

   ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190902090126.png)

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/maven仓库的种类和关系.png)



3. 修改仓库位置：在./conf/settings.xml中添加如下代码

   ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190902090810.png)

   



* maven**标准目录结构**
  1. 项目组成
     * 核心代码部分
     * 配置文件部分
     * 测试代码部分
     * 测试配置文件
  2. 标准目录结构
     * src/main/java 核心代码
     * src/main/resources 配置文件
     * src/test/java 测试代码
     * src/test/resources 测试配置文件
     * src/main/webapp 页面资源，js、css、图片等



## 使用

1. 先进入项目目录
2. 输入命令进行操作



* 命令：
  1. mvn clean：删除target目录，即删除已经编译好的内容
  2. mvn compile：将**核心代码**文件编译为class文件
  3. mvn test：将**核心代码**和**测试代码**编译为class文件
  4. mvn package：将代码全部编译并打包成war包(可在settings.xml中进行配置)
  5. mvn install：将代码全部编译并打包成war包再将这个包安装到本地操作



* 概念模型图：

  ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/maven概念模型图.png)



### IDEA集成maven

1. 打开设置，搜索maven

   ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190902093449.png)

2. 选择好maven的安装目录

   ![1567388162936](C:\Users\Nier\AppData\Roaming\Typora\typora-user-images\1567388162936.png)

3. 配置settings.xml和本地仓库的路径

   ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190902093947.png)

4. 配置Runner

   ![1567388558494](C:\Users\Nier\AppData\Roaming\Typora\typora-user-images\1567388558494.png)



### 使用骨架创建maven项目

1. 创建Maven项目，选择quickstart骨架

   ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190902094533.png)

   ![1567388810743](C:\Users\Nier\AppData\Roaming\Typora\typora-user-images\1567388810743.png)

2. 配置好后点击相应的Mark Directory As...



### 导入jar包

1. 打开pom文件
2. 写入&lt;dependencies>标签
3. ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190902101601.png)



* 如果本地没有该jar包

  1. 搜索maven中央仓库网页，查找需要的jar包

     ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190902101903.png)

  2. 点击相应版本，复制其坐标到pom.xml

     ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190902101957.png)



### 解决jar包冲突

* 在&lt;version>标签下，添加&lt;scope>标签改变其作用域
  * 内容：
    * provided：该jar包只在编译过程中起作用
    * test：该jar包只在测试过程中起作用