---
title: Web服务器软件：Tomcat
date: 2019-08-19 16:13:58
tags: 
- Javaweb
- 学习
- 服务器
- Tomcat
categories: JavaWeb
---

## Web相关概念

1. 软件架构
   1. **C/S**：客户端/服务器端
   2. **B/S**：浏览器/服务器端
2. 资源分类
   1. **静态资源**：所有用户访问后，得到的结果都一样，静态资源可以直接被浏览器解析
      * 如：html，css，JavaScript
   2. **动态资源**：每一个用户访问相同资源后得到的结果可能不一样。动态资源被访问后，需要先转换为静态资源，再返回给浏览器
      * 如：servlet/jsp，php，asp...
3. 网络通信三要素
   1. **IP**：电子设备在网络中的唯一标识
   2. **端口**：应用程序在计算机中的唯一标识。0~65536
   3. **传输协议**：规定了数据传输的规则
      1. 基础协议：
         1. **tcp**：安全协议，三次握手。速度稍慢
         2. **udp**：不安全协议。速度快



## Web服务器软件

* 服务器：安装了服务器软件的计算机
* 服务器软件：接收用户请求，处理请求，做出响应
* web服务器软件：接收用户请求，处理请求，做出响应
  * 在Web服务器软件中，可以部署web项目，让用户通过浏览器来访问
  * web容器



* 常见的java相关的Web服务器软件：
  * webLogic：Oracle公司，大型JavaEE服务器，支持所有JavaEE规范，收费。
  * webSphere：IBM公司，大型JavaEE服务器，支持所有JavaEE规范，收费。
  * JBOSS：JBOSS公司，大型JavaEE服务器，支持所有JavaEE规范，收费。
  * Tomcat：Apache基金组织，中小型JavaEE服务器，仅仅支持少量JavaEE规范，开源的，免费的。
* JavaEE：Java语言在企业级开发中使用的技术规范的总和，一共规定了13项大的规范



## Tomcat

1. 下载：[http://tomcat.apache.org/](http://tomcat.apache.org/)

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190819163625.png)

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190819163747.png)



2. 安装：解压压缩包即可
   * **注意**：安装目录建议不要有中文
3. 卸载：删除目录即可
4. 启动

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/QQ截图20190821110351.png)

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190821110902.png)

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190821111428.png)

* 可能遇到的问题：

  1. 黑窗口一闪而过：

     * 原因：没有正确配置JAVA_HOME环境变量
     * 解决方案：正确配置JAVA_HOME环境变量即可（在环境变量中新建JAVA_HOME环境变量，再将path中的jdk路径修改为%JAVA_HOME%\bin）

  2. 启动报错

     1. 暴力：找到占用的端口号，并找到对应的进程，杀死该进程

        * netstat -ano：在列表中找到对应端口号的PID

          ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190821112940.png)

        * 使任务管理器显示PID

          * win7：任务管理器-->查看-->选择列-->勾选PID

            ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190821113252.png)

          * win10：在名称栏处右键，勾选PID

            ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/无标题.png)

        * 关闭相应进程

     2. 温柔：修改自身端口号

        * 安装目录/conf/server.xml，编辑该文件

        ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190821113904.png)

        注意该文件中的其他端口号也要改，如redirectPort等

        * 一般会将tomcat的默认端口号修改为80。80端口是http协议的默认端口号，访问时可以不用输入端口号

5. 关闭
   * 正常关闭：
     * bin\shutdown.bat
     * 在启动窗口按ctrl+c
   * 强制关闭：
     * 点击启动窗口的×

6. 配置
   * 部署项目的方式：
     1. 直接将项目放到webapps文件夹下即可（localhost\项目名称\资源文件 可以直接访问）
        * \项目名称：项目的访问路径-->虚拟目录
        * 简化方式：将项目打包成一个war包，再将war包放置在webapps目录下。
          * 在webapps目录下，war包会自动解压
          * 可以通过将项目压缩为zip文件再改后缀名得到war包，也可通过专门的war打包软件获得
     2. /conf/server.xml文件的Host标签提中添加配置
     
     ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190821144851.png)
     
     3. 在\conf\Catalina\localhost文件夹下新建deploy.xml（名字随意），添加如下内容
     
        ```xml
        <Context docBase="D:\hello" />
        ```
     
        此时的虚拟目录：xml文件的名称

* 静态项目和动态项目

  * 目录结构

    * 静态项目放置静态资源

    * java动态项目的目录结构：

      -- 项目的根目录

      ​	-- WEB-INF目录：

      ​		-- web.xml：web项目的核心配置文件

      ​		-- classes目录：防止字节码文件的目录

      ​		-- lib目录：防止依赖的jar包



## Tomcat集成到IDEA中

1. 打开IDEA

2. Run-->Edit Configurations

   ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190821150338.png)

3. 选择Template-->Tomcat Server-->Local-->Configure..

   ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190821150548.png)

4. 选择自己的Tomcat目录

   ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190821150650.png)

5. 完成配置

6. 创建JavaWeb项目

   ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190821151107.png)