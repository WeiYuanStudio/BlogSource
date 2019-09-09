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

安装JDK以及IDE（本人选用IDEA EDU授权版本，支持JavaEE开发），服务端选择TomCat9。调试工具方面，推荐使用 Firefox 或者 Google Chrome浏览器，当然一个HTTP调试工具也是十分需要的，这里推荐使用 PostMan。

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

在IDEA中，项目结构是这样的

- out
- src
  - club.piclight.JavaWeb
- web
  - WEB-INF
    - web.xml
  - index.jsp

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

该项注解功能在我的Windows开发环境下无法正常使用编译，暂时放一放。使用了资源注入，Tomecat启动时会将web.xml里面的配置信息主动注射到Servlet里。*Java EE 5与 Tomcat6 以上才支持注解*

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

重定向是利用服务器返回状态码来实现的。301和302分别是永久与临时重定向。

代码示范

```Java
repsonse.setStatus(HttpServletResponse.SC_MOVED_TEMPORARLIRY);
response.setHeader("Location", "https://RedirectURL");
```

更多的玩法，可以参考HTTP协议。各种状态码和Header的组合

## Servlet线程安全

Servlet只会有**一个实例**，如果有多个用户同时请求同一个Servlet的时候，Tomcat派生出多条线程执行Servlet的代码。

Servlet不是线程安全的，多线程的**并发读写**会导致数据的**不同步**问题。解决方式是尽量**不要定义name属性于类**中，定义在doGet()或者doPost()方法中。

虽然使用synchronized(name){}可以解决问题，但是会造成**线程的等待，阻塞**

# JSP介绍

JSP，全名Java Server Page, JSP在执行是会被Tomcat编译，经过观察，发现启动Tomcat后可以找到相应的JSP解释后的.java文件，还有编译后的.class文件

使用IDEA创建了一个JSP文件到web目录下

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

</body>
</html>
```

> 在EE 5 规范中，当整个Web应用只有html和jsp文件时，部署时可以不需要web.xml。Tomcat6 中就不再需要web.xml。

关于JSP的生命周期，JSP也是Servlet，运行时和Servlet一样只有一个实例，也有init()和destroy()方法。除此之外，JSP还有自己的_jspInit()和_jspDestory()方法

JSP源码可以分为两部分，**模板数据**与**元素**。**模板**就是指JSP中的HTML代码，无论如何都不会改变输出。**元素**指的是JSP中的Java部分，包括脚本（Scriptlet，也就是JSP中的Java代码），JSP指令(Direcitive), 以及JSP标签

## JSP语法

### JSP脚本

JSP脚本必须使用`<%`和`%>`包围，否则视为模板数据。中间部分的语法必须符合Java语法，否则会发生编译错误。

### JSP输出

在Java Servlet中使用out.println()方法进行输出，在JSP中不仅可以使用这种方法进行输出，还可以使用JSP的输出语法

```jsp
<%= num %>
```

### JSP注释

在JSP中不仅可以使用Java注释，还可以使用JSP注释语法

```jsp
<%--

--%>
```

### JSP全局变量

JSP可以声明全局变量，以及方法。但是不能直接在`<%`和`%>`中之间声明。`<%!`和`%>`

### JSP的if，for语句

JSP中同样可以使用if语句。if语句中可以包含大段的HTML代码，如果if，for语句块中包含HTML。则语句块前后的大括号绝对不能省略。

### JSP的return

如果遇到JSP页面需要中途停止，可以直接调用return结束该页的输出。

### JSP指令

JSP指令用于声明JSP页面中的属性，格式为`<%@ directive {attribute=value} %>`

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```

该指令中directive位置为page，因此该指令为page指令。该指令包含language和contentType两个属性，注意，**import属性除外，任何page允许的属性都只能出现一次**，否则会出现编译错误。

常见的指令还有page,taglib,include等等

|属性|描述|
|--- |--- |
|buffer|指定out对象使用缓冲区的大小|
|autoFlush|控制out对象的 缓存区|
|contentType|指定当前JSP页面的MIME类型和字符编码|
|errorPage|指定当JSP页面发生异常时需要转向的错误处理页面|
|isErrorPage|指定当前页面是否可以作为另一个JSP页面的错误处理页面|
|extends|指定servlet从哪一个类继承|
|import|导入要使用的Java类|
|info|定义JSP页面的描述信息|
|isThreadSafe|指定对JSP页面的访问是否为线程安全|
|language|定义JSP页面所用的脚本语言，默认是Java|
|session|指定JSP页面是否使用session|
|isELIgnored|指定是否执行EL表达式|
|isScriptingEnabled|确定脚本元素能否被使用|

从runoob抄了一个Table过来~ ~~Turn HTML Table to Markdown真的好用，逃~~

### JSP的include指令

include指令只有一种格式。

```jsp
<%@ include file="relativeURL"%>
```

例如需要为页面增加统一的页眉页脚，即可用这种方式添加。

还有一种方法实现同样的效果，include行为，它使用request.getRequestDispatcher("URI").forward(requst, response)来执行被包含文件

```jsp
<jsp:include page="relativeURI">
```

这两者的不同之处是，前者是在编译的时候将include的页面编译到一起。而后者是分开编译，执行时将结果合并

**include指令是先包含后编译，include行为是先运行后包含。**

### JSP的taglib指令

JSP标签技术，使用标签功能实现视图代码重用。

```jsp
<%@ taglib uri="类库地址" prefix="prefixOfTag" %>
```

#### 关于JSP指令前后导致的网页换行问题

JSP2.1加入了新属性**trimDirectiveWhitespaces** 默认值为false。若开启，则会自动忽略JSP前后带来的空白字符

尽管HTML中空行不影响浏览器里的显示效果。浏览器依然能解析页面，但是如果输出的是XML，可能会出现某些问题，**有的XML解析器不允许空行的出现**。

# JSP行为

标准的JSP行为格式为`<jsp:elements {attribute="value"}* />`



### 记录我遇到的问题

不推荐在JSP页面试图使用response的writer，或者outputStream。会出现谜之问题

使用outputStream输出会可能导致这个页面只有outputSteam才能得以输出。我尝试调用了outputStream，发现这会导致页面只有outputStream才有输出。其他的输出全部消失了。

然后尝试去get response的Writer进行输出，发现我放在页面中部的printTable方法所输出的HTTP内容居然比整个JSP页面内的其他模板数据还要早出现。但是查看编译出来的java文件，编译并没有改变JSP内的顺序。我调用的是response里get到的Writer

根据编译出的.java文件配合调试工具，可以看出，页面中只允许一个outputStream或者其他的输出实例。如果出现两个实例的话。将只有最先被调用的那个实例才得以输出。模板数据是通过pageContext里get到的out进行输出的。

```text
_jspxFactory.getPageContext -> pageContext.getOut -> out
```
