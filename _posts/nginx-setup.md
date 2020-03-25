---
title: Nginx 简单配置
date: 2020-03-25 20:00:00
tags:
---

> Nginx（发音同engine x）是异步框架的 Web服务器，也可以用作反向代理，负载平衡器 和 HTTP缓存。该软件由 Igor Sysoev 创建，并于2004年首次公开发布。同名公司成立于2011年，以提供支持。

> Nginx是免费的开源软件，根据类BSD许可证的条款发布。一大部分Web服务器使用Nginx，通常作为负载均衡器。

<!--more-->

**Tips:**

> 写在前面，调试服务器时请务必打开F12，并在网络设置里面勾选禁用缓存，缓存会误导你，并大大降低你调试的效率。如果你会使用curl，那就再好不过了

[nginx man page](https://linux.die.net/man/8/nginx)

## 安装Nginx服务

这里以debian为例`apt install nginx`各个系统都有各种包管理，就不依次介绍了。

## 配置Nginx的配置文件

如果找不到Nginx配置文件，可以运行`nginx -t`
若安装成功，会返回

```bash
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

即可在这里看到配置文件的路径`/etc/nginx/nginx.conf`。接下来就可以使用vim编辑器编辑Nginx配置文件。PS:默认HTTP服务目录为`/usr/share/nginx/html`或者`/var/www/html`，或者可以在默认配置文件中查看

## 启动Nginx

执行命令`nginx`即可直接启动Nginx，正常情况下，不会有任何返回。（古人云，*nix的美学，没有消息就是最好的消息。）这时候你应该可以正常访问你的站点了。

如果返回如下

```
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] still could not bind()
```

大概是因为Nginx已经运行在后台了，可以`nginx -s stop`停止服务,或者使用htop查看一下后台进程。在htop里面输入`/nginx`即可查找nginx进程了。

## 配置简单的静态HTTP服务

当你启动nginx服务后，这时候试着访问一下80端口，你应该会看到nginx的欢迎界面，接着可以继续下面的配置步骤。

如果你看不到欢迎界面，请尝试本地访问一下，没有浏览器的cli环境，可以使用`curl 127.0.0.1:80`看看，排除掉端口被防火墙拦截的可能。如果本地请求成功，而外部请求失败的话，检查一下防火墙（系统防火墙，以及机房的安全策略之类的）

这个80端口的默认服务位于配置文件 `/etc/nginx/sites-available/default`

下面展示该配置文件的部分信息（有部分删减，去掉注释部分）

```conf
server {
	listen 80 default_server;
	listen [::]:80 default_server;

	root /var/www/html;

	# Add index.php to the list if you are using PHP
	index index.html index.htm index.nginx-debian.html;

	server_name _;

	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
	}
}
```

如果只需要一个简单的HTTP静态服务的话，修改`root /var/www/html;`那一行到自己的静态文件目录即可

## 配置HTTPS

配置HTTPS前，你需要

- 获得签发的证书，以及私钥
- 该签发的域名的解析已经按照需求配置解析到服务器ip上

接着找个地方放置你的证书和私钥，切记不要泄漏私钥，然后在nginx的配置目录`conf.d/`下建立一个配置文件，命名后缀`.conf`

`ssl_certificate`为放置证书的路径
`ssl_certificate_key`为放置私钥的路径

示例：

```conf
server {
	listen 443 ssl;
	ssl on;
	server_name weiyuanstudio.club;
	ssl_certificate /path_to_certificate;
	ssl_certificate_key /path_to_certificate_key;
	ssl_session_timeout 10m;
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
	ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
	ssl_prefer_server_ciphers on;

	location / {
		root /var/www/weiyuanstudio.club;
		index index.html index.htm;
	}
	
	location /tomcat/ {
		proxy_pass http://127.0.0.1:8080/;
	}
}
```

上面配置了一个静态服务`/`以及一个tomcat反代`/tomcat`

## 配置自动HTTPS跳转

这时候可以修改默认的80静态服务配置文件（就是安装完毕后的那个欢迎界面的配置文件`/etc/nginx/sites-available/default`），达到这个效果

其他无关的设置都可以注释掉，最简设置如下，将下面的`example.com`修改为你的域名即可

```conf
server {
	listen 80 default_server;
	listen [::]:80 default_server;

	return 301 https://example.com$request_uri;
} 
```

这样配置完后，向80端口的所有请求，收到服务器返回的301，也就是重定向到HTTPS

用`curl`检查应该大致如下

```bash
ubuntu@VM-0-5-ubuntu:/etc/nginx/sites-available$ curl weiyuanstudio.club -i
HTTP/1.1 301 Moved Permanently
Server: nginx/1.14.0 (Ubuntu)
Date: Wed, 25 Mar 2020 14:32:50 GMT
Content-Type: text/html
Content-Length: 194
Connection: keep-alive
Location: https://weiyuanstudio.club/

<html>
<head><title>301 Moved Permanently</title></head>
<body bgcolor="white">
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx/1.14.0 (Ubuntu)</center>
</body>
</html>
```

能看见响应码为`301`，而且响应头里的`Location`字段正确
