---
layout: post
title: 输入密码检测大写是否锁定JS实现代码
date: 2020-01-10 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
---

```HTML
<div>
    <input class="text" name="passwd" id="loginPasswd" type="password" style="*display:block;" />
    <div style="color:#F90;padding:2px; position:absolute; display:none;" id="capital">大写锁定已开启
    </div>
    <script type="text/javascript">
        (function(){
            var inputPWD = document.getElementById('loginPasswd');
            var capital = false;
            var capitalTip = {
                elem:document.getElementById('capital'),
                toggle:function(s){
                    var sy = this.elem.style;
                    var d = sy.display;
                    if(s){
                        sy.display = s;
                    }else{
                        sy.display = d =='none' ? '' : 'none';
                    }
                }
            }
            var detectCapsLock = function(event){
                if(capital){return};
                var e = event||window.event;
                var keyCode = e.keyCode||e.which; // 按键的keyCode
                var isShift = e.shiftKey ||(keyCode == 16 ) || false ; // shift键是否按住
                if (
                    ((keyCode >= 65 && keyCode <= 90 ) && !isShift) // Caps Lock 打开，且没有按住shift键
                    || ((keyCode >= 97 && keyCode <= 122 ) && isShift)// Caps Lock 打开，且按住shift键
                ){
                    capitalTip.toggle('block');
                    capital=true;
                }else{
                    capitalTip.toggle('none');
                }
            }
            inputPWD.onkeypress = detectCapsLock;
            inputPWD.onkeyup=function(event){
                var e = event||window.event;
                if(e.keyCode == 20 && capital){
                    capitalTip.toggle();
                    return false;
                }
            }
        })();
    </script>
</div>
```
