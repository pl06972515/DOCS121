<h1 align="center">[ 消息队列 ] RabbitMQ</h1>
<p align="center">
</p><br/>


![image-20250922220127550](wwwroot\docImages\image-20250922220127550.png ':size=700')

>[!WARNING|style: flat|label: MQ 架构设计 ]
>
>(`RabbitMQ`) 是一个开源的消息中间件 <span style='color:red'>[ 采用`AMQP`( 高级消息队列协议 ) 进行消息投递 ]</span> [<span style='color:#008B00'>[👓 官方网站 ]</span>](https://www.rabbitmq.com/ ':target=_blank')
>
><span style='color:red'>[ 它允许应用程序之间进行异步通信，提供了一种高效、可扩展、可靠的消息传递机制 ]</span>
>
>![image-20250908234703547](wwwroot\docImages\image-20250908234703547.png)
>
>| [ 相关成员 ]                                              | -                                                            |
>| --------------------------------------------------------- | ------------------------------------------------------------ |
>| <span style='color:RED'>[`Producer`] 生产者</span>        | 生产者是消息的发送方 [ 负责产生并发送消息到`RabbitMQ`]<br/><span style='color:red'>[ 生产者通常将消息发送到交换机`Exchange`]</span> |
>| <span style='color:Blue'>[`Exchange`] 交换机</span>       | 交换机是消息的分发中心 [ 负责将接收到的消息路由到一个或多个队列 ]<br/><span style='color:red'>[ 它定义了消息的传递规则，可以根据规则将消息发送到一个或多个队列 ]</span><br/><span style='color:red'>[`A`] 直连交换机`Dirct Exchange`</span>：将消息路由到与消息中的 [ 路由键(`Routing Key`) - 完全匹配的队列 ]<br/>[`B`] 主题交换机`Topic Exchange`: 根据通配符匹配路由键 [ 将消息路由到一个或多个队列 ]<br/>[`C`] 扇出交换机`Fanout Exchange`：将消息广播到所有与交换机绑定的队列 <span style='color:red'>[ 忽略路由键 ]</span> |
>| <span style='color:Blue'>[`Queue`] 队列</span>            | 队列是消息的存储区 <span style='color:red'>[ 用于存储生产者发送的消息 - 消息最终会被消费者从队列中取出并消费 ]</span><br/><span style='color:Blue'>[ 每个队列都有一个名称，并且可以绑定到一个或多个交换机 ]</span><br/><span style='color:red'>[`A`]  经典队列(`Classic Queue`）: 设计为先进先出(`FIFO`)</span><br/>[`B`] 镜像队列（Mirrored Queue）<br/>[`C`] 队列集群（Quorum Queue）<br/>[`D`] 流队列（Stream Queue） |
>| -                                                         |                                                              |
>| <span style='color:red'>[`Consumer`] 消费者</span>        | 消费者是消息的接收方 <span style='color:red'>[ 负责从队列中获取消息并进行处理 ]</span><br/><span style='color:red'>[ 消费者通过订阅来接收消息 ]</span> |
>| <span style='color:Blue'>[`Binding`] 绑定</span>          | 绑定是交换机和队列之间的关联关系<br/><span style='color:red'> [ 生产者将消息发送到交换机，而队列通过绑定与交换机关联，从而接收到消息 ]</span> |
>| <span style='color:Blue'>[`Virtual Host`] 虚拟主机</span> | 虚拟主机是`RabbitMQ`的 [ 基本工作单元 ]<br/><span style='color:red'>[ 每个虚拟主机拥有自己独立的用户、权限、交换机、队列等资源 - 完全隔离于其他虚拟主机 ]</span> |
>| -                                                         |                                                              |
>| <span style='color:red'>[`Connection`] 连接</span>        | 是指生产者、消费者与`RabbitMQ`之间的网络连接<br/><span style='color:red'>[ 每个连接可以包含多个信道(`Channel`)，每个信道是一个独立的会话通道 - 可以进行独立的消息传递 ]</span> |
>| <span style='color:red'>[`Message`] 消息</span>           | 消息是生产者和消费者之间传递的数据单元<span style='color:red'> [ 消息通常包含消息体 + 可选的属性，如路由键等 ]</span> |
>
>
>
><br/>

