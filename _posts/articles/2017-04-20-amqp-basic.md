---
layout: post
title: "amqp basic"
excerpt: "amqp介绍和rabbitmq特性" 
categories: articles
tags: ['amqp', 'rabbitmq']
comments: true
share: true
image:
  feature: so-simple-sample-image-2.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

消息中间件的产生主要是为了让开发者从编写消息交互组件这个大坑中脱离出来，只把注意力放在应用或业务层，而不去关心这些业务关系不大的东西。这些消息相关的东西由第三方独立提供。QPID就是这么一款中间件，其实现了AMQP标准。要记住AMQP只是个标准，可以找到他的标准文档，但它不是个实物。QPID是一个实现了AMQP标准的实物。比如HTTP就是个标准，而IE浏览器就是实现了这个标准的一个实物。这两个概念要搞清。
所以要知道QPID或RabbitMQ的实现原理，就要知道AMQP这个东西是啥。

“在AMQP模型中，消息的producer将Message发送给Exchange，Exchange 负责交换和路由，将消息正确地转发给相应的Queue。消息的 Consumer 从 Queue 中读取消息。这个过程是异步的，Producer 和 Consumer 没有直接联系甚至可以不知道彼此的存在。Exchange 如何进行路由的呢？这便依靠 Routing Key，每个消息都有一个 routing Key，而每个 Queue 都可以通过一个 Binding 将自己所感兴趣的 Routing Key 告诉 Exchange，这样 Exchange 便可以将消息正确地转发给相应的Queue。”
这个是第一个链接里的原话。简单的说就是我要邮寄一个快递，这个快递包裹就是我的message，我把这个包裹给快递公司，这个快递公司就是exchange。快递公司根据我包裹给出的信息(routing key)会扔到不同城市的收件线路上，这里的收件线路就是queue，而扔这个动作就是路由。最后会有人从相应的收件线路上取到包裹，那么人就是consumer。

对于exchange的route这个动作，有下面三类：

1.fan-out：不看routing key，直接把消息放到所有注册到exchange上的queue上
2.direct：只把消息放到和routing key有相匹配key的queue上
3.topic：这里的routing key和queue的key的匹配不是完全的匹配，而是一个模糊匹配。所以一个routing key可能可以匹配到多个queue

对于qpid和RabbitMQ这种AMQP的实现来说，其一般都会有两个组件：broker和client。broker就是它的服务，跑在某台机子上。而client则是客户端的开发包或者是一些工具，用于去和broker取得通信。

下面学习下具体的产品：

RabbitMQ：

查看队列中的消息个数：

{% highlight shell %}

[root@OS_DEV ~]# rabbitmqctl list_queues

{% endhighlight %}

查看message在queue中的情况：

{% highlight shell %}
[root@OS_DEV ~]# rabbitmqctl list_queues name messages_ready messages_unacknowledged
{% endhighlight %}

查看exchange：

{% highlight shell %}
[root@OS_DEV ~]# rabbitmqctl list_exchanges
{% endhighlight %}

查看exchange和queue的绑定：

{% highlight shell %}
[root@OS_DEV ~]# sudo rabbitmqctl list_bindings
{% endhighlight %}

对于RabbitMQ来说，有一个acknowledgments的选项。其存在的功能是：当consumer从queue中取出一个message来处理的时候，如果没有发送acknowledgment，那么RabbitMQ就不会把这个message从内存中移除。RabbitMQ必须等到consumer给出acknowledgment才会移除这个message。但是RabbitMQ还有一种情况会把这个message发给其它consumer，那就是RabbitMQ发现这个consumer挂掉了（连接中断了）。这个时候RabbitMQ会的把这个消息给其它的consumer，让其去处理，或者继续留在队列里。总之就是保证消息不丢。而且RabbitMQ没有timeout这个概念，你consumer只要不死，我就不会把message给别人处理（所以如果consumer hang住了，可能要杀了这个进程才能让其它consumer处理这个消息）

acknowledgments可以保证message都被consumer处理。但有没有机制可以保证RabbitMQ server挂了后消息不丢呢？默认在RabbitMQ server退出或宕机的时候queue和message的信息都会消失，这就需要手工指定queue和message为durable。（当然啦，这里的原理就是把message和queue都保存在磁盘上，对性能有一定的影响。另外也不能百分百保证不丢失消息，毕竟写如磁盘的时候宕机的话也会有问题）

上面将的这些RabbitMQ有一个特点：一个message只能被一个consumer处理。还有一种模式叫做publish/subscribe，后者的话我producer发出一个消息可以被多个consumer同时处理。不过其实原理上是一样的：一个queue上的一个message只能被一个consumer处理，所以如果我有n个consumer要都处理某个message，那么我需要有n个queue，并且把exchange配制成fan-out。

一个queue可以绑定多个routing key。

可以使用RabbitMQ这种消息中间件来建立RPC模型。原理很简单：

调用者自己有一个队列queue.XXXXX用于接收返回的数据，其调用rpc后，会发送请求到某个队列中（不过是通过exchange转的），这个请求包括了一个correlation_id和一个标明queue.XXXXX是返回队列的属性。响应者在收到这个请求后就会的处理消息，然后把correlation_id和结果返回给queue.XXXXX，这样调用者就知道请求处理完毕了，并且也知道这个响应是来自于哪个请求（通过correlation_id） 


