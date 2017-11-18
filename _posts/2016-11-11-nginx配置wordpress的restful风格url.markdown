---
author: wanshicheng
date: 2016-11-11 12:10:15+00:00
layout: post
title: Nginx配置Wordpress的Restful风格url
cover: 'http://file.wanshicheng.org/assets/img/postcover.jpg'
categories: linux
tags: Linux Nginx
---

开始做seo的优化，当然牵扯到固定链接，wordpress提供多种类型的链接形式：

```vim
/%year%/%monthnum%/%day%/%postname%/
/%year%/%monthnum%/%postname%/
/%year%/%monthnum%/%day%/%postname%.html
/%year%/%monthnum%/%postname%.html
/%category%/%postname%.html
/%post_id%.html
/%postname%/
```

我选择了/%postname%伪静态，虽然现在貌似没什么差别了，但还是该下吧。下面就出现了修改固定链接后，访问文章会出现404错误。

Wordpress官方给出了新的开启固定链接的方法，非常简单的。将下列代码粘贴到nginx的conf配置文件里。

```vim
location/{
try_files$uri$uri//index.php?$args;
}
rewrite/wp-admin$$scheme://$host$uri/ permanent;
```

接着重启nginx就可以正常访问了!!!
