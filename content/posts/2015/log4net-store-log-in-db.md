---
title: log4net自定义日志内容入库
description: 
date: 2015-10-28 17:58:17
categories:
- C#
tags:
- SQLite
- log4net
---

在系统开发中，日志记录是最基本的功能，良好的日志记录习惯与记录格式，对开发调试、错误定位有很大的帮助。  
log4net是在.net开发中使用的最多的日志框架，使用很方便，只需要简单的配置，就能够将日志信息输出到文本文件中。其实log4net也支持将用户自定义的日志类型直接输出到各类型数据库中。这样就更方便的在程序中以表格或其他方式对日志进行展示。  
以sqlite数据库为例，展示log4net使用反射机制将自定义日志内容存入数据库中。  

## 具体步骤

### 1.在数据库中创建日志表  
``` SQL
CREATE TABLE LOG (logtime timestamp,message varchar,user varchar)
```

### 2.创建自定义日志类
``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Loglite
{
    class LogModel
    {
        public string User
        {
            get;
            set;
        }



        public DateTime LogTime
        {
            get;
            set;

        }

        public string Message
        {
            get;
            set;
        }


        public LogModel(string user, string message)
        {
            this.User = user;
            this.Message = message;
            this.LogTime = DateTime.Now;
        }
    }
}

```
### 3.自定义视图转换器  
``` csharp
using log4net.Layout.Pattern;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using System.Text;

namespace Loglite
{
    class LoglitePatternConverter:PatternLayoutConverter
    {

        protected override void Convert(System.IO.TextWriter writer, log4net.Core.LoggingEvent loggingEvent)
        {
            if (Option != null)
            {
                // Write the value for the specified key
                WriteObject(writer, loggingEvent.Repository, LookupProperty(Option, loggingEvent));
            }
            else
            {
                // Write all the key value pairs
                WriteDictionary(writer, loggingEvent.Repository, loggingEvent.GetProperties());
            }

        }
        /// <summary>
        /// 通过反射获取传入的日志对象的某个属性的值
        /// </summary>
        /// <param name="property"></param>
        /// <returns></returns>
        private object LookupProperty(string property, log4net.Core.LoggingEvent loggingEvent)
        {
            object propertyValue = string.Empty;

            PropertyInfo propertyInfo = loggingEvent.MessageObject.GetType().GetProperty(property);
            if (propertyInfo != null)
                propertyValue = propertyInfo.GetValue(loggingEvent.MessageObject, null);

            //Console.WriteLine(property + " : " + propertyValue);
            return propertyValue;
        }
    }
}

```

### 4.自定义视图
``` csharp
using log4net.Layout;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Loglite
{
    class LogliteLayout:PatternLayout
    {
        public LogliteLayout()
        {
            this.AddConverter("property", typeof(LoglitePatternConverter));

        }
    }
}

```
### 5.修改log4net.xml  
``` xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <section name="log4net" type="System.Configuration.IgnoreSectionHandler, log4net" />
  </configSections>
  <log4net>
    <appender name="ADONetAppender" type="log4net.Appender.ADONetAppender,log4net">
      <!--BufferSize为缓冲区大小-->
      <bufferSize value="2" />

      <!--<param name="BufferSize" value="2" />-->
      <!--引用-->
      <connectionType value="System.Data.SQLite.SQLiteConnection, System.Data.SQLite, Version=1.0.96.0, Culture=neutral, PublicKeyToken=db937bc2d44ff139" />
      <!--连接字符串-->
      <connectionString value="Data Source=log.sqlite" />
      <!--插入语句-->
      <commandText value="insert into LOG (LOGTIME,MESSAGE,USER) Values(@LogTime,@Message,@User);" />
      <commandType value="Text"/>


      <!--记录时间-->
      <parameter>
        <parameterName value="@LogTime" />
        <dbType value="DateTime" />
        <layout type="log4net.Layout.RawTimeStampLayout" />
      </parameter>

      <!--异常消息-->
      <parameter>
        <parameterName value="@Message" />
        <dbType value="String" />
        <layout type="Loglite.LogliteLayout">
          <conversionPattern value="%property{Message}" />
        </layout>
      </parameter>


      <parameter>
        <parameterName value="@User" />
        <dbType value="String" />
        <layout type="Loglite.LogliteLayout">
          <conversionPattern value="%property{User}" />
        </layout>
      </parameter>
    </appender>
    <appender name="LogAllToFile" type="log4net.Appender.RollingFileAppender,log4net">
      <!--输出格式
                     每种转换符号都以%开始，后面跟着一个格式符号和换符号。
                     %-数字　：该项的最小长度，小于最小长度的用空格填充
                     %m(message):输出的日志消息
                     %n(new line):换行 
                     %d(datetime):输出当前语句运行的时刻 
                     %r(run time):输出程序从运行到执行到当前语句时消耗的毫秒数 
                     %t(thread id):当前语句所在的线程ID 
                     %p(priority): 日志的当前优先级别，即DEBUG、INFO、WARN…等 
                     %c(class):当前日志对象的名称，
                     %L(line )：输出语句所在的行号 
                     %F(file name)：输出语句所在的文件名
                     %logger　日志名称
                 -->
      <param name="File" value="log\"/>
      <param name="AppendToFile" value="true"/>
      <param name="MaxSizeRollBackups" value="100"/>
      <param name="MaximumFileSize" value="1KB"/>
      <param name="StaticLogFileName" value="false"/>
      <param name="DatePattern" value="yyyy-MM-dd".log""/>
      <param name="RollingStyle" value="Date"/>
      <layout type="Loglite.LogliteLayout">
        <param name="ConversionPattern" value="记录时间：%date 线程ID:[%thread] 日志级别：%-5level  %n用户：%property{User}  %n日志描述：%property{Message} %n异常信息：%exception%n" />
      </layout>




    </appender>
    <logger name="LogToSqlite">
      <level value="INFO"/>
      <appender-ref ref="ADONetAppender"/>
    </logger>
    <logger name="LogToFile">
      <level value="ALL"/>
      <appender-ref ref="LogAllToFile"/>
    </logger>
    <!--所有logger的基础，root的设置在所有logger中都起作用。 
        当在root和logger中重复设定相同的appender时，你会发现同一日志信息将被记录两次。-->
    <!--<root>
      <level value="ERROR"/>
      -->
    <!--ALL, DEBUG, INFO, WARN, ERROR, FATAL, OFF-->
    <!--
      -->
    <!--<appender-ref ref="LogAllToFile"/>-->
    <!--
      <appender-ref ref="ADONetAppender"/>
    </root>-->
  </log4net>
</configuration>

```
### 6.调用log4net存储日志  
``` csharp
LogModel logModel = new LogModel("steeeeps", "app start");

string assemblyFilePath = Assembly.GetExecutingAssembly().Location;
string assemblyDirPath = Path.GetDirectoryName(assemblyFilePath);
string configFilePath = assemblyDirPath + " \\log4net.xml";
            log4net.Config.XmlConfigurator.ConfigureAndWatch(new FileInfo(configFilePath));

log4net.ILog logdb = null;
log4net.ILog logfile = null;

logdb = (log4net.ILog)log4net.LogManager.GetLogger("LogToSqlite");
logdb.Info(logModel);

logfile = (log4net.ILog)log4net.LogManager.GetLogger("LogToFile");
logfile.Error(logModel, new Exception("app start has error"));
```