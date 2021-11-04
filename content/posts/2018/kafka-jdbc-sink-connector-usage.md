---
title: JDBC sink connector 使用方法
description: Kafka Connect 是在0.9版本之后增加的新特性,可以更方便的创建和管理数据流管道。它为Kafka和其它系统创建规模可扩展的、可信赖的流数据提供了一个简单的模型，通过connectors可以将大数据从其它系统导入到Kafka中，也可以从Kafka中导出到其它系统。
date: 2018-01-12 22:03:00
categories:
- 大数据
- Kafka
tags:
- Kafka
---



Kafka Connect 是在0.9版本之后增加的新特性,可以更方便的创建和管理数据流管道。

它为Kafka和其它系统创建规模可扩展的、可信赖的流数据提供了一个简单的模型，通过connectors可以将大数据从其它系统导入到Kafka中，也可以从Kafka中导出到其它系统。

Kafka Connect可以将完整的数据库注入到Kafka的Topic中，或者将服务器的系统监控指标注入到Kafka，然后像正常的Kafka流处理机制一样进行数据流处理。而导出工作则是将数据从Kafka Topic中导出到其它数据存储系统、查询系统或者离线分析系统等，比如数据库、Elastic Search、Apache Ignite等。

下面以 Confluent platform 提供的 JDBC sink connector和社区提供的 mongodb sink connector 为例，介绍 connector 的用法。Confluent Platform 详细使用请看[帮助文档](http://docs.confluent.io/3.2.0/platform.html)

### JDBC sink connector 使用步骤

1. 下载 [Confluent Platform 3.2](https://www.confluent.io/download/),该版本使用的 Kafka 版本为 10.0.2.0。Confluent  platform需要在 linux 或者 osx 下使用，同时需要/var/lib目录的写权限，window 暂时支持的不好。

2. 下载安装 sqlite

3. 进入 Confluent platform 的的文件目录

4. 在单独的 terminal 中启动 Zookeeper

   ```bash
   $ ./bin/zookeeper-server-start ./etc/kafka/zookeeper.properties
   ```

5. 在单独的 terminal 中启动 kafka

   ```bash
   $ ./bin/kafka-server-start ./etc/kafka/server.properties
   ```

6. 在单独的 terminal 中启动Schema Registry

   ``` bash
   $ ./bin/schema-registry-start ./etc/schema-registry/schema-registry.properties
   ```

7. 修改 JDBC sink connector 配置文件，文件位于 **./etc/kafka-connect-jdbc/sink-quickstart-sqlite.properties**,测试可以使用默认配置即可，内容如下:

   ``` bash
   name=test-sink
   connector.class=io.confluent.connect.jdbc.JdbcSinkConnector
   tasks.max=1
   topics=orders
   connection.url=jdbc:sqlite:test.db
   auto.create=true
   ```

8. 以单机模式启动 connector

   ```bash
   $ ./bin/connect-standalone etc/schema-registry/connect-avro-standalone.properties etc/kafka-connect-jdbc/sink-quickstart-sqlite.properties
   ```

9. 使用kafka-avro-console-producer工具往 kafka topic 中推送数据，并且定义数据格式

   ```bash
   $ bin/kafka-avro-console-producer \
    --broker-list localhost:9092 --topic orders \
    --property value.schema='{"type":"record","name":"myrecord","fields":[{"name":"id","type":"int"},{"name":"product", "type": "string"}, {"name":"quantity", "type": "int"}, {"name":"price",
    "type": "float"}]}'
   ```

10. 继续在该窗口中推送需要保存在 sqlite 中的消息数据

    ```javascript
    {"id": 999, "product": "foo", "quantity": 100, "price": 50}
    ```

11. 检查 sqlite 中数据是否保存成功

    ```bash
    $ sqlite3 test.db
    sqlite> select * from orders;
    foo|50.0|100|999
    ```

    ​

> 详细步骤请查看[jdbc-sink-connector](http://docs.confluent.io/3.2.0/connect/connect-jdbc/docs/sink_connector.html)