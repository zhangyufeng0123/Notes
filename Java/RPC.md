# RPC

## 大纲

- 理论：RPC核心原理，技术介绍
- 实战：RPC代码实现，案例
- 总结

## 理论

1. 概念：

   RPC：Remote Procedure Call，即远程过程调用

   ​	分布式常见的通讯方式，从跨进城到跨物理机已经有几十年历史

   跨进城交互形式：

   ​	RESTFUL

   ​	WEBSERVICE

   ​	HTTP

   ​	基于DB做数据交换

   ​	基于MQ做数据交换

2. 交互形式对比：

   1. 依赖中间件做数据交互：异步

      系统A -> 数据存储（DB，MQ，redis） -> 系统B

      常用：mysql，rabbitmq，kafka，redis，可用性要求较高的时候，常用mysql，rabbitmq做存储中间件，任务存储没任务分发

   2. 直接交互：同步

      客户端 -> http, rpc, webservice -> 服务端

      常用：restful, web service, http, rpc

   3. rpc中：

      server：Provider 服务提供者

      client：Consumer 服务消费者

      Stub：存根，服务描述

3. 现有框架对比：

|    xx    |   grpc   | thrift     | RMI        | dubbo    | hadoopRPC  |
| :------: | :------: | ---------- | ---------- | -------- | ---------- |
| 开发语言 |  多语言  | 多语言     | java       | java     | java       |
|  序列化  | protobuf | thrift格式 | java序列化 | hession2 | R/Writable |
| 注册中心 |  false   | false      | jdk自带    | zk等     | false      |
|  跨语言  |   true   | true       | false      | false    | false      |
| 服务定义 | protobuf | thrift文件 | java接口   | java接口 | java接口   |
| 服务治理 |  false   | false      | false      | true     | false      |

4. 核心原理：三部分组成
   - server：服务提供
   - client：服务消费
   - registry：服务注册与发现
5. 技术栈：
   1. 基础：javacore, maven, 反射
   2. 动态代理：生成client存根世纪调用对象，java动态代理
   3. 序列化：java对象与二进制数据互转 fastjson
   4. 网络通信：用来传输序列化后的数据 jetty, URLConnection
6. 代码实现：
   1. 建工程
   2. 实现序列化模块
   3. 实现网络模块
   4. 实现server
   5. 实现client
   6. gk-rpc使用案例
7. 理解