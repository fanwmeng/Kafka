# 3 配置

Kafka使用属性文件中的键-值对格式化配置。这些值既可以从一个文件中获取也可以程序化。

## 3.1 节点配置

节点配置就是Kafka服务端实例的配置，就是集群中单个实例的配置。必须要配置的包括如下：

* broker.id
* log.dirs
* zookeeper.connect

主题级别的配置和默认值在如下展开更详细的讨论。

**zookeeper.connect**：Zookeeper host字符串，类型为string，是Kafka启动需要连接的Zookeeper服务器的地址，格式为ip:port

**advertised.host.name**：已经废弃掉，只有属性**advertised.listeners**或者**listeners**没有被设置的时候使用。使用**advertised.listeners**代替。推送主机名给Zookeeper，客户端来使用。在Iaas环境中，这不同于节点绑定的接口。如果该值没有设置并且配置了属性host.name，则使用host.name的值。否则将使用java.net.InetAddress.getCanonicalHostName\(\)返回的值。

**advertised.listeners**：如果监听器发生改变，推送到Zookeeper为客户端使用。在IaaS环境中这与节点绑定的接口不同。如果该值没有设置，则使用配置**listeners**的值。string类型

**advertised.port**：已经被废弃掉。只有**advertised.listeners**和**listeners**没有被设置的时候才会使用。推送到Zookeeper为客户端使用。在IaaS环境中不同于节点绑定的接口。如果该值没有设置，将推送节点绑定的端口到Zookeeper。int类型

**auto.create.topics.enable**：开启服务端主题的自动创建。boolean类型。默认值是true。

**auto.leader.rebalance.enable**：开启leader的自动平衡。如果需要，后台线程会定期检查和触发leader的平衡。boolean类型的值，默认值是true。

**background.threads**：多个后台处理任务的线程数。int类型的值，默认值为10。可选值为1 2 3 ....

**broker.id**：当前服务器的实例id。如果没有设置，唯一的实例id会被生成。为了避免多个zookeeper之间生成的和用户配置的broker的id冲突，生成的broker的id从Zookeeper接收到的reserved.broker.max.id 在+ 1。int类型，默认值是-1。

**compression.type**：指定topic内容的压缩方式。配置接受标准的编解码方式如'gzip'，'snappy'，'lz4'，除此之外还接受'uncompressed'，等同于不对内容压缩；如果设置为'producer'，则保留使用producer设置的编解码方式。string类型，默认值就是producer。

**delete.topic.enable**：允许删除主题。如果关闭该配置，则通过管理工具删除主题将不起作用。boolean类型的值，默认值是false。

**host.name**：已经被废弃，只有当**listeners**没有设置的时候才会使用。推荐使用**listeners**代替。服务实例的主机名。如果设置了该值，则只绑定改地址，如果没有设置，则绑定所有的接口。

**leader.imbalance.check.interval.seconds**：控制器触发分区重新平衡检查的频率。long类型，默认值是300，表示每5分钟检查一次。

**leader.imbalance.per.broker.percentage**：每个服务实例允许leader重新平衡的比率。控制器将按照上边的值对每个实例触发leader平衡操作。该值是指的百分比。值是int类型的，默认值是10，表示10%的概率来允许leader再平衡。

**listeners**：监听器列表。监听器名称和需要监听的URI的列表，用都好分隔。如果监听器名称不是安全协议，**listener.security.protocol.map**配置也需要设置值。指定hostname为0.0.0.0来绑定到所有接口。hostname为空表示绑定到默认的接口。string类型的值。默认值为null。例如：正确的配置listeners的值为：

PLAINTEXT://myhost:9092,SSL://:9091 CLIENT://0.0.0.0:9092,REPLICATION://localhost:9093

**log.dir**：保存日志数据的目录。作为log.dirs配置属性的补充。string类型的值，默认值是/tmp/kafka-log

**log.dirs**：日志数据保存的目录。如果没有设置，则使用log.dir

**log.flush.interval.messages**：消息被刷到磁盘之前在分区日志中累积的消息数。

**log.flush.interval.messages**：任意主题中的消息在刷到磁盘之前在内存中的毫秒数。如果没有设置，**log.flush.scheduler.interval.msg**的值将被使用





