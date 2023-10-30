# RocketMQ ONS SDK

这篇文档记录了如何通过阿里云官方提供的 RocketMQ ONS SDK 接入阿里云的消息队列服务，并提供了完整的示例程序。

## Install {id="install"}

首先通过 Maven 安装依赖:

```xml
<!-- https://mvnrepository.com/artifact/com.aliyun.openservices/ons-client -->
<dependency>
    <groupId>com.aliyun.openservices</groupId>
    <artifactId>ons-client</artifactId>
    <version>2.0.6.Final</version>
</dependency>
```
<warning>
需要注意：阿里云目前 RocketMQ 实例氛围 4.x 和 5.x 版本，其中4.x 版本某些区域是不支持 2.x 版本的SDK的。
</warning>

## Producer Examples {id="producer"}

生产者接入需要如下的配置信息:

| 配置项                    | 配置描述                      |
|------------------------|---------------------------|
| `AccessKey`            | 阿里云的AK                    |
| `SecretKey`            | 阿里云的SK                    |
| `SendMsgTimeoutMillis` | 发送消息超时时间,例如 "3000" 表示 3 秒 |
| `NAMESRV_ADDR`         | 命名服务的地址                   |

<note>
在发送消息之前，需要首先确认已经在阿里云 Rocket MQ 管理后台创建了 Topic。
</note>

### 发送普通消息 {id="send_simple_message"}

发送普通消息的示例代码如下：

<procedure>
<step>
首先需要配置属性:

```Java
Properties properties = new Properties();
properties.put(PropertyKeyConst.AccessKey, "xxxx");
properties.put(PropertyKeyConst.SecretKey, "xxxx");
properties.put(PropertyKeyConst.SendMsgTimeoutMillis, "3000");
properties.put(PropertyKeyConst.NAMESRV_ADDR, "http://MQ_INST_xxxx.ap-southeast-1.mq.aliyuncs.com:80");
```
</step>
<step>
然后构造并启动生产者:

```Java
Producer producer = ONSFactory.createProducer(properties);
producer.start();
```
</step>

<step>
构造消息对象:

```Java
Message message = new Message("DEV_JUN_XIAO_TEST", "*", "Hello MQ".getBytes());
message.setKey(UUID.randomUUID().toString());
```
</step>

<step>
发送消息，发送完成之后关闭生产者(如果频繁发送，可以不主动关闭，节省内存资源):

```Java
producer.send(message);
producer.shutdown();
```
</step>
</procedure>
