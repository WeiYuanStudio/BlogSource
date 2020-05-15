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

Spring boot 的学习笔记

## Spring Initializ

俗话说的好，

> 工欲善其事，必先利其器

这次一次性把Spring boot 2.2.6 项目初始化的依赖过一遍。以下是我个人一点点翻看写的，可能会有疏漏，欢迎提出您宝贵的建议

<!--more-->

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
        这个可以自动根据数据库模型建立一个REST接口，方便，但是可塑性不强，我第一次用的时候，以为这个是用来做RESTful接口的，根本不知道怎么修改。栽跟头在上面，如果是做Web开发，而不是一些简单的RESTful接口对数据库CRUD的话，请到上面找Spring Web，他有RESTfulController
    - Spring Session
        Spring 框架提供的session，可以接入Redis，解决分布式服务共享Session的问题
    - Rest Repositories HAL Browser
        REST Repositories附带一个HAL Browswer，方便在浏览器上直接操作Rest Repositories接口，有Django内味了
    - Spring HATEOAS
        REST HATEOAS规范的接口，Hypermedia API的设计被称为HATEOAS，访问api时，返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。暂时用不上
    - Spring WebServices
        提供SOAP服务，简而言之就是提供XML格式的HTTP接口
    - Jersey
        提供RESTful接口
    - Vaadin
        Web UI 库，商业库 <https://vaadin.com> 我尝试看了下他们的Demo，可以做到基本上不用写Web前端代码（HTML/CSS/JS），通过Java代码实现前端，就像Java Swing那样。虽说效果挺炫酷的，暂时没有需求。
 - Template Engines
    下面的都是模版技术栈，如果做前后端解耦的，可以直接全部略过。
    - Thymeleaf
        这个好像是Spring项目中用的最多的模版技术吧
    - Apache Freemarker
    - Mustache
    - Groovy Templates
 - Security
    安全部分
    - Spring Security
        Spring安全认证，可以拦截请求，做登录
    - OAuth2 Client
        OAuth 客户端
    - OAuth2 Resource Server
        OAuth 资源服务
    - Spring LDAP
        LDAP轻型目录访问协议
    - Okta
        Okta第三方认证服务
 - SQL
    - JDBC API
        较原始的JDBC，需要自己写SQL语句。
    - Spring Data JPA
        JPA，基于Hibernate。使用方法名即可访问数据库。十分便捷

---
**参考资料**

[实战Spring Boot 2.0 Reactive编程系列 - WebFlux初体验 By 零壹技术栈lv-4 @ 掘金](https://juejin.im/post/5b3a24386fb9a024ed75ab36)

[Spring 官方](https://spring.io)
