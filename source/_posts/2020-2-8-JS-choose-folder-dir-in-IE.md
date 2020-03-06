---
layout: post
title: js选择文件夹路径(IE)
date: 2020-02-08 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - IE
---

语法：==strDir=Shell.BrowseForFolder(Hwnd,Title,Options,[RootFolder])==

---

参数：
- Hwnd：包含对话框的窗体句柄（handle），一般设置为0
- Title：将在对话框中显示的说明，为字符串
- Options：使用对话框的特殊方式，为长整数，一般设置为0
- RootFolder：（可选的），用来设置浏览的最顶层文件夹，缺省时为“桌面”，可以将其设置为一个路径或“特殊文件夹常数”

```javascript
<input id="show">
<button onclick="clickBtn()">点击</button>

function click() {
    try {
        var Message = "\u8bf7\u9009\u62e9\u6587\u4ef6\u5939"; //选择框提示信息
        var Shell = new ActiveXObject("Shell.Application");
        var FolderObj = Shell.BrowseForFolder(0, Message, 64, 17); //起始目录为：我的电脑
        //var FolderObj = Shell.BrowseForFolder(0, Message, 0); //起始目录为：桌面
        var filePath;
        if (Folder != null) {
            filePath = FolderObj.Items().Item().Path;
            if (filePath.charAt(0) == ':') {
                alert('请选择文件夹.');
                return;
            }
            document.getElementById(path).value = filePath;
        }
    }
    catch (e) {
        alert(e + '请设置IE，Internet选项－安全－自定义级别－将ActiveX控件和插件前3个选项设置为启用，然后再尝试。');
        return;
    }
}
```
