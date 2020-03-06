---
layout: post
title: 实现访问Apache时的目录浏览功能
date: 2020-01-22 12:00:00
categories:
    - Server
tags:
    - Apache
    - Server
    - 服务器
    - Config
    - 配置
---

# Apache File List View
### 1. 实现原理 

通过apache的一个模块：mod_autoindex

这个模块的主要功能是自动对目录中的内容生成列表，这样当我们对服务器地址进行访问时在浏览器端可以看到访问目录下文件列表，点击它浏览器能打开的则打开查看，不能打开的则弹出是否保存的对话框，当然也可以右键链接另存为，这样就实现了访问下载文件的功能

对于mod_autoindex更多知识可以查看[apache手册](http://www.jinbuguo.com/apache/menu22/mod/mod_autoindex.html)或者可以点击下载[apache中文手册](http://download.csdn.net/detail/chen_gong1992/9700259)

### 2. 配置httpd.conf文件 

apache的目录索引样式用mod_autoindex模块一般默认为开启状态。

找到httpd.conf文件，查找下面的内容
```
<Directory>
    Options FollowSymLinks
    AllowOverride None
    Order deny, allow
#   Deny from all
    Allow from all
</Directory>
```
如果有#号注释则去掉，没有这句话就补上去。这样就开启了mod_autoindex

LoadModule autoindex_module modules/mod_autoindex.so

另外，检查一下访问权限，如果发现下面有deny from all请注释掉，改成allow from all。

### 3.设置请求目录 
当从服务器请求一个目录的时候，可能来自: 
- mod_dir的DirectoryIndex指定首页 
- mod_autoindex目录

如果你想直接访问mod_autoindex目录列表,那就删除服务器目录下的mod_dir的DirectoryIndex指定首页，或者在配置文件httpd.conf中修改。

### 4.优化显示 
将下面的代码加到httpd.conf文件中

```
    Options Indexes FollowSymLinks
    IndexOptions FancyIndexing FoldersFirst NameWidth=* DescriptionWidth=* SuppressHTMLPreamble HTMLTable
    IndexOptions Charset=utf-8 IconHeight=16 IconWidth=16 SuppressRules
    IndexIgnore web header.html footer.html actions defects
    HeaderName /web/header.html
    ReadmeName /web/footer.html
    Include conf/extra/httpd-autoindex.conf
    IndexOrderDefault Ascending Date
    ServerSignature Off
```

下面对上面的配置代码进行解读:

- **Indexes**：索引, 默认是开启索引浏览,如果不想开启前面加个-, -Indexes就是禁止目录浏览
- **IndexOptions**：FancyIndexing 开启富样式索引，这里说一下：IndexOptions指令的FancyIndexing选项可以让列表头部增加可以产生按Name, Lastmodified, Size, Description排序产生的目录列，跟在后面的命令这个主要是美化样式显示。
- **FoldersFirst**：文件夹优先，这里指让目录中的文件夹排在前面
- **NameWidth**、**DescriptionWidth**： 文件名和描述文字的长度，这里*表示长度自适应，当然默认的情况下是没有描述的，需要使用AddDescription指令添加描述SuppressHTMLPreamble 是去掉apache自动生成一些HTML代码 例如 Index/of
- **ofHTMLTable**：是启用HTML表格样式，以表格的方式展示列表 
- **Charset=utf-8**：设置字符集,建议用utf-8,用GB2312会乱码 
- **IconHeight**、**IconWidth**：设置图标的大小 
- **SuppressRules**：在FancyIndexing开启的情况下取消hr下划线 
- **IgnoreCase**：排序不区分大小写 
- **IndexIgnore**：设置哪些类型的文件不显示出来, 如IndexIgnore README .htaccess *.bak *~ 
- **HeaderName**：/web/header.html 这个是头文件, 注意是绝对路径 
- **ReadmeName**：/web/footer.html 这个结尾的文件, appache如果发现了这些文件，就在文件列表之前首先显示这些文件的内容，可以看出，其实个性化的原理就是把Apache的目录列表嵌在了header.htm与footer.html中间的表格中，两个文件的其他部分都可以自定义内容的。
- **Include conf/extra/httpd-autoindex.conf**： 加载apache目录下的extra/httpd-autoindex.conf文件为文件添加小图标，httpd-autoindex.conf文件里有关于小图标的设置，这里就不展开了。
- **IndexOrderDefault Ascending Date**：默认按文件的创建时间排序； 
- **ServerSignature Off**：防止服务器的信息泄露 这个不用管，off掉就行 

==header.html==

```HTML
<!Doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Mr.CG的站点</title>
    <style type="text/css">
        p{
            font-family: "微软雅黑";
            color: #0080FF;
            font-weight: bolder;
            font-size:36px;
            text-align: center;
        }
        h3{
            width: 79%;
            margin: 0 auto;
            text-align: right;
            color: #757575;
        }
        table {
            width:80%;
            border-collapse:collapse;
            border:1px #858585 solid;
            margin:0 auto
        }
        table td { 
            line-height:17px;
            font-size:15px;
            text-align:left;
            border:1px #858585 solid;
        }
        table td:nth-child(1){ 
            width:20px;
            text-align:right;
        }
        table tr:nth-child(odd){ 
            background:#f0f0f0;
        }
        table tr:hover{
            background:#ACD6FF;
            color:#990000;
        }
        table th {
            background:#999999;
            line-height:17px;
        }
        table th  a:link {
            text-decoration: none; 
            color:#FFFFFF;
        }
        table th  a:visited {
            color:#FFFFFF;
        }
        table th  a:hover {
            color:#FFFF00;
        }
    </style>
</head>
<p>Welcome</p>
<body>
```
==footer.html==

```HTML
    <h3> Power by Mr.CG<h3>
</body>
</html>
```

