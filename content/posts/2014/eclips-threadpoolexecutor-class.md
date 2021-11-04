---
title: eclipse 跳至ThreadPoolExecutor.class 解决方法
description: 
date: 2014-12-10 13:17:04
categories:
- Java
tags:
- Eclipse
- Java
---

在eclipse里面编写java代码时，经常遇见这样一个情况，在tomcat启动后，保存更新后的java文件，代码经常跳转至 ThreadPoolExecutor.class，停留在workerDone(this)一行代码.

解决方法：

在eclipse中，进入window->preferences->java->Debug，将Suspend execution on uncaught exceptions 前的复选框去掉。

> 解决方法参考至：[http://stackoverflow.com/questions/6290470/eclipse-debugger-always-blocks-on-threadpoolexecutor-without-any-obvious-excepti](http://stackoverflow.com/questions/6290470/eclipse-debugger-always-blocks-on-threadpoolexecutor-without-any-obvious-excepti)

	