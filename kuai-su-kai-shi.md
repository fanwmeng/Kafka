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

## 四、发送消息

Kafka自带了一个命令行客户端可以接收文件或者标注输入然后作为消息发送到Kafka集群。默认的，每一行内容被当做一个消息。

启动发送者，在控制台发送消息给服务端：

`bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test`

此时控制台就进入交互模式，允许你从控制台输入内容，回车键后默认发送改行内容为消息到服务器。如：

This is the first message

This is the second message

如下图：![](/assets/import2-4.png)

## 五、接收消息

同上，Kafka自带了一个命令行消费者将会接收到的消息输出到标准输出。

启动消费者，可以在控制台看到收到的消息被实时输出：

`bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning`

This is the first message

This is the second message

如下图：实时输出了收到的消息：![](/assets/import2-5.png)

## 六、启动多节点的集群

到目前为止，我们已经成功的运行了一个节点\(broker，这里把broker翻译成节点，也许不太合适，其实就是一个Kafka的运行实例\)的Kafka，但这没啥意思。对Kafka来说，一个单独的节点\(broker\)就是容量为1的一个集群，所以集群模式无非就是多个实例，其他并没有什么不同。为了体验一下，我们将我们的集群扩为3个节点\(仍然还是在我们本地机器上\)。

首先，给每个节点准备一个配置文件，直接复制已有的即可，不过里面有些配置属性要改一下三个节点，保留原来的配置文件作为第一个节点使用，另外copy两个配置文件作为第二个、第三个节点的配置文件只用

cp server.properties server-1.properties

cp server.properties server-2.properties

编辑这两个配置文件，主要是要改一下broker.id、监听的端口号、日志存放目录，如下

conf/server-1.properties:

```
  broker.id=1

  listners=PLAINTEST://9093

  log.dir=/Users/fwmeng/kafka1
```

conf/server-2.properties:

```
  broker.id=2

  listners=PLAINTEST://9094

  log.dir=/Users/fwmeng/kafka2
```

其中broker.id属性是集群中每个节点唯一不变的名字。我也也必须覆盖坚挺的端口号和日志目录，因为我们运行的三个节点都在同一台机器上，所以我们必须保证所有的节点不能去注册相同的端口和覆写别人的数据。

之前已经运行过test主题的服务，已经存在日志了，如果要测试，需要将原来的数据删除，或者创建一个新的主题。这里采用了前者。

还是如二中所述，先启动zookeeper服务:

bin/zookeeper-server-start.sh config/zookeeper.properties

启动三个kafka的broker，分别指定使用不同的配置文件：

bin/kafka-server-start.sh config/server.properties

bin/kafka-server-start.sh config/server-1.properties

bin/kafka-server-start.sh config/server-2.properties &       这里将第三个实例使用后台方式启动运行。

创建topic，有三个复制因子

bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 3 --partitions 1 --topic test

现在针对这个test主题，集群中有三个节点，如何知道每个节点在做什么呢？可以使用如下命令：

bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic test![](/assets/import2-6.png)

如下是输出的解释，第一行给出了所有分区的一个概况，接下来的每一行是对每一个分区的信息j进行了展示。因为我们只有一个分区，所以接下来只有一行。

Topic：主题名字

Partition：分区号，从0开始，只有一个分区

Leader：所有节点中作为Leader的节点，节点号就是在server.properties中配置的broker.id的值。Leader是负责对所有的读写请求响应的节点。分区中的所有节点随机选择一个作为Leader。

Replicas：列出所有复制此分区日志的节点列表。不管这些节点是不是Leader或者是不是存活的。

Isr：是Replicas中处于同步的集合。这是Replicas的一个子集，就是存活的节点并且和Leader保持一致的节点。

通过截图可知，主题test唯一的分区的Leader节点是0

也是如上四中所说，启动producer发送消息到test主题。因为Leader是节点0，所以我们发送消息给0节点，而不是1和2节点。

bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test     完成后输入了msg11作为消息，如下图

![](/assets/import2-8.png)

启动消费者，指定leader的端口号消费：

bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic test  从最开始接收消息，如下图：![](/assets/import2-9.png)

如果不是从最开始消费消息，而是从指定的偏移量开始，可以使用--offset &lt;String : consumer offset&gt;,如果使用了此选项，则必须使用--partition来制定那个分区。offset中的String，可以是"earliest"代表从最开始的消息或者"latest"\(默认值\)代表从最新的消息开始消费。如果是数字，则是一个非负整数,从0开始，表示第一条消息。如果该值大于消息总数+1，那么也是从最新的消息开始。如下图：![](/assets/import2-10.png)

现在来看一下容错性，节点1是Leader，我们现在将该进程停掉。

ps -ef \| grep "server.properties" 可以看到如下内容：

501  2774   403   0 10:18下午 ttys001    1:08.08 /Library/Java/JavaVirtualMachines/jdk1.8.0\_73.jdk/Contents/Home/bin/java...

可知pid是2774，现在直接kill掉：kill -9 2774

可以看到zookeeper输出了日志：![](/assets/import2-11.png)

而原来的follower节点1的日志如下：![](/assets/import2-12.png)![](/assets/import2-13.png)

producer日志没有变化，并且继续发送日志仍然是成功的。![](/assets/import2-15.png)

consumer先报错连接不到Leader，然后重新确定Leader后也可以正常接收消息了：如下：![](/assets/import2-15.png)

而看下此时主题test的分区信息：![](/assets/import2-14.png)Leader变为了节点2，活跃节点只有1和2，没有0了。

## 七 使用Kafka Connect导入导出数据

从终端写入数据到Kafka或者将数据写回到控制台可以很方便的开始kafka使用。但或许你希望从使用其它来源的数据或将数据从Kafka导出到其他系统。对很多系统来说，你可以使用Kafka Connect导入导出数据，而不用写集成代码。

Kafka Connect是Kafka自带的用于导入导出数据的工具。它是运行了连接器\(Connector\)的可扩展工具，连接器实现了和外部系统交互的自定义逻辑。在本节的内容，列出如何运行一个带有简单连接器的Kafka Connect来实现将数据从文件导入到一个Kafka主题中，以及将数据从Kafka主题导出到一个文件中。

首先，创建用于测试的种子数据：这些数据放在指定的文件中，腰背导入到Kafka中。

`echo  -e "foo\nbar" > test.txt`

接下来，采用standalone模式运行两个连接器，也就是它们运行在本地独立的专用进程中。我们提供三个参数化的配置文件。第一个是Kafka Connect的配置，包含通用的配置如连接到哪个Kafka节点，以及数据序列化的格式。剩下的两个配置文件，每个配置文件指定一个创建的连接器的信息。这些文件信息包括一个唯一的连接器名称、需要实例化的连机器类以及其他连机器所需的配置信息。

执行如下命令，启动connector：

bin/connect-standalone.sh config/connect-standalone.properties config/config-file-source.properties config/config-file-sink.properties 

输出信息较多，只截图部分如下：![](/assets/import2-17.png)

上述简单的配置文件已经被包含在Kafka的发行包中，它们将使用默认的之前我们启动的本地集群配置创建两个connector：第一个作为源connector从一个文件中读取每行数据然后将他们发送Kafka的topic，第二个是一个输出\(sink\)connector从Kafka的topic读取消息，然后将它们输出成输出文件的一行行的数据。在启动的过程你讲看到一些日志消息，包括一些提示connector正在被实例化的信息。一旦Kafka Connect进程启动以后，源connector应该开始从`test.txt`中读取数据行，并将他们发送到topic`connect-test`上，然后输出connector将会开始从topic读取消息然后把它们写入到`test.sink.txt`中。

注意：根据官方文档，这么做后并没有将信息输出到test-sink.txt中。但是每次向test.txt中写入数据，会看到offset的提交日志输出来。如下图：![](/assets/import2-18.png)

启动控制台消费进程，也没有收到数据：

_**`bin/kafka-console-consumer.sh --bostrap-server localhost:9092 --topic connect-test --from-beginning`**_

如下图：









