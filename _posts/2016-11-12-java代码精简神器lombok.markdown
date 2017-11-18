---
author: 万仕诚
date: 2016-11-12
layout: post
title: Java代码精简神器Lombok
cover: 'http://file.wanshicheng.org/assets/img/postcover.jpg'
categories: java
tags: java lombok
---

## 简介


“代码模版”是一个用来描述在Java程序中大量重复，极少调整的代码的术语。Java饱受诟病的一点就是在各种项目中出现大量这样的代码。通常都会定义大量的JavaBean，然后通过IDE去生成其属性的构造器、getter、setter、equals、hashcode、toString方法，当要对某个属性进行改变时，比如命名、类型等，都需要重新去生成上面提到的代码模板。这个问题通常是各种类库的设计决策的结果，但是加剧了语言本身的限制。Lombok项目的目的就是通过注解减少糟糕臃肿的代码。

它不同寻常地用注解指明使用，实现绑定，甚至通过框架来生成代码，但一般不用于生成直接被应用使用的代码。这样做有时候是因为注解在开发时迫切需要处理。Lombok项目就是做这个的。通过和IDE集成，Lombok能够注入开发人员立即可用的代码。例如，通过简单添加@Data注解得到一个数据类，结果如下，在IDE中生成一些新的方法。

![jnbjan2010-dataannotation](http://file.wanshicheng.org/wp-content/uploads/2016/11/jnbJan2010-DataAnnotation.png)


## 安装


它在项目官网上仅仅是一个jar文件。包括开发API文档和IDE集成的安装。在许多系统上，只需双击jar文件就能执行安装程序，也能通过线面的命令安装：

```sh
java -jar lombok.jar
```

安装程序会发现本地支持的IDE。如果没有找到正确的安装的IDE，可以选择手动安装。只有Eclipse和NetBeans支持这种方式。IntelliJ IDEA不支持这种安装方式，但是却可以使用该插件。JDeveloper对此插件的似乎不太好，笔者没有试过。

![lombokinstaller](http://file.wanshicheng.org/wp-content/uploads/2016/11/LombokInstaller.png)

IntelliJ IDEA的安装方式如下：

打开IDEA的Settings面板，并选择Plugins选项，然后点击 “Browse repositories..”。

![idea-plugin](http://fiel.wanshicheng.org/wp-content/uploads/2016/11/idea-plugin-1024x694.jpg)

在输入框输入”lombok”，得到搜索结果，选择“Lombok Plugin”，点击安装，然后安装提示重启IDEA，安装成功。

![idea-lombok-plugin](http://file.wanshicheng.org/wp-content/uploads/2016/11/idea-lombok-plugin.jpg)

在自己的项目里添加lombok的编译支持(此处本人所操作的项目为maven项目),在pom文件里面添加如下依赖：

```java
<dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.16.6</version>
</dependency>
```

然后就可以尽情在自己项目里面编写简略风格的Java代码

参考资料：

[https://projectlombok.org/](https://projectlombok.org/)

[http://jnb.ociweb.com/jnb/jnbJan2010.html](http://jnb.ociweb.com/jnb/jnbJan2010.html)
