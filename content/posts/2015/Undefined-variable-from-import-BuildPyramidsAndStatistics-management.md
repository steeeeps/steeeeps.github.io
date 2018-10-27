---
title: Undefined-variable-from-import-BuildPyramidsAndStatistics_management
date: 2015-11-24 17:53:54
categories:
- Python
tags:
- Python
- arcpy
---

使用arcpy.BuildPyramidsAndStatistics_management ()构建栅格数据金字塔和做统计值时，提示该方法为定义:  
> Undefined variable from import: BuildPyramidsAndStatistics_management.    

查看arcpy源码发现，时帮助文档中单词拼写出现了问题，正确的写法应该是:   
``` bash
arcpy.BuildPyramidsandStatistics_management()
```
