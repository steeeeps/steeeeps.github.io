---
title: javascript引入页面方式
description: 
date: 2012-12-15 17:34:04
categories:
- JavaScript
tags:
- JavaScript
---

这一周开始学习大师 Nicholas C. Zakas 所著的[High Performance JavaScript](http://www.amazon.com/Performance-JavaScript-Faster-Application-Interfaces/dp/059680279X/ "High Performance JavaScript ")，第一次尝试阅读英文原版，感觉难度也不是想象中的那么大，遇见不懂得单词直接“有道”。对于有一定javascript基础的开发人员来说，我觉得读一点英文原版的技术书籍，对技术的理解还是会有很大的帮助。所以也希望自己能继续坚持下来。

在学习了第一章Loading and Execution(javascript的加载与执行)后，自己感触也挺深的，结合自己这两个月时间所做的项目，对javascript的加载与执行进行一次总结，希望对以后的工作有所帮助。

关于javascript在浏览器中的加载，其实有点经验的开发人员大都知道，由于浏览器使用单进程来处理UI的更新和javascript的加载与执行，所以，当浏览器自上而下解析html页面时，遇上script标签的时候，浏览器就会去下载script标签中引入的外部javascript文件和执行内联或下载完成以后的javascript代码，而造成页面的等待，即使现在的主流浏览器都允许并行下载javascript，来节省下载js所用的时间，但是javascript下载的过程中也会影响其他的资源下载，比如说图片，而且，浏览器仍然会等待所有的javascript下载完成并执行完成以后才会继续解析页面，所以javascript的阻塞性质仍然存在。引用书中的一张图片来说明：

![](http://steeeeps.github.com/images/blog/jscodeandexecutiontime.png)

**所以针对javascript加载的方式，书中提到了以下几点方法来优化：**

*   1、将script放在页面底部，紧邻`&lt;/body&gt;`标签，这一方法也是最简单，用的最多的。
*   2、减少script标签，将所使用的外部js文件组合成一个js文件(组合的时候需要注意不同js文件间的依赖关系，以及变量的命名冲突),为了降低文件大小，也可以选择将组合后的js代码进行压缩。当然这一点对于少量页面的网站来说可能还比较适用，但是对于大型网站来说，这样组合js的方式，就比较麻烦了。yahoo在其CDN上就使用了在服务器合并js的方法：

例如：
[http://l.yimg.com/zz/combo?cv/eng/externals/yfpad/combo/120416/a/yfpadobject.js&cv/eng/externals/yfpad/combo/120416/a/yfpad_useragent.js](http://l.yimg.com/zz/combo?cv/eng/externals/yfpad/combo/120416/a/yfpadobject.js&cv/eng/externals/yfpad/combo/120416/a/yfpad_useragent.js)，这是在yahoo官网上使用到的部分js文件，他在后台多个js文件合并成一个返回给前台使用。

当然，上面两种方式只能减轻js加载对页面解析的影响，而不能做到真正的无阻塞加载 。

**关于无阻塞加载js， Nicholas C. Zakas介绍了以下几种方式：**

*   1、使用script标签的defer属性，不过这个属性只有IE和Firefox支持，对于跨浏览器的解决方案不是很好，所以可能用到的也比较少。
*   2、动态创建script标签，添加到页面。比如：
<pre class="brush: jscript;">var script = document.createElement ("script");
script.type = "text/javascript";
script.src = "file1.js";
document.getElementsByTagName("head")[0].appendChild(script);</pre>

这种方式的最大好处就是在于，无论在哪里执行这段脚本，js文件的下载与执行都不会阻塞其他页面的处理。然而，这种处理方式也有一点弊端，就是不能很好地处理不同js文件之间的依赖关系，除了在firefox和opera中，浏览器会按照你添加js文件的顺序去下载js文件，然后按照顺序执行js，在其他的浏览器中，会立即执行服务器返回的js，这个执行顺序全取决于服务器返回js文件的顺序，所以对于存在js依赖关系的js文件，就容易产生错误。

*   3、使用ajax获取javascript脚本，然后通过动态添加script标签的方式，将获取的javascript代码作为内联的javascript代码添加到页面。这样做的优点在于，获取到的javascript脚本不会立即执行，我们可以自己控制加载到页面的顺序和执行的顺序，来解决js文件之间的依赖关系。当然，这样做最大的缺点就在于，不能够获取跨域的js文件。

这上面就是书中作者提到的几种解决加载javascript阻塞页面的方法。

对于上面提到的无阻塞加载的js方式中，如何解决js文件之间的依赖关系，是每一个方法都必须注意得一点，所以作者在最后推荐的无阻塞加载方式中也提到了两个轻量级的js框架，[lazyload](https://github.com/rgrove/lazyload/)和[labjs](http://labjs.com/)，这两个框架很好的处理了js加载中不同js文件的依赖关系。关于这两个js框架我也没怎么使用过，有兴趣的朋友可以去官网上下载源码研究。

**除了这两个框架之外，我还想向大家推荐两种异步加载js的方式：**

*   1、基于AMD规范的[RequireJS](http://requirejs.org/)。
*   2、基于CMD规范的[SeaJS](http://seajs.org/docs/)。

前者是国外支持AMD规范比较好的js框架，后者是支付宝大牛玉伯所开发的。关于AMD规范和CMD规范以及两者的区别请点击[http://www.zhihu.com/question/20351507/answer/14859415](http://www.zhihu.com/question/20351507/answer/14859415)查看。

由于公司一直都是AMD规范另一实现者[dojo](http://dojotoolkit.org/)的追随者，所以在学习dojo AMD模块加载机制中，对requirejs也有了一定的了解，而SeaJS我也没有具体使用过，但是对两者一起努力推广javascript模块化开发的思想，是十分敬佩的！

其实从我个人而言，我是推崇使用AMD规范来进行javascript模块化开发和加载(我没用过CMD规范，CMD规范的追随者别喷我…)，因为随着web应用的升温，现在的javascript再也不是以前开发者口中的“玩具式语言”，所以对于javascript开发的规范性和易用性，现在要求越来越高，而使用AMD模块化开发，能够极大地改善代码的可维护性，应用的性能和互用性。我们可以不再为臃肿的js文件而烦恼，也不需要去担心过度的对js文件细粒度划分带来的js加载问题。我们只需要按照AMD规范来定义模块，来编写我们的逻辑、业务层的代码，对于我们的开发效率也会有很大的提升。

另外，对于ArcGIS Javascript Api的使用者来说，在3.0版本后已经开始使用dojo1.7版本，所以我也推荐在3.0及后续版本中使用AMD规范来进行ArcGIS Javascript Api的开发。

以上便是我学习High Performance JavaScript 第一章后对js加载的一些心得与总结，这其中可能有些理解和用词欠妥，如果你有更好的理解，欢迎批评指正。
