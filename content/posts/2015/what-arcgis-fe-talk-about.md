---
title: 当我们在谈论 ArcGIS 前端工程师时，工程师们在谈论什么
date: 2015-07-06 22:55:41
categories:
- GIS
- ArcGIS API for JavaScript
tags:
- ArcGIS API for JavaScript
- Dojo
- 前端
---



随着HTML5的火热，不管是在校的GIS学生、马上进入工作岗位的GIS毕业生，还是有几年其他ArcGIS 开发框架开发经验的开发人员，看着ESRI停止对Flex 和 Silverlight API的更新，大力推荐开发者使用 ArcGIS API for JavaScript 来构建应用系统时，是不是都会感觉有一点小激动，小心脏扑通扑通的？都在想着能不能借HTML5的东风实现自己当上CTO、迎娶白富美、走上人生巅峰的梦想？

那么当我们在谈论 ArcGIS 前端工程师时，工程师们在谈论什么呢？

<!-- more -->

![ags-front-end-develope-skills](/images/2015/ags-front-end-develope-skills.png)

这个技能图参考了前两年网上流传的一张“[前端工程师技能](http://www.xiaomayi88.com/uploads/img/140411/97-14041113555Q96.jpg "前端工程师技能")”图。看了这张图之后，你有什么感想？是不是觉得村里的小芳也挺不错的，长得好看又善良，一双美丽的大眼睛，辫子粗又长。

其实相比于互联网行业的前端工程师所需要掌握的技能，ArcGIS 前端工程师已经很幸福了。

面对这么多技能，作为新手，都需要怎样的一步一步去学习这些知识呢？

### ArcGIS 前端工程师入门 ### 

作为入门ArcGIS 前端工程师，无论你是学生还是有其他框架开发经验的工作人员。我假设你是刚开始接触前端开发和 ArcGIS API for JavaScript，那么你首先需要去学习以下几点：

1. 学习[HTML](http://www.w3school.com.cn/html/index.asp)、[CSS](http://www.w3school.com.cn/css/index.asp)、[JavaScript](http://www.w3school.com.cn/js/index.asp)基础知识。

2. 学习[Dojo](http://dojotoolkit.org/documentation/#tutorials)基本知识，包括但不限于[AMD](http://www.cnblogs.com/snandy/archive/2012/03/12/2390782.html)规范、dojo基础架构、dojoConfig配置、dojo面向对象、dijit 使用等。

3. 学会使用[web inspector](http://www.360doc.com/content/12/1107/20/7851074_246467307.shtml)或[firebug](http://www.ruanyifeng.com/blog/2008/06/firebug_tutorial.html)对浏览器兼容性调试与修改。对于很多企事业单位都是用IE或360浏览器（老版本360安全浏览器。新版的360安全浏览器和极速都使用chromium内核），那么，也应该学会在IE8下对浏览器兼容性问题进行调试与修改。学会使用[IETester](http://www.kafan.cn/edu/4009951.html)对网站在个版本IE下的显示效果进行查看与调试。

4. 学会[JSON](http://www.w3school.com.cn/json/)的使用。

5. 学习[ArcGIS API for JavaScript](https://developers.arcgis.com/javascript/)基本知识，包括但不限于[下载并离线部署](https://developers.arcgis.com/javascript/jshelp/intro_accessapi.html)、[代理配置](https://developers.arcgis.com/javascript/jshelp/ags_proxy.html)。学会使用并理解ArcGIS 基本对象模型(如果有AO、AE、Flex等开发经验，很容易理解)，[Map](https://developers.arcgis.com/javascript/jssamples/#map)、[Graphic](https://developers.arcgis.com/javascript/jssamples/#graphics)、[符号化](https://developers.arcgis.com/javascript/jssamples/#renderers,_symbols,_visualization)、[Task](https://developers.arcgis.com/javascript/jshelp/intro_querytask.html)、[Geoprocessor](https://developers.arcgis.com/javascript/jshelp/intro_gp_overview.html)。

6. 选择合适的开发工具，如果单纯的前端开发，推荐使用sublime text。当然，这种情况比较少，因为大多数情况下你需要和后台开发人员协同工作或者你也会涉及到后台代码编写，那根据实际情况选择visual studio、eclipse、myeclipse或其他IDE。

如果你学习并在实际开发中运用到了以上几点，恭喜你，你需要开始进入苦逼的进阶阶段去涉猎更多开发时需要用到的知识了。

### ArcGIS 前端工程师进阶 ###

1. 去学习HTML5，CSS3的知识， 在系统中做更炫、功能更丰富的应用。

2. 学习JavaScript 模块化开发知识，深入学习Dojo框架及AMD规范的使用，学习自定义dojo 小部件。

3. 深入学习ArcGIS API for JavaScript 框架，

4. 跟着你的开发团队，学习合适的后台开发语言Java或C#，至少要能看懂基本的后台代码，比如：能看懂后台如何捕获前台的ajax请求，读取数据库，处理业务逻辑，返回json数据到前台。要是能写，那便是极好了。

5. 学会使用Eclipse、MyEclipse、Visual Studio 断点调试后台代码。

6. 学习安装ArcGIS Server 和 ArcMap，学会使用两者发布地图服务、切片服务、要素服务、GP服务等。熟练在ArcGIS API for JavaScript中对这几种服务的调用方式。

7. 学习使用[jQuery](http://www.w3school.com.cn/jquery/index.asp)，学习使用jQuery第三方插件来增强系统用户体验。是不是有疑问为什么要学jQuery呢？因为jQuery丰富的第三方插件是dojo欠缺的。当然使用dojo也是完全能做出相同的效果的。另外，不要再反复质疑ArcGIS API for JavaScript 可不可以使用jQuery来做?为什么API不选择jQuery而选择dojo作为基础框架。这一点自己来找答案，找到答案的同时就表明你已步入高级进阶阶段了。

8. 学习一点基础[SQL](http://www.w3school.com.cn/sql/index.asp)知识，有助于使用QueryTask 功能。

9. 学习使用[JSONP](http://baike.baidu.com/link?url=2Lb2pRKPuNAGlNd6GqcVQkLAm0BiCHKli9lqJbjPDElKCkwLxfX_xmdxFxqEALBa1A-qCPJ5MoYXbtUCIX3IN_)来解决跨域数据访问问题。

10. 学习[HTTP协议](http://kb.cnblogs.com/page/130970/)，知道GET和POST的区别，知道常见状态码的含义。

11. 学习使用Svn来协同工作。

12. 学习使用fiddler来查看网络请求信息。

### ArcGIS 前端工程师高级进阶 ###  

1. 深入学习ArcGIS Server，学习使用Web App Buider搭建应用和定制开发(用过的都说好^_^)。

2. 学习[dom](http://www.w3school.com.cn/htmldom/index.asp)、[bom](http://baike.baidu.com/subview/126558/5073177.htm)基础知识，对页面调优、提高页面加载效率有好处。学习[提高JavaScript编程性能](http://book.douban.com/subject/5362856/)的知识，学习[提高网站性能](http://book.douban.com/subject/3132277/)的知识。

3. 学习使用socket，学会使用[tcpudp抓包工具](http://www.zendstudio.net/archives/tcp-udp-socket-toolkit/)来查看请求信息。

4. 学习使用Git(如果团队使用Git协同工作)。学习[CORS](http://www.cnblogs.com/Darren_code/p/cors.html)基础知识。

5. 提升自我界面设计、交互设计能力，学习[Responsive UI&nbsp;Design](http://blog.jobbole.com/tag/%E5%93%8D%E5%BA%94%E5%BC%8Fweb%E8%AE%BE%E8%AE%A1/)基本知识。学习使用[bootstrap](http://www.bootcss.com/)来搭建响应式页面。

6. 学习使用[LESS](http://less.bootcss.com/)来定制[修改 dojo 主题样式](http://steeeeps.net/2013/04/14/dijit-custom-theme/)（如果使用的dijit比较多，又不想使用dojo现有的几种主题可以学学）。

7. 学习[NodeJS](https://nodejs.org/)基本使用，因为LESS及其他很多优秀的前端框架是使用NodeJS来构建的，例如很多压缩、混淆前端代码的框架。

8. 如果你发现使用ArcGIS API for API 调用GP能实现很多牛逼的功能，那我推荐你也去学习学习python来编写GP脚本，发布GP服务。

9. 在IE8下，很多HTML5的功能没法使用，比如socket、file api 等，但是这些是可以通过swf来弥补的。所以也可以去了解一下flex相关知识。毋庸置疑，现在的webgis系统，大多数都是基于ArcGIS API for Flex 框架开发的。

10. 学习面向对象编程知识、[函数式编程知识](http://www.ruanyifeng.com/blog/2012/04/functional_programming.html)、[JavaScript编程模式](http://www.jianshu.com/p/ba77bee85cb9)、[JavaScript设计模式](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#writingdesignpatterns)。

11. **学会使用Google、学会使用Google、学会使用Google。他们说的重要的要说三遍、三遍、三遍。**

12. **ArcGIS的发展是建立在整个IT行业基础之上的，前端亦是如此，所以去学习所有[互联网前端工程师必备技能](http://www.xiaomayi88.com/uploads/img/140411/97-14041113555Q96.jpg)吧**！躲在被窝哭去吧，^_^。

_注：因为我从一直从事ArcGIS 二次开发工作，文中介绍的技能主要针对ArcGIS 前端工程师。如有补充，请留言分享，谢谢！_

祝早日成为一名优秀的ArcGIS前端工程师。

ArcGIS 前端工程师技能脑图：[请点击查看](http://www.processon.com/view/link/559d2e1ee4b0b75ad8b5cffd "ArcGIS 前端工程师技能脑图") 