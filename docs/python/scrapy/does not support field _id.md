在Scrapy中插入数据到MongoDB时提示以下错误：

```python
Traceback (most recent call last):
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\defer.py", line 653, in _runCallbacks
    current.result = callback(current.result, *args, **kw)
  File "D:\GeekParity\geekparity\geekparity\pipelines.py", line 18, in process_item
    projects.insert_one(item)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\pymongo\collection.py", line 653, in insert_one
    document["_id"] = ObjectId()
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\scrapy\item.py", line 66, in __setitem__
    (self.__class__.__name__, key))
KeyError: 'ProjectItem does not support field: _id'
```

错误原因：

​	MongoDB在插入数据时会尝试或者添加_id这个字段，如果找不到_id这个字段，就会Scrapy提示不支持这个字段。这个错误类似于官方文档这个[示例](https://doc.scrapy.org/en/latest/topics/items.html#setting-field-values)，

```python
>>> product['last_updated'] = 'today'
>>> product['last_updated']
today

>>> product['lala'] = 'test' # setting unknown field
Traceback (most recent call last):
    ...
KeyError: 'Product does not support field: lala'
```

解决办法：

1. 在定义item的时候，添加_id字段
2. 在插入时先将item对象转换成dict实例,如：dict(item)

个人比较喜欢第一种方案，这样就不用多次进行dict转换。