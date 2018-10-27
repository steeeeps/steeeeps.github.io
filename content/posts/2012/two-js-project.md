---
title: 分享两个arcgis javascript api 小工程
date: 2012-12-05 17:39:23
categories:
- GIS
- ArcGIS API for JavaScript
tags:
- ArcGIS API for JavaScript
---

不知是不是传说中“世界末日”的原因，感觉今年北京的冬天比去年冷多了。转眼间一年过去了，自己写javascript也整整一年了，在这一年里也学到了不少web开发的东西。虽然一年内一直在和javascript打交道，但是自己还是感觉像一个初学者一样。感觉javascript看上去仍像是一个平静的湖面，当你脚插进去才感觉深似海啊！

<!-- more -->

在这一年内其实很少写有关于arcgis javascript api(以下简称ags api)的代码的。但是由于自己有一些AO开发的基础，加上自己这一年来一直在编写javascript和使用dojo框架，所以对于QQ群里或者论坛上的一些关于ags api的提问，我都会去关注一下，或者查查ags api samples，所以这一年下来对ags api的使用也有一些了解。自己大致估算了一下这一年内问的最多的问题差不多都是和下面几点相关的：

1、如何部署ags api

2、如何设置proxy

3、如何解决跨域问题

当然这两个都不是这篇文章讲的重点，上面几点的解决方案，网上有很多不错的文章来介绍，还请百度。如果细心一点的同学，会很快解决的。

回到主题吧，写这篇文章主要目的是想通过这一年来我使用ags api写的两个小工程，来分享一下我学习ags api的一点经验。一个是我去年到公司刚开始学习javascript和ags api的时候写的一个小demo(别名checkData)，另一个是我今年6、7月份写的一个ags api 聚类展示的代码（别名cluster），其实具体啥时候写的我不记得了，我就记得我写出来的第二周ags api3.0就推出了，而且在这个版本中也加入了聚类展示的功能。所以后来也就放在那了。

checkData是一个入门级的demo，当时主要是用来入门练习ags api，主要的练习的功能点就只包括：

1、点数据的展示、添加(没有保存)、属性编辑

2、编程的方式创建dojo小部件

3、自定义小部件

![checkData](images/2012/checkData.png)

> [下载源码](https://github.com/steeeeps/checkData "点击下载 checkData 源码")

cluster的难度相比于上一个demo，难度就要偏高一些了，用过flex clsuter的同学们肯定都对flex那种让人眼花的绚丽流连忘返呢，所以很多使用ags api的同学都在问，为什么ags api迟迟不推出cluster功能呢，其实在这之前国外一个大牛，自己也写了一个，但是那个版本的扩展性貌似不是太好，所以我在写clsuter的时候也参照了不少他里面的代码以及百度地图cluster的思想。当然与后面esri官方推出的cluster版本来说，我还是推荐使用官方的版本，毕竟整个ags api的稳定性还是挺高的。而我分享的这个，主要目的还是为了交流，希望能够给使用ags api的新手们一些帮助。在这这个demo里面，我当时主要学习到的包括：

1、dojo类的继承

2、简单的点抽希的思想.（其实关于点抽希的思想我还是理解的不够透彻，有比较了解的朋友还请多多交流）

在cluster里面，我并没有加入像flex中对聚合的点华丽的展示方式，而是将聚合点通过鼠标上浮或点击事件传回，方便开发者自定义显示，比如下图中我就使用一个网上开源的js标签云的方式来显示聚合点的信息。这个实例的地图和数据都是使用[舌尖上的中国](http://tm.arcgisonline.cn:8038/App101/A_Bite_Of_China/ "舌尖上的中国")上面的。当然我这个和它相比，就比较逊色呢，呵呵

![cluster](/images/2012/cluster.png)

> [下载源码](https://github.com/steeeeps/cluster "点击下载cluster 源码")

这两个demo从易到难，有兴趣的同学可以下载源码看一下，当然可能由于书写代码的时候的对javascript、dojo和ags api的某些知识点的理解不够准确，代码中可能还或多或少有些问题，还请你指出、交流。

关于一些ags api学习的经验，其实说到经验我又不知道怎么说了，因为我本身写ags api的代码也比较少，但是我经常在群里看大家问的一些问题，其实很多问题的定位都不应该是在ags api这一层的，但大家上来就问ags api里该怎么解决。所以我建议大家有时间的话可以多花点时间在dojo框架和原生的javascript的学习上。而对于ags api的一些问题，ags api samples上的例子什么的只要你仔细、耐心的查找，90%是能找到解决方法的。

最后，预祝大家都能够安安全全、欢欢乐乐的进入2013。
