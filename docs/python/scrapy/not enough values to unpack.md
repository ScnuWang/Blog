使用FormRequest获取小米有品产品数据时提示：

```python
2018-07-18 14:53:45 [twisted] CRITICAL: 
Traceback (most recent call last):
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\defer.py", line 1386, in _inlineCallbacks
    result = g.send(result)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\scrapy\crawler.py", line 81, in crawl
    start_requests = iter(self.spider.start_requests())
  File "D:\GeekParity\geekparity\geekparity\spiders\xiaomi_spider.py", line 14, in start_requests
    FormRequest(url='https://youpin.mi.com/app/shopv3/pipe',formdata=form,callback=self.parse)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\scrapy\http\request\form.py", line 31, in __init__
    querystr = _urlencode(items, self.encoding)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\scrapy\http\request\form.py", line 66, in _urlencode
    for k, vs in seq
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\scrapy\http\request\form.py", line 65, in <listcomp>
    values = [(to_bytes(k, enc), to_bytes(v, enc))
ValueError: not enough values to unpack (expected 2, got 1)
```

由于最开始的form直接给的字符串，所以不能识别，需要转换成dict对象