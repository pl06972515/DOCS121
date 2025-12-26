<h1 align="center">[ 消息队列 ] RabbitMQ</h1>
<p align="center">
</p><br/>


>[!WARNING|style: flat|label: MQ 架构设计 ]
>
>- (`RabbitMQ`) 是一个开源的消息中间件 <span style='color:red'>[ 采用`AMQP`( 高级消息队列协议 ) 进行消息投递 ]</span> [<span style='color:#008B00'>[👓 官方网站 ]</span>](https://www.rabbitmq.com/ ':target=_blank')
>
>   <span style='color:red'>[ 它允许应用程序之间进行异步通信，提供了一种高效、可扩展、可靠的消息传递机制 ]</span>
>
>
><br/>
>
><span style='color:Blue'>[ 使用场景 ]</span>
>
>- <span style='color:Blue'>削峰填谷：在电商`双11`场景中，利用`MQ`进行`削峰填谷`是非常常见且有效的技术手段</span>
>
>   <span style='color:red'>[ 其核心思想是：将高并发请求(`如订单、支付等`)通过消息队列进行异步处理，避免瞬时流量直接冲击后端服务和数据库，从而实现系统的平稳运行 ]</span>
>
>- <span style='color:Blue'>异步处理( 秒杀系统 ) ：用户下单后，订单消息进入消息队列`MQ`[ 由后台服务异步处理后续步骤 - 如库存扣减、订单创建等 ]</span>
>- <span style='color:red'>异步处理( 轮询场景 ) </span>
>- <span style='color:red'>延迟处理( 轮询场景 ) </span>
>
><br/>
>
>![image-20250908234703547](wwwroot\docImages\image-20250908234703547.png)
>
>| [ 相关成员 ]                                      | -                                                            |
>| ------------------------------------------------- | ------------------------------------------------------------ |
>| <span style='color:RED'>[`Producer`] </span>      | <span style='color:Blue'>[ 生产者 ]</span> 消息的发送方，负责产生并发送消息到`MQ`<span style='color:red'>( 通常将消息发送到交换机`Exchange`)</span> |
>| <span style='color:red'>[`Consumer`] </span>      | <span style='color:Blue'>[ 消费者 ]</span> 消息的接收方，负责从队列中获取消息并进行处理 <span style='color:red'>( 通过订阅来接收消息 )</span> |
>| -                                                 |                                                              |
>| <span style='color:Blue'>[`Message`] </span>      | <span style='color:Blue'>[ 消息 ]</span> 生产者和消费者之间传递的 <span style='color:Blue'>[ 数据单元 ]</span> <span style='color:red'>( 通常包含消息体 + 可选的属性，如路由键等 )</span> |
>| <span style='color:Blue'>[`Queue`] </span>        | <span style='color:Blue'>[ 队列 ]</span> 消息的存储区域<br/><span style='color:red'>[`A`]  经典队列(`Classic Queue`）: 设计为先进先出(`FIFO`)</span><br/>[`B`] 仲裁队列(`Quorum Queue`) :  新一代高可用队列类型，提供更高的数据一致性和可靠性<span style='color:red'>( 集群分布式 )</span><br/>[ 仲裁队列不支持：消息优先级，队列过期参数`x-message-ttl`]<br/>[`C`] 流式队列(`Stream Queue`)：不完善( 建议优先选择`Kafka`) |
>| -                                                 |                                                              |
>| <span style='color:RED'>[`Exchange`]</span>       | <span style='color:red'>[ 交换机 ] 消息的分发中心 ( 负责将接收到的消息 → 路由到一个或多个队列 )</span><br/><span style='color:red'>[`A`] 直连交换机`Dirct Exchange`</span>：将消息路由到与消息中的<span style='color:Blue'> [ 路由键：匹配队列 ]</span><br/>[`B`] 主题交换机`Topic Exchange`: 根据通配符匹配路由键 <span style='color:Blue'>[ 将消息路由到一个或多个队列 </span>]<br/>[`C`] 扇出交换机`Fanout Exchange`：将消息广播到所有与交换机绑定的队列 <span style='color:red'>[ 忽略路由键 ]</span> |
>| <span style='color:RED'>[`Binding`]</span>        | <span style='color:red'>[ 路由绑定 ] 交换机 & 队列之间的绑定</span><br/><span style='color:red'> [ 生产者将消息发送到交换机，而队列通过路由绑定与交换机建立关联 - 从而接收到消息 ]</span> |
>| -                                                 |                                                              |
>| <span style='color:Blue'>[`Virtual Host`] </span> | <span style='color:Blue'>[ 虚拟主机 ]</span> 是`MQ`的基本工作单元<br/><span style='color:red'>[ 每个虚拟主机拥有自己独立的用户、权限、交换机、队列等资源 - 完全隔离于其他虚拟主机 ]</span> |
>| <span style='color:red'>[`Connection`]</span>     | <span style='color:red'>是指生产者、消费者与`RabbitMQ`之间的 [ 网络连接 ]</span><br/><span style='color:red'>[ 每个连接可以包含多个信道`Channel`，每个信道是一个独立的会话通道 - 可以进行独立的消息传递 ]</span> |
>
>
>
><br/>

