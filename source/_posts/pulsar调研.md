---
title: pulsar调研
date: 2022-01-10 18:54:43
categories: 
- 技术
type: 
---

# pulsar调研

1、pulsar单机部署

pulsar可以单机部署，单机部署包含一个pulsar broker，zookeeper和bookzeeper（中文意义：簿记员，记账员）。

注意：pulsar默认需要2G的jvm堆内存，所以启动的时候可能会因为堆内存不够而无法启动，可以前往conf/pulsar_env.sh修改PULSAR_MEM这个参数。



单机部署的时候，可以选择安装*builtin connectors* 和*tiered storage offloaders* ，这两个组件的具体作用后面再补充。这两个组件不是必须要安装的。



启动pulsar后。可以生产和消费消息。需要注意的是，pulsar可以不用生产消息也可以消费消息，这句话的含义是，当你启动一个consumer去订阅一个topic并且消费消息的时候，如果provider并没有这个topic和相关的消息，生产者会主动创建一个topic，并产生消息，供消费者消费。

+ 保证消息到达 [Apache BookKeeper](http://bookkeeper.apache.org/) ，持久化的消息存储
+  [Pulsar Functions](https://pulsar.apache.org/docs/en/functions-overview)  轻量级计算框架，数据流处理
+  [Pulsar IO](https://pulsar.apache.org/docs/en/io-overview) 处理数据的流入流出pulsar
+ [Tiered Storage](https://pulsar.apache.org/docs/en/concepts-tiered-storage) 数据冷热数据转移



## message

生产者生产消息，消费者消费消息，消费者没消费一条消息，会向broker传递一个ack，表示该条消息已被消费。pulsar默认会保留所有的消息，只有当消费者将消息全部消费之后，pulsar才会删除这些消息。如果消费者消费消息失败，可以重试消费。



| 元素                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| value/data payload  | 消息值                                                       |
| key                 | 每条消息可以设置一个key值，可选                              |
| properties          | key/value形式的map，用户自定义的属性map                      |
| producer name       | 生产者名字，可选，不设定则为默认名                           |
| sequence id         | 每一条消息都属于某个topic的一个有序的序列，sequence id表示该消息的序号 |
| publish time        | 消息产生的时间，由生产者自动提供                             |
| event time          | 应用标注的时间，个人理解为消息被消费的时间戳                 |
| TypedMessageBuilder | 构造消息，具体用处后面再研究                                 |

消息的默认大小为5M，也自定义大小。

+ 在`broker.conf` file. 

  ```bash
  # The max size of a message (in bytes).
  maxMessageSize=5242880
  ```

  

+ 在`bookkeeper.conf` file. 

  ```bash
  # The max size of the netty frame (in bytes). Any messages received larger than this value are rejected. The default value is 5 MB.
  nettyMaxFrameSizeBytes=5253120
  ```

  

## Producers

producer生产某个topic的消息给broker，broker处理这些消息。

消息发送模式分为异步和同步两种方式。

| 模式 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| 同步 | 生产者将消息发送给broker之后，会同步等待broker返回一个ack，若未收到ack，则认为该消息发送失败。 |
| 异步 | 生产者将消息发送给一个队列，然后立即返回，消息队列后台处理。若消息队列满了，则直接返回失败或者消息堵塞。消息队列大小可自定义。 |

接入模式（access mode）

对于生产者有多种接入模式

| 接入模式         | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| shared           | 多个生产者可以将消息发送到同一个topic，这是默认模式          |
| exclusive        | 只能一个生产者发布消息到某个topic，即独占该topic。其他生产者发布消息到该topic将会报错。当old生产者经历完一个网络分片之后将会被驱逐，然后选举一个新的生产者独占该topic。选举方式后续研究。 |
| WaitForExclusive | 当一个topic被某个生产者独占的时候，新的生产者将等待一个pending时间，而不是直接报错，时间到了，选择一个producer独占该topic。 |

## 消息压缩

支持以下类型

+ [LZ4](https://github.com/lz4/lz4)
+ [ZLIB](https://zlib.net/)
+ [ZSTD](https://facebook.github.io/zstd/)
+ [SNAPPY](https://google.github.io/snappy/)

## Batching

消息可以一个批次一个批次地发送，提高发送的效率。

## chunking

当消息过大，可以分块发送，消费者收到这些chunk之后再组装成一条完整的消息。

