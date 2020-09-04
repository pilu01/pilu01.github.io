---
title: records 源码分析
urlname: xgy79u
date: 2019-12-24 09:51:52 +0800
tags: []
categories: []
---

先来一波魔法方法，

```
__init__, __new__ 构造函数，先__new__, 后__init__
__del__  对象生命周期结束后，执行的操作

属性访问控制
__getattr__(self, name) #访问不存在属性时的行为
__setattr__(self, name) 对属性赋值和修改

```
