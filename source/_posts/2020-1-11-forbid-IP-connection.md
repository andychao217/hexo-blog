---
layout: post
title: 防止恶意解析(禁止通过IP直接访问网站)
date: 2020-01-11 12:00:00
categories:
    - Server
tags:
    - Server
    - 服务器
    - 配置
    - Config
---

# 防止恶意解析

## 禁止通过 IP 直接访问网站

1. 什么是恶意解析

一般情况下，要使域名能访问到网站需要两步，第一步，将域名解析到网站所在的主机，第二步，在 web 服务器中将域名与相应的网站绑定。但是，如果通过主机 IP 能直接访问某网站，那么把域名解析到这个 IP 也将能访问到该网站，而无需在主机上绑定，也就是说任何人将任何域名解析到这个 IP 就能访问到这个网站。可能您并不介意通过别人的域名访问到您的网站，但是如果这个域名是未备案域名呢？一旦被查出，封 IP、拔线甚至罚款的后果都是需要您来承担的。某些别有用心的人，通过将未备案域名解析到别人的主机上，使其遭受损失，这是一种新兴的攻击手段。

2. Apache 服务

在用 apache 搭建的 WEB 服务器的时候，如何想只能通过设定的域名访问，而不能直接通过服务器的 IP 地址访问呢，有以下两种方法可以实现（当然肯定还会有其他方法可以实现），都是修改 httpd.conf 文件来实现的，下面举例说明。

    在httpd.conf文件最后面，加入以下代码

```
    NameVirtualHost *:80
    <VirtualHost *:80>
        ServerName 221.*.*.*
        <Location />
            Order Allow,Deny
            Deny from all
        </Location>
    </VirtualHost>
    　　　　
    <VirtualHost *:80>
        DocumentRoot "/www/web"
        ServerName www.wzlinux.com
    </VirtualHost>

```

说明：上部分是实现拒绝直接通过 221._._._这个 IP 的任何访问请求，这时如果你用 221._.*.*访问，会提示拒绝访问。下部分就是允许通过 www.wzlinux.com 这个域名访问，主目录指向/www/web(这里假设你的网站的根目录是/www/web)。

3. Tomcat 服务

修改 server.xml 这个配置文件。

比如服务器 IP 地址是 192.168.1.2 ，相应域名是 www.wzlinux.com。

打开 %TOMCAT_HOME%/conf/server.xml 文件，找到 Engine 节点作如下 Xml 代码。

```
<Engine name="Catalina" defaultHost="www.piis.cn">
　　<Host name="www.piis.cn" appBase="webapps"
　　      unpackWARs="true" autoDeploy="true"
　　      xmlValidation="false" xmlNamespaceAware="false"/>
　　<Host name="192.168.1.2" appBase="ipapps"
　　      unpackWARs="true" autoDeploy="true"
　　      xmlValidation="false" xmlNamespaceAware="false"/>
</Engine>
```

注意事项：

1. Engine 节点配置的 defaultHost 表明缺省访问的 Host。defaultHost 对应的名称必须存在于 Engine 节点下配置的 host 节点中。

2. 当一台机器有多个 IP，而按照规定只允许通过一个指定的域名访问时很有用。此时，把 defaultHost 指定为非域名对应的 host，这样不通过域名访问时就都定位到指定的非域名 HOST 了。
3. Host 节点 name 对应 IP 地址，以及域名。一个 Host 只有指定一个 IP 或域名。
4. Host 节点的 appBase ，对应的是存放 web 应用的目录。这里输入的目录相对于 %TOMCAT_HOME%，如上面的 www.wzlinux.com 对应的目录是 %TOMCAT_HOME%/webapps，而 192.168.1.2 对应的目录是 %TOMCAT_HOME%/ipapps。

5. Nginx 服务

定义一个默认的空主机名，禁止其访问，需要通过的域名一定要在其他 server 配置。

```
server {
    listen       80 default;
    server_name  "";
    return  444;
}

或者

server {
    listen       80 default;
    server_name  _;
    return  444;
}
```
