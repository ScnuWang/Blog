在给scrapy添加定时抓取模块的时候引入APScheduler模块时，

整合代码：

```
def crawlxiaomi():
    runner = CrawlerRunner(get_project_settings())
    runner.crawl(XiaomiSpider)
    runner.crawl(WangyiSpider)
    d = runner.join()
    d.addBoth(lambda _: reactor.stop())
    reactor.run()

if __name__ == '__main__':
    sched = BlockingScheduler()
    sched.add_job(crawlxiaomi,'interval',seconds=10)
    sched.start()
```

提示如下错误信息：

```
Unhandled Error
Traceback (most recent call last):
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\base.py", line 428, in fireEvent
    DeferredList(beforeResults).addCallback(self._continueFiring)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\defer.py", line 321, in addCallback
    callbackKeywords=kw)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\defer.py", line 310, in addCallbacks
    self._runCallbacks()
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\defer.py", line 653, in _runCallbacks
    current.result = callback(current.result, *args, **kw)
--- <exception caught here> ---
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\base.py", line 441, in _continueFiring
    callable(*args, **kwargs)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\base.py", line 1238, in _reallyStartRunning
    self._handleSignals()
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\posixbase.py", line 295, in _handleSignals
    _SignalReactorMixin._handleSignals(self)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\base.py", line 1203, in _handleSignals
    signal.signal(signal.SIGINT, self.sigInt)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\signal.py", line 47, in signal
    handler = _signal.signal(_enum_to_int(signalnum), _enum_to_int(handler))
builtins.ValueError: signal only works in main thread

2018-07-23 16:24:22 [twisted] CRITICAL: Unhandled Error
Traceback (most recent call last):
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\base.py", line 428, in fireEvent
    DeferredList(beforeResults).addCallback(self._continueFiring)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\defer.py", line 321, in addCallback
    callbackKeywords=kw)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\defer.py", line 310, in addCallbacks
    self._runCallbacks()
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\defer.py", line 653, in _runCallbacks
    current.result = callback(current.result, *args, **kw)
--- <exception caught here> ---
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\base.py", line 441, in _continueFiring
    callable(*args, **kwargs)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\base.py", line 1238, in _reallyStartRunning
    self._handleSignals()
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\posixbase.py", line 295, in _handleSignals
    _SignalReactorMixin._handleSignals(self)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\base.py", line 1203, in _handleSignals
    signal.signal(signal.SIGINT, self.sigInt)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\signal.py", line 47, in signal
    handler = _signal.signal(_enum_to_int(signalnum), _enum_to_int(handler))
builtins.ValueError: signal only works in main thread
```

```
2018-07-23 15:42:26 [apscheduler.executors.default] ERROR: Job "crawlxiaomi (trigger: interval[0:01:00], next run at: 2018-07-23 15:43:26 CST)" raised an exception
Traceback (most recent call last):
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\apscheduler\executors\base.py", line 125, in run_job
    retval = job.func(*job.args, **job.kwargs)
  File "D:\GeekParity\geekparity\run.py", line 18, in crawlxiaomi
    reactor.run()
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\base.py", line 1242, in run
    self.startRunning(installSignalHandlers=installSignalHandlers)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\base.py", line 1222, in startRunning
    ReactorBase.startRunning(self)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\twisted\internet\base.py", line 730, in startRunning
    raise error.ReactorNotRestartable()
twisted.internet.error.ReactorNotRestartable
```

解决办法：使用TwistedScheduler调度器，不知道是否是因为，Scrapy是基于Twisted开发的

整合代码：

```
def crawlxiaomi():
    runner = CrawlerRunner(get_project_settings())
    runner.crawl(XiaomiSpider)
    runner.crawl(WangyiSpider)
    d = runner.join()
    d.addBoth(lambda _: reactor.stop())


if __name__ == '__main__':
    sched = TwistedScheduler()
    sched.add_job(crawlxiaomi,'interval',seconds=60*1)
    sched.start()
    
     try:
        reactor.run()
    except Exception:
        pass
```

**上面这种方式有个问题，就是定时任务只执行一次。**暂不知道原因，后续跟进！！！

解决方案：再加入多进程

```python
sched = BlockingScheduler()

def crawl():
    process = CrawlerProcess(get_project_settings())
    process.crawl(XiaomiSpider)
    process.crawl(WangyiSpider)
    process.start()

@sched.scheduled_job("interval",seconds=60)
def run():
    process = multiprocessing.Process(target=crawl)
    process.start()


if __name__ == '__main__':
    sched.start()
```

参考：[用apscheduler 和 scrapy 做定时抓取爬虫为什么只爬取第一次？](https://www.zhihu.com/question/56701176/answer/340272979 )