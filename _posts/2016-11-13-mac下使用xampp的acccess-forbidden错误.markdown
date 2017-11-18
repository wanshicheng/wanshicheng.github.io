---
author: 万仕诚
date: 2016-11-13
layout: post
title: Mac下使用xampp的Acccess forbidden错误
cover: 'http://file.wanshicheng.org/assets/img/postcover.jpg'
categories: linux
tags: mac xampp
---

万事开头难，安装完xampp后，一直如下报错：

**Access forbidden!**

You don’t have permission to access the requested directory. There is either no index document or the directory is read-protected.
If you think this is a server error, please contact the webmaster.

**Error 403**

[wanshicheng.org](http://wanshicheng.org/)
Apache/2.4.17 (Unix) OpenSSL/1.0.1i PHP/5.6.15 mod_perl/2.0.8-dev Perl/v5.16.3

查了半天资料，都是乱说，后来看到了JONATHAN NICOL的文章，终于弄出来了，整理一下。

第一步，配置本地hosts

```shell
sudo vi /etc/hosts
```

在后面添加你的域名(wanshicheng.org)

```vim
127.0.0.1    wanshicheng.org
```

第二步 启用apache的虚拟主机功能

打开配置文件

```shell
vi /Applications/XAMPP/xamppfiles/etc/httpd.conf
```

找到下面这一行，去掉最前面的＃

```vim
#Include    /Applications/XAMPP/etc/extra/httpd-vhosts.conf
```

第三步 配置虚拟主机

首先打开配置文件

```shell
vi /Applications/XAMPP/etc/extra/httpd-vhosts.conf
```

清空里面的内容，因为是新安装的，不清空也可以

先把localhost的virsualhost配置一下，不然会出错，一般都是卡在这里了。我的网站编辑器有问题，查看页面源码可以看到配置文件。

默认虚拟主机

```vim
# localhost
<VirtualHost *:80>
    ServerName localhost
    DocumentRoot "/Applications/XAMPP/xamppfiles/htdocs"
    <Directory "/Applications/XAMPP/xamppfiles/htdocs">
        Options Indexes FollowSymLinks Includes execCGI
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

然后添加自己的虚拟主机

```vim
<virtualhost *:80>
ServerName mysite.local
DocumentRoot "/Users/yourusername/path/to/your/site"
<directory "/Users/yourusername/path/to/your/site">
Options Indexes FollowSymLinks Includes ExecCGI
AllowOverride All
Require all granted
</directory>
ErrorLog "logs/mysite.local-error_log"
</virtualhost>
```

第四步 如果经过以上步骤重启apache还是有403错误， 就找到/Applications/XAMPP/xamppfiles/etc/httpd.conf

修改里面的User和Group， User修改为你的电脑的用户名， group修改为staff

也就是

```vim
#User    daemon
User    timothy
#Group    daemon
Group    staff

```

参考资料：[http://jonathannicol.com/blog/2012/03/11/configuring-virtualhosts-in-xampp-on-mac/](http://jonathannicol.com/blog/2012/03/11/configuring-virtualhosts-in-xampp-on-mac/)
