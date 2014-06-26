---
layout: post
title: "nginx配置www域名和无www域名互转"
date: 2014-06-26 15:42:45 +0800
comments: true
categories: nginx
---

最近申请了自己的域名，并在dnspod上将域名指向在digitalocean申请的vps，vps上安装了nginx，一番折腾后发现访问有www的域名和没有www的域名返回不同页面，google了一下，最后问题解决。

www.renhai.me转向renhai.me，配置如下：<!--more-->
```
    server {
        listen       80;
        server_name  www.renhai.me renhai.me;
        if ($host = 'www.renhai.me' ) {
                rewrite ^/(.*)$ http://renhai.me/$1 permanent;
        }
        
        ......
        
    }
```
同理，也可以将renhai.me转向www.renhai.me，配置如下：
```
    server {
        listen       80;
        server_name  www.renhai.me renhai.me;
        if ($host = 'renhai.me' ) {
                rewrite ^/(.*)$ http://www.renhai.me/$1 permanent;
        }
        
        ......
        
    }
```
另一种方式是：
```
    server {
        listen       80;
        server_name  www.renhai.me;

        location / {
            root   html;
            index  index.html index.htm;
            
            ......
            
        }
    }
    server {
        listen  80;
        server_name renhai.me;
        return  301 http://www.renhai.me$request_uri;
    }
```
推荐使用第一种方式。

测试一下：
[http://www.renhai.me](http://www.renhai.me)
[http://renhai.me](http://renhai.me)

