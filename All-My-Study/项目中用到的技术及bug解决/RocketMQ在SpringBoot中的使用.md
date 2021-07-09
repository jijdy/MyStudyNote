**引入Maven依赖**

~~~xml
<!-- RocketMQ-->
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-spring-boot-starter</artifactId>
    <version>2.0.2</version>
</dependency>
~~~

**属性配置**

~~~properties
# rocketmq配置，默认端口为9876，
rocketmq.name-server=127.0.0.1:9876
rocketmq.producer.group=default
~~~

**使用步骤**

* 使用RocketMQTemplate来对rocketMQ进行操作，简化了连接和基础的使用
* 使用@RocketMQMessageListener(consumerGroup = "default", topic = "VOTE_TOPIC")注解并且这个类需要实现RocketMQListener<MessageExt>接口，来通过topic风格进行操作，对其中的onMessage方法进行重写，

**启动和业务**

1. 在mq的服务器中启动mqnamesrv.cmd和mqbroker.cmd -n 127.0.0.1:9876 autoCreateTopicEnable=true两个Windows脚本文件，来开启mq的注册服务器和mq的执行服务器，主机地址加端口(默认为9876)，主题由使用者自己定义
2. 使用业务(点赞) ==》发送MQ到mq服务器 ==》mq收到消息 ==》向WebSocket发送消息并到前端服务器
3. 