---
layout: post
author: 万仕诚
title: 'Linux卸载virtualenv后的bash错误'
date: 2017-12-28
cover: 'http://file.wanshicheng.org/assets/img/postcover.jpg'
categories: linux
tags: linux virtualenv bash
---
卸载 virtualenv 后，每次登陆都会提示错误：
```bash
-bash: /usr/share/virtualenvwrapper/virtualenvwrapper_lazy.sh: No such file or directory
```
虽然对操作系统的使用没有什么影响，但是每次登陆看到这个心里都会很不爽。

为了解决问题，我查看了所有的环境变量配置文件：

- /etc/profile
- /etc/bash.bashrc
- ~/.bashrc
- ~/.profile
- ~/.bash_profile

都没有发现相关信息。

后来在“/etc/bash_completion.d/virtualenvwrapper”找到了如下代码：

```bash
# bash completion snippet for virtualenvwrapper

USE_FULL=no

if [ "$USE_FULL" = "yes" ]; then
    . /usr/share/virtualenvwrapper/virtualenvwrapper.sh
else
    . /usr/share/virtualenvwrapper/virtualenvwrapper_lazy.sh
fi
```
这样删除该文件:
```bash
sudo rm /etc/bash_completion.d/virtualenvwrapper
```
问题就解决了。

原来在 Linux 文件夹中还有一个存放补全脚本的文件夹“/etc/bash_completion.d/”。


