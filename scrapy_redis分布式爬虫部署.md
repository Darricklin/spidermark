## 1、Linux下安装scrapy

​		如果没有pip先下载

​		sudo apt-get install python-pip

​		scrapy框架有可能依赖于下面的两个库

​		sudo apt-get install python-dev

​		sudo apt-get install libevent-dev

​		pip install scrapy 

## 2、redis在linux和windows下的配置

**总体思路：在linux下开启redis服务，在windows终端连接linux的redis，然后打开可视化界面。在linux的pycharm中连接redis数据库，通过对数据的增删改查，在windows的可视化中会同步显示。**

**注意：linux和windows的redis配置文件中都需要对可连接ip和protected mode进行禁用设置，这样windows连接linux的redis时才可以联通。**

Linux下安装：

​		cd  压缩包的路径下(cd  Desktop)

​		tar  -zxvf  redis-3.2.8.tar.gz

​		cp  -r  ./redis-3.2.8  /usr/local/redis  (如果被拒绝，用sudo cp ...)

​		sudo make install 

​			如果有错，请输入指令：sudo make MALLOC=libc

​		运行起来服务器：

​		cd  /usr/local/redis/src

​		./redis-server

​		服务器启动以后，该redis数据就可以处理数据了；操作该数据库需要打开其客户端

​		另开一个终端

​		cd  /usr/local/redis/src

​		./redis-cli

​		127.0.0.1:6379>ping	

​		PONG 说明服务器打开成功

​		关闭服务器

​		在客户端输入命令：shutdown  [nosave|save]  

​		【注意】如果是远程连接的客户端，则不能shutdown



​		远程访问：redis-cli  -h  xxx.xx.xx.xx  -port  6379 -a password

​				redis-cli -h 10.36.135.43

​			在windows中，redis-server默认不允许远程访问，可以在配置文件中打开

​			配置即是redis目录下的redis.windows.conf 

​			然后修改如下：

​			56行注释掉  即： # bind 127.0.0.1

​			75行把后面值改成 no  即：protected-mode no

​			然后把服务器重启即可



## 3、redis介绍

​		Redis 是完全开源免费的，遵守BSD协议，是一个高性能的key-value数据库

​		Redis与其他key-value缓存机制相比有一下特点：

​			Redis支持数据的持久化，可以将内存中的数据保存到磁盘

​			Redis不仅支持key-value类型的数据，同时还支持list、set、zset、hash

​			Redis支持数据的备份，即master-slave模式的数据备份

​		

​	支持的五大数据类型：string、hash、list、set、zset

​	如何将数据存入Redis数据？通过指令来存（每种数据类型存储都对应着一种指令）

​	1）字符串：string是redis的最基本的类型，redis默认最大能够存储512M的数据   字符串的指令是set和get

​		（1）设置键值

​			普通键值：set key value (如：set name  'wuyali')

​			键的生存时间：setex key seconds 10  (如：setex sex 10 'M')

​			 设置多个键值：mset  key value  [key value ...]

​		(2) 获取 ：get key

​		     获取多个：get key [key ... ]

​		(3) 运算 ：要求值是字符串类型的数字

​			加1：incr  key

​			减1：decr key

​			加若干：incrby key intnum

​			减若干：decrby key intnum

​		(4) 其他

​			追加值： appned key value

​			获取值得长度：strlen key

​	2、key的操作

​		1）查找键，参数支持正则

​			keys pattern

​		2）判断是否存在

​			exists key

​		3）查看key对应的value的类型

​			type key

​		4）删除键及其对应的值

​			del key [key ... ]

​		5）设置过期

​			expire key seconds

​		6）查看有效时间

​			ttl key

​	3、hash

​		hash在这里主要是存储对象（或者字典）

​		{	

​			name:"JackMa",

​			age:18

​		}

​		指令：hset

​		1、设置

​			设置单值：hset key field value （此时key代表数据中的键，值就是field value这个键值对）

​			设置多值：hmset key field value [field value]

​		2、获取

​			获取一个属性的值：hget key field

​			获取多个属性的值：hmget key field [field...]

​			获取key对应的所有的属性和值：hgetall key

​			获取所有的属性：hkeys key

​			获取所有的值：hvals key 

​			其他指令：

​			hlen key 

​			hexiste key field

​			hdel key field

​	4、list类型： 列表的元素类型是string

​			["qwer",'asdw','1234']

​		1、设置

​			在头部插入：lpush key value [ value....]

​			在尾部插入：rpush key value [vlaue ...]

​			在一个元素的前部|后部插入：linsert key before|after pivot value   (如：linsert stars before 'A' 'B')

​			设置指定索引的元素值

​			lset key index value

​		2、获取

​			1）移除并返回key对应的list的第一个元素：lpop key

​			2）移除并返回key对应的list的嘴一个元素：rpop key

​			3）返回存储在key的列表中的指定范围的元素：lrange key start end 

​			llen key

​			lindex key index

​			ltrim key start end



​	5、set ： 无序集合，元素类型为string类型，元素不能重复

​			添加元素：sadd key member [member ... ]

​			获取：

​        			根据可以返回集合所有的元素：smembers key

​				返回几个的元素个数：scard  key

​			运算

​				交集：sinter key [key ... ]

​				并集：sunion key [ key ... ]				

​				差集：sdiff key [key ...]

​				判断元素是否在集合中：sismember key member

​		6、zset ： 有序集合，元素类型string，元素具有唯一性

​				添加： zadd key score member [score member]

 					（score 优先级别  索引顺序从0开始排）

​				获取：zrange key star end

​				zcard key 获取个数

​				score的值在min和max之间的元素的个数： zcount key min max    		

​			【注】在一个Redis链接中总共有16个数据，我们可以自主选择，指令如下：	

​			select  0-15



## 4、分布式使用	

​	scrapy_redis组件

​	pip install scrapy_redis

  	1、scrapy和scrapy_redis的区别

​		scrapy是一个通用的爬虫框架，不支持分布式

​		scrapy_redis就是为实现scrapy的分布式而诞生的，它里面提功了redis的组件，通过这些redis组件，就可以实现分布式

​	2、官网案例

​		http://github.com/rmax/scrapy-redis

​		三个样本

​		dmoz.py    传统的CrawlSpider，目的就是把数据保存在redis，运行方式		指令：scrapy crawl dmoz

​		myspider_redis.py 继承自RedisCrawlSpider，start_url被redis_key给取代了，其他地方不变


分布式爬虫开发的步骤：	

​	把原来普通的部署在分布式的系统上运行，就构成了分布式爬虫

​	1、环境部署

​		scrapy： 爬虫运行的基础  (如果服务端不参与分布式的爬取可以不装)

​		scrapy_redis ：组件（也可以认为是scrapy和redis交互的中间件），用于把scrapy爬虫和redis数据库联系起来

​		redis服务器：用于存储分布式爬虫爬取到的数据，一般情况下我们将redis服务器部署在Linux系统上，Windows上也可安装（但是这个服务不能用于生产环境）；无论是在Linux上还是在Windows上,都必须配置其能够远程访问



​	2、测试redis服务器是否联通

​		如果ping了不PONG

​		1）服务器没有配置远程连接

​		2）服务器崩溃

​		3）服务器出现冲突 config set stop-writes-on-bgsave-error no	

​		4）其他意外情况：http://www.baidu.com



​	3、在普通的scrapy爬虫框架下去爬取，保证爬虫爬取的数据没有错误再去分布式系统上部署

​		测试数据格式是否正确

​		先测：json   目的主要是看代码是否有误 【比如：用xpath的时候路径写错等】

​		再测redis  目的主要是看一下有木有系统级的错误 【如：redis服务器崩溃】

4、部署分布式

​	服务器端（master端）：

​	可以用某一台主机作为redis服务器的运行方（即服务端），也称为master

​	客户端（slaver端）：

​	1）把普通爬虫修改成分布式，去掉start_urls(不让slaver随意的执行)，替换成redis_key（为了让master能够控制slaver的爬去）

	2）修改自己配置文件，即在配置文件中加上scrapy_redis的组件

5、爬去
	slaver端：首先要运行起来，等待master发出起始地址再开始
	master端：在适当的时候想redis服务器中放入一个起始地址的列表

