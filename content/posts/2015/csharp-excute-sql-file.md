---
title: c# 执行sql脚本文件
description: 
date: 2015-03-07 23:16:39
categories:
- C#
tags:
- C#
- SQL
---

在工作中，之前遇见过这种情况：

需要通过sql脚本在多个服务器上创建数据库，比如在oracle中创建一个数据库并导入数据，我之前是这样做的：

> 1.  打开PlSql
> 2.  打开创建表空间sql脚本，执行。
> 3.  打开创建用户sql脚本，执行。
> 4.  打开创建表结构sql脚本，执行。
> 5.  打开初始数据sql脚本，执行。

然后要创建多个数据库的话，同上同上….

然后根据不同的用户需求，我不仅要装oracle，还得装sqlserver和postgresql。反复的手工操作是很苦逼的。

为了简化手工操作，便使用c# 写了这样一个工具，主要功能在于：

1. 手动选择数据库类型，oracle、sqlserver、postgresql。
2. 手动配置需要执行的sql脚本与执行顺序。
3. 根据配置的sql脚本顺序执行，并返回执行结果。


以oracle数据库为例，sql脚本配置如下：

> create_tablespace.sql
> create_user.sql
> create_tables.sql
> import_data.sql   

其实使用c#来操作这三大主流数据库是非常简单的，网上有很多例子，我只是简单的封装了一下，在完成的过程中主要注意以下几点：

1. 工具需要即拿即用，不需要安装oracle客户端即可使用，所以需要下载操作相关dll，注意下载时需要根据实际需求区分64、32位版本。在此提供我用到的[oracle11g_64位dll](http://pan.baidu.com/s/1c0jKrYg "oracle11g_64_dll")。
2. 操作postgresql需要下载[Npgsql.dll](http://npgsql.projects.pgfoundry.org/)。
3. 虽说是执行sql脚本文件，其实本质还是执行的一条条sql语句，不同的数据库平台语句分割方式不一样，oracle和postgresql使用“;”而sqlserver使用&nbsp;“GO”，这一点可以在导出的sql脚本中查看。
4. oracle创建表空间时需要先创建表空间目录，所以推荐在创建表空间脚本中不写创建路径，系统会自动将表空间创建于默认目录，路径为：[ORACLE_INSTALL_DIR]\product\11.1.0\db_1\database。
5. postgresql创建表空间需要在数据库安装路径中手动创建表空间文件夹，该路径为[POSTGRESQL_INSTALL_DIR]\tablespace\，并且将新建的表空间文件夹的写权限赋予TrustedInstaller 和 user 用户。或者也可以选择使用默认表空间来导入数据。

主要调用代码如下：
``` csharp
DBModel model = new DBModel("Oracle", "127.0.0.1", "1521", "sys", "admin", "", "orcl");
            string filepath = "test.sql";
            IDBOperate dboperate = null;
            if (model.Type == "Oracle")
            {
                dboperate = new OraceDBOperate();
            }
            else if (model.Type == "Postgresql")
            {
                dboperate = new PostgreSqlDBOperate();
            }
            else if (model.Type == "Sqlserver")
            {
                dboperate = new SqlServerDBOperate();
            }
            try
            {
                dboperate.OpenConnection(model);
                dboperate.ExecuteCmdFile(filepath);
                dboperate.CloseConnection();
            }
            catch (Exception ex)
            {
                throw (ex);
            }

```

该工具数据库操作代码，有需要的朋友请查看源码：[DBManager](https://github.com/steeeeps/DBManager "源码").

**此文与代码中有任何错误的地方，欢迎交流指正**。