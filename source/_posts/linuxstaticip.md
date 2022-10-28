---
title: Linux静态IP设置
date: 2022-05-19 18:31:11
tags: 
- tech
- linux
- linux静态IP设置
---
# Linux静态ip设置

找到安装时的ens33文件位置：相关命令`cd /etc/sysconfig/network-scripts`

这里注意登录的用户是否为root用户，修改ifcfg-ens33文件需要root用户，因为只有root用户有写权限

![image01](./img/1664266461228.jpg)

编辑ifcfg-ens33文件：`vim/vi ifcfg-ens33`

![image02](./img/1664266517317.jpg)

使用DHCP的好处是不会发生ip冲突，但是这样的话，虚拟机每次登录的ip地址可能会变，所以静态ip就很有必要

进入编辑页面：**i** 进入编辑模式，**ESC**退出编辑模式进入命令模式，在命令模式下输入**:wq** 以保存编辑的信息

![image03](./img/1664266130579.jpg)

ifcfg-ens33文件中：

+ TYPE=Ethernet      #网络类型（通常是Ethernet，万维网）
+ HWADDR=00:0C:29:76:75:F9       #MAC地址
+ DEVICE=ens33 	#接口名（设备，网卡）
+ UUID="911d4460-38cc-4250-baea-321ca2ce195b"      #随机id
+ ONBOOT=yes        #系统启动的时候网络接口是否有效（yes/no）
+ BOOTPROTO="static"    #IP的配置方法\[none\|static\|bootp\|dhcp\]（引导时不使用协议|静态分配IP|BOOTP协议|DHCP协议）
+ IPADDR=192.168.157.132     #IP地址
+ GATEWAY=192.168.157.2    #网关
+ DNS1=192.168.157.2     #域名解析器

配置完成之后，需要重启网络服务或者重启系统才能生效`service network restart`或`reboot`