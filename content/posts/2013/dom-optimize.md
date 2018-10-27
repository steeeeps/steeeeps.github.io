---
title: dom编程优化
date: 2013-02-03 17:20:04
categories:
- JavaScript
tags:
- JavaScript
---

在浏览器中，dom的实现和javascript的实现通常是保持相互独立的，比如chrome使用webKit渲染页面，而使用v8作为javascript引擎，firefox使用Gecko渲染页面，使用IonMonkey作为最新的javascript引擎。

有一个非常形象的比如就是将dom和js分别看做两座岛屿，而两者之间以收费桥连接，当js访问dom时，就需要过桥，需要交过桥费，所以访问的次数越多，教的过桥费也就越多（摘自[《High Performance Javascript](http://www.amazon.com/Performance-JavaScript-Faster-Application-Interfaces/dp/059680279X "High Performance Javascript")》）。所以合理的处理好dom和js的交互，对页面的性能也有很大的提升。
 

合理的操作dom主要包括访问和修改dom元素、注册dom元素事件、浏览器重绘与重排，前两者是最普遍的优化问题，也是最易优化的，但是理解重绘与重排对大型的web程序来说，是十分重要的。

### 访问和修改dom元素

访问和修改dom多采用innerHTML和DocumentFragment，并注意尽量减少dom和javascript交互的次数。

例如下面的代码创建多个节点：

``` javascript
function CreateNodes(){
    for(var i = 0;i &lt; 10000;i++){
        var tmpNode = document.createElement("div");
        tmpNode.innerHTML = "test" + i + " &lt;br /&gt;";
        document.body.appendChild(tmpNode);
    }
}
```

就不如使用DocumentFragment或者innerHTML快速：

``` javascript
function CreateFragments(){
    var fragment = document.createDocumentFragment();
    for(var i = 0;i &lt; 10000;i++){
        var tmpNode = document.createElement("div");
        tmpNode.innerHTML = "test" + i + "&lt;br /&gt;";
        fragment.appendChild(tmpNode);
    }
    document.body.appendChild(fragment);
}
```

### 添加dom事件

利用javascript事件冒泡的特性，来最小化事件句柄数量。

例如将下面给td标签注册事件的例子：

``` javascript
function addTdEvent1(){
    var table = document.getElementById('table');
    var tds = table.getElementsByTagName("td");
    for (var i = 0, len = tds.length; i &lt; len; i++) {
        var td = tds[i];
        td.onclick = function(){
            alert("单元格被点击了");
        };
    }

}
```

稍作修改为：

``` javascript
function addTdEvent2(){
    var table = document.getElementById('table');
    table.onclick = function(evt){
        evt = evt || window.event;
        var target = evt.target;
        if (target.tagName = "TD") {
            alert("单元格被点击了");
        }
    }
}
```

就能减少很多事件句柄数量。

### 修改dom样式造成重绘与重排

当浏览器下载完页面所有html标记、javascript、css、和图片之后，会解析html文件并创建两个内部数据结构：一个用来表示页面结构的dom树，另一个是表示节点如何显示的渲染树。渲染树中每个需要显示的dom树节点存放至少一个节点（隐藏的dom元素在渲染树中没有对应的节点），一旦dom树和渲染树构造完毕，浏览器就根据渲染树上的节点样式绘制出页面。由于浏览器的流布局，对渲染树的计算通常只需要遍历一次就可以完成。但是table及其内部元素除外，他可能需要更多次计算才能确定好其在渲染树中节点的属性，通常需要花费3倍的时间（所以不推荐使用table布局）。

重绘是指一个元素外观的改变所引起浏览器的行为，比如改变visibility(改变display属于重排，因为display为none的时候渲染树是没有这个节点的)，背景色等属性。浏览器会根据元素的新属性重新绘制，因为不会带来重新的布局，所以不会触发重排。

重排主要是针对当元素的几何属性(比如宽和高)受到修改了、或者在段落中添加了文字，浏览器需要重新计算元素的几何属性时。浏览器需要重构渲染树，这个过程称为重排。

以下几点是常见的触发重排的操作：

1. 添加或删除可见的dom元素，修改dom元素display样式所造成的dom树结构的变化。
2. 通过修改dom元素的位置、宽、高，或者由于添加文字或背景变成不同尺寸的图片所造成的几何属性的变化。
3. 最初的页面渲染、浏览器窗口改变尺寸、滚动条的出现。
4. 当某一个dom元素的几何属性高改变引起浏览器对页面进行重排的时候，有可能会引起其他兄弟元素、子元素、父元素的重排和重绘，甚至整个文档的重排与重绘，性能的消耗是非常大的，所以在现在的浏览器引擎中，都会通过队列来批量操作来优化重排的过程，但是如果访问以下属性的话，浏览器就会强迫队列刷新所有需要重排的部分来获取最新的信息： &nbsp;_offsetTop、offsetLeft、 offsetWidth、offsetHeight、scrollTop、scrollLeft、scrollWidth、scrollHeight、clientTop、clientLeft、clientWidth、clientHeight、getComputedStyle() (currentStyle in IE)_

所以，在多次使用这些值时应进行缓存。

### 如何减少重排次数

1. 将多次改变样式的操作合并为修改cssText或者css(推荐后者，方便做到html、css、js分离)。
2. 当需要多次操作dom元素时，将dom元素从文档流中拿出（使用documentFragment或者隐藏该节点），多次操作完成后再放回文档中。
3. 在需要经常获取那些引起浏览器重排的属性值时，比如offsetTop等，将值缓存为局部变量。

> 参考自[《High Performance Javascript》](http://www.amazon.com/Performance-JavaScript-Faster-Application-Interfaces/dp/059680279X "High Performance Javascript")

> 参考自[浏览器的重绘与重排](http://kb.cnblogs.com/page/169820/ "浏览器的重绘与重排")
