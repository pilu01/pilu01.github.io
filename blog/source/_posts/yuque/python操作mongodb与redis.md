---
title: python操作mongodb与redis
urlname: egavm9
date: 2020-01-03 09:39:44 +0800
tags: []
categories: []
---

一，Python 操作 mongodb，针对增删改查

| 操作符             | 意义     |
| ------------------ | -------- |
| \$gt               | 大于     |
| \$gte              | 大于等于 |
| \$lt               | 小于     |
| \$lte              | 小于等于 |
| \$ne               | 不等于   |
| 集合（collection） | 表       |
| 文档（document）   | 行       |

```python
from pymongo import MongoClient
client = MongoClient('db_url')
database = client.数据库名
collection = database.集合名

# 增
collection.insert_many([
{'a': 1, 'b': 2},
{'a':3, 'b': 4}
])
collection.insert_one({})

# 删
# 删除年龄为0的数据
collection.delete_many({'age': 0})


# 改
# 存在则更新，不存在插入
collection.update_many(
	{'name': '公孙小八'},
    {'$set': {'address': '美国', 'age': 80}},
    upsert=True
)


# 查
# 查询年龄大于21小于25，并且name 不等于 小七,
# 不返回 address和 age 字段
rows = collection.find(用于过滤记录的字典，用于限定字段的字典)
rows = cpllection.find({
	'age': {'$lt':25, '$gt':21},
    'name': {'$ne': '小七'}
}, {
	'address': 0,
    'age': 0
})
```
