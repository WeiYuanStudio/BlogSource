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

## Linux部署Tomcat9

这里使用我个人最喜欢在服务器上部署的Linux发行版debian

首先输入命令`apt install tomcat9`

完成安装后将启动服务加入daemon中`systemctl enable tomcat9`，之后重启

此时访问ip:8080应该可以看到面板启动成功。

然后可以将已经打包好的war文件放到站点目录下，默认装完tomcat后，是放置在`/var/lib/tomcat9/webapps/ROOT/index.html`下的。如果你将打包好的Project.war放到`/var/lib/tomcat9/webapps/`下，重启服务，你会看到项目被自动解压到该目录下了，不过这种带项目名方式，得通过访问`ip:8080/Project`这样的方式进行访问。聪明的方法是将该目录清空，将.war打包文件改为ROOT.war，部署到服务器后重启一下就可以了。

## docker部署

[docker tomcat official images](https://hub.docker.com/_/tomcat)

使用dockers部署前建议先将项目编译打包成war文件。

下面为示范

```bash
docker run -d -p <host port>:8080 -v <host dir>:/usr/local/tomcat/webapps tomcat:<tag>
```

若无构建docker镜像，可以直接`-v`映射`webapps`路径到宿主机下使用。

最简启动

```text
docker run -d -p <host_port>:8080 -v <dir to war>:/usr/local/tomcat/webapps tomcat
```

例如我将打包好的`war`文件放置到`/Users/weiyuan/.tomcat-webapps`目录下。然后我想让服务部署在宿主机端口`8080`

```text
docker run -d -p 8080:8080 -v /Users/weiyuan/.tomcat-webapps:/usr/local/tomcat/webapps tomcat
```

**注意了**，tag一定要选对，tag一定要选对，tag一定要选对！请严格按照开发环境所编译打包时的jdk version选择。否则可能会出现只能访问静态页面无法访问动态Servlet页面的问题。花费了我近一个下午才找到这个问题。log不报错，让我蒙在鼓里。直到去docker官网看了一眼才知道latest居然还在用jdk8。所以所谓的latest，并不一定latest，笑

**更新:**大概时隔一年左右吧，现在官方已经将latest更新到`openjdk11`与`tomcat9`了

## Windows部署Tomcat9

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

## <jsp:include />行为

include行为用于运行时包含某文件。如果被包含的文件为JSP程序，则会先执行JSP程序，并将结果包含起来。

include行为语法为

```jsp
<jsp:include flush="true" page="/head.jsp"></jsp:include>
Context
<jsp:include flush="true" page="/foot.jsp"></jsp:include>
```

属性page必须，为被包含文件的相对路径。必须为本Web程序内的文件。flush设置读入被保存文件内容时是否清空缓存

## Java Bean(POJO)

Java Bean行为是一组Java Bean相关的行为，包括useBean，setProperty,getProperty行为。Java Bean类简单的来说就是含有多个private字段，以及各个字段的getter()setter()合集在一个类中。
要注意的是**习惯把boolean类型的属性的getter方法写作isXxx()，而不是getXxx()**

- 提供一个默认的无参构造函数。
- 需要被序列化并且实现了 Serializable 接口。
- 可能有一系列可读写属性。
- 可能有一系列的 getter 或 setter 方法。

下面为实验例程中的纯html页面部分表单内容，POST请求到jsp页面

```html
<form action="/URL/person.jsp" method="post">
    Name:<input type="text" name="name" />
    Age:<input type="text" name="age" />
    <input type="submit" value="Send">
</form>
```

person.jsp内的内容,jsp行为标签内的id为Bean实例化名，是必填项，在下方页面需要通过该实例名访问Bean。class则是Bean的类名。

setProperty行为标签会自动从request里面获取提交的数据，然后赋值给Java Bean属性。property为*意味着将请求中所有属性赋值给Bean。

scope是该Bean的对象范围，当为page时只对该页面有效。当为request时只对当前的request有效（有时候一个jsp页面会嵌套其他jsp页面，forward，include行为贯穿若干个JSP页面），当为session时对当前用户有效，当为application时对当前应用程序有效，默认为page。范围排序是page -> request -> session -> application依次增大。

注意，property内只允许一个参数，实测用逗号隔开是行不通的，可以多加一个标签去设置另一个参数。

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<jsp:useBean id="person" class="club.piclight.JavaWeb.userBean"/>
<jsp:setProperty name="person" property="*" />
<html>
<head>
    <title>Title</title>
</head>
<body>
    <table>
        <tr>
            <td>Name:</td>
            <td>
                <%= person.getName()%>
            <td/>
        </tr>
        <tr>
            <td>Age:</td>
            <td>
                <%= person.getAge()%>
            </td>
        </tr>
    </table>
</body>
</html>
```

### 有关于<jsp:useBean />的 scope实验

以下为实验代码

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<jsp:useBean id="count" class="club.piclight.JavaWeb.CountBean" scope="session" />
<html>
<head>
    <title>Hello World</title>
</head>
<body>
    <h1>Hello WeiYuan, Count<%= count.getCountNum()%></h1>
</body>
</html>
```

```java
package club.piclight.JavaWeb;

public class CountBean {
    private int countNum = 0;

    public int getCountNum() {
        return ++countNum;
    }
}
```

当scope设置为page时，每次刷新页面，计数都是1。将scope设置为session时，若浏览器接受cookie的话，即可看见计数的叠加。不同设备之间的计数互不干扰。不论是page还是session若没有设置Cookie的话，第一包过去，响应头都会返回Set-Cookie字段。若设置为application时，所有设备请求的时候，都是共享一个计数器实例。注意，如果访问的是静态页面，将不会返回session

## <jsp:forward />行为

Servlet中能通过request.getRequestDispatcher("someServlet").forward("request, response")跳转到另一个Servlet或者文件。<jsp:forward />也可以做到。

下面是自己写的一段实验代码。该页面有一个表单，内有两个单选按钮，选择一个按钮后，按下Go按钮提交表单，这会直接提交给当前页面。在当前页面的页首有一段jsp代码判断request里面的param，让页面转发到PlaceA，或者PlaceB页面。这两个页面在WEB-INF文件夹下，getParam一定要做异常处理，不然第一次访问该页没有发送参数。会无法解析param导致jsp运行时错误。无法返回JSP页面下方的html内容

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<jsp:useBean id="count" class="club.piclight.JavaWeb.CountBean" scope="application"/>
<%
    String Place = null;
    try {
        Place = request.getParameter("Place");
        if (Place.equals("A")) {
%>
<jsp:forward page="/WEB-INF/PlaceA.html"/>
<%
} else if (Place.equals("B")) {
%>
<jsp:forward page="/WEB-INF/PlaceB.html"/>
<%
    }
%>
<%
    } catch (NullPointerException e) {
        e.printStackTrace();
    } finally {
    }
%>
<html>
<head>
    <title>Hello World</title>
</head>
<body>
<h1>Hello WeiYuan, Count <%= count.getCountNum()%>
</h1>
<h2>Where You Want To Go ???</h2>
<h3>Here Are Some Choise</h3>
<form action="index.jsp" method="post">
    <div>PlaceA</div>
    <input type="radio" name="Place" value="A"/>
    <div>PlaceB</div>
    <input type="radio" name="Place" value="B"/>
    <input type="submit" value="Go" class="button"/>
</form>
</body>
</html>
```

## <jsp:directive 行为>

<jsp:directive />行为相当于JSP指令。<jsp:directive.page />相当于<%@ page%>指令，<jsp:directive.include />行为相当于<%@ include />指令

<jsp:directive />行为与JSP指令可以相互改写，他是为XML格式的JSP准备的。推荐使用这种格式

## JSP隐藏对象

Servlet和JSP中输出数据都需要使用out对象，Servlet中的out对象是通过response.getWriter()方法获取的。而JSP中并没有定义out对象，却可以直接使用这是因为out是JSP内置的隐藏对象。JSP中有9种内置对象。out request config session application page pageContext exception

|对象|描述|
|--- |--- |
|request|HttpServletRequest 接口的实例|
|response|HttpServletResponse 接口的实例|
|out|JspWriter类的实例，用于把结果输出至网页上|
|session|HttpSession类的实例|
|application|ServletContext类的实例，与应用上下文有关|
|config|ServletConfig类的实例|
|pageContext|PageContext类的实例，提供对JSP页面所有对象以及命名空间的访问|
|page|类似于Java类中的this关键字|
|Exception|Exception类的对象，代表发生错误的JSP页面中对应的异常对象|

## JSP配置

JSP同样可以像Servlet一样在配置文件中进行配置

```xml
<servlet>
    <servlet-name>configuration</servlet-name>
    <jsp-file>/configuration</jsp-file>
</servlet>
<servlet-mapping>
    <servlet-name>configuration</servlet-name>
    <url-pattern>/configuraion</url-pattern>
</servlet-mapping>
```

## EL表达式

JSP可以使用EL表达式，形如`${}`，用来方便读取对象。EL表达式写在JSP的HTML代码当中，不允许写在JSP标签`<% %>`内

count为类CountBean的实例化对象，countNum为private成员

```jsp
<h1>Hello WeiYuan, Count <%= count.getCountNum()%></h1>
```

以上代码可以写成

```jsp
<h1>Hello WeiYuan, Count ${count.countNum} </h1>
```

EL表达式居然可以直接访问private成员吗？网上看见有的说法是，编译时转换成了getCountNum()的形式，如果没有了getter就无法正常运行了。经过测试“没有了getter就无法正常运行了”属实，如果没有getter会抛出错误`javax.el.PropertyNotFoundException: 类型[club.piclight.JavaWeb.CountBean]上找不到属性[countNum]`

在缓存中找到编译成java文件的对应代码，如下。

```java
out.write("<h1>Hello WeiYuan, Count ");
out.write((java.lang.String) org.apache.jasper.runtime.PageContextImpl.proprietaryEvaluate("${count.countNum}", java.lang.String.class, (javax.servlet.jsp.PageContext)_jspx_page_context, null));
out.write(" </h1>\r\n");
```

EL表达式还可以读取request，session中的属性，其他JSP隐藏对象的属性。还可以进行Java中允许的操作符运算

```jsp
${ param.foo } <!--相当于request.getParameter("foo")-->
${ initParam.foo } <!--相当于config.getInitParameter("foo")-->
${ header.host } <!--相当于requset.getHeader("host")-->
```

|隐含对象|描述|
|--- |--- |
|pageScope|page 作用域|
|requestScope|request 作用域|
|sessionScope|session 作用域|
|applicationScope|application 作用域|
|param|Request 对象的参数，字符串|
|paramValues|Request对象的参数，字符串集合|
|header|HTTP 信息头，字符串|
|headerValues|HTTP 信息头，字符串集合|
|initParam|上下文初始化参数|
|cookie|Cookie值|
|pageContext|当前页面的pageContext|

# 会话跟踪Cookie

Cookie 类的一些方法

|方法|描述|
|--- |--- |
|public void setDomain(String pattern) |设置 cookie 的域名|
|public String getDomain() |获取 cookie 的域名|
|public void setMaxAge(int expiry) |设置 cookie 有效期，以秒为单位，默认有效期为当前session的存活时间|
|public int getMaxAge() |获取 cookie 有效期，以秒为单位，默认为-1 ，表明cookie会活到浏览器关闭为止|
|public String getName() |返回 cookie 的名称，名称创建后将不能被修改|
|public void setValue(String newValue) |设置 cookie 的值|
|public String getValue() |获取cookie的值|
|public void setPath(String uri) |设置 cookie 的路径，默认为当前页面目录下的所有 URL，还有此目录下的所有子目录|
|public String getPath() |获取 cookie 的路径|
|public void setSecure(boolean flag) |指明 cookie 是否要加密传输|
|public void setComment(String purpose) |设置注释描述 cookie 的目的。当浏览器将 cookie 展现给用户时，注释将会变得非常有用|
|public String getComment() |返回描述 cookie 目的的注释，若没有则返回 null|

Session 类的一些方法

Cookie是最早的会话跟踪技术，Session则建立在Cookie的基础之上，当然，在浏览器禁用Cookie的情况下，依然可以通过URL重写技术实现session

Cookie侧重于在客户端浏览器记录数据，而session则侧重将会话信息记录在服务端。


### 记录我遇到的问题

不推荐在JSP页面试图使用response的writer，或者outputStream。会出现谜之问题

使用outputStream输出会可能导致这个页面只有outputSteam才能得以输出。我尝试调用了outputStream，发现这会导致页面只有outputStream才有输出。其他的输出全部消失了。

然后尝试去get response的Writer进行输出，发现我放在页面中部的printTable方法所输出的HTTP内容居然比整个JSP页面内的其他模板数据还要早出现。但是查看编译出来的java文件，编译并没有改变JSP内的顺序。我调用的是response里get到的Writer

根据编译出的.java文件配合调试工具，可以看出，页面中只允许一个outputStream或者其他的输出实例。如果出现两个实例的话。将只有最先被调用的那个实例才得以输出。模板数据是通过pageContext里get到的out进行输出的。

```text
_jspxFactory.getPageContext -> pageContext.getOut -> out
```
