brew install Hbase

修改hbase-env.sh
开启HBASE_MANAGES_ZK,改值默认是注释的，作用是：使用自带的ZooKeeper。我们为了方便，使用Hbase自带的ZooKeeper

```shell
export HBASE_MANAGES_ZK=true
export HBASE_HOME=/usr/local/Cellar/hbase/1.2.6/libexec
```

修改hbase-site.xml
在原来的configuration标签内部添加如下配置数据
```xml
  <property>
      <name>hbase.rootdir</name>
      <value>hdfs://localhost:9000/hbase</value>
  </property>
  <property>
      <name>hbase.cluster.distributed</name>
      <value>true</value>
  </property>
```

需要注意的是：
hbase.rootdir这个值需要设置成之前Hadoop的core-site.xml配置的fs.default.name值。忘记的话，请自己打开Hadoop的配置文件core-site.xml查看。

运行Hbase

运行Hbase前，我们需要先运行Hadoop伪分布式模式,再运行Hbase

```shell
cd /usr/local/Cellar/hadoop/2.8.2  # 进入hadoop目录
sbin/start-dfs.sh  # 运行hadoop
cd /usr/local/Cellar/hbase/1.2.6  # 进入Hbase目录
bin/start-hbase.sh   # 运行Hbase
```
