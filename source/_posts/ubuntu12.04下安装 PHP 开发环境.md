---
layout: post
title: ubuntu12.04下安装 PHP 开发环境
date: 2013-11-01 11:21:04
tags: Linux
---

安装mysql

sudo apt-get install mysql-server-5.5 mysql-client-5.5
安装过程中会要求书图密码

安装apache2

sudo apt-get install apache2
安装php5

sudo apt-get install php5 libapache2-mod-php5
测试php

sudo vim /var/www/info.php
写入一下内容：

<?php
phpinfo();
?>
浏览localhost/info.php

安装phpmyadmin

sudo apt-get install phpmyadmin
中间和有要求输入密码

配置

为phpmyadmin添加链接

sudo ln -s /usr/share/phpmyadmin /var/www/phpmyadmin
修改php.ini配置文件

sudo vim /etc/php5/apache2/php.ini
在最后一行加上

extension=mysqli.so
否则会出现缺少 mysqli 扩展。请检查 PHP 配置 错误

重新启动apache2和mysql

sudo /etc/init.d/apache2 restart
sudo /etc/init.d/mysql restart




