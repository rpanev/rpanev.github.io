---
id: 470
title: LAMP Server on CentOS 6
date: 2014-03-02T16:31:19+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=470
permalink: /lamp-server-on-centos-6.html
wpgplus_message:
  - ""
wpgplus_publish:
  - 'no'
tie_views:
  - "209"
image: /wp-content/uploads/2014/03/lamp.png
categories:
  - code
  - Linux
---
Прост bash скрипт с който да инсталирате LAMP (Linux Apache MySql Php) на вашият сървър. Кода е както следва :

<pre>#!/bin/bash
#Linux Apache MySql Php install
#Last updated: March - 2014
#http://panevifo.eu
#Update your system
echo -e "Check for updates"
yum -y update 
#HTTPD
echo -e "Install HTTPD"
yum -y install httpd
service httpd start
#install php
echo -e "Install PHP"
yum -y install php php-devel php-gd php-imap php-ldap php-mysql php-odbc php-pear php-xml php-xmlrpc php-pecl-apc php-mbstring php-mcrypt php-mssql php-snmp php-soap php-tidy curl curl-devel perl-libwww-perl ImageMagick libxml2 libxml2-devel mod_fcgid php-cli httpd-devel
#install mysql-server
echo -e "Install MySql Server"
yum -y install mysql-server
service mysqld restart
#HTTPD
echo -e "Install HTTPD"
yum -y install httpd
#add Mysql and HTTPD to chkconfig 
/sbin/chkconfig --levels 235 httpd on 
/sbin/chkconfig --levels 235 mysqld on
exit 0
</pre>

След инсталацията напишете следната команда :

<pre># mysql_secure_installation
</pre>

и следвайте инструкциите :

<pre>NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MySQL
SERVERS IN PRODUCTION USE! PLEASE READ EACH STEP CAREFULLY!

In order to log into MySQL to secure it, we'll need the current
password for the root user. If you've just installed MySQL, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MySQL
root user without the proper authorisation.

Set root password? [Y/n] 
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
... Success!

By default, a MySQL installation has an anonymous user, allowing anyone
to log into MySQL without having to have a user account created for
them. This is intended only for testing, and to make the installation
go a bit smoother. You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] 
... Success!

Normally, root should only be allowed to connect from 'localhost'. This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] 
... Success!

By default, MySQL comes with a database named 'test' that anyone can
access. This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] 
- Dropping test database...
... Success!
- Removing privileges on test database...
... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] 
... Success!

Cleaning up...

All done! If you've completed all of the above steps, your MySQL
installation should now be secure.

Thanks for using MySQL!
</pre>