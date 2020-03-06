---
layout: post
title: JSON Schema 验证
date: 2020-02-13 12:00:00
categories:
    - JSON
tags:
    - JSON
---

```JSON
{
  "$schema": "http://json-schema.org/draft-06/schema#",  //schema验证版本
  "title": "Cat",   //json标题
  "properties": {   //数据包含属性
    "name": {
      "type": "string", //数据类型
      "minLength": 8,   //数据长度最短范围
      "maxLength": 10   //数据长度最长范围
    },
    "age": {
      "type": "number",
      "description": "your cat's age in years",
      "minimum": 0,    //数据最小值
      "minimum": 5    //数据最大值
    },
    "declawed": {
      "type": "boolean"
    },
    "description": {
      "type": "string"
    }
  },
  "required": [  //必须有的数据
    "name",
    "age",
    "declawed"
  ]
}
```

[json schema lint](https://jsonschemalint.com/)