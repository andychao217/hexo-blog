---
layout: post
title: 根据JSON数据动态生成表格，并可编辑
date: 2020-01-16 12:00:00
categories:
	- JavaScript
	- HTML
tags:
    - JavaScript
    - 前端
    - JSON
    - HTML
---

JSON

```JSON
getdata([
    {
        "id": "1", 
        "国家": "中国", 
        "capital": "北京", 
        "hhh": "dfa", 
        "location": "Asia"
    }, 
    {
        "id": "2", 
        "国家": "美国", 
        "capital": "纽约", 
        "hhh": "dfa", 
        "location": "America"
    }, 
    {
        "id": "3", 
        "国家": "英国", 
        "capital": "伦敦", 
        "hhh": "dfa", 
        "location": "Europe"
    }, 
    {
        "id": "4", 
        "国家": "日本", 
        "capital": "东京", 
        "hhh": "dfa", 
        "location": "Asia"
    }, 
    {
        "id": "5", 
        "国家": "韩国", 
        "capital": "首尔", 
        "hhh": "dfa", 
        "location": "Asia"
    }, 
    {
        "id": "6", 
        "国家": "法国", 
        "capital": "柏林", 
        "hhh": "dfa", 
        "location": "Europe"
    }
])
```
HTML

```HTML
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="utf-8">
		<meta http-equiv="X-UA-Compatible" content="IE=edge">
		<meta name="viewport" content="width=device-width, initial-scale=1">
		<link href="../../../ICPAS/Wnmp/WWW/css/bootstrap.css" rel="stylesheet">
	</head>
	<body>
		<div class="container">
			<hr>
		  	<div class="row">
			  	<div class="col-md-12 col-sm-12 col-xs-12" id="content"></div>
		  	</div>
		</div>

		<!-- winbatchadd Modal -->
		<div id="modal" class="modal fade" tabindex="-1" role="dialog">
			<div class="modal-dialog">
				<div class="modal-content">
					<div class="modal-header">
						<h4 class="modal-title" >编辑</h4>
					</div>
					<div class="modal-body">
						<div class="row"  id="modal_content"></div>
					</div>
					<div class="modal-footer">
						<div class="btn-group btn-group-justified">
							<div class="btn-group">
								<button id="btnAdd" type="submit" class="btn btn-success">确定</button>
							</div>
							<div class="btn-group">
								<button type="button" class="btn btn-danger" data-dismiss="modal">取消</button>
							</div>
						</div>
					</div>
				</div><!-- /.modal-content -->
			</div><!-- /.modal -->
		</div>
		<!-- winbatchadd Modal -->
		
		<script src="../../../ICPAS/Wnmp/WWW/js/jquery-1.11.3.min.js"></script>
		<script src="../../../ICPAS/Wnmp/WWW/js/bootstrap.js"></script>
		<script src="3.js"></script>
		<script  src="4.json?callback=getdata"></script>
	</body>
</html>
```
JavaScript

```javascript
// JavaScript Document

var bgColor;
var list = [];

$(function(){
	for(var i=0;i<list.length;i++){
		list[i].toolbar = '<button class="btn btn-link btn-block" onclick="edit1(' + i + ')">编辑</button>'	
	}

   	var content = window.document.getElementById("content");
	//创建表格
	var table = window.document.createElement("table");
	table.border = 1;
	table.setAttribute('class','table table-responsive table-bordered');
	content.appendChild(table);

	//创建标题行
	var thead = window.document.createElement("thead");
	var itemHead = list[0];
	for (var index in itemHead) {
		//创建标题单元格添加到thead
		var th = window.document.createElement("th");
		th.style.textAlign = "center";
		th.innerHTML = index;
		thead.appendChild(th);
	}
	table.appendChild(thead);

	//遍历对象，创建行和单元格
	for (var i = 0; i < list.length; i++) {
		var item = list[i];
		//创建行
		var tr = window.document.createElement("tr");
		if (i % 2 == 0) {
			tr.style.backgroundColor = "white";
		} else {
			tr.style.backgroundColor = "whitesmoke";
		}
		//注册指向行的事件
	   	tr.onmouseover = function () {
			bgColor = this.style.backgroundColor;
			this.style.backgroundColor = "lightgray";
		}
		tr.onmouseout = function () {
			this.style.backgroundColor = bgColor;
		}
		table.appendChild(tr);
		for (var key in item) {
			//创建单元格
			var td = window.document.createElement("td");
			td.innerHTML = item[key];
			td.style.textAlign = "center";
			tr.appendChild(td);
		}
	}

	$('#btnAdd').click(function(){
		var label = $('#modal_content').find('label');
		var input = $('#modal_content').find('input');
		var object = new Object();
		for(var i = 0; i<label.length;i++){
			object[label[i].innerText] = input[i].value;
		}

		console.log(object);
	});
});

function edit1(index){
	$('#modal_content').empty();
	var modal_content = document.getElementById('modal_content');
	var data = list[index]
//	console.log(data);
	delete data.toolbar;
	for(var index in data){
	//	console.log(modal_content);
		var form_group = window.document.createElement("div");
		form_group.setAttribute('class','form-group col-md-6 col-sm-6 col-xs-6');
		modal_content.appendChild(form_group);
		var label = window.document.createElement("label");
		label.innerHTML = index;
		label.setAttribute('class','control-label');
		form_group.appendChild(label);
		var input = window.document.createElement("input");
		input.value = data[index];
		input.setAttribute('class','form-control');
		form_group.appendChild(input);
	}
	$('#modal').modal('show');
}

function getdata(result){
	list = result;
}
```
