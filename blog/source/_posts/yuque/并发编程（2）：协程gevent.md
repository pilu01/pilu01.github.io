---
title: 并发编程（2）：协程gevent
urlname: aybe8d
date: 2020-07-22 16:39:33 +0800
tags: []
categories: []
---

高并发编程时，采用多线程（或进程）是一种不可取的解决方案，因为线程（或进程）本质上都是操作系统的资源。每个线程都是需要额外占用内存的，由于线程的调度由操作系统完成，调度器会因为时间片用完等原因强制夺取某个线程的控制权，开发者还需要考虑加锁、使用队列等操作，这些都容易造成高并发情况下的性能瓶颈。

协程是用户空间线程，复杂的逻辑和异步都封装在底层，开发者还是在使用同步的方式编程，但这种协作式的任务调度可以让用户自己控制使用 CPU 的时间，除非自己放弃，否则不会被其他协程抢夺到控制权。

信号量（Semaphore）可以用来保证多协程（或者线程）并发访问共享资源时不会发生冲突

```python
import gevent

def a():
    print('Start a')
    gevent.sleep(1)
    print('End a')

def b():
    print('Start b')
    gevent.sleep(2)
    print('End b')

gevent.joinall([
    gevent.spawn(a),
    gevent.spawn(b),
])
# 协程，总共耗时 2秒

# 通过信号量控制共享数据访问
from gevent.coros import BoundedSemaphore
sem = BoundedSemaphore(1)
IN_POOL_TASKS = []
def put_new_page(p, queue):
    global IN_POOL_TASKS
    sem.acquire()
    sleep(0)
    if p not in IN_POOL_TASKS:
        queue.put(p)
        IN_POOL_TASKS.append(p)
    sem.release()


```

一，猴子补丁
**from **gevent **import **monkey
monkey.patch_all()
monkey patch 指的是在运行时动态替换,一般是在 startup 的时候.
;把标准库中的 thread/socket 等给替换掉.这样我们在后面使用 socket 的时候可以跟平常一样使用,无需修改任何代码,但是它变成非阻塞的了.

1. 协程，并发访问目标 url
2. 保存需要数据响应，失败则删除代理，继续测试，5 次以上 url 存入队列
