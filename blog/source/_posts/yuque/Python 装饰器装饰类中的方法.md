---
title: Python 装饰器装饰类中的方法
urlname: ypueow
date: 2019-12-17 15:24:09 +0800
tags: []
categories: []
---

```python
# 增加一个self, 出现异常，调用类另一个方法
def catch_exception(origin_func):
    def wrapper(self, *args, **kwargs):
        try:
            u = origin_func(*args, **kwargs)
            return u
        except Exception:
            self.revive()
            return 'an Exception raised.'
        return wrapper

class Test(object):
    def __init__(self):
        pass

    def revive(self):
        print('revive from exception.')

    @catch_exception
    def read_value(self):
        print('here I will do something.')


# 通过添加一个self参数，类外面的装饰器就可以直接使用类里面的各种方法，也可以直接使用类的属性。

```
