## 1、Scrapy

​	是一个爬虫框架，提取结构性的数据。其可以应用在数据挖掘，信息处理等方面。提供了许多的爬虫的基类，帮我们更简便使用爬虫。基于Twisted

## 2、scrapy

​	首先安装依赖库Twisted

​	pip install （依赖库的路径）

​	在这个网址http://www.lfd.uci.edu/~gohlke/pythonlibs#twisted 下面去寻找符合你的python版本和系统版本的Twisted

​	然后在安装scrapy框架

​	pip install scrapy

​	http://www.lfd.uci.edu/-gohlke/pythonlibs#twisted

​	【注意】路径名不能有中文，不能用管理员进入cmd，电脑系统用户路径不能是中文



## 3、框架简介

​	该框架是一个第三方的框架，许多功能已经被封装好（比如：下载功能）

​	由五部分构成：

​	引擎、下载器、爬虫、调度器、管道（item和pipeline）

​	以上五部分我们只需要关系其中的两部分：爬虫和管道

​	spiders：蜘蛛或爬虫，我们分析网页的地方，我们主要的代码写在这里

​	管道：包括item和pipeline，用于处理数据

​	其他部分：了解

​	引擎：用来处理整个系统的数据流，触发各种事务（框架的核心）

​	下载器：用于下载网页内容，并且返回给蜘蛛（下载器基于Twisted的高效异步模型）

​	调度器：用来接收引擎发过来的请求，压入队列中等处理任务

## 4、简单用法

​	（1）创建项目

	指令：scrapy startproject 项目名
	（2）项目目录结构
		firstSpider
			firstSpider
				spiders           爬虫目录（写代码位置）
					__init__.py
					myspider.py       爬虫文件,以后的爬虫代码写在这里
				__init__.py
				items.py          	  定义数据结构地方
				middlewares.py    中间件（了解）
				pipelines.py      管道文件
				settings.py       项目配置文件
			scrapy.cfg
	
		项目创建处理，里面是没有爬虫的，我们需要通过指令来创建一个爬虫：
		cd firstSpider/firstSpider
		scrapy genspider qiubai “www.qiushibaike.com"
		以上指令完事后，就会在firstSpider/firstSpider/spiders里面自动创建一个qiubai.py
		name: 爬虫的名字，启动的时候根据爬虫的名字启动项目
		allowed_domains：允许的域名，就是爬取的时候这个请求要不要发送，如果是该允许域名之下的url，就会发送，如果不是，则过滤掉这个请求，这是一个列表，可以写多个允许的域名
		start_urls：爬虫起始url，是一个列表，里面可以写多个，一般只写一个
		def parse(self, response): 这个函数非常重要，就是你以后写代码的地方，parse函数名是固定的，当收到下载数据的时候会自动的调用这个方法，该方法第二个参数为response，这是一个响应对象，从该对象中获取html字符串，然后解析之。【注】这个parse函数必须返回一个可迭代对象
	（3）定制item.py，其实就是您的数据结构，格式非常简单，复制粘贴即可
	（4）撰写蜘蛛
		打印response对象，简单跑一把
		指令：cd firstSpider/firstSpider/spiders
			scrapy crawl qiubai
		【注意】抓取的时候会出错
		pip install pypiwin32


		根据response获取网页内容
			response.text    字符串类型
			response.body    二进制类型
	（5）运行
			scrapy crawl qiubai -o qiubai.json
			scrapy crawl qiubai -o qiubai.xml
			scrapy crawl qiubai -o qiubai.csv
用Scrapy写爬虫的一步骤：

1）创建项目 scrapy startproject 项目名

2）创建爬虫 scrapy genspider 爬虫名 域名

​		运行爬虫 scrapy crawl 爬虫名 [-o xx.json/xml/csv]

3）根据需求编写item

4）在spiders里面解析数据

5）在管道中处理解析完的数据

## 5、原理

见图





