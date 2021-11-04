---
title: 数据库性能优化(一)—SQL 语句优化
description: 在系统开发和系统运行过程中，随着数据库中数据量越来越大的时候，应用程序与数据库查询越来越慢，这时候我们通常都会首先想到去做一些数据库的性能优化相关的工作，而这方面的工作首先想到也最最容易做的就是SQL 语句优化。
date: 2017-11-09 22:38:29
categories:
- 数据库
tags:
- 数据库
---


### SQL 语句优化

在系统开发和系统运行过程中，随着数据库中数据量越来越大的时候，应用程序与数据库查询越来越慢，这时候我们通常都会首先想到去做一些数据库的性能优化相关的工作，而这方面的工作首先想到也最最容易做的就是SQL 语句优化。


sql 语句优化总结了最常见的下面这几条，虽然说是 sql 语句的优化，但是 sql 查询和索引是分不开的，所以这些优化的建议很多也是建立在索引的基础上的。

主要包括一下几点：

1. 对查询进行优化，应尽量避免全表扫描，首先应考虑在where 及 order by涉及的列上建立索引。

2. 应尽量避免在where子句中使用!=或<>操作符，否则引擎将放弃使用索引而进行全表扫描。

   > (因为SQL只对<，<=，=，>，>=，BETWEEN，IN，以及某些时候的LIKE才会使用索引)

3. 应尽量避免在where 子句中对字段进行 null值判断，否则将导致引擎放弃使用索引而进行全表扫描。 
   > 如： select id from t where num isnull 可以在num上设置默认值0，确保表中num列没有null值，然后这样查询： select id from t wherenum=0 

4. 应尽量避免在where 子句中使用 or来连接条件，否则将导致引擎放弃使用索引而进行全表扫描。

   > 如： select id from t where num=10 ornum=20 可以这样查询： select id from t wherenum=10 union all select id from t where num=20 

5. in 和not in 也要慎用，否则会导致全表扫描，如：select id from t where numin(1,2,3) 对于连续的数值，能用 between 就不要用 in了。

   > 如： select id from t where num between1 and 3

6. 应尽量避免在where子句中对字段进行表达式操作，这将导致引擎放弃使用索引而进行全表扫描。如：  select id from t wherenum/2=100 应改为:   select id from t wherenum=100*2 

7. 应尽量避免在where子句中对字段进行函数操作，这将导致引擎放弃使用索引而进行全表扫描。

   > 如：select id from t wheredatediff(day,createdate,‘2005-11-30’)=0 此句是找‘2005-11-30’生成的id 应改为: select id from t wherecreatedate>=‘2005-11-30’ and   createdate<'2005-12-1' 

8. 不要在where 子句中的“=”左边进行函数、算术运算或其他表达式运算，否则系统将可能无法正确使用索引。

9. 在使用索引字段作为条件时，如果该索引是复合索引，那么必须使用到该索引中的第一个字段作为条件时才能保证系统使用该索引，否则该索引将不会被使用，并且应尽可能的让字段顺序与索引顺序相一致。

10. 并不是所有索引对查询都有效，SQL是根据表中数据来进行查询优化的，当索引列有大量数据重复时，SQL查询可能不会去利用索引。

  > 如一表中的字段sex，数据male、female几乎各一半，那么即使在sex上建了索引也对查询效率起不了作用。

11. 索引并不是越多越好，索引固然可以提高相应的select 的效率，但同时也降低了 insert 及 update的效率，因为 insert 或update时有可能会重建索引，所以怎样建索引需要慎重考虑，视具体情况而定。

    > 一个表的索引数最好不要超过6个，若太多则应考虑一些不常使用到的列上建的索引是否有必要。

12. 尽可能的使用varchar代替 char，因为首先变长字段存储空间小，可以节省存储空间，其次对于查询来说，在一个相对较小的字段内搜索效率显然要高些

13. 尽量不要使用select * from t，用具体的字段列表代替“*”，不要返回用不到的任何字段。

14. 修改代码编写方式，使用数据库递归查询替代代码中循环加分支判断的计算查询方式。

    ​

    ​

    >  参考自:[http://www.cnblogs.com/yunfeifei/p/3850440.html](http://www.cnblogs.com/yunfeifei/p/3850440.html)







