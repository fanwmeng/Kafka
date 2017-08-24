# 快速开始

本节内容就是演示下如何快速简单的启动kafka server，然后在控制台启动一个producer和一个consumer，发送消息，消费消息。

本节内容假定是第一次启动，不存在之前的kafka和zookeeper数据。因为Kafka控制台脚本在基于Unix和Window平台的系统是有区别的，在Windows平台使用bin\windows\来代替基于Unix的bin/，使用后缀为.bat的脚本代替.sh的脚本。

一、下载代码

下载0.11.0.0正式版，并解压

tar -xzf kafka\_2.11-0.11.0.0.tgz    \(tar -xzf kafka\_2.11-0.11.0.0.tgz /usr/local/kafka\_2.11-0.11.0.0 前提是在usr/local中要存在kafka\_2.11-0.11.0.0这个目录，这是和cp不同的地方，cp会自动创建不存在的目录\)

cd  kafka\_2.11-0.11.0.0

解压到kafka\_2.11-0.11.0.0目录中，可以解压到指定位置的指定目录，这里解压到了/usr/local/kafka\_2.11-0.11.0.0目录中。

二、启动服务

Kafka使用了Zookeeper来存储管理信息，所以首先需要一个Zookeeper服务。可以自己下载安装，这里使用Kafka自带的。在Kafka中已经打包好了控制台脚本执行文件来一个快速的一个节点的Zookeeper实例。

目前已经进入了kafka\_2.11-0.11.0.0目录中，如下：

![](/assets/import2-1.png)

启动Zookeeper：bin/zookeeper-server-start.sh  config/zookeeper.properties![](/assets/import2-2.png)

启动Kafka server: bin/kafka-server-start.sh config/server.properties![](/assets/import2-3.png)





