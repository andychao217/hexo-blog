---
layout: post
title: Capitalizes the first letter of every word in a string
date: 2020-03-12 12:00:00
categories:
    - Python
tags:
    - Python
---

### capitalize_every_word

Capitalizes the first letter of every word in a string.

Uses `str.title` to capitalize the first letter of every word in the string.

```python
def capitalize_every_word(string):
    return string.title()
```

```python
capitalize_every_word('hello world!') # 'Hello World!'
```
