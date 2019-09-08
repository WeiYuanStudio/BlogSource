---
title: Java Web Servlet 笔记
date: 2019-09-07 18:56:00
thumbnail:
categories:
 - 笔记
 - Java
tags:
 - Java
 - JavaEE
 - Servlet
---

这学期的语言课程只有**Java课程设计**了，现在还没上第一节课，但是根据同学的反馈来看，貌似是开发小项目，选择两个课题进行开发。（估计是放养自学，两星期一节课，差评！这学期专业相关的课貌似有点少啊

从题目来看大多是GUI程序，要求菜单完成，大概再下个学期就是Java Web开发了吧。所以先入手Java Web吧，作业就拿Web完成，用Java开发GUI程序真的有点尴尬，我觉得Java相比起来还是适合开发Web端，桌面端还是C#来吧。术业有专攻。再说了，如果未来做Java开发的话，进企业后，大概率还是去做Web开发吧。

去图书馆借了本一看名字就很“实用”的书。于是就有了下面的故事

感想：
1. vim等一众editor固然是仙器，但是让你做一遍J2EE Web开发，仙都搞不定好吗，那万恶的web.xml配置文件，谁来试试背这个模板，还有，maven等项目构建工具都还没上，再加上近几年前端也走向工具构建，什么webpack啊，Orz，看又看不懂，敢随便动配置文件分分钟炸毛给你看 ~~虽然说到企业也很大可能只是照着别人画好的框架填代码，根本没有碰这些东西的机会就是了~~

2. 体会到了之前大佬们的说法。随着计算机学科的壮大，越来越细的分工，底层的封装。开发时越是看不到底层，越是迷迷糊糊。各种令人头晕目眩的工具链，构建工具，脚手架。开发时修改配置文件无从下手，遇到问题很难自己解决。

<!--more-->

# 安装环境

安装JDK以及IDE（本人选用IDEA EDU授权版本，支持JavaEE开发），服务端选择TomCat9

## Windows安装Tomcat9
TomCat用的是Choco包管理器安装。安装完毕后，已经自动配置了环境环境变量。命令行`tomcat9`就可以启动了。

安装完毕后**tomcat.exe**位于choco的bin文件夹中，其他文件位于Windows隐藏的的系统盘文件夹ProgramData中

 - conf 文件夹中存放的是xml配置文件
    - server.xml **最重要**配置端口信息，站点信息等
    - tomcat-users.xml 配置tomcat面板登录用户
    - context.xml 配置xml存放路径
    - web.xml 配置servlet，定向url到具体servl class资源
 - logs 存放log
 - temp
 - webapps 目录下存放多个站点文件夹，通过`ip:8080/文件夹名`即可访问
 - work 貌似是运行时jsp编译的缓存文件夹，大概是 jsp -> java -> class?

# 使用IDE构建项目

在IDE中选择
 - JavaEE版本
 - SDK 版本
 - 服务端类型（选择了Tomcat9，打开选项的时候，整个人都不好了，什么JBoss，Glassfish Spring，还有一堆听都没听说过的Orz
 - Lib & Framework （表示就勾了一个Web Application

一步一步接着走IDE就帮你构建好一个项目了。

# 配置web.xml以路由url

如下是IDE自动构建的web.xml文件，位于WEB-INF目录中
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
</web-app>
```
如果需要将一个url路由到某个继承了Servlet的类的话，可以在这个xml文件中的**web-app**tag中加入**servlet**和**servlet-mapping**tag，IDE貌似也有相关的向导可以自动添加tag

示范如下
```xml
<servlet>
    <servlet-name>UserPage</servlet-name>
    <servlet-class>club.piclight.JavaWeb.UserPage</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>UserPage</servlet-name>
    <url-pattern>/user</url-pattern>
</servlet-mapping>
```
从Java EE5开始，还可以为一个servlet-name设置多个url了

当访问`http://host.com/user`这样的url时，就会路由到club.piclight.JavaWeb.UserPage这个已经继承了Servlet的类

在我写这一段的时候，我发现还有一个不需要修改web.xml配置文件以达到mapping的方法————在继承了Servlet的类中class声明前加入**WebServlet注解**，例如`@WebServlet(name = "UserPage", urlPatterns = "/user")`这样。当然还有很多选项，IDE会很方便的给出提示。

# 配置web.xml中init-param的以配置参数

在文件web.xml -> web.app(tag) -> servlet(tag)中添加init-param(tag)

示范
```xml
<init-param>
    <param-name>adminName</param-name>
    <param-value>Linus</param-value>
</init-param>
```
接着在Servlet类中使用`getInitParameter("adminName")`即可得到Linus(value)。**注意，这个是配置文件的参数，与HTTP网络请求的参数没有关系** ~~被书误导了~~

# 配置web.xml中context-param以达到全局效果

由于init-param tag内嵌于servlet tag,所以仅能被当前servlet访问，如果想做到全局访问，可以使用web.app(tag) -> context-param(tag)

示范
```xml
<context-param>
    <param-name>adminName</param-name>
    <param-value>Linux</param-value>
</context-param>
```
这时候想要访问该参数需要在`getInitParameter("adminName")`修改为`getServletContext().getInitParameter("adminName")`

# 资源注射(@Resource)

该项注解功能在我的Windows开发环境下无法正常使用编译，暂时放一放。使用了资源注入，Tomecat启动时会将web.xml里面的配置信息主动注射到Servlet里。

*Java EE 5与 Tomcat6 以上才支持注解*

例如
```Java
@Resource(name="messageName")
private String message;

OR

private @Resource(name="messageName") String message;
```
```xml
    <env-entry>
        <env-entry-name>messageName</env-entry-name>
        <env-entry-type>String</env-entry-type>
        <env-entry-value>Hello World</env-entry-value>
    </env-entry>
```

资源注射的原理是JNDI，（Java命名与目录接口）如果不使用注解注入的话，还可以这样获取到这三个资源
示范
```Java
Context context = new InitialContext();
String message = (String)context.lookup.("messageName")
```

实验用例
```Java
Context context = null;
String infoString = null;
try {
   context = new InitialContext();
   infoString = (String) context.lookup("UserInfo");
} catch (NamingException e) {
   e.printStackTrace();
}
out.println("Info:" + infoString);
```
> 实验失败，Tomcat命令行log: javax.naming.NameNotFoundException: Name [UserInfo] is not bound in this Context. Unable to find [UserInfo]. 页面为null

# HTTP请求

## GET Method

以下是一个简单的表单请求的html示范
```html
<form action="/url" method="get">
    <input type="text" name="info">
    <input type="submit">
</form>
```

在servlet类中调用HttpServletResponse对象的getParameter("key")方法会返回GET请求中key所对应的value
以下为servlet类中调用GET请求的参数示范
```Java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html");

        PrintWriter out = resp.getWriter();
        out.println("Info=" + req.getParameter("info"));
    }
```

action为请求URL参数，实际请求时会请求至`主域 + /url`这样子，method为请求方式，name中的info作为post请求的键值名(key)，与input text框中的数据作为数据(value)一起传送。以GET方法提交数据时，浏览器会把表单内容组织成一个查询字符串并加载URL+?后面。各个变量之间以&分隔，GET请求适用于查询。

## POST Method

与GET Method十分相似

以下是一个简单的表单请求的html示范
```html
<form action="/url" method="post">
    <input type="text" name="infoA">
    <input type="text" name="infoB">
    <input type="submit">
</form>
```

在servlet类中调用HttpServletResponse对象的getParameter("key")方法会返回GET请求中key所对应的value
以下为servlet类中调用POST请求的参数示范
```Java
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html");

        PrintWriter out = resp.getWriter();
        out.println("Info=" + req.getParameter("infoA"));
        out.println("Info=" + req.getParameter("infoB"));
    }
```


## Encoding编码问题

如果查询内容中有中文字符，务必注意Tomcat上Encoding的设置。如果出现意外，最好在server.xml将Encoding设置为UTF-8

```xml
<Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectProt="8443" URIEncoding="UTF-8"/>
```

# 关于Servlet生命周期的简单讨论

在CGI编程中，用户每一次请求CGI程序时，都会开辟一个独立的进程处理该请求，完成请求后销毁。效率相对较低

Servlet解决了这个问题，服务器会在启动时（若load-on-startup为1）或者是用户第一次请求时（若load-on-startup为0）才初始化一个Servlet对象。当服务器关闭时销毁这个Servlet对象。

load-on-startup (tag)，位于web.xml -> web-app(tag) -> servlet(tag)中

**init(ServletConfig config)** 当第一次加载Servlet时会调用该方法

**Service(ServletRequest req, ServletResponse resp)** 客户端每次请求Servlet时都会调用该方法，该方法会判断访问类型，根据HttpServletRequest的getMethod()方法来判断执行doGet()还是doPost()方法

**destory()** 当Servlet要结束生命周期时会执行，容器关闭时执行

根据上文，可以将初始化资源放入init()方法中，销毁资源放入destory()方法中，以提高性能。

## 生命期注解

从Java EE 5 开始支持@PostConstruct与@PreDestroy注解，这两个注解可以修饰一个**非静态方法**的**void**方法，而且该方法**不能有抛出异常声明**

**PostConstruct注解** 标注的方法运行于Servlet构造方法之后，init()方法之前。

**PreDestroy注解** 标注的方法运行于destory()方法之后，之后服务器卸载服务完毕

# Servlet的转发与重定向

开始之间，先学习一下容易弄混的两个名词

>**forward（转发）：**
>
>是服务器请求资源,服务器直接访问目标地址的URL,把那个URL的响应内容读取过来,然后把这些内容再发给浏览器.浏览器根本不知道服务器发送的内容从哪里来的,因为这个跳转过程实在服务器实现的，并不是在客户端实现的所以客户端并不知道这个跳转动作，所以它的地址栏还是原来的地址  
>**redirect（重定向）：**
>
>是服务端根据逻辑,发送一个状态码（比如302（临时重定向，301永久重定向））,告诉浏览器重新去请求那个地址.所以地址栏显示的是新的URL.

简单地说，**转发**仅仅是是**服务端的行为，客户端不可见、透明**，而**重定向是客户端执行**的行为。

放置在每个站点项目下的WEB-INF中的文件是受到保护的，如果在该目录中有需要让外部访问的文件。可以使用Servlet的Forward，Forward不仅可以转发到Servlet, JSP文件，还可以转发至html文件，甚至是WEB-INF下的文件

## Forward 转发

**Forward**是通过**RequestDispatched**对象的**forward(ServletRequest request, ServletResponse response)**方法实现的。RequestDispatched对象可以通过HttpServletRequest的getRequestDispatched得到

代码示范 当请求键值target的参数为welcome时，重定向到站点目录下的WEB-INF下的welcome.html

```Java
if (req.getParameter("target").equals("welcome")) {
            RequestDispatcher dispatcher = req.getRequestDispatcher("/WEB-INF/welcome.html");
            dispatcher.forward(req, resp);
}
```
在写这一段代码的时候，我注意到一个问题。当前开发环境是Windows环境，路径`/WEB-INF/welcome.html`是不符合Windows的文件系统的`\`反斜杠的。但是部署到服务器上，服务器可能是Linux系统，那么到底应该怎么写才是规范的呢？我尝试了`/WEB-INF/welcome.html`以及`\\WEB-INF\\welcome.html`（注意转义字符问题，需要双反斜杠）。重新编译之后，发现都能在Windows环境下的Tomcat运行，并正常相应请求。

思考了一下，因为主流部署平台以及tomcat的开源特性以及书写时转移符反斜杠带来的麻烦问题，个人还是觉得不要使用反斜杠

## Redirect 重定向
