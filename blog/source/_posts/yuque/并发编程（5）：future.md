---
title: 并发编程（5）：future
urlname: vfxhe0
date: 2020-07-23 10:06:57 +0800
tags: []
categories: []
---

concurrent.futures 中包含 ThreadPoolExecutor 和 ProcessPoolExecutor 这两个执行器，分别产生线程池和进程池。我们把清理不可用代理的代码改成使用 ThreadPoolExecutor 模式

```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor

def check_proxy(p):
    try:
        fetch('http://baidu.com', proxy=p['address'])
    except RequestException:
        p.delete()
        return False
    return True


def use_thread_pool_executor():
    with ThreadPoolExecutor(max_workers=5) as executor:
        for p in Proxy.objects.all():
            executor.submit(check_proxy, p)


 """
 max_workers参数定义了供执行器使用的线程数量。
 executor.submit表示手动提交单个任务，submit执行的返回值是一个concurrent.futures.Future实例，
 如果调用Future实例的result方法就可以获得结果，结果被返回之前进程是阻塞的。
 还可以给Future实例添加回调函数
 """


from functools import partial
# 偏函数，固定一个参数，返回新函数

def when_done(p, f):
    print '[{}]:{}'.format(p.address, 'succeed' if f.result() else 'failure')


def use_thread_pool_executor_with_cb():
    with ThreadPoolExecutor(max_workers=5) as executor:
        for p in Proxy.objects.all():
            result=executor.submit(check_proxy, p)
            result.add_done_callback(partial(when_done, p))
```
