---
layout: post
title: "oslo messaging listener代码分析"
excerpt: "文档中主要涉及到一些AMQP的术语， 需要有KOMBU和RabbtiMQ相关知识。" 
categories: articles
tags: ['oslo.messaging', 'RabbtiMQ', 'KOMBU']
comments: true
share: true
image:
  feature: so-simple-sample-image-5.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---



A notification listener exposes a number of endpoints, each of which
contain a set of methods. Each method corresponds to a notification priority.   
一个通知监听器会暴露一些endpoints， 每一个endpoints都包含一套方法。 每一个方法对应一个通知的优先级。 

To create a notification listener, you supply a transport, list of targets and
a list of endpoints.  
为了创建一个通知监听器， 你需要提供一个transport, targets列表和endpoints列表。 

A transport can be obtained simply by calling the get_transport() method::  
一个transport可以简单得通过调用get_transport()方法来获取： 
    transport = messaging.get_transport(conf)

which will load the appropriate transport driver according to the user's
messaging configuration configuration. See get_transport() for more details.  
它会根据用户的消息配置来导入适当的transport驱动。 更多细节查看get_transport()方法

The target supplied when creating a notification listener expresses the topic
and - optionally - the exchange to listen on. See Target for more details
on these attributes.  
当创建一个通知监听器时， 这个target被用来当作topic(可选，exchange)来监听。 查看Target获取关于这些属性的更多细节。 


Notification listener have start(), stop() and wait() messages to begin
handling requests, stop handling requests and wait for all in-process
requests to complete.  
通知监听器有start(), stop(), wait() 方法来开始处理请求， 停止处理请求和等待所有进行中的请求完成。 


Each notification listener is associated with an executor which integrates the
listener with a specific I/O handling framework. Currently, there are blocking
and eventlet executors available.  
每一个通知监听器都被关联到一个executro上， 将这个监听器和一个指定的I/O处理框架。 当前有blocking和eventlet executors可用。 

A simple example of a notification listener with multiple endpoints might be::  
一个简单的例子关于一个监听器关联多个endpoints: 
```python
    from oslo.config import cfg
    from oslo import messaging

    class NotificationEndpoint(object):
        def warn(self, ctxt, publisher_id, event_type, payload, metadata):
            do_something(payload)

    class ErrorEndpoint(object):
        def error(self, ctxt, publisher_id, event_type, payload, metadata):
            do_something(payload)

    transport = messaging.get_transport(cfg.CONF)
    targets = [
        messaging.Target(topic='notifications')
        messaging.Target(topic='notifications_bis')
    ]
    endpoints = [
        NotificationEndpoint(),
        ErrorEndpoint(),
    ]
    pool = "listener-workers"
    server = messaging.get_notification_listener(transport, targets, endpoints,
                                                 pool)
    server.start()
    server.wait()
```

A notifier sends a notification on a topic with a priority, the notification
listener will receive this notification if the topic of this one have been set
in one of the targets and if an endpoint implements the method named like the
priority .  
一个通知者发送一个通知到一个带有优先级的topic上， 如果这个topic已经被设置为这些targets中的一个和一个endpoint使用这个优先级命名它的方法， 那么这个通知监听器会接收这个通知。 

Parameters to endpoint methods are the request context supplied by the client,
the publisher_id of the notification message, the event_type, the payload and
metadata. The metadata parameter is a mapping containing a unique message_id
and a timestamp.  
endpoint方法的参数通过客户端提供的请求上下文， 通知消息的pubisher_id, event_type, payload和metadata。 这metadata参数是一个包含一个唯一的message_id和一个时间戳的映射。 

By supplying a serializer object, a listener can deserialize a request context
and arguments from - and serialize return values to - primitive types.  
通过提供一个序列化的对象， 一个监听器可以从序列化返回的数据到原始类型来反序列化一个请求的上下文和参数。 

By supplying a pool name you can create multiple groups of listeners consuming
notifications and that each group only receives one copy of each notification.  
通过提供一个pool名字， 你可以创建多个组的监听器来消费通知，同时每个组只是接收到每个通知的一份复制。 

An endpoint method can explicitly return messaging.NotificationResult.HANDLED
to acknowledge a message or messaging.NotificationResult.REQUEUE to requeue the
message.  
一个endpoint方法可以明确的返回messaging.NotificationResult.HANDLED来确认一个消息或者返回一个messaging.NotificationResult.REQUEUE是这个消息重新排队。 

The message is acknowledged only if all endpoints either return
messaging.NotificationResult.HANDLED or None.  
这个消息除非所有endpoints返回messaging.NotificationResult.HANDLED或者None时，才会被确认。  

Note that not all transport drivers implement support for requeueing. In order
to use this feature, applications should assert that the feature is available
by passing allow_requeue=True to get_notification_listener(). If the driver
does not support requeueing, it will raise NotImplementedError at this point.  
记住不是所有的transport驱动会支持重新排队。 为了使用这个功能， 应用程序需要通过传递allow_requeue=True给get_notification_listener()方法来宣称这个功能是可用的。
 如果这个驱动不支持重新排队， 它会在这个点上引发 messaging.NotificationResult.HANDLED 。 

==============================================================================

```python

from oslo.messaging.notify import dispatcher as notify_dispatcher
from oslo.messaging import server as msg_server


def get_notification_listener(transport, targets, endpoints,
                              executor='blocking', serializer=None,
                              allow_requeue=False, pool=None):
    """Construct a notification listener

    The executor parameter controls how incoming messages will be received and
    dispatched. By default, the most simple executor is used - the blocking
    executor.

    :param transport: the messaging transport
    :type transport: Transport
    :param targets: the exchanges and topics to listen on
    :type targets: list of Target
    :param endpoints: a list of endpoint objects
    :type endpoints: list
    :param executor: name of a message executor - for example
                     'eventlet', 'blocking'
    :type executor: str
    :param serializer: an optional entity serializer
    :type serializer: Serializer
    :param allow_requeue: whether NotificationResult.REQUEUE support is needed
    :type allow_requeue: bool
    :param pool: the pool name
    :type pool: str
    :raises: NotImplementedError
    """
    transport._require_driver_features(requeue=allow_requeue)
    dispatcher = notify_dispatcher.NotificationDispatcher(targets, endpoints,
                                                          serializer,
                                                          allow_requeue, pool)
    return msg_server.MessageHandlingServer(transport, dispatcher, executor)
```
