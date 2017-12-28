---
layout: post
author: 万仕诚
title: '使用Python多进程解析今日头条自定义的Json爬取图片'
date: 2017-12-08
cover: 'http://file.wanshicheng.org/assets/img/postcover.jpg'
categories: python
tags: python json 多进程
---
今日头条为了防止别人爬取它的图片，也是想了不少办法。首先，它的图片采用的 ajax 请求得到的，它并没有把图片地址直接放在 html 标签里，而是放在一段隐藏较深的 Json 里面。然后又自定义了图片地址的这段Json，不过这也仅仅是对新手有效。

![](http://file.wanshicheng.org/assets/img/spider01.png)

其实一行代码也就够了。

```python
result.group(1).replace('\\\"', '\"').replace('\\\\/', '/')
```

![](http://file.wanshicheng.org/assets/img/spider02.png)

20多分钟的战果。

#### 总结

对于爬虫的数据库首选 MongDB，相比 MySQL，SQL Server等，它不需要预先定义表结构，这样可以根据目标网站的结构随时储存你想要的数据。Python 的多线程不支持多核CPU，这对计算密集型的任务是很不利的，对于爬虫这种IO密集型的作业其实没有太多影响，Python 的多进程可以运用的场景更多，只不过对资源的开销很大。

[Demo地址](https://github.com/wanshicheng/spider-tour/tree/master/Jiepai)
