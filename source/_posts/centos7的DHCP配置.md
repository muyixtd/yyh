---
title: centos7的DHCP配置
date: 2023-05-06 17:37:56
tags:
---
动态分配TCP/IP（IP地址、子网掩码、默认网关、DNS服务器）

分配出去的信息是有租约的。

DHCP系统组成：

DHCP client(客户端)：通过DHCP协议请求获取IP地址等网络参数的设备。

DH Server(服务器)：能提供DHCP功能的服务器或网络设备。

DHCP Relay(中继)：负责转发DHCP服务器和DHCP客户端之间的DHCP报文，协助DHCP服务器向DHCP客户端动态分配网络参数的设备。

报文：

DHCP Discover：客户端用来寻找DHCP服务器

DHCP Offer：DHCP服务器来响应DHCP Discover报文，此报文携带了各种配置信息。

DHCP Request：客户端请求配置确认，或者续借租期

DHCP Ack：服务器对Reqest报文的确认响应

DHCP Nak：服务器对Request报文的拒绝响应

DHCP Release：客户端要释放地址时用来通知服务器

VMVare WorkStation中v8网络自带NAT，自带虚拟网关、自带DHCP服务器的网络，需要关闭自带的DHCP服务，设置物理机上的V8网卡的IP地址为192.168.74.1，虚拟网关的地址为192.168.74.220。第一台虚拟机连接V8网络，并且IP地址为192.168.74.254，并按一下步骤配置DHCP服务：

安装DHCP服务：yum -y install dhcp

复制示例文件：cp -a /usr/share/doc/dhcp-4.2.5/dhcpd.conf.example /etc/dhcp/dhcpd.conf

编辑这个文件：vim /etc/dhcp/dhcpd.conf

指定dhcp的网络，地址池的IP地址范围，网关地址，广播地址以及默认租期和最长租期

	subnet 192.168.74.0 netmask 255.255.255.0 { #子网号和掩码号

		range 192.168.74.100 192.168.74.200; #ip地址范围
  
		option domain-name-servers 114.114.114.114 #域名服务器地址

		option routers 192.168.74.220; #域名

		option broadcast-address 192.168.74.255; #网关地址

		default-lease-time 600; #默认租约时间长度

		max-lease-time 7200; #最大租约时间长度

	}

保存以上文件。

关闭防火墙：systemctl stop firewalld

重启DHCP服务：systemctl restart dhcpd

第二台虚拟机的网卡连接net网络，并且其IP地址为自动获取，即

BOOTPROTO=dhcp ONBOOT=yes。

使用ifup ens33后，用ifconfig查看，其ip地址。
