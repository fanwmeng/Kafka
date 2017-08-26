# 快速开始

本节内容就是演示下如何快速简单的启动kafka server，然后在控制台启动一个producer和一个consumer，发送消息，消费消息。

本节内容假定是第一次启动，不存在之前的kafka和zookeeper数据。因为Kafka控制台脚本在基于Unix和Window平台的系统是有区别的，在Windows平台使用bin\windows\来代替基于Unix的bin/，使用后缀为.bat的脚本代替.sh的脚本。

## 一、下载代码

下载0.11.0.0正式版，并解压

`tar -xzf kafka_2.11-0.11.0.0.tgz`    \(tar -xzf kafka\_2.11-0.11.0.0.tgz /usr/local/kafka\_2.11-0.11.0.0 前提是在usr/local中要存在kafka\_2.11-0.11.0.0这个目录，这是和cp不同的地方，cp会自动创建不存在的目录\)

`cd  kafka_2.11-0.11.0.0`

解压到kafka\_2.11-0.11.0.0目录中，可以解压到指定位置的指定目录，这里解压到了/usr/local/kafka\_2.11-0.11.0.0目录中。

**注意：本章中下边所有的命令都是在该目录执行的，以该目录为当前目录**

## 二、启动服务

Kafka使用了Zookeeper来存储管理信息，所以首先需要一个Zookeeper服务。可以自己下载安装，这里使用Kafka自带的。在Kafka中已经打包好了控制台脚本执行文件来一个快速的一个节点的Zookeeper实例。

目前已经进入了kafka\_2.11-0.11.0.0目录中，如下：

![](/assets/import2-1.png)

**启动Zookeeper：**

`bin/zookeeper-server-start.sh config/zookeeper.properties`     ![](/assets/import2-2.png)

**启动Kafka server:**

`bin/kafka-server-start.sh config/server.properties`       ![](/assets/import2-3.png)

## 三、创建主题

创建名为"test"的主题，只有一个分区一个副本

`bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test`

可以看到控制台输出 Created topic "test".

如果查看创建了的topic的列表：

`bin/kafka-topic.sh --list --zookeeper localhost:2181`

可以看到控制台输出结果：

test

四、发送消息

Kafka自带了一个命令行客户端可以接收文件或者标注输入然后作为消息发送到Kafka集群。默认的，每一行内容被当做一个消息。

启动发送者，在控制台发送消息给服务端：

`bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test`

此时控制台就进入交互模式，允许你从控制台输入内容，回车键后默认发送改行内容为消息到服务器。如：

This is the first message

This is the second message

如下图：![](/assets/import2-4.png)

五、接收消息

同上，Kafka自带了一个命令行消费者将会接收到的消息输出到标准输出。

启动消费者，可以在控制台看到收到的消息被实时输出：

`bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning`

This is the first message

This is the second message

如下图：实时输出了收到的消息：![](/assets/import2-5.png)

六、启动多节点的集群

到目前为止，我们已经成功的运行了一个节点\(broker，这里把broker翻译成节点，也许不太合适，其实就是一个Kafka的运行实例\)的Kafka，但这没啥意思。对Kafka来说，一个单独的节点\(broker\)就是容量为1的一个集群，所以集群模式无非就是多个实例，其他并没有什么不同。为了体验一下，我们将我们的集群扩为3个节点\(仍然还是在我们本地机器上\)。

首先，给每个节点准备一个配置文件，直接复制已有的即可，不过里面有些配置属性要改一下三个节点，保留原来的配置文件作为第一个节点使用，另外copy两个配置文件作为第二个、第三个节点的配置文件只用

cp server.properties server-1.properties

cp server.properties server-2.properties

编辑这两个配置文件，主要是要改一下broker.id、监听的端口号、日志存放目录，如下

conf/server-1.properties:

      broker.id=1

      listners=PLAINTEST://9093

      log.dir=/Users/fwmeng/kafka1

conf/server-2.properties:

      broker.id=2

      listners=PLAINTEST://9094

      log.dir=/Users/fwmeng/kafka2

其中broker.id属性是集群中每个节点唯一不变的名字。我也也必须覆盖坚挺的端口号和日志目录，因为我们运行的三个节点都在同一台机器上，所以我们必须保证所有的节点不能去注册相同的端口和复写别人的数据。





