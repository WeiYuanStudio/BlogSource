---
title: Spring 学习笔记
thumbnail:
date: 2020-4-26 20:00:00
categories:
 - 笔记
 - Java
tags:
 - Java
 - Spring boot
---

## Spring Initializ

俗话说的好，

> 工欲善其事，必先利其器

这次一次性把Spring boot项目初始化的依赖过一遍。以下是我个人一点点翻看写的，可能会有疏漏，欢迎发送邮件，或者在我的博客Github仓库中的issue提出您宝贵的建议

 - Developer Tools
    - Spring Boot DevTools
        Spring boot开发工具，顾名思义，功能方面主要是热模块更新，使得修改代码之后，不用等待漫长的Spring冷启动过程
    - Lombok
        懒人必备，什么你还在用IDE生成冗长的getter/setter？赶紧试试这个吧，`@Data`注解可以在编译时自动生成全部的getter/setter，还有自动构造生成哦，记得打上插件，不然IDE无法识别报错
    - Spring Configuration Processor
        配置文件处理器，可以很方便的将配置文件中的值，通过注解注入到类中
 - Web
    - Spring Web
        Web项目必备，还支持RESTful接口哦，使用Spring MVC，默认使用tomcat作为web容器
    - Spring Reactive Web
        没看出什么名堂，响应式编程。暂时用不到，看帖子说是适合业务处理非常耗时的场景，这时Servlet可以将其委托给另一个线程，然后去接受新的请求。
    - Rest Repositories
        这个可以自动根据数据库模型建立一个Rest接口，方便，但是可塑性不强，我第一次用的时候，以为这个是用来做RESTful接口的，根本不知道怎么修改。栽跟头在上面，如果是做Web开发，而不是一些简单的RESTful接口对数据库CRUD的话，请到上面找Spring Web，他有RESTfulController
    - Spring Session
        Spring 框架提供的session，可以接入Redis，解决分布式服务共享Session的问题
    - Rest Repositories HAL Browser
        Rest Repositories附带一个HAL Browswer，方便在浏览器上直接操作Rest Repositories接口，有Django内味了
    - Spring HATEOAS

---
参考资料：

[实战Spring Boot 2.0 Reactive编程系列 - WebFlux初体验 By 零壹技术栈lv-4 @ 掘金](https://juejin.im/post/5b3a24386fb9a024ed75ab36)
