---
title: 在java中执行javascript代码
description: JavaScript 相对Java来说，在某些方面是比较方便的，之前项目某个需要验证的功能，没有使用正则表达式，而是使用一堆数字和运算符来验证，在Java中实现就稍微复杂了一些。但是JavaScript实现是比较简单的。
date: 2015-01-07 23:27:57
categories:
- Java
tags:
- Java 
- JavaScript
---

JavaScript 相对Java来说，在某些方面是比较方便的，之前项目某个需要验证的功能，没有使用正则表达式，而是使用一堆数字和运算符来验证，在Java中实现就稍微复杂了一些。但是JavaScript实现是比较简单的。

好在JDK1.6之后新增了ScriptEngine类，允许Java程序直接调用javascript代码。简单的使用例子：
``` java
import javax.script.ScriptEngine;
import javax.script.ScriptEngineManager;
import javax.script.ScriptException;

public class ScriptEngineDemo {

/**
* @param args
*/
public static void main(String[] args) {
// TODO Auto-generated method stub
ScriptEngineManager mgr = new ScriptEngineManager();
ScriptEngine engine = mgr.getEngineByName("JavaScript");

try {
String script=" 1 === true ";
boolean result = Boolean.valueOf(engine.eval(script).toString());
System.out.println(result);//输出为 ：false
} catch (ScriptException e) {
// TODO Auto-generated catch block
e.printStackTrace();
}

}

}
```