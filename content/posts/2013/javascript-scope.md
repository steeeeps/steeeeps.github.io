---
title: javascript 作用域的工作原理
date: 2013-02-03 17:27:08
categories:
- JavaScript
tags:
- JavaScript
---

**函数作用域**

每一个javascript函数都是一个对象，拥有一系列可被访问和仅能被javascript引擎访问的内部属性，其中一个内部属性就是函数的作用域。当一个函数创建的时候。Javascript引擎就会创建这个函数的作用域并且用一些对象来填充，这些对象代表创建这个函数的环境中可以访问的数据，比如创建一个全局函数，他的作用域中就填入了一个单独的可变对象，这个全局对象代表了所有全局范围定义的变量，比如说window、location、document之类的对象。如果创建的函数是一个嵌套的函数（闭包），此作用域上还会加上父函数中定义的对象。

<!-- more -->

例如：

``` javascript
function  add (num1 , num2 ) {
    var sum=num1+num2;
    return sum;
}
```

![1](/images/2013/1.png)

当该函数被调用时,例如 var total=add(5,10);会建立另一个内部对象，称作“运行时上下文”,一个运行时上下文定义了一个函数运行时的环境，函数每次被调用都会产生一个独立的上下文，直到函数执行完毕，运行时上下文就被销毁。

当上下文被创建时，它会使用函数作用域中所包含的对象来产生自己的作用域链，并且将函数作用域中的对象按照他们在函数中出现的顺序复制到上下文作用域的前端，比如说将函数的所有局部变量、命名参数、参数集合以及this关键字。当上下文销毁时。这个作用域也会被销毁。

![2](/images/2013/2.png)

所以在函数的运行过程中，每遇到一个变量，解析器都会在作用域链中去查找与变量相对应的标识符，比如说上面函数中的sum，这个搜索过程会从第一个作用域前端一直查找，直到查找到了对应的标识符将其返回，或者直到作用域链的末端也未找到的话则返回undefined。所以在函数中，对于局部变量和全局变量的访问，由于全局变量存在更深的位置，所以它的读写速度就会比局部变量慢一些。

所以当你多次访问一个存储位置较深的变量时。可以将这个变量保存为局部变量，来重复使用，比如说下面这个例子：

``` javascript
function initUI(){
var bd = document.body,
		links = document.getElementsByTagName("a"),
		i = 0, len = links.length;
while (i < len) {
update(links[i++]);
}
document.getElementById("go-btn").onclick = function(){
start();
};
bd.className = "active";
}
```

可以将其改为这个：

``` javascript
function initUI(){
    var doc = document,
        bd = doc.body,
        links = doc.getElementsByTagName("a"),
        i= 0,
        len = links.length;
    while(i < len){
        update(links[i++]);
    }
    doc.getElementById("go-btn").onclick = function(){
        start();
    };
    bd.className = "active";
}
```

在上面的例子中，将三次引用到的document这个全局变量，存储为局部变量，这样就不会在整个作用域链中去查找三次，节省一定的性能。当然这一个简单的函数在性能上不会有太多的提升，但是如果在整个页面中存在几十个这样的反复访问，那么通过这样的优化在性能上就会有很大的改进，特别是OPOA这种程序。

**慎用with表达式**

在javascript中有一个with表达式，比如下面这个例子：

``` javascript
function initUI(){
    with (document){     //avoid!
        var bd = body,
            links = getElementsByTagName("a"),
            i= 0,
            len = links.length;
        while(i < len){
            update(links[i++]);
        }
        getElementById("go-btn").onclick = function(){
            start();
        };
        bd.className = "active";
    }
}
```

这个函数中使用with表达式来避免多次书写document，虽然看起来方便了不少，但实际上对其他变量的访问的代价确更高了，在使用width表达式的时候，改变了作用域链，如下所示：

![3](/images/2013/3.png)

如图所以，一个容纳了document对象所有属性的对象被插入到作用域的最前端，这样是访问document对象的属性变得非常快，但是访问其他变量确变得更慢了，所以，尽量避免使用with表达式，将document对象存储为局部变量，不仅方便而且还能提升整体性能。

与with表达式相似的还有tay catch，当语句发生错误时，会在作用域最前端插入异常对象。但是如果在catch语句中没有对其他变量的访问，仅仅是作用域临时被改变了，是不会影响代码的性能的。

**闭包的作用域**

闭包是javascript最强大的一个方面，它允许函数访问局部范围之外的数据。

比如这样一段代码：

``` javascript

function assignEvents(){
    var id = "xdi9592";
    document.getElementById("save-btn").onclick = function(event){
        saveDocument(id);
    };
}
```

如上面所讲的一样，当a s s i g n E v e n t s函数被调用时，他的作用域链上会存在两个对象，第一个对象是被调用时产生的运行时上下文对象(Activation object)，第二个对象是创建函数时产生的全局对象。在函数被执行的时候，闭包被创建，这时候闭包的作用域对象就和这些对象一起被初始化。

![4](/images/2013/4.png)

当闭包被执行时，又会产生一个闭包自己的运行时上下文。并且插入作用域链的最前端。


![5](/images/2013/5.png)

所以使用闭包就会存在两个问题，第一个就是和其他函数调用时访问变量一样的问题，如图所示，当闭包直接去访问外部的saveDocument函数和id变量时，就会存在一定的性能损失，第二点就是，闭包的作用域属性和创建它的函数含有相同的运行时上下文引用，一般来说，当函数执行完毕时运行时上下文会一起被销毁，但是，当涉及到闭包的时候，他就无法正常的被销毁，所以说闭包比非闭包函数需要更多的内存，而且使用不当时，也更容易导致内存泄露。

**同理，当我们访问原型链上的属性或嵌套的成员变量时，将位置比较深的变量，并且多次访问的变量存储为局部变量，都会提升变量的访问速度。**

> 参考自[《High Performance Javascript》](http://www.amazon.com/Performance-JavaScript-Faster-Application-Interfaces/dp/059680279X "High Performance Javascript")
