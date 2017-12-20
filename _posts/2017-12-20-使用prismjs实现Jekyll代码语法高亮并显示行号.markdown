---
layout: post
author: 万仕诚
title: '使用prismjs实现Jekyll代码语法高亮并显示行号'
date: 2017-12-20
cover: 'http://file.wanshicheng.org/assets/img/postcover.jpg'
categories: jekyll
tags: jekyll prismjs
---
在 jekyll 上设置代码语法高亮有好几种选择，然而要使代码能显示行号似乎有些困难，至少在百度上是很难找到满意的答案的。

其实，jekyll 本身是可以开启行号的，但是这样是不利于网页的美化的。由于我们无法控制 jekyll 输出的内容，当在页面使用其它语法高亮插件时，就会出现格式错误。那就只能让 jekyll 输出不带行号的代码块，直接让 js 插件生成行号。

prismjs 是一款轻量级可扩展的代码语法高亮库，它的使用非常简单，只需页面中引入 prism.css 和 prism.js 文件就能够轻易的将其整合进项目中使用：

```html
<!DOCTYPE html>
<html>
<head>
	...
	<link href="prism.css" rel="stylesheet" />
</head>
<body>
	...
	<script src="prism.js"></script>
</body>
</html>
```
prismjs 的配置是直接[官网下载页面](http://prismjs.com/download.html)进行的，它有其中主题样式，支持目前各种主流的语言，还有几十种插件。为了显示行号，一定要将“Line Numbers”这个插件勾上。

根据官方文档，还需要给 pre 标签的 class 属性添加“line-numbers”值，让其显示行号，给 style 属性添加 “white-space: pre-wrap”值，让其自动换行。由于我在页面中使用了 jQuery，可以直接使用下面的代码实现：

```html
<script type="text/javascript">
  $('pre').addClass("line-numbers").css("white-space", "pre-wrap");
</script>
```

参考资料：

[Prism 官网](http://prismjs.com/)

[Line Numbers](http://prismjs.com/plugins/line-numbers/)

[GitHub](https://github.com/PrismJS/prism)
