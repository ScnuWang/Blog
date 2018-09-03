Windows操作系统下scrapy 在启动的时候提示如下错误信息：

```
Process Process-1:
Execution of job "run (trigger: cron[minute='*/1'], next run at: 2018-09-03 16:37:00 CST)" skipped: maximum number of running instances reached (1)
Traceback (most recent call last):
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\multiprocessing\process.py", line 258, in _bootstrap
    self.run()
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\multiprocessing\process.py", line 93, in run
    self._target(*self._args, **self._kwargs)
  File "D:\GeekParity\geekparity\run.py", line 12, in crawl
    process = CrawlerProcess(get_project_settings())
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\scrapy\utils\project.py", line 63, in get_project_settings
    init_env(project)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\scrapy\utils\conf.py", line 84, in init_env
    cfg = get_config()
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\scrapy\utils\conf.py", line 98, in get_config
    cfg.read(sources)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\configparser.py", line 697, in read
    self._read(fp, filename)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\configparser.py", line 1015, in _read
    for lineno, line in enumerate(fp, start=1):
UnicodeDecodeError: 'gbk' codec can't decode byte 0xae in position 215: illegal multibyte sequence
```

用IDE调试了半天，原来是因为在启动的时候，会读取scrapy.cfg文件，但是这个文件里面又有中文注释，导致，一直提示编码问题。

解决办法：去掉scrapy.cfg文件中的中文注释

分析：

在上面寻找原因的过程中，发现conf.py文件中的get_config()方法在调用configparse.py文件中的read()方法时，未提供编码格式，所以默认的编码格式就编程None，取系统默认的编码格式（Windows：gbk/Linux:utf-8）。所以出现，Linux服务器正常，Windows异常的情况。