---
title: 让Nginx支持WordPress的固定链接
date: 2014-04-03 13:31:15
categories:
- Nginx
tags:
- Nginx
- Wordpress
---


最近给博客搬了家，但是之前的固定链接打开全是404，造成了之前很多页面没法使用固定链接访问，虽然使用那个默认连接能够访问，但还是有些不方便，今天找了下解决方案，终于恢复了，解决方案如下：

由于是使用lnmp一键安装包，所以要让nginx支持wordpress固定链接非常简单，因为安装后/usr/local/nginx/conf/目录下有一个wordpress.conf文件，将其包含进nginx.conf即可。

具体的操作步骤如下：

``` bash
cd /usr/local/nginx/conf/

cp nginx.conf nginx.conf.bak   // 将原配置文件备份一下

vi nginx.conf
```

将nginx.conf中的代码:

``` bash
        {
                listen       80;
                server_name gevin.me;
                index index.html index.htm index.php;
                root  /home/wwwroot;
                location ~ .*\.(php|php5)?$
                        {
                                fastcgi_pass  unix:/tmp/php-cgi.sock;
                                fastcgi_index index.php;
                                include fcgi.conf;
                        }

                location /status {
                        stub_status on;
                        access_log   off;
                }
```

替换成（增加了include wordpress.conf;）

``` bash
server
        {
                listen       80;
                server_name gevin.me;
                index index.html index.htm index.php;
                root  /home/wwwroot;
                        include wordpress.conf;
                location ~ .*\.(php|php5)?$
                        {
                                fastcgi_pass  unix:/tmp/php-cgi.sock;
                                fastcgi_index index.php;
                                include fcgi.conf;
                        }

                location /status {
                        stub_status on;
                        access_log   off;
                }

```
然后，重启lnmp即可.  
``` bash
/root/lnmp restart
```

最后附上lnmp一键安装包中自带的wordpress.conf的内容，假如你不想包含wordpress.conf，你也可以将wordpress.conf里面的内容拷到nginx.conf里面。  
wordpress.conf内容如下：  
``` bash
location / {
if (-f $request_filename/index.html){
                rewrite (.*) $1/index.html break;
        }
if (-f $request_filename/index.php){
                rewrite (.*) $1/index.php;
        }
if (!-f $request_filename){
                rewrite (.*) /index.php;
        }
}
```

> 方案参考自：[http://www.ctrol.cn/post/freesource/websys/11-04-ctrol-2292.html](http://www.ctrol.cn/post/freesource/websys/11-04-ctrol-2292.html)