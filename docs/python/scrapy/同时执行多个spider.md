### 一、覆写crawl.py

#### 1.分析

由于单个Spider执行的命令是scrapy crawl spider名称，

查看crawl的源码crawl.py的run方法中提示只能执行一个spider，源码如下：

scrapy/commands/crawl.py

```python
    def run(self, args, opts):
        if len(args) < 1:
            raise UsageError()
        elif len(args) > 1:
            raise UsageError("running 'scrapy crawl' with more than one spider is no longer supported")
        spname = args[0]

        self.crawler_process.crawl(spname, **opts.spargs)
        self.crawler_process.start()
```

那么如果我们获取到所有的spider列表，然后遍历执行，这样即可实现同时执行多个spider。

#### 2.实现

步骤一：在spiders同级目录新建一个目录commands,并新建文件crawlextend.py

步骤二：复制scrapy/commands/crawl.py中所有代码到crawlextend.py中

步骤三：复写run方法如下内容：

```python
    def run(self, args, opts):
        # 获取爬虫列表
        spd_loader_list = self.crawler_process.spider_loader.list()
        # 遍历各爬虫
        for spname in spd_loader_list or args:
            self.crawler_process.crawl(spname, **opts.spargs)
            print('爬虫---', spname,' ---启动')
        self.crawler_process.start()
```

步骤四：在settings.py文件中添加COMMANDS_MODULE = 'geekparity.commands'，其中geekparity为爬虫工程名

步骤五：执行命令：scrapy crawlextend即可



### 二、使用scrapy自带的CrawlerProcess

示例来自官方文档

```python
import scrapy from scrapy.crawler import CrawlerProcess
class MySpider1(scrapy.Spider): 
	# Your first spider definition ...
class MySpider2(scrapy.Spider): 
	# Your second spider definition
	...
# 如果需要使用settings文件里面的请求头，需要引入get_project_settings
process = CrawlerProcess() 
process.crawl(MySpider1) 
process.crawl(MySpider2) 
process.start() # the script will block here until all crawling jobs are finished
```



### 三、使用scrapy自带的CrawlerRunner

示例来自官方文档

```python
import scrapy 
from twisted.internet import reactor 
from scrapy.crawler import CrawlerRunner 
from scrapy.utils.log import configure_logging
class MySpider1(scrapy.Spider): 
	# Your first spider definition ...
class MySpider2(scrapy.Spider): 
	# Your second spider definition ...
configure_logging() 
runner = CrawlerRunner() 
runner.crawl(MySpider1) 
runner.crawl(MySpider2) 
d = runner.join() 
d.addBoth(lambda _: reactor.stop())
reactor.run() # the script will block here until all crawling jobs are finished
```

指定抓取顺序：

```python
from twisted.internet import reactor, defer 
from scrapy.crawler import CrawlerRunner 
from scrapy.utils.log import configure_logging
class MySpider1(scrapy.Spider): 
	# Your first spider definition ...
class MySpider2(scrapy.Spider): 
	# Your second spider definition ...
configure_logging() 
runner = CrawlerRunner()
@defer.inlineCallbacks 
def crawl(): 
	yield runner.crawl(MySpider1) 
	yield runner.crawl(MySpider2) 
	reactor.stop()
crawl() reactor.run() # the script will block here until the last crawl call is finished
```

