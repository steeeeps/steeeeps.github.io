---
title: 高可用数据库架构设计
date: 2017-11-09 23:08:29
categories:
- 数据库
tags:
- 数据库
- 架构设计
---


高可用性这个名词可能做 web 开发的同学听的也比较多。

一般 web 系统的高可用性可以通过集群部署加负载均衡的技术来实现。但是由于数据库的特殊性，除了使用集群部署和负载均衡的技术之外，更重要的是如何保持数据库之间的数据统一性。

数据库同步机制有很多种，用较多的就是主从流复制机制。



### 主从流复制机制

Postgresql 在9.0之后引入了主从的流复制机制，包含一个主库和从库，所谓流复制，就是从库通过 tcp 流从主库同步相应的数据，主要有以下特点：

1. 从服务器通过TCP流从主服务器中实时同步相应的数据。这样当主服务器数据丢失时从服务器中仍有备份。
2. PostgreSQL流复制默认是异步的。
3. 在主服务器上提交事务和从服务器上变化可见之间有一个小的延迟，这个延迟远小于基于文件日志传送，通常1秒能完成。如果主服务器突然崩溃，可能会有少量数据丢失。
4. 主服务器支持读写，插入数据或删除数据，在从服务器上能看到相应的变化。但从服务器上只能读，不能写。

postgres 自带的主从流复制机制虽然实现了数据的同步与备份，确保了数据的统一性，但是没有提供负载均衡能力，所以还是没有实现数据库的高可用性。要实现数据库的高可用性，还需要引入第三方框架 pgpool。

### Pgpool

pgpool 是一个位于 postgresql 服务器和客户端之间的中间件，是 pg 集群开源实现中比较成功的项目之一。它主要提供下面几个功能：

1. 连接池：pgpool-II 保持已经连接到 PostgreSQL 服务器的连接， 并在使用相同参数（例如：用户名，数据库，协议版本） 连接进来时重用它们。 它减少了连接开销，并增加了系统的总体吞吐量。
2. 复制：pgpool-II 可以管理多个 PostgreSQL 服务器。 激活复制功能并使在2台或者更多 PostgreSQL 节点中建立一个实时备份成为可能， 这样，如果其中一台节点失效，服务可以不被中断继续运行。
3. 负载均衡：如果数据库进行了复制（可能运行在复制模式或者主备模式下）， 则在任何一台服务器中执行一个 SELECT 查询将返回相同的结果。 pgpool-II 利用了复制的功能以降低每台 PostgreSQL 服务器的负载。 它通过分发 SELECT 查询到所有可用的服务器中，增强了系统的整体吞吐量。在理想的情况下，读性能应该和 PostgreSQL 服务器的数量成正比。 负载均衡功能在有大量用户同时执行很多只读查询的场景中工作的效果最好。
4. 限制超过限度的连接：PostgreSQL 会限制当前的最大连接数，当到达这个数量时，新的连接将被拒绝。增加这个最大连接数会增加资源消耗并且对系统的全局性能有一定的负面影响。 pgpoo-II 也支持限制最大连接数，但它的做法是将连接放入队列，而不是立即返回一个错误。
5. 并行查询：使用并行查询时，数据可以被分割到多台服务器上， 所以一个查询可以在多台服务器上同时执行，以减少总体执行时间。并行查询在查询大规模数据的时候非常有效。

这样就可以通过在PG自身提供的流复制机制的搭建主从结构集群上，使用Pgpool提供的连接池、负载均衡、自动故障切换等功能，来实现数据库的高可用性架构能力。

当然负载均衡只针对读操作，写操作只发生在主节点上。为了避免单点故障，Pgpool自身也可以配置为主从结构，对外提供虚拟IP地址，当主节点故障后，从节点提升为新的主节点并接管虚拟IP。

### 架构图

![架构图](/images/2017/pg-ha.png)



> 参考自：
>
> [http://blog.csdn.net/wlwlwlwl015/article/details/53287855](http://blog.csdn.net/wlwlwlwl015/article/details/53287855)
>
> [https://segmentfault.com/a/1190000007012082](https://segmentfault.com/a/1190000007012082)
