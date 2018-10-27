---
title: 将python程序注册为windows服务
date: 2015-12-14 16:56:18
categories:
- Python
tags:
- Python
---

根据项目需求，需要将python写的程序，注册成windows服务，方便用户启动、停止与管理。  
注册方法如下：  

<!-- more -->

1. 下载并安装[Windows Resource Kit](http://www.microsoft.com/download/en/details.aspx?DisplayLang=en&id=17657)，默认安装目录 ：C:\Program Files\Windows Resource Kits\ 。注册服务只使用srvany.exe文件，可将该文件单独拷出。
2. 打开cmd，输入如下命令创建服务：
``` bash
sc create "[YourService]" binPath= "C:\Program Files\Windows Resource Kits\srvany.exe"
```
3. 打开注册表，在HKEY_LOCAL_MACHINE > SYSTEM > CurrentControlSet > Services项中找到刚才创建的服务。
4. 右键服务名>新建>项,将新项重命名为Parameters。
5. 在刚才创建的Parameters上右键>新建>字符串值，将新值重命名为Application，双击Application，修改数据为[python安装目录]\python.exe [你的python程序].py。
6. 
设置完成。

> 参考：[http://stackoverflow.com/questions/8666373/start-python-py-as-a-service-in-windows](http://stackoverflow.com/questions/8666373/start-python-py-as-a-service-in-windows)

