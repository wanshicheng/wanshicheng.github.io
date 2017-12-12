---
layout: post
author: 万仕诚
title: 'macOS安装Hadoop-单机-伪分布式配置'
date: 2017-11-20
cover: 'http://file.wanshicheng.org/assets/img/postcover.jpg'
categories: 大数据
tags: hadoop java 大数据
---
## 安装Homebrew
安装Homebrew的方法：

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

需要注意的是：
Homebrew安装的软件都集中在/usr/local/Cellar里面

## ssh登录本地

因为安装 hadoop 需要远程登入的功能,所以需要安装ssh工具。macOS 只需在“系统偏好设置”的“共享”的“远程登录”勾选就可以使用ssh了。

```shell
ssh-keygen -t rsa -P ""
cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
```
这样就可以生成ssh公钥，接下来进行测试登录本地是否成功
```shell
ssh localhost
```
登录成功显示结果如下

```shell
Last login: Sun Dec 10 10:46:47 2017
```
如果没有执行远程登陆勾选操作，在运行ssh localhost的时候会出现：mac ssh: connect to host localhost port 22: Connection refused。

## 安装Java

macOS 没有自带 Java，输入以下代码，自动安装 Java

```shell
brew install java
```

安装完成后， Homebrew 会自动给你配置环境变量，输入以下代码，查看 Java 是否安装完成

```shell
java -version
```

安装成功显示结果如下

```shell
java version "9.0.1"
Java(TM) SE Runtime Environment (build 9.0.1+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.1+11, mixed mode)
```

## 安装 Hadoop
输入以下代码，自动安装hadoop
```shell
brew install hadoop
```
通过Homebrew默认会安装软件的最新stable版本。
### 测试Hadoop是否安装成功
#### 测试单机模式
Hadoop 默认模式为非分布式模式（本地模式），无需进行其他配置即可运行。非分布式即单 Java 进程，方便进行调试。
与其它的安装方式不同的是，通过 Homebrew 进行安装，hadoop 的一些文件会放在安装目录的 libexec 文件夹下。

运行 ./bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar 可以看到所有例子，包括 wordcount、terasort、join、grep 等。

在此我们选择运行 grep 例子，我们将 input 文件夹中的所有文件作为输入，筛选当中符合正则表达式 dfs[a-z.]+ 的单词并统计出现的次数，最后输出结果到 output 文件夹中。

```shell
cd /usr/local/Cellar/hadoop/2.8.2/
mkdir ./input
cp ./libexec/etc/hadoop/*.xml ./input   # 将配置文件作为输入文件
hadoop jar ./libexec/share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar grep ./input ./output 'dfs[a-z.]+'
cat ./output/*          # 查看运行结果
```
执行成功后如下所示，输出了作业的相关信息，输出的结果是符合正则的单词 dfsadmin 出现了1次。

注意，Hadoop 默认不会覆盖结果文件，因此再次运行上面实例会提示出错，需要先将 ./output 删除。

```shell
rm -r ./output
```

#### Hadoop伪分布式配置
Hadoop 可以在单节点上以伪分布式的方式运行，Hadoop 进程以分离的 Java 进程来运行，节点既作为 NameNode 也作为 DataNode，同时，读取的是 HDFS 中的文件。

Hadoop 的配置文件位于 /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/ 中，伪分布式需要修改2个配置文件 core-site.xml 和 hdfs-site.xml 。Hadoop的配置文件是 xml 格式，每个配置以声明 property 的 name 和 value 的方式来实现。

修改配置文件 core-site.xml，将当中的

```xml
<configuration>
</configuration>
```
修改为下面配置：

```xml
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/usr/local/Cellar/hadoop/2.8.2/tmp</value>
        <description>Abase for other temporary directories.</description>
    </property>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
同样的，修改配置文件 hdfs-site.xml：

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/usr/local/Cellar/hadoop/2.8.2/tmp/dfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/usr/local/Cellar/hadoop/2.8.2/tmp/dfs/data</value>
    </property>
</configuration>
```

注意，如果将 tmp 目录直接放在 hadoop 目录下，而不是 2.8.2 目录，执行 brew cleanup 的时候会将 tmp 目录删除掉。

Hadoop 的运行方式是由配置文件决定的（运行 Hadoop 时会读取配置文件），因此如果需要从伪分布式模式切换回非分布式模式，需要删除 core-site.xml 中的配置项。

此外，伪分布式虽然只需要配置 fs.defaultFS 和 dfs.replication 就可以运行（官方教程如此），不过若没有配置 hadoop.tmp.dir 参数，则默认使用的临时目录为 /tmp/hadoo-hadoop，而这个目录在重启时有可能被系统清理掉，导致必须重新执行 format 才行。所以我们进行了设置，同时也指定 dfs.namenode.name.dir 和 dfs.datanode.data.dir，否则在接下来的步骤中可能会出错。

配置完成后，执行 NameNode 的格式化
```shell
hdfs namenode -format
```
接着开启 NameNode 和 DataNode 守护进程
```shell
./sbin/start-dfs.sh  #start-dfs.sh是个完整的可执行文件，中间没有空格
```
若出现如下SSH提示，输入yes即可。
```shell
Are you sure you want to continue connecting (yes/no)? yes
```
启动时可能会出现如下警告
```shell
WARNING: All illegal access operations will be denied in a future release
17/12/10 11:21:16 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
```
提示可以忽略，并不会影响正常使用。

启动完成后，可以通过命令 jps 来判断是否成功启动，若成功启动则会列出如下进程: "NameNode"、"DataNode" 和 "SecondaryNameNode"（如果 SecondaryNameNode 没有启动，请运行 sbin/stop-dfs.sh 关闭进程，然后再次尝试启动尝试）。如果没有 NameNode 或 DataNode ，那就是配置不成功，请仔细检查之前步骤，或通过查看启动日志排查原因。

成功启动后，可以访问 Web 界面 http://localhost:50070 查看 NameNode 和 Datanode 信息，还可以在线查看 HDFS 中的文件。

#### 运行Hadoop伪分布式实例
上面的单机模式，grep 例子读取的是本地数据，伪分布式读取的则是 HDFS 上的数据。要使用 HDFS，首先需要在 HDFS 中创建用户目录
```shell
hdfs dfs -mkdir -p /user/wanshicheng
```
接着将 ./etc/hadoop 中的 xml 文件作为输入文件复制到分布式文件系统中，即将 /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/ 复制到分布式文件系统中的 /user/wanshicheng/input 中。我使用的是 wanshicheng 用户，并且已创建相应的用户目录 /user/wanshicheng ，因此在命令中就可以使用相对路径如 input，其对应的绝对路径就是 /user/wanshicheng/input:
```shell
hdfs dfs -mkdir input
hdfs dfs -put /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/*.xml input
```
复制完成后，可以通过如下命令查看文件列表
```shell
hdfs dfs -ls input
```
伪分布式运行 MapReduce 作业的方式跟单机模式相同，区别在于伪分布式读取的是HDFS中的文件（可以将单机步骤中创建的本地 input 文件夹，输出结果 output 文件夹都删掉来验证这一点）。
```shell
hadoop jar ./libexec/share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar grep input output 'dfs[a-z.]+'
```
查看运行结果的命令（查看的是位于 HDFS 中的输出结果）：
```shell
hdfs dfs -cat output/*
```
结果如下，注意到刚才我们已经更改了配置文件，所以运行结果不同
```shell
1	dfsadmin
1	dfs.replication
1	dfs.namenode.name.dir
1	dfs.datanode.data.dir
```
我们也可以将运行结果取回到本地：
```shell
rm -r ./output    # 先删除本地的 output 文件夹（如果存在）
hdfs dfs -get output ./output     # 将 HDFS 上的 output 文件夹拷贝到本机
cat ./output/*
```
Hadoop 运行程序时，输出目录不能存在，否则会提示错误 "org.apache.hadoop.mapred.FileAlreadyExistsException: Output directory hdfs://localhost:9000/user/hadoop/output already exists" ，因此若要再次执行，需要执行如下命令删除 output 文件夹:
```shell
hdfs dfs -rm -r output # 删除 output 文件夹
```
运行 Hadoop 程序时，为了防止覆盖结果，程序指定的输出目录（如 output）不能存在，否则会提示错误，因此运行前需要先删除输出目录。在实际开发应用程序时，可考虑在程序中加上如下代码，能在每次运行时自动删除输出目录，避免繁琐的命令行操作
```java
Configuration conf = new Configuration();
Job job = new Job(conf);

/* 删除输出目录 */
Path outputPath = new Path(args[1]);
outputPath.getFileSystem(conf).delete(outputPath, true);
```
若要关闭 Hadoop，则运行
```shell
./sbin/stop-all.sh
```
