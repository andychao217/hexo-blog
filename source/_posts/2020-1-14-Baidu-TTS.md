---
layout: post
title: 百度云TTS文字转语音
date: 2020-01-14 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - 百度
    - TTS
---

```HTML
<!DOCTYPE html>
<html>
    <head>
    	<meta charset="UTF-8">
    	<title>百度地图将文字转化为语音并播放</title>
    	<!-- 这里调用的是百度文字转语音开放API -->
    </head>
    <body>
        <div>
            <input type="text" id="ttsText">
            <input type="button" id="tts_btn" onclick="doTTS()" value="播放">
        </div>
        <script>
            function doTTS() {
                var ttsText = document.getElementById('ttsText').value;
                
                var ttsAudio = new Audio();
                ttsAudio.src = 'http://tts.baidu.com/text2audio?lan=zh&ie=UTF-8&per=3&spd=5&text=' + ttsText
                ttsAudio.play();
            }
            /*
            代码中改变传参可更改配置：
            
            lan=zh（语言zh:中文；en:英文；fr:法文；）
            
            ie=UTF-8（字符集）
            
            per=3（每3个字符停顿）
            
            spd=5（语音播放速度，数字越大越快0-15）
            
            text=“”（需要转换的文字）
            */
            </script>
    </body>
</html>
```
