---
title: IE 清空file input值
date: 2012-10-10 21:38:32
categories:
- JavaScript
tags:
- JavaScript
- IE
---
在web 页面使用file input 控件进行上传操作时，有时会需要在上传完成后对file input中的值清空，来做其他的操作，一般我们会想到直接使用  

``` javascript
document.getElementById("myFile").value = "";
```

来完成我们的操作。这个方法也是非IE浏览器主要使用的方法。  

<!-- more -->


但是在IE中，由于IE自身的安全策略，认为 file input 是read only 的控件，所以IE是不允许这样操作的，但是在IE中也可以使用如下两个方法完成： 

1. 如果file input在表单中的话，可以直接使用document.getElementById(“myForm”).reset();来重置整个表单。  
2. 如果没有使用表单的话，可以结合使用cloneNode、replaceChild两个方法来完成，如下面代码所示：  

``` javascript
var oldfile=document.getElementById("myFile");
var newfile=oldfile.cloneNode(false);
oldfile.parentNode.replaceChild(newfile,oldfile);

```

