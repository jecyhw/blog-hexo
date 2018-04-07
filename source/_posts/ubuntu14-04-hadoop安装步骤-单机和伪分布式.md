---
title: ubuntu14.04 hadoop安装步骤(单机和伪分布式)
tags:
  - ubuntu hadoop
categories:
  - hadoop
abbrlink: 2943276818
date: 2018-04-05 19:53:16
---

 
### 创建hadoop用户
<!-- more -->
添加hadoop用户（用户名字可以自己起）
```bash
sudo useradd -m hadoop -s /bin/bash 
```
给hadoop用户设置密码，这里简单设置为：1，按照提示输入两次密码
```bash
sudo passwd hadoop
```
给hadoop用户增加管理员权限，方便部署
```bash
sudo adduser hadoop sudo
```
最后注销当前用户，使用hadoop账户登陆(注意别忘了)
使用hadoop用户登陆之后，需要先更新一下apt
```bash
sudo apt-get update
```
### 安装、配置ssh无密码登陆

集群、单节点模式都需要用到ssh登陆，ubuntu默认安装了ssh client，所以我还需要装ssh server
```bash
sudo apt-get install openssh-server
```
安装之后，使用如下命令登陆本机(首次登陆会有提示，输入yes即可)
```bash
ssh localhost
```
由于每次ssh登陆都需要输入密码，所以我们需要配置成无密码登陆比较方便，先退出刚才的ssh，然后利用ssh-keygen生成密钥，并将密钥加入到授权中：
```
exit #退出刚才的ssh localhost
cd ~/.ssh/ #若没有该目录，请先执行一次ssh localhost
ssh-keygen -t rsa #会有提示，都按回车就可以
cat ./id_rsa.pub >./authorized_keys #加入授权
```
再用ssh locahost命令，无需密码就能直接登陆了
```bash
ssh localhost
```
### 安装jdk
jdk8下载地址
> http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
下载之后进行解压
```bash
cd ~/Downloads/
tar zxvf jdk-8u91-linux-x64.tar.gz
```
将解压的文件夹移动到`/usr/local`目录下
```bash
sudo mv jdk1.8.0_91 /usr/local
``` 
配置jdk环境变量,使用`gedit`打开`/etc/profile`文件
```bash
sudo gedit /etc/profile
```
在文件最后一行复制以下几行，并保存
```bash
export JAVA_HOME=/usr/local/jdk1.8.0_91
export JRE_HOME=/usr/local/jdk1.8.0_91/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH 
```
执行`source /etc/profile`使修改立即生效

使用`java -version`查看安装是否成功
### 安装hadoop2

hadoop2下载地址
> http://hadoop.apache.org/releases.html

解压下载好的hadoop
```bash
cd ~/Downloads/ #进入到下载目录
tar zxvf hadoop-2.7.2.tar.gz 
```
将解压后的hadoop移动到/usr/local目录下
```bash
sudo mv hadoop-2.7.2 /usr/local
```
将hadoop文件夹及子目录的所有者更改为hadoop用户
```bash
cd /usr/local
sudo chown -R hadoop hadoop-2.7.2
```
查看hadoop版本，是否可以成功使用
```bash
./bin/hadoop version
```
### hadoop单机配置(非分布式)
hadoop默认模式为非分布式模式，无需进行其他配置即可运行。非分布式即单java进程，方便进行调试。

现在来执行例子来感受hadoop的运行。hadoop附带了丰富的例子(可以通过运行 `./bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar`来查看所有的例子)
```bash
./bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar 
```
我们选择运行grep例子，我们讲input文件夹中的所有文件作为输入，筛选当中符合正则表达式dfs[a-z.]+的单词病统计出现的次数，最后输出结果到output文件夹中。
```bash
cd /usr/local/hadoop-2.7.2/
mkdir ./input
cp ./etc/hadoop/*.xml ./input/ #将配置文件作为输入文件
./bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar grep ./input ./output 'dfs[a-z.]+'

cat ./output/* #查看程序的执行结果
```
注意:hadoop默认不会覆盖结果文件，如果再次运行上面实例，需要先将output文件夹删除，否则会提示出错。

### hadoop伪分布式配置
Hadoop 可以在单节点上以伪分布式的方式运行，Hadoop 进程以分离的 Java 进程来运行，节点既作为 `NameNode` 也作为 `DataNode`，同时，读取的是 HDFS 中的文件。

Hadoop 的配置文件位于 `/usr/local/hadoop-2.7.2/etc/hadoop/` 中，伪分布式需要修改2个配置文件 `core-site.xml` 和 `hdfs-site.xml` 。Hadoop的配置文件是 xml 格式，每个配置以声明 property 的 name 和 value 的方式来实现。

修改配置文件 `core-site.xml`
```bash
sudo gedit ./etc/hadoop/core-site.xml
```
修改为下面配置：
```xml
<configuration>
    <property>
         <name>hadoop.tmp.dir</name>
         <value>file:/usr/local/hadoop-2.7.2/tmp</value>
         <description>Abase for other temporary directories.</description>
    </property>
    <property>
         <name>fs.defaultFS</name>
         <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
同样的，修改配置文件 `hdfs-site.xml`，如下：
```bash
sudo gedit ./etc/hadoop/hdfs-site.xml
```
```xml
<configuration>
    <property>
         <name>dfs.replication</name>
         <value>1</value>
    </property>
    <property>
         <name>dfs.namenode.name.dir</name>
         <value>file:/usr/local/hadoop-2.7.2/tmp/dfs/name</value>
    </property>
    <property>
         <name>dfs.datanode.data.dir</name>
         <value>file:/usr/local/hadoop-2.7.2/tmp/dfs/data</value>
    </property>
</configuration>
```
Hadoop配置文件说明

Hadoop 的运行方式是由配置文件决定的（运行 Hadoop 时会读取配置文件），因此如果需要从伪分布式模式切换回非分布式模式，需要删除 `core-site.xml` 中的配置项。

此外，伪分布式虽然只需要配置 `fs.defaultFS` 和 `dfs.replication` 就可以运行（官方教程如此），不过若没有配置 `hadoop.tmp.dir` 参数，则默认使用的临时目录为 `/tmp/hadoo-hadoop`，而这个目录在重启时有可能被系统清理掉，导致必须重新执行 format 才行。所以我们进行了设置，同时也指定 `dfs.namenode.name.dir` 和 `dfs.datanode.data.dir`，否则在接下来的步骤中可能会出错。

配置完成后，执行 `NameNode` 的格式化:

```bash
 ./bin/hdfs namenode -format
```

接着开启 `NameNode` 和 `DataNode` 守护进程。
```bash
 ./sbin/start-dfs.sh
```

如果报以下错误
```bash
Starting namenodes on [localhost]
localhost: Error: JAVA_HOME is not set and could not be found.
localhost: Error: JAVA_HOME is not set and could not be found.
Starting secondary namenodes [0.0.0.0]
0.0.0.0: Error: JAVA_HOME is not set and could not be found.
```

则需要修改`./etc/hadoop/hadoop-env.sh` 
```bash
gedit ./etc/hadoop/hadoop-env.sh #用gedit打开
```
```bash
# 找到下面这行
export JAVA_HOME=${JAVA_HOME}
# 修改成下面，使用jdk的绝对路径
export JAVA_HOME=/usr/local/jdk1.8.0_91
```

再重新执行`./sbin/start-dfs.sh`即可

启动完成后，使用jps来判断是否成功启动。若成功启动则会列出如下进程: “NameNode”、”DataNode” 和 “SecondaryNameNode”（如果 SecondaryNameNode 没有启动，请运行 sbin/stop-dfs.sh 关闭进程，然后再次尝试启动尝试）。如果没有 NameNode 或 DataNode ，那就是配置不成功，请仔细检查之前步骤，或通过查看启动日志排查原因。
``` bash
jps
```

成功启动后，可以访问 Web 界面 http://localhost:50070, 查看 NameNode 和 Datanode 信息，还可以在线查看 HDFS 中的文件。
![hadoop的web界面](ubuntu14-04-hadoop安装步骤-单机和伪分布式\7d7a8f05.png)
### 运行hadoop伪分布式实例
---

上面的单机模式，grep 例子读取的是本地数据，伪分布式读取的则是 HDFS 上的数据。要使用 HDFS，首先需要在 HDFS 中创建用户目录：
```bash
./bin/hdfs dfs -mkdir -p /user/hadoop
#-p表示父目录
```

接着将 ./etc/hadoop 中的 xml 文件作为输入文件复制到分布式文件系统中，即将 /usr/local/hadoop/etc/hadoop 复制到分布式文件系统中的 /user/hadoop/input 中。我们使用的是 hadoop 用户，并且已创建相应的用户目录 /user/hadoop ，因此在命令中就可以使用相对路径如 input，其对应的绝对路径就是 /user/hadoop/input:
```bash
./bin/hdfs dfs -mkdir input
 ./bin/hdfs dfs -put ./etc/hadoop/*.xml input
```

复制完成后，可以通过如下命令查看文件列表：
```bash
./bin/hdfs dfs -ls input
```

伪分布式运行 MapReduce 作业的方式跟单机模式相同，区别在于伪分布式读取的是HDFS中的文件（可以将单机步骤中创建的本地 input 文件夹，输出结果 output 文件夹都删掉来验证这一点）。

先删除单机步骤中创建的input文件夹，还有输出结果output文件夹
```bash
rm -rf input
rm -rf output
```

执行grep例子
```bash
./bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar grep input output 'dfs[a-z.]+'
```

结果如下，注意到刚才我们已经更改了配置文件，所以运行结果不同。
```bash
./bin/hdfs dfs -cat output/*
```

我们也可以将运行结果取回到本地：
```bash
rm -rf ./output #需要先删除本地output文件夹
./bin/hdfs dfs -get output ./output
cat ./output/*
```

Hadoop 运行程序时，输出目录不能存在，否则会提示错误 “org.apache.hadoop.mapred.FileAlreadyExistsException: Output directory hdfs://localhost:9000/user/hadoop/output already exists” ，因此若要再次执行，需要执行如下命令删除 output 文件夹:
```bash
./bin/hdfs dfs -rm -r output

```
### 启动yarn

修改配置文件 mapred-site.xml
```bash
#先进行重命名：
mv ./etc/hadoop/mapred-site.xml.template  ./etc/hadoop/mapred-site.xml
#使用gedit修改
sudo gedit ./etc/hadoop/mapred-site.xml
#用以下文本替换
```
```xml
<configuration>
     <property>
         <name>mapreduce.framework.name</name>
         <value>yarn</value>
    </property>
</configuration>
```
接着修改配置文件 yarn-site.xml：
```xml
<configuration>
    <property>
         <name>yarn.nodemanager.aux-services</name>
         <value>mapreduce_shuffle</value>
    </property>
</configuration>
```
启动 YARN 了（需要先执行过 ./sbin/start-dfs.sh）：
```bash
./sbin/start-yarn.sh
./sbin/mr-jobhistory-daemon.sh start historyserver #开启历史服务器，才能在Web中查看任务运行情况
```        
开启后通过 jps 查看，可以看到多了 NodeManager 和 ResourceManager 两个后台进程
```bash
jps
```
启动 YARN 之后，运行实例的方法还是一样的，仅仅是资源管理方式、任务调度不同。观察日志信息可以发现，不启用 YARN 时，是 “mapred.LocalJobRunner” 在跑任务，启用 YARN 之后，是 “mapred.YARNRunner” 在跑任务。启动 YARN 有个好处是可以通过 Web 界面查看任务的运行情况：http://localhost:8088/cluster，如下图所示。
![开启YARN后可以查看任务运行信息](ubuntu14-04-hadoop安装步骤-单机和伪分布式\13d31f6b.png)

不启动 YARN 需重命名 mapred-site.xml


如果不想启动 YARN，务必把配置文件 mapred-site.xml 重命名，改成 mapred-site.xml.template，需要用时改回来就行。否则在该配置文件存在，而未开启 YARN 的情况下，运行程序会提示 “Retrying connect to server: 0.0.0.0/0.0.0.0:8032” 的错误，这也是为何该配置文件初始文件名为 mapred-site.xml.template。

关闭 YARN 的脚本如下：
```bash
./sbin/stop-yarn.sh
./sbin/mr-jobhistory-daemon.sh stop historyserver
```

Hadoop单机和伪分布式的配置就好了。