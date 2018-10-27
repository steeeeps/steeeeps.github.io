---
title: 可动态添加的TabContainer
date: 2013-03-09 17:14:38
categories:
- JavaScript
tags:
- JavaScript
---


公司产品新的安装包需要使用一个TabContainer，由于整个安装包大小不大，所以使用js框架不太适合，于是使用原生javascript完成了一个。  

所有js压缩后不到6kb，还算满足要求。

<!-- more -->

![tabcontainer](/images/2013/tabcontainer.png)



包含一个dom.js,lang.js,tabContainer.js。tabContainer.min.js为前面三个js文件的压缩文件。

TabContainer的样式参考了[http:tympanus.net/codrops/2012/04/12/animated-content-tabs-with-css3/](http:tympanus.net/codrops/2012/04/12/animated-content-tabs-with-css3/)

## api介绍：

#### window.dom(dom)介绍

``` javascript
      dom.hasClass(/*String ('a')*/classStr, /*node*/node)  

      dom.removeClass(/*String ('a' or 'a b')*/classStrs, /*node*/node)  

      dom.addClass(/*String ('a' or 'a b')*/classStrs, /*node*/node)  

      dom.getDom(/*String (name:'name' or class:'.class')*/param, /*node*/root, /*String ('div')*/tag)  

      dom.createDom(/*String ('div')*/tag,/*Object*/ opts,/*domNode*/parentNode)  

      dom.getFirstElementChild(/*domNode*/node)  
 ```

#### window.lang(lang)介绍

``` javascript

	 lang.trim(/*String*/str)  

      lang.stopPropagation(/*Event*/event)  

      lang.srcElement(/*Event*/event)  

      lang.isFunction(it);  

      lang.isString(str);  

      lang.isIp(ip);  

      lang.isNull(str);  

      lang.isLinuxPath(str);  

      lang.isWinPath(str);  

      lang.isNum(str);  

      lang.hasChinese(str);  

      String.toFirstCharUpperCase();    
```

#### window.TabContainer(TabContainer)介绍

``` javascript

var tabContainer = new TabContainer(/*domNode*/node);  
      tabContainer.addTab({  
          title: "title",                 
          content: "&lt;p&gt;tab内容&lt;/p&gt;",     
          closable: false                
      });  
      tabContainer.addTabBtn.onclick = function(){  
          tabContainer.addTab({  
              title: "title",  
              content: "&lt;p&gt;tab内容&lt;/p&gt;",
              closable: false  
          });  
      };  
      var selected=tabContainer.selectedTab;  
      var title=selected.tabtitle;  // title.getAttribute("tabtitle");  
      var allTabs=tabContainer.getTabs();  
```

> [下载源码](https://github.com/steeeeps/tabContainer)
