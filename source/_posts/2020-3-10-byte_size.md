---
layout: post
title: Returns the length of a string in bytes
date: 2020-03-10 12:00:00
categories:
    - Python
tags:
    - Python
---

### byte_size

Returns the length of a string in bytes.

`utf-8` encodes a given string and find its length.

```python
def byte_size(string):
    return(len(string.encode('utf-8')))
```

```python
byte_size('😀') # 4
byte_size('Hello World') # 11
```