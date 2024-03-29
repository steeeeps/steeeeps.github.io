
---
title: 数据库性能优化(三)—数据库表分区
date: 2017-11-09 22:58:29
description: 当对 sql 语句也做了优化，同事对数据表也建立了相应的索引后，数据库的整体查询性能能提高不少。但是随着系统的运行我们又会遇见其他问题。
categories:
- 数据库
tags:
- 数据库
---



当对 sql 语句也做了优化，同事对数据表也建立了相应的索引后，数据库的整体查询性能能提高不少。但是随着系统的运行我们又会遇见其他问题。

我相信大家在开发过程中也都遇见过这种场景：随着使用时间的增加，数据库中的数据量也不断增加，因此数据库查询越来越慢。而且许多数据是历史数据并且随着时间的推移它们的重要性逐渐降低，可能很少会用到，但是这些数据又不能删除，所以如果能找到一个办法将这些可能不太重要的数据隐藏，数据库查询速度将会大幅提高。


比如说我们的系统运行日志，还有车辆轨迹监控数据。都存在这种问题。因此，就需要一个高效的把历史数据从当前查询中隐藏起来并且不造成数据丢失的方法。而数据库表分区就能达到这样的效果。

### 数据库表分区含义

数据库表分区指的是把大的物理表分成若干个小的物理表，并使得这些小物理表在逻辑上可以被当成一张表来使用。

分区表有一张主表和若干子表组成，主表是正常的普通表，但正常情况下并不存储任何数据，子表继承并属于主表，用来存储具体的数据。如下图所示：

![](http://www.jasongj.com/img/sql/SQL3/partition_arch.png)

### 数据库表分区优势

1. 在特定场景下，查询性能极大提高，尤其是当大部分经常访问的数据记录在一个或少数几个分区表上时。表分区减小了索引的大小，并使得常访问的分区表的索引更容易保存于内存中。
2. 当查询或者更新访问一个或少数几个分区表中的大部分数据时，可以通过顺序扫描该分区表而非使用大表索引来提高性能。
3. 可通过添加或移除分区表来高效的批量增删数据。如可使用ALTERTABLE NO INHERIT可将特定分区从主逻辑表中移除（该表依然存在，并可单独使用，只是与主表不再有继承关系并无法再通过主表访问该分区表），或使用DROPTABLE直接将该分区表删除。这两种方式完全避免了使用DELETE时所需的VACUUM额外代价。
4. 很少使用的数据可被迁移到便宜些的慢些的存储介质中



> 数据库表分区详细内容与具体实现，参考[http://www.jasongj.com/2015/12/13/SQL3_partition/](http://www.jasongj.com/2015/12/13/SQL3_partition/)