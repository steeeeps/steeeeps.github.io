---
title: 不要节省敲打引号的时间
date: 2013-06-03 11:02:36
categories:
- 程序员
tags:
- 程序员
---


上周五准备下班的时候，系统在chrome下运行点击某一个按钮没反应，查看控制台，发现抛出这样一个错误：

> **Uncaught SyntaxError: Unexpected token ILLEGAL**

以前遇见过，感觉也就是标点符号一类的输入不正确，于是开着eclipse开始查看这个页面以及和这个页面相关的js代码，但是一看这几个页面的svn提交日期居然都是3月20多号，而且自己也隐约能够记得，周四检验功能的时候，系统的确是没有问题的。

<!-- more -->

我的脑海中开始浮现**程序员常说的话：**

> *   昨天还好好的
> *   我好几个星期都没碰这块代码了！
> *   … …

我开始思考用哪句话来面对这个突如其来的难题…

虽然如此，但是在产品等着发布的压力之下，我也只有硬着头皮开始一步一步的查看同事休假遗留下来的代码。从html到javascript，一行代码一行代码的仔细检查，均没有发现问题。

于是我又开始使用chrome开发者工具一点一点分析页面解析过的代码，终于发现了一段类似于这样的代码：

   ``` html
   <div onclick="alert('错误" 用法')>错误用法</div>
   ```

    而**原文件中的代码**是这样的:

    ``` html
    <div onclick=alert('错误 用法')>错误用法</div>
    ```

**现在已经很明显了，问题就出在源码里`onclick`事件调用`alert`函数传入一个带空格的参数，并且注册`onclick`事件时没有加引号所引起的。**

但是，在HTML5中，对于标签的属性的确是可以不用加引号的，浏览器会自动解析并且添加引号，这一点是没有错的。主要错误的原因就是：传递给函数的参数是程序通过ajax请求，从数据库中获取的，比如参数`a`，然后再将`a`作为参数传入函数中。当`a`为一个不包含空格的词时，整个程序运行perfect。但是当`a`中间包含一个或多个空格时，并且注册事件没有使用引号，这时按照浏览器的解析原则，就出现了文中所描述的错误。

我想，当时编写这个代码的同事，在检验功能时，数据库返回的参数`a`应该是没有空格的，当自己根据HTML5中这个规则添加属性时，肯定还在偷着乐，HTML5真方便…

很多程序员都想用最短的时间敲最多的代码、完成最多的功能，包括我。所以很多时候我们都会去忽略一些规范，比如最简单的命名规范，从而达到快速。当然，忽略命名规范是肯定不会出现文中这个错误。但是为了程序的健壮性，为了对以后维护代码的程序员负责，不要节省敲打引号的时间，不要轻易的忽略编程的规范性。

突然想起一句话：**码农何必为难码农!**