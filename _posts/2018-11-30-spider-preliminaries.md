---
layout: post
title: Python 爬虫实践 1 - 爬虫预备知识
date: 2018-11-30 21:14:20 +0300
author: 沉一叶
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: Software.jpg # Add image post (optional)
tags: [Scrapy, Python]
categories: Scrapy
---
爬虫是能够自动抓取网络信息的一种程序，是从互联网获取对于我们有价值的信息的第一步。
为了学会有效地获取网络信息，接下来计划使用一个月的时间，进行爬虫的项目实践。

第一部分，是开展爬虫工作的一些预备知识。

目录：

* 目录
{:toc}

<!-- GFM-TOC -->
<!-- [一 爬虫能做什么](#爬虫能做什么) -->
<!-- GFM-TOC -->
## 爬虫能做什么
1. 搜索引擎---百度、Google、垂直领域搜索引擎
2. 推荐引擎---今日头条
3. 机器学习数据样本
4. 数据分析（如金融数据分析等）、舆情分析

## 技术选型
这里使用的是 Python 爬虫技术。

scrapy VS request+beautifulsoup
- request 和 beautifulsoup 都是库，而 scrapy 是框架
- scrapy 中可以加入 requests 和 beautifulsoup
- scrapy 基于 twisted，性能是最大的优势
- scrapy 易于扩展，提供了很多内置功能
- scrapy 内置的 css 和 xpath 的 selector 非常方便，beautifulsoup 最大的缺点就是慢。

## 网页分类
1. 静态网页：静态博客，如 Github Pages 等
2. 动态网页
3. Webservice（Rest API）：电商网站

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

## 算法
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
1. 将访问过的 URL 保存到数据库中
2. 将访问过的  URL 保存到 set 中，只需要 O(1) 的代价就可以查询 URL，但是需要占用内存，在数据量大的时候不适用。1 亿 URL：100000000*2byte*50 = 9G
3. 将URL进行MD5等方法哈希后保存到set中，MD5编码占用128bit，可以将set长度压缩。（Scrapy）
4. 使用 bitmap 方法，将访问过的 URL 通过哈希函数映射到某一位，但是哈希冲突比较严重。
5. 使用 bloomfilter 方法对 bitmap 进行改进，多重哈希函数降低哈希冲突。