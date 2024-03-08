# 安装jdk服务

### jdk官网下载, 解压

```sh

# 下载
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

# 解压缩
tar -zxvf jdk-8u202-linux-x64.tar.gz

```


### 剪贴到/usr/local目录下

```sh

# 注：此目录通常编译或安装不是直接来自官方仓库或软件包管理器的软件包

mv jdk1.8.0_202/ /usr/local/
cd /usr/local/jdk1.8.0_202
ll

```


### 手动配置Java环境变量

```sh

# 修改配置环境
vim /etc/profile

# 把下面的配置放到文件的最后一行

export JAVA_HOME=/usr/local/jdk1.8.0_202    # 这里设置解压的Java目录文件
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib

# 让配置立即生效
source /etc/profile

# 检查Java环境是否成功安装
## 获取java版本号
java -version
    
## 查看JDK的安装路径（安装后才有）
which java

```


# 安装kafka服务

### 到官网下载kafka压缩包，解压缩

```sh

# 官网下载
https://kafka.apache.org/downloads

# 解压缩
tar -zxvf kafka_2.13-3.2.0.tgz

```


### 修改kafka的配置文件：config/server.properties

```sh

# 现在不用改，但是后面如果要配置kafka集群的话，要保证每个服务器的broker.id都是不一样的
broker.id=0
# 配置当前主机的地址，默认端口号就是9092
listeners=PLAINTEXT://192.168.19.11:9092
advertised.listeners=PLAINTEXT://192.168.19.11:9092
# 配置日志文件的路径
log.dirs=/root/kafka_2.12-3.1.0/data/kafka-logs
# 连接zookeeper
zookeeper.connect=127.0.0.1:2181

```


### 修改zookeeper的配置文件：config/zookeeper.properties

```sh

dataDir=../zkData
dataLogDir=../zkLogs
audit.enable=true

```


### 启动kafka

```sh

# 进入bin目录
# 先启动zookeeper
./zookeeper-server-start.sh -daemon ../config/zookeeper.properties

# 然后启动kafka
./kafka-server-start.sh -daemon ../config/server.properties &

# 查看是否启动成功
ps -aux | grep server.properties
ps -aux | grep zookeper.properties

```


### kafka概念介绍

![概念图](kafka.png)