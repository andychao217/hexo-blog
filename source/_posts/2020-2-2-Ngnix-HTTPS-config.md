---
layout: post
title: Ngnix HTTPS 配置
date: 2020-02-02 12:00:00
categories:
    - Server
tags:
    - Ngnix
    - Server
    - 服务器
    - Config
    - 配置
---

# Ngnix HTTPS 配置

```
server {
    listen       80;
        
```
---
## *
```
	listen       443 ssl;
    server_name  andychao217.cn alias www.andychao217.cn;
	ssl_certificate      C:/UPUPW_NP7.0/Nginx/conf/ssl/server.crt;
    ssl_certificate_key  C:/UPUPW_NP7.0/Nginx/conf/ssl/server.key;
    ssl_session_timeout  5m;
    ssl_protocols  SSLv2 SSLv3 TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers   on;
    if ($scheme = http) {
        return   301 https://$host$request_uri;
    }
 ```    
---
```
    location / {
        root   C:/UPUPW_NP7.0/vhosts/andychao217.cn;
        index  index.html index.htm default.html default.htm index.php default.php app.php u.php;
		include        C:/UPUPW_NP7.0/vhosts/andychao217.cn/up-*.conf;
    }
	autoindex off;
	include advanced_settings.conf;
	#include expires.conf;
	location ~* .*\/(attachment|attachments|uploadfiles|avatar)\/.*\.(php|PHP7|phps|asp|aspx|jsp)$ {
        deny all;
    }
    location ~ ^.+\.php {
        root           C:/UPUPW_NP7.0/vhosts/andychao217.cn;
        fastcgi_pass   bakend;
        fastcgi_index  index.php;
		fastcgi_split_path_info ^((?U).+\.php)(/?.+)$;
		fastcgi_param  PATH_INFO $fastcgi_path_info;
		fastcgi_param  PATH_TRANSLATED $document_root$fastcgi_path_info;
```
---
## *
```
		fastcgi_param  HTTPS  $https if_not_empty;
```			
---
```
        include        fastcgi.conf;
    }
}
#server andychao217.cn end}
```
