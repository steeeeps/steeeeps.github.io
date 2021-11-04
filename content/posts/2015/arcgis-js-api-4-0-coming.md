---
title: 拥抱 ArcGIS API for JavaScript 4.0
description: 
date: 2015-08-16 22:38:29
categories:
- GIS
- ArcGIS API for JavaScript
tags:
- ArcGIS API for JavaScript
---
2015年7月，ESRI 推出了 [ArcGIS API for JavaScript 3.14](https://developers.arcgis.com/javascript/jshelp/) 和 [ArcGIS API for JavaScript 4.0 Beta 1 ](https://developers.arcgis.com/javascript/beta/)两个 API 版本。  

前者是常规版本的一次更新，包含了对3.13bug的修改以及功能类和小部件的增加。  

而4.0则是ArcGIS API for JavaScript的一次重大变革。除了新添加对万众瞩目的 Web 3D 的支持，API自身的变化和对最新的HTML5、CSS3元素的支持使API变的更简洁、强大、灵活、高效。  

4.0的变革，主要体现在：   

> 1. 使用 [ES5 Accessors API](http://odoe.net/blog/arcgis-js-api-4-0beta1-accessors/) 代替使用事件来监控属性值的变化。[查看详情](https://developers.arcgis.com/javascript/beta/api-reference/esri-core-Accessor.html)。  
> 2. 绘图逻辑修改，增加MapView和SceneView来分别绘制2D、3D地图。  
> 3. 模块与包名的改变，使API变的更简洁，例如使用 ArcGISTiledLayer 替换 ArcGISTiledMapServiceLayer。  
> 4. 只支持使用 AMD 规范来定义、加载模块。  
> 5. 使用promise来解决事件监听带来的无限回调，让代码更简洁、优雅。  
> 6. 对现代浏览器的依赖，例如3D需要IE11。中国的工程师可能需要逐步引导用户由IE8 升级至新版本IE或Chrome、Firefox、Safari。

更多内容与用法，请点击查看:[ArcGIS API for JavaScript Guide](https://developers.arcgis.com/javascript/beta/guide/)

