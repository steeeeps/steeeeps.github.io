---
title: mongodb sink connector 使用方法
date: 2018-01-12 22:17:00
categories:
- 大数据
- Kafka
tags:
- Kafka
---

mongodb sink connector 大致用法与 jdbc sink connector 相似。只是 jdbc sink connector 是 Confluent 官方提供。

mongodb sink connector 是开源社区提供，我选择是的[hpgrahsl/kafka-connect-mongodb](https://github.com/hpgrahsl/kafka-connect-mongodb)。

# mongodb sink connector 使用步骤

1. 下载上述 kafka connector mongodb,执行 mvn clean package 对工程打包。

2. 将打包后的jar 包注册到java CLASSPATH 中，或者将打包后的kafka-connect-mongodb-1.0-SNAPSHOT-jar-with-dependencies.jar 文件拷贝至Confluent platform安装目录/share/java/confluent-common 中，我选择的是后者。

3. 在Confluent 安装目录/etc/ 目录下创建 mongodb 文件夹，将 **kafka-connect-mongodb目录/config/MongoDbSinkConnector.properties** 文件拷贝于此。关于此文件的配置信息请参考[mongodb-sink-connector-configuration](https://github.com/hpgrahsl/kafka-connect-mongodb/blob/master/README.md)

4. 下载并安装 mongodb

5. 开启三个 terminal，分别执行

   ``` bash
   1) $ ./bin/zookeeper-server-start ./etc/kafka/zookeeper.properties
   2) $ ./bin/kafka-server-start ./etc/kafka/server.properties  
   3) $ ./bin/schema-registry-start ./etc/schema-registry/schema-registry.properties
   ```

6. 以单机模式启动 connector

   ``` bash
   $ ./bin/connect-standalone etc/schema-registry/connect-avro-standalone.properties etc/mongodb/MongoDbSinkConnector.properties
   ```

7. 使用kafka-avro-console-producer工具往 kafka topic 中推送数据，并且定义数据格式

   ```bash
   $ bin/kafka-avro-console-producer \
    --broker-list localhost:9092 --topic test \
    --property value.schema='{"type":"record","name":"myrecord","fields":[{"name":"id","type":"int"},{"name":"product", "type": "string"}, {"name":"quantity", "type": "int"}, {"name":"price",
    "type": "float"}]}'
   ```

8. 继续在该窗口中推送需要保存在 sqlite 中的消息数据

   ```javascript
   {"id": 999, "product": "foo", "quantity": 100, "price": 50}
   ```

9. 使用 mongodb 可视化工具或者命令行检查 mongodb 中数据是否保存成功.