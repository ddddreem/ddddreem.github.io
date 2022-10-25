---
title: Nginx操作手册
date: 2022-09-18 15:25:11
tags: 
- tech
- nginx
---
# Nginx操作手册

## Nginx在linux中安装

+ 下载nginx安装包，解压缩，在解压目录下执行执行`./configure`命令。

<font color=blue size=4>**注**</font>：执行`./configure`命令需要pcre依赖、gcc依赖和zlib依赖。可以通过执行`yum -y install pcre pcre-devel `、`yum -y install zlib zlib-devel` 和`yum -y install gcc`来分别安装依赖，或者执行`yum -y install make zlib zlib-devel gcc-c++ libtool openssl openssl-devel`来一键安装上述依赖。

+ 在执行完`./configure`后，执行`make && make install`命令进行安装，安装完成后，会在`/usr/local`下生成一个`/nginx`文件夹，里面包含了nginx的启动文件和配置文件，配置nginx的功能主要在`/usr/local/nginx/conf/nginx.conf`这个文件中配置。

## Nginx常用命令

+ 查看Nginx版本信息：在`/usr/local/nginx/sbin`中执行`./nginx -v` 命令
+ 启动命令：在`/usr/local/nginx/sbin`中执行`./nginx` 命令
+ 停止命令：在`/usr/local/nginx/sbin`中执行`./nginx -s stop` 命令
+ 重启命令：在`/usr/local/nginx/sbin`中执行`./nginx -s reload` 命令

## 1.Nginx配置反向代理

### 1)nginx正向代理和反向代理的概念

+ <font color=blue size=4>**正向代理**</font>：客户端不能访问外网，需要配置代理服务器，可以通过代理服务器代理上网。即将请求发到一个自身可以访问的服务器上，该服务器可以访问自身且可以访问外网。如图：

  ![image-20220918122857171](./img/image-20220918122857171.png)

+ <font color=blue size=4>**反向代理**</font>：我们只需要将请求发送到反向代理服务器，有反向代理服务器去选择目标服务器获取数据，再返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是地理服务器的地址。隐藏了真实响应服务的服务器的IP地址。如图：

  ![image-20220918124022699](./img/image-20220918124022699.png)

  <font color=blue size=4>**例子**</font>：（内网状态下，虚拟机环境中）比如现在我需要访问的的地址是192.168.137.100:9001这个端口服务，在Nginx中可以配置监听的端口和服务器地址，当客户端发出服务请求时，监听到服务，并根据配置将请求转发到真实的服务地址。相当于不能直接请求目标服务器的服务，该服务器对外是透明的。

### 2)Nginx反向代理配置

<!-- more -->

实现nginx的反向代理功能只需要修改`/usr/local/nginx/conf`文件夹中的`nginx.conf`文件即可，具体配置如下：

各个关键字的含义：

+ worker_processes：表示工作进程，即用来监听请求的进程，数字表示监听进程的数量

+ events：`use poll;`用来配置询问响应请求的顺序，`worker_connections 1024;`表示每个工作进程所支持的最大连接数；

  eg:

  ![](./img/image-20220920114153982.png)

  其中，`use`后面跟的是Nginx轮询方法，不写的话，Nginx会根据操作系统自动选择合适的轮询方式

其他配置参考其他博主的分享:https://blog.51cto.com/u_15196075/5667301

### 3)配置实例

在虚拟主机中配置监听的端口号和监听的主机地址：其中主机地址一般为暴露给外部的nginx的主机地址

![image-20220920115237579](./img/image-20220920115237579.png)

1号位置填写nginx监听的端口号；

2号位置填写nginx服务器的地址；

3号位置表示nginx监听到80端口需要跳转的服务，即映射关系；

4号位置的root主要用来实现动态资源和静态资源的分开映射， root用来设置映射的基地址。

其他的譬如`error_page`用来设置映射错误的时候跳转的页面。

注：如果代理服务填的是其他服务器上的服务，也可以完成代理，如：

![image-20220920122640448](./img/image-20220920122640448.png)

效果如下：

![image-20220920122736884](./img/image-20220920122736884.png)

跳转到百度页面：

![image-20220920122815682](./img/image-20220920122815682.png)

## 2.Nginx配置负载均衡

负载均衡在反向代理的基础上增加了类似集群的东西，通过设置多个映射服务完成负载均衡，即设置`upstream`关键字，在里面配置可以响应接口请求的一系列映射关系。

eg：

![image-20220920120030251](./img/image-20220920120030251.png)

1号位置用来配置一些映射关系，`server 服务地址:端口 weight=x`，server是固定用法，标志后面的服务地址，weight用来设置映射的权重，如果weight大的话，nginx代理到这个服务的请求就会变多。

<font color=blue size=4>**注意**</font>：在配置`upstream`的时候，后面跟着的集群名称尽量少带`__`下划线，这样可能导致映射失败。

效果如下：

+ 启动两台tomcat服务器，绑定不同的端口号；
+ 在地址栏中键入`http://192.168.137.100`即可访问不同地址的两台服务器，这里我让他访问服务器里的一个html文件；

![image-20220920123701372](./img/image-20220920123701372.png)

分别启动两台tomcat服务器：

![image-20220920124628459](./img/image-20220920124628459.png)

![image-20220920124442016](./img/image-20220920124442016.png)

![image-20220920124511583](./img/image-20220920124511583.png)

+ 会配置如上两个服务，Nginx差不多就学完了，能满足日常使用了，如果不追求其他的东西的话。



## 3.Nginx配置动静分离



## 4.Nginx配置高可用集群































































