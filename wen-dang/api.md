# 2 APIS

Kafka有伍哥核心api：

1.生产者API允许应用发送数据流到Kafka集群主题中

2.消费者API允许应用从kafka集群中读取数据流

3.流式API允许从输入流主题传输数据流到输出流主题中

4.连接器API允许实现连接器，即持续的从数据源系统或应用中拉取数据到Kafka或者推送Kafka的数据到下游系统或应用中。

5.管理员客户端API允许管理和监控主题，brokers和其他Kafka对象。

Kafka通过语言独立的协议对外暴漏了所有这些功能，这些协议都有各种语言版本的客户端。但是只有Java客户端是在Kafka项目中集成维护的，其他语言的客户端都是作为独立的开源项目。个语言客户端可在如下地址中查看\([https://cwiki.apache.org/confluence/display/KAFKA/Clients\)。](https://cwiki.apache.org/confluence/display/KAFKA/Clients%29。)

## 2.1 生产者API

生产者API允许应用发送数据流到Kafka集群中。Kafka生产者使用手册地址为_**\(**_[https://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html\](https://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html%29_**\)\*\*\_

为使用生产者，使用如下的maven依赖引入jar包

&lt;dependency&gt;

```
&lt;groupId&gt;org.apache.kafka&lt;/groupId&gt;

&lt;artifactId&gt;kafka-clients&lt;/artifactId&gt;

&lt;version&gt;0.11.0.0&lt;/version&gt;
```

&lt;/dependency&gt;

## 2.2 消费者API

消费者API允许应用读取Kafka集群中主题的数据流。Kafka消费者使用手册地址为\([https://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html\](https://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html\)\)

使用如下maven配置引入消费者依赖：

&lt;dependency&gt;

```
&lt;groupId&gt;org.apache.kafka&lt;/groupId&gt;

&lt;artifactId&gt;kafka-clients&lt;/artifactId&gt;

&lt;version&gt;0.11.0.0&lt;/version&gt;
```

&lt;/dependency&gt;

## 2.3 流API

流API允许从输入主题传输数据到输出主题。

使用流API库的使用手册地址为:

\(https://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/streams/KafkaStreams.html\)

使用如下的maven配置引入Kafka流

&lt;dependency&gt;

    &lt;groupId&gt;org.apache.kafka&lt;/groupId&gt;

    &lt;artifactId&gt;kafka-streams&lt;/artifactId&gt;

    &lt;version&gt;0.11.0.0&lt;/version&gt;

&lt;/dependency&gt;

2.4 连接API

连接API允许实现连接器，这些连接器持续的从某些数据源系统拉取数据到Kafka或者将数据从Kafka推送到某些相关联的数据系统。

很多连接使用者没必要直接使用这些API，而是可以使用预构建的连接器而不需要编写任何代码。连接的文档请参见 **Kafka连接**一节.

如果想实现用户连接器，参考文档\(https://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/connect\)





