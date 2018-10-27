---
title: 自定义dojo主题
date: 2013-04-14 11:10:07
categories:
- JavaScript
tags:
- dojo
---

到现在的dojo1.8.3版本中，dojo框架提供了四个主题，**nihilo**、**tundra**、**soria**和**claro**，我个人是非常喜欢**claro**主题的风格，淡淡的蓝，用了一些css3的效果，整个体验都很不错,但是谁也不喜欢自己做的系统主题样式和别人一致，就像不喜欢和别人撞衫一样，所以在平时开发中也会想尽一切办法来改变dojo主题。下面就分享两个自定义dojo主题的方法。

<!-- more -->

## 重写样式  

这个方法可能是使用起来最简单，适用面最广的一种方法，用浏览器调试工具找出dijit的样式，修改为自己的想要的样式，比如我想修改dojo的Dialog的样式，我只需要找到Dialog对应的css样式，修改为以下css代码：
``` css
    /*------------对话框-----------*/ 
    .steeeeps .dijitDialog {
        border: 1px solid #bec0e1;
        -webkit-box-shadow: 0 1px 5px rgba(0, 0, 0, 0.25);
        -moz-box-shadow: 0 1px 5px rgba(0, 0, 0, 0.25);
        box-shadow: 0 1px 5px rgba(0, 0, 0, 0.25);
        -webkit-border-radius: 6px;
        -moz-border-radius: 6px;
        border-radius: 6px;
    }

    /*对话框标题栏*/ 
    .steeeeps .dijitDialogTitleBar {
        background: rgb(248, 248, 248); /* Old browsers */
        background: -moz-linear-gradient(top, rgba(248, 248, 248, 1) 0%, rgba(229, 229, 229, 1) 100%); /* FF3.6+ */
        background:-webkit-gradient(linear, left top, left bottom, color-stop(0%,rgba(248,248,248,1)), color-stop(100%,rgba(229,229,229,1))); /* Chrome,Safari4+ */
        background:-webkit-linear-gradient(top,rgba(248, 248, 248, 1) 0%, rgba(229, 229, 229, 1) 100%); /* Chrome10+,Safari5.1+ */
        background:-o-linear-gradient(top,rgba(248, 248, 248, 1) 0%, rgba(229, 229, 229, 1) 100%); /* Opera 11.10+ */
        background:-ms-linear-gradient(top,rgba(248, 248, 248, 1) 0%, rgba(229, 229, 229, 1) 100%); /* IE10+ */
        background:linear-gradient(to bottom,rgba(248, 248, 248, 1) 0%, rgba(229, 229, 229, 1) 100%); /* W3C */
        filter:progid:DXImageTransform.Microsoft.gradient( startColorstr ='#f8f8f8', endColorstr = '#e5e5e5', GradientType = 0); /* IE6-9 */
        height:20px;
        line-height:20px;
        padding:5px 7px 5px 7px;
        border:1px solid #bec0e1;
        border-top:none;
    }

    /*对话框标题*/
    .steeeeps .dijitDialogTitle {
        padding-left: 11px;
        font-size: 16px;
        color: #234580;
        font-family: "雅黑";
    }

    /*对话框内容显示区域*/ 
    .steeeeps .dijitDialogPaneContent {
        border-top: none;
        padding: 20px 35px;
        background: #fafafa repeat-x top left;
        height: auto !important;
    }
    ```

然后将body标签修改为：

``` html
    <body class="claro steeeeps" role="main" />
```
浏览器在解析html时，就会根据CSS优先级法则自动覆盖claro主题中Dialog的样式。如图所示是上面样式的效果图  

![dojo dialog](/images/2013/steeeeps_dialog.png)

虽然这样重写dijit样式的方式是比较简单、快捷，但是对于一个主题来说通常这样修改的地方都零零散散，如果需要修改多个控件，那修改的地方就比较多，显得比较麻烦。    

## 使用LESS CSS创建dojo主题  

随着整个Dojo Tookit越来越强大，从Dojo1.6开始增加了使用[LESS CSS](http://www.lesscss.net)框架来创建Dijit的主题。使用LESS来创建Dijit主题是一个非常明智的改变，因为这能够使开发者定制自己的主题越来越简单。下面就介绍一下Dojo是如何使用LESS来创建Dijit主题的，以及如何来定制我们自己的主题。

### LESS

LESS CSS背后的思想是十分简单的：通过使用变量(variables)、混入(mixins)、操作(operations)以及函数(functions)等动态行为来扩展CSS。简单来说：弥补了CSS的继承限制。LESS能够在服务器端的[Node.js](http://nodejs.org/)环境中使用，也能够通过在客户端引入JavaScript文件来使用。我们将使用第一种方法来创建Dojo主题。在Node.js环境中，我们使用nmp来安装LESS(假设你的机器上已经安装了Node.js和npm)：

``` bash
npm install less
```

### Dijit中LESS的使用

打开dojo1.6+安装目录中claro主题的位置：
``` bash
cd dijit/themes/claro/
```

在打开的目录中你能看见一些{WidgetName}.css文件和{WidgetName}.less，这些.css文件就是使用LESS构建的。再打开每个{WidgetName}.less文件之前，先打开variables.less文件，这个文件包含了其他.css文件中使用的变量。可以认为variables.less是整个主题构建中最基础的文件。下面是variables.less文件中的一些代码片段：

``` css
    /* General */
@text-color: #000000;               /* Text color for enabled widgets */

@border-color: #b5bcc7;             /* Border color for (enabled, unhovered) TextBox, Slider, Accordion, BorderContainer, TabContainer */
@popup-border-color: #769dc0;       /* Border for Dialog, Menu, Tooltip.   Must also update tooltip.png (the arrow image file) to match */
@minor-border-color: #d3d3d3;       /* Color of borders inside widgets: horizontal line in Calendar between weeks, around color swatches in ColorPalette, above Dialog action bar */

@disabled-border-color: #d3d3d3;    /* Border color for disabled/readonly Button, TextBox etc. widgets */
@disabled-background-color: #efefef;/* Disabled button, textbox, etc. */
@disabled-text-color: #818181;      /* Text color for disabled/readonly widgets */

/* ... */

/* Input widgets
@focused-border-color: #769dc0;             /* Focused textbox, editor, select, etc. */
@error-border-color: #d46464;               /* Border for textbox in error state */
@error-focused-border-color: #ce4f4f;       /* Border of textbox in error state, and focused */
@erroricon-background-color: #d46464;       /* Background color for exclamation point validation icon (for TextBox in error state) */
@textbox-background-color: #fff;            /* Default background color of TextBox based widgets */
@textbox-hovered-background-color: #e9f4fe; /* Background color when hovering a unfocused TextBox, Select, Editor, or other input widget */
@textbox-focused-background-color: @textbox-background-color;
@textbox-error-background-color: @textbox-background-color;
@textbox-disabled-background-color: @disabled-background-color;

/* mixins */
.border-radius (@radius) {
    -moz-border-radius: @radius;
    border-radius: @radius;
}

.box-shadow (@value) {
    -webkit-box-shadow: @value;
    -moz-box-shadow: @value;
    box-shadow: @value;
}
```

一些LESS CSS基本用法：

*   使用‘@’来定义一个变量。
*   在使用变量时，定义属性名，并将带有‘@’前缀的变量名作为值。
*   定义混入，提供一个带有参数的选择器名称，并且定义该属性的子属性。

可以使用下面的语句，在.less文件中引入其他.less文件：

``` css
    @import "variables";
```

现在，打开 Calendar.less文件，并且查找”@border-color”的实例，你会发现这些实例都引用自在variables.less中定义的变量 “@border-color” 。所有以‘@’作为前缀的变量都会在编译时被替换为相应的值。

### 创建你自己的Dijit主题

自定义主题最简单的方式就是复制一份最新的，并且官方提供支持的主题，这里，我推荐claro.claro是一款有专业外观的蓝色主题，使用了被大部分浏览器所支持的Css gradients,transitions和roundeed corners等css3效果.

使用cp命令快速复制claro文件夹到你所定义主题名的文件夹。

``` bash
    cpmac claro davidwalsh  (原文作者名)
```
在开始修改代码之前，应该首先将目录下所有”claro”关键字修改为”{你自己的主题名}”.例如我将以‘davidwalsh’作为主题名，所以我将’claro’替换为’davidwalsh’.然后打开variables.less文件，做任何你觉得合适的颜色变化。我喜欢绿色，所以我将文件中的颜色值调整为呈绿色的。

在将variables.less文件修改为我理想中的设计后。如果我们想要让小部件与claro主题中的看起来不一样，我们还需要打开相应的{WidgetName}.less文件做适当的调整。当根据你的喜好修改完所有的.less文件后,就需要将.less文件编译为能被浏览器识别的css文件。

### 编译LESS主题

在编译主题之前，还需要介绍下目录中的compile.js文件，该文件是用来浏览当前目录以及‘form’和‘layout’子目录中所有的.less文件，所有的.less文件都会被解析并且使用variables.less中定义的值来替换相应的变量，并且创建同名的css文件。

定位到你定义主题的文件夹，使用下面的命令来调用compile.js:

``` bash
    node compile.js
```

通过运行这个命令，将会创建于.less文件相对应的同名.css文件。快速扫描这些文件将确认所有的变量都被替换在相应的位置，现在，你自定义的主题已经完成。

### 使用自定义主题

找到themeTester.html文件(/dijit/themes/themeTester.html),将自己定义的主题添加至该文件。

``` javascript
   // Fill in menu/links to get to other themes.       
// availableThemes[] is just a list of 'official' dijit themes, you can use ?theme=String
// for 'un-supported' themes, too. (eg: yours)
var availableThemes = [
    { theme:"davidwalsh", author:"David Walsh", baseUri:"../themes/" },
    { theme:"claro", author:"Dojo", baseUri:"../themes/" },
    { theme:"tundra", author:"Dojo", baseUri:"../themes/" },
    { theme:"soria", author:"nikolai", baseUri:"../themes/" },
    { theme:"nihilo", author:"nikolai", baseUri:"../themes/" }
];
```

我推荐使用这个方法，因为这样能针对每个小部件来比较你的主题。你也希望在你的开发过程中不断地来调整你的主题。

LESS与静态的css文件相比，自定义主题能简单很多倍。LESS避免了开发者了多次运行查找/替换命令，可以使我们更动态、有组织的创建样式表。现在创建Dijit主题的复杂度成指数倍降低，我期望看见更多主题出现。

![davidwalsh_theme](/images/2013/davidwalsh_theme.png)

关于上面的方法我在补充三点：

*   在替换’.claro’为自定义主题名时，也需要将’claro.css’文件和’claro_rtl.css’文件名中的’claro’替换为自定义主题名。
*   在使用Node.js来编译compile.js时，如果你下载的是Node.js 0.8 之后的版本，最好下载dojo1.8+，不然会因为Node.js中的模块的改变而报错。
*   以上操作需要使用完整的、未压缩的dojo版本

> 使用LESS CSS创建dojo主题翻译自：[Create Your Own Dijit CSS Theme with LESS CSS](http://davidwalsh.name/dijit-theme)
