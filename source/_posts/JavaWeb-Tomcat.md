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

5. 关闭

6. 配置