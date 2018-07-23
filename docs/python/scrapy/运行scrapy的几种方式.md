1. 命令窗口使用 scrapy crawl  spider名称

2. 新建py文件，导入from scrapy.cmdline import execute

   > ```python
   > execute('scrapy crawl wangyi'.split())  # 等效于execute('scrapy,crawl,wangyi'.split(,))
   > ```

3. 使用CrawlerProcess

   > ```python
   > from scrapy.crawler import CrawlerProcess 
   > from scrapy.utils.project import get_project_settings
   > # 引入设置文件中的配置，比如user-agent
   > process = CrawlerProcess(get_project_settings())
   > # 'followall' 是project中已经定义了的spider的name
   > process.crawl('followall', domain='scrapinghub.com') 
   > process.start() # 脚本将在此处等待，直到爬取任务完成
   > ```

4. 使用CrawlerRunner

   > ```python
   > from twisted.internet import reactor 
   > import scrapy 
   > from scrapy.crawler import CrawlerRunner 
   > from scrapy.utils.log import configure_logging
   > from scrapy.utils.project import get_project_settings
   > class MySpider(scrapy.Spider): 
   >     # 定义你自己的spider
   > configure_logging({'LOG_FORMAT': '%(levelname)s: %(message)s'}) 
   > runner = CrawlerRunner(get_project_settings())
   > d = runner.crawl(MySpider) # 也可以是spider的name
   > d.addBoth(lambda _: reactor.stop()) 
   > reactor.run() # 脚本将在此处等待，直到爬取任务完成
   > ```