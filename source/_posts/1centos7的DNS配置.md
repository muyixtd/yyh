---
title: centos7的DNS配置
date: 2023-05-06 18:30:01
tags:
---
CentOS配置DNS的详细步骤如下：

打开终端或控制台，通过root权限登录系统。

编辑本地DNS服务器列表。可以编辑主机文件/etc/hosts或者修改/etc/resolv.conf文件。打开/etc/resolv.conf并添加DNS服务器的IP地址。例如：

nameserver 8.8.8.8
nameserver 8.8.4.4
保存并退出文件。

测试DNS是否配置成功。使用nslookup命令或dig命令查询一个有效域名。例如，运行以下命令检查百度的IP地址：

nslookup baidu.com
如果返回了baidu.com的IP地址，则表示DNS服务器已经正确工作。

（可选）设置DNS缓存时间。在/etc/named.conf （BIND9）或 /etc/nscd.conf（nscd）文件中配置DNS缓存问题。

重启与网络相关的服务器服务，以便使DNS服务器的更改生效。

注意：这些详细步骤基于CentOS 7.x版本，如果您是其他版本的CentOS操作系统，具体步骤可能会略微不同。
