---
title: 在 Python 中编写抽象类
urlname: sv1fam
date: 2019-12-17 16:16:30 +0800
tags: []
categories: []
---

- 抽象类方法需要继承子类全部实现
- 抽象类直接实例化、子类不完全实现都会报错

```python
from abc import ABC, abstractmethod

class People(ABC):
    @abstractmethod
    def walk(self):
        pass

    @abstractmethod
    def eat(self):
        pass

    def dance(self):
        print('我正在跳舞')


class Jack(People):
    def walk(self):
        print("Jack is walking")

    def eat(self):
        print("Jack is eating")


```
