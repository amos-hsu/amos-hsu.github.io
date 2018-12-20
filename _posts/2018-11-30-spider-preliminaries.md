---
layout: post
title: 爬虫的一些预备知识
date: 2018-11-30 21:14:20 +0300
author: 沉一叶
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: software.jpg # Add image post (optional)
img1: scrapy.png
tags: [Scrapy, Python]
categories: Scrapy
---

目录：

* 目录
{:toc}

<!-- GFM-TOC -->
<!-- [一 爬虫能做什么](#爬虫能做什么) -->
<!-- GFM-TOC -->

爬虫是能够自动抓取网络信息的一种程序，是从互联网获取对于我们有价值的信息的第一步。
为了学会有效地获取网络信息，接下来计划使用一个月的时间，进行爬虫的项目实践。

## 爬虫能做什么
1. 搜索引擎---百度、Google、垂直领域搜索引擎
2. 推荐引擎---今日头条
3. 机器学习数据样本
4. 数据分析（如金融数据分析等）、舆情分析

## 技术选型

当然，人生苦短，我用 Python啦。相关的 Pyhton 库或者框架也很多。

对于 scrapy VS request + beautifulsoup
- request 和 beautifulsoup 都是库，而 scrapy 是框架
- scrapy 中可以加入 requests 和 beautifulsoup
- scrapy 基于 twisted，性能是最大的优势
- scrapy 易于扩展，提供了很多内置功能
- scrapy 内置的 css 和 xpath 的 selector 非常方便，beautifulsoup 最大的缺点就是慢。

这里选择了 Scrapy 框架。
Scrapy 使用 Twisted 异步网络库来处理网络通信，可以灵活的完成各种需求。
- 优点：灵活的定制化爬取；文档完善；采用布隆过滤方案进行 URL 去重；可以处理不完整的 HTML等等。
- 缺点： 不支持分布式部署；原生不支持抓取 JavaScript 的页面；全命令行操作，需要一定学习周期。

## 网页分类
- 静态网页：静态博客，网页内容生成后，不再发生变化，如 Github Pages 等
- 动态网页：网页内容生成后，还可以随着时间、环境或者数据库操作的结果而发生变化
- Webservice（Rest API）：电商网站

## 正则表达式
为什么要学习正则表达式？正则表达式规定了一种匹配字符串的模式。用于：
- 测试字符串的模式；
- 替换特定文本；
- 基于模式匹配提取字符串中的特定子串。

元字符：

| 字符 | 类型 | 含义|
|:----:|:---:|:----:|
|.   | |  除\n以外的任意单字符|
|*  | 限定符 | 前面子表达式重复零次或多次，即任意多次，'.*' 匹配任意字符或字符串|
|+  | 限定符 | 前面子表达式重复一次或多次|
|？ | 限定符 | 前面子表达式零次或者一次，或者指定一个非贪婪限定符|
|{ } | 限定符 | 限定符表达式的匹配的次数，如 {m, n}，限定 m-n 个字符|
|^  | 定位符 | 字符串开始位置|
|$  | 定位符 | 字符串结尾位置|
|(pattern) | 定位符 | 字符串开始和结尾位置|
| \| |             | 或    |
|(?:pattern)  |             | 匹配但不获取结果，非获取匹配   |  
|[xyz]|         |字符集合，匹配所包含的任何一个字符|
| \\s  |          | 匹配空格 ，\\s+匹配一个或多个空格，\\S是其非       |
| \\d  |          | 匹配数字 ，\\D是其非       |
| \\w|          |匹配字母、数字、下划线。等价于'[A-Za-z0-9_]' ，\\W是其非|
|\\b  |	      |匹配一个单词边界，即字与空格间的位置|
| \\u4E00-\\u9FA5|   |     匹配汉字    |

'\*' 与 '+' 限定符都是**贪婪**的，因为它们会尽可能多的匹配文字，只有在它们的后面加上一个 '?' 就可以实现非贪婪或最小匹配。

举例：匹配出生年月日，如"2001年6月1日"、"2001/6/2"、"2001-06-01"、"2001-6-1"，
regex_str = ".*(\d{4}[年/-]\d{1,2}([月/-]\d{1,2}|[月/-]\$|\$))"

## 字符编码
- ASCII编码：美国标准编码，1 个字节（8bit，255种）
- Unicode：两个字节，包含了ASCII码
- UTF-8：UTF全称为"Unicode Transformation Format"，UTF-8 是 Unicode 可变长编码，1-4 个字节

Python2  默认ASCII编码，Python3 默认Unicode编码。

## 爬取策略算法
- 网站 URL 的树结构
![](https://upload-images.jianshu.io/upload_images/12927609-536979642544111e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 深度优先遍历算法

对树的每一个分支深入到底，每个结点只访问一次，可以使用栈非递归实现。二叉树：先序遍历、中序遍历、后序遍历。

特点：不全部保留结点，占用空间小；有回溯操作，运行速度慢。

Scrapy默认使用深度优先遍历。

```Python
def depth_tree(node):
    if node is not None:
        print(node.elem)
        if node.left is not None:
            return depth_tree(node.left)
        if node.right is not None:
            return depth_tree(node.right)
```

- 广度优先遍历算法

按树的层次进行遍历，可以使用队列非递归实现。

特点：保留全部结点，占用空间大；无回溯操作，运行速度块。

```Python
def level_search(root):
    if root is None:
        return
    queue = []
    node = root
    queue.append(root)
    while queue:
        node = queue.pop(0)
        print(node.elem)
        if node.lchild is not None:
            queue.append(node.lchild)
        if node.rchild is not None:
            queue.append(node.rchild)
```

## 爬虫去重策略
1. 直接存储
- 将访问过的 URL 保存到数据库中
- 将访问过的  URL 保存到 set 中，只需要 O(1) 的代价就可以查询 URL，但是需要占用内存，在数据量大的时候不适用。1 亿 URL：100000000*2byte*50 = 9G。
2. 哈希存储
- 将URL进行 MD5 等方法哈希后保存到 set 中，MD5 编码占用 128bit，可以将 URL 长度压缩。Scrapy 采用的即这种哈希存储。
- 使用 bitmap 方法，将访问过的 URL 通过哈希函数映射到某一位，但是仍然会有哈希冲突。
- 使用布隆过滤器 bloomfilter 方法对 bitmap 进行改进，利用多重哈希函数降低哈希冲突。


## 最简单的爬虫框架
![迷你爬虫框架](https://upload-images.jianshu.io/upload_images/12927609-5d62f46762941a05.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

框架目录结构
```Bash
config_load.py    配置文件加载
crawl_thread.py    爬取线程
mini_spider.py    主线程
spider.conf    配置文件
url_table.py    url队列、url表
urls.txt    种子url集合
webpage_parse.py    网页分析
webpage_save.py    网页存储
```

配置文件 spider.conf
```Bash
[spider]
url_list_file: ./url.txt;   #种子文件路径
output_dir: ./output;       #抓取结果存储目录
max_depth: 1;               #最大抓取深度，种子为0级
crawl_interval: 1;          #抓取间隔，单位：秒
crawl_timeout: 1;           #抓取超时
target_url： .*.html        #需要存储的目标网页 URL pattern
thread_count: 6;            #抓取线程数
```

爬取过程

- 创建初始 URL 集合，从初始 URL 开始抓取

- 爬取算法采用BFS算法，不停地将解析出的 URL 放入队列中，直到达到指定深度。
```Python
# 向队列填充URLs
cur_depth = 0
depth = conf.max_depth
while cur_depth <= depth:
    for host in hosts:
        url_queue.put(host)
        time.sleep(conf.crawl_interval)
    cur_depth += 1
    web_parse.cur_depth = cur_depth
    url_queue.jion()
    hosts = copy.deepcopy(u_table.todo_list)
    u_table.todo_list = []
```

- 使用 URL 表记录已经遍历过的 URL

- 多个抓取线程：爬虫是 IO 密集任务，多线程可以有效提升效率

- 页面分析

- 分析结果存储

## Scrapy 入门知识

Scrapy 是基于事件驱动网络框架 Twisted 编写的。Twisted 是一个异步阻塞框架。

### Scrapy 框架
<div align="center"><img src="{{"/assets/img/scrapy.png" | prepend: site.github.url }}"></div>

组件

- Engine：引擎负责控制数据流在系统中各个组件中流动，并在相应动作发生时触发事件
- Schedule：调度器从引擎接受 Request 并将它们入队，以便之后引擎请求给他们提供给引擎
- Downloader：下载器负责获取页面数据并提供给引擎，而后提供给Spider
- Spider：Spider 是用户编写的用于分析 Response 并提取 Item 或提取更多需要下载的 URL 的类。每个 Spider 负责处理特定网站
- Item Pipline：负责处理被 Spider 提取出来的 Item。典型的功能有清洗、验证、持久化操作
- Downloader middlewares：下载器中间件是在 Engine 及 Downloader 之间的特定钩子，处理 Downloader 传递给 Engine 的 Response。其提供了一个简便的机制，通过插入自定义代码来扩展 Scrapy 的功能
- Spider middlewares：Engine 及 Spider 之间的特定钩子，处理 Spider 的输入（Response）和输出（Items 及 Requests）。其提供了一个简便的机制，通过插入自定义代码来实现扩展 Scrapy 的功能

数据流
<div align="center"><img src="{{"/assets/img/scrapy-flow.jpg" | prepend: site.github.url }}"></div>

### 入门教程

**创建项目**
```Shell
$ scrapy startproject tutorial
```

**项目目录结构**
```Shell
tutorial/
    scrapy.cfg            # 项目的配置文件
    tutorial/             # 该项目的python模块。之后您将在此加入代码
        __init__.py
        items.py          # 项目中的item文件
        pipelines.py      # 项目中的pipelines文件
        settings.py       # 项目的设置文件
        spiders/          # 放置spider代码的目录
            __init__.py
```

**编写第一个爬虫**

Spider 是用户编写的用于单个网站爬取数据的类。其包含了一个用于下载的初始 URL，以及如何跟进网页中的链接以及如何分析页面中的内容的方法。

创建 Spider，必须继承 scrapy.Spider 类，且定义：
- name：唯一
- start_urls：Spider 启动时进行爬取的 URL 列表。
- parse() 方法：解析每个初始 URL 完成下载后生成的 Response 对象，提取数据以及生成需要进一步处理的 URL 的 Request 对象。

Spider代码：（/tutorail/spiders/quotes_spider.py）
```Python
import scrapy
class QuotesSpider(scrapy.Spider):
    name = "quotes"
    def start_resquests(self):
        urls = {
            'http://quote..'
        }
        for url in urls:
            yield scrapy.Resquest(url=url, callback=self.parse)

    def parse(self, Response):
        page = response.url.split("/")[-2]
        filename = 'quotes-%s.html' % page
        with open(filename, 'wb') as f:
            f.write(response.body)
        self.log('Saved file %s' % filename)
```

**运行爬虫**
```Shell
$ scrapy crawl quotes
```

**保存数据**
```Shell
$scrapy crawl quotes -o quotes.json
```

## 其他工具

### xpath

xpath 是一门在 XML 文档中查找信息的语言。

xpath 有以下特点：
1. xpath 使用路径表达式在 XML 和 HTML 中进行导航
2. xpath 包含标准函数库
3. xpath 符合 W3C 标准

xpath 节点

- 节点类型：元素 属性 文本 命名空间 处理指令 注释 文档根节点
- 节点关系：父 子 同胞 先辈 后代

xpath 语法

使用路径表达式在 XML 文档中选取节点。节点是通过路径或者 step 来选取的。

选取节点：

| 路径表达式 |  描述  |
| :----:  | :---: | 
| article | 选取所有 article 元素的所有子节点|
| /article| 选取根元素 article|
| article/a|选取所有属于 article 的子元素的 a 元素|
| //div   | 选取所有 div 子元素，不管他们在文档中出现的位置|
| article//div| 选取所有属于 article 元素的后代的 div 元素，不管其出现位置|
| @        | 选取属性                   |
| //@class | 选取所有名为 class 的属性 |

谓语：

谓语用来查找某个特定的节点或者包含某个指定的值的节点。谓语被嵌在方括号中。

| 路径表达式 |  描述  |
| :----:  | :---: | 
| /article/div[1] | 选取属于 article 子元素的第一个 div 元素 |
| /article/div[last()] | 选取属于 article 子元素的最后一个 div 元素 |
| /article/div[last()-1] | 选取属于 article 子元素的倒数第二个 div 元素 |
| //div[@lang] | 选取所有拥有 lang 属性的 div 元素 |
| //div[@lang='eng'] | 选取所有 lang 属性为 eng 的 div 元素 |

选取未知节点：

| 路径表达式 |	结果 |
| :---: | :---: |
| /bookstore/*	| 选取 bookstore 元素的所有子元素 |
| //* | 选取文档中的所有元素 |
| //title[@*] | 选取所有带有属性的 title 元素 |
| /div/a \| //div/p | 选取所有 div 元素的 a 和 p 元素 |

### CSS 选择器

在 CSS 中，选择器时一种模式，用于选择需要添加样式的元素。

| 选择器 |	藐视 |
| :---: | :---: |
| *     | 选取所有节点 |
| #container | 选取 id 为 conatainer 的节点 |
| .container | 选取所有 class 包含 container 的节点 |
| li a | 选取所有 li 下的所有 a 节点 |
| ul + p | 选择 ul 后面的所有 p 元素|
| ul ~ p | 选取与 ul 相邻的所有 p 元素 |
| div#container >ul | 选取 id 为 container 的 div 的第一个 ul 子元素 |
| a[title] | 选取所有有 title 属性的 a 元素 |
| a[href=“http://job.com”] | 选取所有 href 属性为... 值的 a 元素 | 
| a[href*=“job”] | 选取所有 href 属性包含 job 的 a 元素 |
| a[href^=“job”] | 选取所有 href 属性以 job 开头的 a 元素 |
| a[href$=“.job”] | 选取所有 href 属性以 .job 开头的 a 元素 |
| input[type=radio]:checked | 选择选中的 radio 的元素 |

举例

```Python
class JobboleSpider(scrapy.Spider)
    def parse(self, response):
        """
        获取页面内容
        """
        
        # 通过xpath选取
        title = response.xpath('//div[@class ="entry-header"]/h1/text()').extract_first("")
        create_date = response.xpath('//p[@class="entry-meta-hide-on-mobile"]/text()').extract_first("").split()[0]
        praise_num = response.xpath('//span[contains(@class, "vote-post-up")]/h10/text()').extract_first("")
        fav_num = response.xpath('//span[contains(@class, "bookmark-btn")]/text()').extract_first("")
        match_re = re.match(".*？(\d+).*", fav_num)
        if match_re:
            fav_num = match_re.group(1)
        else:
            fav_num = 0
        comment_num = response.xpath('//a[@href="#article-comment"]/span/text()').extract_first()
        match_re = re.match(".*？(\d+).*", comment_num)
        if match_re:
            comment_num = match_re.group(1)
        else:
            comment_num = 0
        content = response.xpath("//div[@class='entry']").extract()[0]
        tag_list = response.xpath('//p[@class="entry-meta-hide-on-mobile"]/a/text()').extract()
        tag_list = [element for element in tag_list if not element.strip().endswith("评论") ]
        tags = ",".join(tag_list)

        #通过CSS选择器，CSS选择器更加简洁
        title = response.css(".entry-header h1::text").extractextract_first("")
        create_time = response.css("p.entry-meta-hide-on-mobile::text").extract_first("").splite()[0]
```
