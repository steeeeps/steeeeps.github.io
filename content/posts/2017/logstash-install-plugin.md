---
title: logstash 插件安装方式
date: 2017-12-26 22:10:49
categories:
- 大数据
tags:
- ELK
---



logstash提供了很多filter、input、output插件，但是有些是需要自己安装的，以logstash-output-mongodb为例，纪录安装插件的方法。

### 在线安装

以下两个操作需要联网，并确保能访问[https://rubygems.org/](https://rubygems.org/),如果不能访问可以使用代理或者修改logstash 安装目录下的 gemfile文件中的源为淘宝镜像https://ruby.taobao.org/。

#### 直接安装

1. 进入logstash安装目录，输入以下代码进行安装

   ``` bash
   bin/logstash-plugin install
   ```

#### 源码安装

1. 下载插件源码[logstash-output-mongodb](https://github.com/logstash-plugins/logstash-output-mongodb),并解压。

2. 打开解压目录，打开 gemfile文件并找到或者添加如下配置：

   ```bash
   gem "logstash-output-mongodb", :path => "file:///C:/soft/logstash-output-mongodb-3.1.1"
   ```

3. 执行代码

   ```bash
    bin/logstash-plugin install --no-verify
   ```

4. 安装成功

> 这一步网上介绍的都是属于离线安装模式，但是在实际操作中还是 **需要联网** 



### 离线安装

生产环境很多服务器无法联网，无法安装插件，可以先在能够联网的服务器上使用以上两种方式进行安装，然后使用以下两种方式将插件拷贝至无法联网的服务器。

#### 直接拷贝

1. 拷贝 [logstash安装目录]/vender/bundle/jruby/1.9/gems/ 下面的你要的插件整个文件夹到目标机器同目录下。
2. 拷贝 [logstash安装目录]/vendor/bundle/jruby/1.9/specifications/ 下面对应的声明文件到目标机器同目录下。
3. 手动修改[logstash安装目录]/Gemfile 文件，加入插件行如：gem "logstash-output-mongodb"

> 这种方法我测试了，安装无效， **不推荐继续使用**。

#### 离线打包

使用官方推荐的离线插件管理方法进行安装

1. 进入logstash安装目录，运行以下命令，对插件打包。

   ```bash
   bin\logstash-plugin prepare-offline-pack --output logstash-output-mongodb.zip logstash-output-mongodb
   ```

2. 将运行结果生成的logstash-output-mongodb.zip文件拷贝至目标服务器，运行以下命令，进行离线安装。

   ```bash
   bin\logstash-plugin install file:///c:/path/logstash-output-mongodb.zip
   ```

3. 提示，Install successful，安装成功。

> **logstash离线安装插件推荐使用此方式进行安装**。