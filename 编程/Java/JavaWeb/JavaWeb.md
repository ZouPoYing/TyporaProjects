# JavaWeb

## JavaWeb概述

### 前言

web开发：

- web，网页的意思
- 静态web
  - 提供给所有人看的数据始终不会发生变化！
- 动态web
  - 提供给所有人看的数据始终会发生变化，每个人在不同的时间，不同的地点看到的信息各不相同！

在Java中，动态web资源开发的技术统称为JavaWeb

### 静态web

- *.html,这些都是网页的后缀，如果服务器上一直存在这些东西，我们就可以直接进行读取。通络；

![1567822802516](https://cdn.jsdelivr.net/gh/oddfar/static/img/JavaWeb.assets/1567822802516.png)

- 静态web存在的缺点
  - 它无法和数据库交互（数据无法持久化，用户无法交互）

### 动态web

页面会动态展示： Web的页面展示的效果因人而异

![1567823191289](https://cdn.jsdelivr.net/gh/oddfar/static/img/JavaWeb.assets/1567823191289.png)

缺点：

- 加入服务器的动态web资源出现了错误，我们需要重新编写我们的后台程序，重新发布，停机维护

优点：

- Web页面可以动态更新，所有用户看到都不是同一个页面
- 它可以与数据库交互 （数据持久化：注册，商品信息，用户信息........）

![1567823350584](https://cdn.jsdelivr.net/gh/oddfar/static/img/JavaWeb.assets/1567823350584.png)

## web服务器

### 技术发展

**ASP**

**php**

- PHP开发速度很快，功能很强大，跨平台，代码很简单 （70% , WP）
- 无法承载大访问量的情况（局限性）

**JSP/Servlet : **

B/S：浏览和服务器

C/S: 客户端和服务器

- sun公司主推的B/S架构
- 基于Java语言的 (所有的大公司，或者一些开源的组件，都是用Java写的)
- 可以承载三高问题带来的影响；
- 语法像ASP ， ASP-->JSP , 加强市场强度

### web服务器

服务器是一种被动的操作，用来处理用户的一些请求和给用户一些响应信息

**Tomcat**

Tomcat是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，最新的Servlet 和JSP 规范总是能在Tomcat 中得到体现，因为Tomcat 技术先进、性能稳定，而且**免费**，因而深受Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web 应用服务器。

Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用[服务器 (opens new window)](https://baike.baidu.com/item/服务器)，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选。对于一个Java初学web的人来说，它是最佳的选择

Tomcat 实际上运行JSP 页面和Servlet