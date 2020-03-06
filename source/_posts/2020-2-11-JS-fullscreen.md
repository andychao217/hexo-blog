---
layout: post
title: JS控制全屏
date: 2020-02-11 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
---

```HTML
<div id="fullscreen_btn" class="btn-group">
    <a id="fullScreen" class="btn btn-link" onclick="fullscreen()" ><i class="fa fa-expand fa-2x"></i></a>
</div>
```
```javascript
if (!!window.ActiveXObject || "ActiveXObject" in window) {
    document.getElementById('fullscreen_btn').style.display = 'none';
}else{
    document.getElementById('fullscreen_btn').style.display = '';
};
								
document.addEventListener("fullscreenchange", function () {  
    fullscreenState.innerHTML = (document.fullscreen) ? "" : "not ";  
}, false);  

document.addEventListener("mozfullscreenchange", function () {  
    fullscreenState.innerHTML = (document.mozFullScreen) ? "" : "not ";  
}, false);  

document.addEventListener("webkitfullscreenchange", function () {  
    fullscreenState.innerHTML = (document.webkitIsFullScreen) ? "" : "not ";  
}, false);  
  
document.addEventListener("msfullscreenchange", function () {  
	fullscreenState.innerHTML = (document.msFullscreenElement) ? "" : "not ";  
}, false); 

function fullscreen() {  
    if(document.fullscreen || document.mozFullScreen || document.webkitIsFullScreen || document.msFullscreenElement){
        if (document.exitFullscreen) {  
        	document.exitFullscreen();  
        }else if (document.mozCancelFullScreen) {  
        	document.mozCancelFullScreen();  
        }else if (document.webkitCancelFullScreen) {  
        	document.webkitCancelFullScreen();  
        }else if (document.msExitFullscreen) {  
        	document.msExitFullscreen();  
        }
        document.getElementById('fullScreen').innerHTML = '<i class="fa fa-expand fa-2x"></i>';
        document.getElementById('fullScreen').title = window.parent.languageData.main_fullscreen;
    }else{
    	var docElm = document.documentElement;  
    	//W3C   
    	if (docElm.requestFullscreen) {  
    		docElm.requestFullscreen();  
    	}else if (docElm.mozRequestFullScreen) {  
    		docElm.mozRequestFullScreen();  
    	}else if (docElm.webkitRequestFullScreen) {  
    		docElm.webkitRequestFullScreen();  
    	}else if (elem.msRequestFullscreen) {  
    		elem.msRequestFullscreen();  
    	}
    	document.getElementById('fullScreen').innerHTML = '<i class="fa fa-compress fa-2x"></i>';
    	document.getElementById('fullScreen').title = window.parent.languageData.main_exitfullscreen;
    } 
}
```
