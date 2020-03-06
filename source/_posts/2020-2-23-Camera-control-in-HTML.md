---
layout: post
title: HTML5拍照,摄像机功能实战
date: 2020-02-23 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - Video
---

## 拍照
HTML5的getUserMedia API为用户提供访问硬件设备媒体（摄像头、视频、音频、地理位置等）的接口，基于该接口，开发者可以在不依赖任何浏览器插件的条件下访问硬件媒体设备。

1. 获取视频流，并用video标签播放。
```javascript
    <video id="video" autoplay></video>
    
    --------------------------------------------------------------
    
    const videoConstraints = { width: 1366, height: 768 };
    const videoNode = document.querySelector('#video');
    let stream = await navigator.mediaDevices.getUserMedia({ audio: true, video: videoConstraints });
    videoNode.srcObject = stream;
    videoNode.play();

```
2. 多个摄像头设备，如何切换？

```javascript
    // enumerateDevices获取所有媒体设备
    const mediaDevices = await navigator.mediaDevices.enumerateDevices();
    // 通过设备实例king属性videoinput，过滤获取摄像头设备
    const videoDevices = mediaDevices.filter(item => item.kind === 'videoinput') || [];
    // 获取前置摄像头
    const videoDeviceId = videoDevices[0].deviceId;
    const videoConstraints = { deviceId: { exact: videoDeviceId }, width: 1366, height: 768 };
    let stream = await navigator.mediaDevices.getUserMedia({ audio: true, video: videoConstraints });
    // 获取后置摄像头
    const videoDeviceId = videoDevices[1].deviceId;
    const videoConstraints = { deviceId: { exact: videoDeviceId }, width: 1366, height: 768 };
    let stream = await navigator.mediaDevices.getUserMedia({ audio: true, video: videoConstraints });
    
    // 依次类推...

```

3. 拍照保存图片

```javascript
    // 通过canvas捕捉video流，生成base64格式图片
    const canvas = document.createElement('canvas');
    const context = canvas.getContext('2d');
    canvas.width = 1366;
    canvas.height = 768;
    context.drawImage(videoNode, 0, 0, canvas.width, canvas.height);
    download(canvas.toDataURL('image/jpeg'));
    // 下载图片
    function download (src) {
        if (!src) return;
        const a = document.createElement('a');
        a.setAttribute('download', new Date());
        a.href = src;
        a.click();
    }

```

4. 关闭摄像头设备

```javascript
    let stream = await navigator.mediaDevices.getUserMedia({ audio: true, video: videoConstraints });
    // 3s后关闭摄像头
    setTimeout(function () {
        stream.getTracks().forEach(track => track.stop());
        stream = null;
    }, 3000);

```
到此完成简单的相机拍照功能
## 摄像
摄像基本流程，是录制一段视频流并保存为音视频文件。HTML5 MediaRecorder对象提供了多媒体录音和录视频功能。

==注意：摄像预览播放器video标签要设置静音muted（消除回声导致的刺耳噪音）==

```javascript
    const videoConstraints = { width: 1366, height: 768 };
    let stream = await navigator.mediaDevices.getUserMedia({ audio: true, video: videoConstraints });
    let mediaRecorder = new MediaRecorder(stream);
    let mediaRecorderChunks = []; // 记录数据流
   
    mediaRecorder.ondataavailable = (e) => {
        mediaRecorderChunks.push(e.data);
    };
    
    mediaRecorder.onstop = (e) => {
        // 通过Blob对象，创建文件二进制数据实例。
        let recorderFile = new Blob(mediaRecorderChunks, { 'type' : mediaRecorder.mimeType });
        mediaRecorderChunks = [];
        const file = new File([this.recorderFile], (new Date).toISOString().replace(/:|\./g, '-') + '.mp4', {
            type: 'video/mp4'
        });
        download(URL.createObjectURL(file));
        // 下载视频
        function download (src) {
            if (!src) return;
            const a = document.createElement('a');
            a.setAttribute('download', new Date());
            a.href = src;
            a.click();
        }
    };
    
    mediaRecorder.stop(); // 停止录制，触发onstop事件

```
