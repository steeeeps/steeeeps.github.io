---
title: 基于nodejs的js、css压缩打包程序
date: 2013-03-10 17:07:53
categories:
- JavaScript
tags:
- Nodejs
---

之前在项目中一直想再上线之前将js、css文件进行打包压缩处理，由于js代码基本上都是用的dojo框架，dojo虽然提供了一个build功能，但是处于当时对这个功能的使用不是很明白，于是就想到了yui提供的yuicompressor功能，再加上一直对nodejs比较感兴趣，所以自己使用yuicompressor和nodejs编写了一套压缩打包的程序。
 
 <!-- more -->

虽然说yuicompressor是使用nodejs来调用的，但是其本质还是是调用的java。不过结合着nodejs的异步操作，使用起来还是不错的。

对于nodejs，nodejs使用事件轮询机制来实现异步，真的是相当不错的，大量的js文件压缩，使用异步操作，效率是非常高的。不过在使用nodejs过程中，如何控制异步操作的流程，是必须要好好考虑的，不过所幸，github上有很多关于控制异步的框架可以拿来使用学习。

### 使用说明：

- 安装nodejs

- 打开config.json,修改需要打包的配置信息。

- 运行 node pack.js 进行打包合并处理。

- 运行 node compress.js 进行压缩处理。

### 配置说明

``` javascript
    {
        "baseDir":"H:\\cbtest\\from",
        "targetDir":"H:\\cbtest\\to",
        "layers": [
            {
                "name": "a.js",
                "compress":true,
                "depends": [
                    {
                        "name": "applications.js",
                        "depends": [
                            {
                                "name": "effects.js",
                                "depends": []
                            },{
                                "name":"mgr\\util.js",
                                "depends":[]
                            }
                        ]
                    },{
                        "name":"portal/message.js",
                        "depends":[]
                    }
                ]
            },
            {
                "name": "b.js",
                "compress":false,
                "depends": [
                    {
                        "name": "applications.js",
                        "depends": [
                            {
                                "name": "effects.js",
                                "depends": []
                            },{
                                "name":"mgr/util.js",
                                "depends":[]
                            }
                        ]
                    },{
                        "name":"portal/message.js",
                        "depends":[]
                    }
                ]
            }

        ]
    }

```

- baseDir为原路径，targetDir为目标路径，用layers为表示打包后的js文件，name为打包后的文件名，compress可以设置是否对打包的js文件进行压缩。depends表示打包文件的依赖，通过使用depends来获取逐级依赖文件。

- 所有使用到的js或css文件如果不是填的绝对路径的话，都会以baseDir为相对路径进行查找。

- 在压缩的过程中，默认的是将源文件以.uncompress.js重命名，并以原文件名对文件进行压缩，如果配置文件中出现targetDir信息，就会将源文件所存在的目录结构拷贝至目标目录，并对文件进行压缩。

- 在打包过程中，如果出现targetDir信息，同上。

> [下载源码](https://github.com/steeeeps/CompressPackage)