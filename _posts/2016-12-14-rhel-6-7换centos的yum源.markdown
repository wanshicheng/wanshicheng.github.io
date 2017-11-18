---
author: 万仕诚
date: 2016-12-14
layout: post
title: RHEL 6.7换CentOS的yum源
cover: 'http://file.wanshicheng.org/assets/img/postcover.jpg'
categories: linux
tags: linux
---
R
HEL 6.7的源需要序列号，可以使用CentOS的yum源来替代。

首先要删除RHEL中的yum命令。

```shell
rpm -aq | grep yum |xargs rpm -e --nodeps
```

下载rpm包

```shell
wgethttp://mirrors.163.com/centos/6.7/os/x86_64/Packages/python-iniparse-0.3.1-2.1.el6.noarch.rpm
wgethttp://mirrors.163.com/centos/6.7/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.30-30.el6.noarch.rpm
wgethttp://mirrors.163.com/centos/6.7/os/x86_64/Packages/yum-metadata-parser-1.1.2-16.el6.x86_64.rpm
```

安装rpm包

```shell
rpm -ivh python-iniparse-0.3.1-2.1.el6.noarch.rpm
rpm -ivh yum*
```

删除所有yum源

```shell
rm/etc/yum.repos.d/*
```

下载yum源

```shell
cd /etc/yum.repos.d/
wget http://mirrors.163.com/.help/CentOS6-Base-163.repo
```

替换所有$releasever为当前版本号

```shell
vi CentOS6-Base-163.repo
```

最后清除缓存及测试

```shell
yumcleanall&yummakecache
yumupdate-y
```


