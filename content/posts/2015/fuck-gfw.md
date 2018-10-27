---
title: 外面的世界很精彩
date: 2015-07-16 22:48:14
categories:
- 工具
tags:
- VPS
---

博客一直搭建在[digitalocean](https://www.digitalocean.com/?refcode=b4fcae38bf56)提供的vps上，5$一月，很便宜，之前一直使用免费vpn，但是每小时需要更换密码，不方便。  

反正vps闲着也是闲着，就搭建了一个vpn，方便工作时google用。  

在网上找了一个CentOS 6一键安装PPTP VPN脚本，还挺简单，如果有朋友需要，可以试一试。  

<!-- more -->

## 环境

> CentOS 6.x 32位/64位
> XEN/KVM/OpenVZ

## 安装步骤

1. 依次运行以下代码  
``` bash
cd /root
wget http://www.huangjiexing.com/wp-content/uploads/sh/pptpd.sh
chmod +x pptpd.sh
./pptpd.sh
```

2. 稍等片刻后，将返回以下信息表示成功  
``` bash
VPN service installed successfully, your VPN username is vpn, VPN password is XXXXXX
```
3. 这是预设账号，运行以下命令添加帐户，按格式在新的一行写入你需要的账号和密码。
``` bash
vi /etc/ppp/chap-secrets
```

4. 安装成功。  

如果需求不大，可以使用[1小时vpn](http://freevpn1.wwdhz.com/)账号。    
如果没有vps，推荐购买[海贝vpn](http://www.17mayi.net/sign/)，同事一直在用，说还不错。  
如果需要购买vps，推荐使用digitalocean，性价比较高，现在注册充值5美元送10美元，相当于5美元使用3个月,[点击购买](https://www.digitalocean.com/?refcode=b4fcae38bf56)  
 
技术转载自：[CentOS 6一键安装PPTP VPN脚本](http://www.huangjiexing.com/398.html)

