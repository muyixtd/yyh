---
title: centos7的web服务之虚拟用户配置
date: 2023-05-06 16:02:35
tags:
---
在CentOS7中，配置Apache虚拟用户的步骤如下：

创建一个非特权用户
sudo useradd -m -d /var/www/virtualuser -s /usr/sbin/nologin virtualuser
此命令将创建一个名称为 virtualuser 的非特权用户，并将主目录设置为 /var/www/virtualuser。此外还禁用了登录shell。

设置apache用户组
sudo groupadd apache_virtualuser
sudo usermod -aG apache_virtualuser apache
sudo usermod -aG apache_virtualuser virtualuser
上述命令创建名为 apache_virtualuser 的组，并将 apache 和 virtualuser 用户添加到该组中。这将允许您授予虚拟用户与 apache 相同的权限。

创建网站目录
sudo mkdir -p /var/www/example.com/public_html
sudo chown -R virtualuser:apache_virtualuser /var/www/example.com
sudo chmod 2775 /var/www/virtualuser
此命令将创建一个名为 example.com 的网站目录，将其拥有者设置为 virtualuser 用户并启用 apache_virtualuser 组对其进行读写操作的权限。

配置Apache虚拟主机

<VirtualHost *:80>
ServerAdmin admin@example.com
ServerName example.com
ServerAlias www.example.com
DocumentRoot /var/www/example.com/public_html

<Directory /var/www/example.com/public_html>
Options Indexes FollowSymLinks MultiViews
AllowOverride All
Order allow,deny
allow from all
</Directory>

ErrorLog /var/log/httpd/example.com_error.log
CustomLog /var/log/httpd/example.com_access.log common

</VirtualHost>

上述配置将创建Apache虚拟主机，该虚拟主机监听端口 80，并使用 example.com 和 www.example.com 作为服务器名称和别名。此外还配置了文档根目录、错误日志和访问日志等。

重启apache
sudo systemctl restart httpd
在修改完配置之后，需要重新启动Apache才能使配置生效。

配置完成后，您应该能够使用虚拟用户设置的用户名和密码登录并上传新的文件到网站目录中。
