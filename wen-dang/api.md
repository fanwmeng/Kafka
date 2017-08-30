# 2 APIS

Kafka有伍哥核心api：

1.生产者API允许应用发送数据流到Kafka集群主题中

2.消费者API允许应用从kafka集群中读取数据流

3.流式API允许从输入流主题传输数据流到输出流主题中

4.连接器API允许实现连接器，即持续的从数据源系统或应用中拉取数据到Kafka或者推送Kafka的数据到下游系统或应用中。

5.管理员客户端API允许管理和监控主题，brokers和其他Kafka对象。

Kafka通过语言独立的协议对外暴漏了所有这些功能，这些协议都有各种语言版本的客户端。但是只有Java客户端是在Kafka项目中集成维护的，其他语言的客户端都是作为独立的开源项目。个语言客户端可在如下地址中查看\([https://cwiki.apache.org/confluence/display/KAFKA/Clients\)。](https://cwiki.apache.org/confluence/display/KAFKA/Clients%29。)

2.1 生产者API



