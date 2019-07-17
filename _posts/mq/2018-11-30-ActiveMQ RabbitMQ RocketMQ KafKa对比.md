---
layout: post
title:  "ActiveMQ RabbitMQ RocketMQ KafKa对比"
date:   2018-11-30 14:31:01 +0800
categories: mq
---

ActiveMQ和 RabbitMq 以及Kafka在之前的项目中都有陆续使用过，当然对于三者没有进行过具体的对比，以下摘抄了一些网上关于这三者的对比情况，我自己看过之后感觉还

是可以的，比较清晰的反馈了这三个的具体情况已经使用场景，具体的对比如下：

##### 1)TPS比较
Kafka最高，RabbitMq 次之， ActiveMq 最差。

##### 2)吞吐量对比
kafka具有高的吞吐量，内部采用消息的批量处理，zero-copy机制，数据的存储和获取是本地磁盘顺序批量操作，具有O(1)的复杂度，消息处理的效率很高。  
rabbitMQ在吞吐量方面稍逊于kafka，他们的出发点不一样，rabbitMQ支持对消息的可靠的传递，支持事务，不支持批量的操作；基于存储的可靠性的要求存储可以采用内存或者硬盘。

##### 3)在架构模型方面
RabbitMQ遵循AMQP协议，RabbitMQ的broker由Exchange,Binding,queue组成，其中exchange和binding组成了消息的路由键；客户端Producer通过连接channel和server进行通信，Consumer从queue获取消息进行消费（长连接，queue有消息会推送到consumer端，consumer循环从输入流读取数据）。rabbitMQ以broker为中心；有消息的确认机制。

kafka遵从一般的MQ结构，producer，broker，consumer，以consumer为中心，消息的消费信息保存的客户端consumer上，consumer根据消费的点，从broker上批量pull数据；无消息确认机制。

##### 4)在可用性方面
rabbitMQ支持miror的queue，主queue失效，miror queue接管。  
kafka的broker支持主备模式。  
activeMq也支持主备模式。

##### 5)在集群负载均衡方面
kafka采用zookeeper对集群中的broker、consumer进行管理，可以注册topic到zookeeper上；通过zookeeper的协调机制，producer保存对应topic的broker信息，可以随机或者轮询发送到broker上；并且producer可以基于语义指定分片，消息发送到broker的某分片上。

rabbitMQ的负载均衡需要单独的loadbalancer进行支持。

##### 综合对比：
- ActiveMQ: 历史悠久的开源项目，已经在很多产品中得到应用，实现了JMS1.1规范，可以和spring-jms轻松融合，实现了多种协议，不够轻巧（源代码比RocketMQ多），支持持久化到数据库，对队列数较多的情况支持不好。
- RabbitMq：它比kafka成熟，支持AMQP事务处理，在可靠性上，RabbitMq超过kafka，在性能方面超过ActiveMQ。  
- Kafka：
    Kafka设计的初衷就是处理日志的，不支持AMQP事务处理，可以看做是一个日志系统，针对性很强，所以它并没有具备一个成熟MQ应该具备的特性  
    Kafka的性能（吞吐量、tps）比RabbitMq要强，如果用来做大数据量的快速处理是比RabbitMq有优势的。

下面是网上引用的详细对比：
原文图片：http://blog.csdn.net/oMaverick1/article/details/51331004

<figure>
    <img src="{{ site.baseurl }}/images/各mq对比.jpg" alt="image">
    <figcaption>
        各mq对比
    </figcaption>
</figure>