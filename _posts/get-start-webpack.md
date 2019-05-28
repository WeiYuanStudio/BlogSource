---
title: 随处可见的Webpack笔记
categories:
  - 笔记
  - 前端
tags:
  - WebPack
  - 前端
date: 2019-05-28 20:54:18
thumbnail:
---
这是一片个人向的WebPack笔记

<!--more-->

# 安装Webpack

在安装Webpack之前，建议先通过系统的包管理器安装npm包管理器，以用于安装Webpack

使用命令`$ npm init`初始化包信息文件**package.json**

使用命令`$ npm install webpack webpack-cli`安装Webpack & Webpack-cli到当前项目，建议安装到当前项目，以便版本管理，安装到全局只需要加上参数｀-g｀

建立一个html文件到当前目录的**public目录**下，如**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>LightMessage</title>
</head>
<body>
    <h1 id="myTitle">Hello World</h1>
    <script src="./run.js"></script>
</body>
</html>
```

现在用浏览器打开该html文件，应该可以看到是没有任何格式Hello World的一号标题显示在网页上。现在我们再创建一个JS文件，目标是修改页面内的内容，但是不放置到**public目录**下，因为我们要让Webpack为我们打包JS，文件名就用**myScript.js**吧

```JavaScript
function changeTitle(info) {
	document.getElementById('myTitle').innerText = info;
}

module.exports = changeTitle;
```

这是一个很简单的一个修改标题函数。

接下来，编写执行入口，创建文件**main.js**
```JavaScript
//导入函数
const changeTitle = require('./myScript')

changeTitle('Hello Webpack');
```
