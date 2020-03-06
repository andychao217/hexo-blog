---
layout: post
title: Echarts Ajax加载数据
date: 2020-02-25 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - Echart
---

```javascript
echartPie.showLoading();
echartGauge.showLoading();
echartBar.showLoading();
setInterval(function (){
	$.ajax({
		type : "get", 
		async : false,   //异步请求（同步请求将会锁住浏览器，用户其他操作必须等待请求完成才可以执行）
		url : "../php/preview.php",    //请求发送到TestServlet处
		dataType : "json",        //返回数据形式为json
		success : function(resData) {
				//请求成功时执行该函数内容，result即为服务器返回的json对象
			if (resData) {
				echartPie.hideLoading();
				echartGauge.hideLoading();
				echartBar.hideLoading();
				
				resData.online_status[0].name = window.parent.languageData.terminalstate_online;
				resData.online_status[1].name = window.parent.languageData.terminalstate_offline;
				echartPie.setOption({
					series: [{
						data: resData.online_status
					}]
				});
				
				option.series[0].data[0].value = resData.CPU_usage;
				option.series[1].data[0].value = resData.RAM_usage;
				echartGauge.setOption(option,true);
				
				echartBar.setOption({
					series: [{
						data: resData.tasks_status
					}]
				});
				}
		},
		error : function() {
				//请求失败时执行该函数
			toastr.error(window.parent.languageData.echart_fail, window.parent.languageData.common_tip);
			echartPie.hideLoading();
			echartGauge.hideLoading();
			echartBar.hideLoading();
		}
	});
},1000);
```