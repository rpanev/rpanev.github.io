---
id: 45
title: Install Redis and Redis PHP on cPanel
date: 2019-06-05T05:13:17+00:00
author: Panev
layout: post
#guid: https://www.rpanev.pro/?p=45
#permalink: /sysadmin/linux/install-redis-and-redis-php-on-cpanel.html
image: /wp-content/uploads/2019/06/cpanel.jpg
categories:
  - centos
  - system-administration
---
# Install Redis and Redis PHP on cPanel

Install the EPEL Repository on CentOS 7

{% highlight bash %}
yum install epel-release
yum update
yum install redis
systemctl restart redis
systemctl enable redis
{% endhighlight %}

Install the <a href="https://redis.io/" rel="noopener noreferrer" target="_blank">Redis</a> PHP Extension EasyApache 4

EasyApache 4 supports multiple versions of PHP so we need to install the Redis PHP extension on each PHP version. Run the following commands to install and enable the Redis PHP extension on each PHP version you have installed on your server:

{% highlight bash %}
yes | /opt/cpanel/ea-php71/root/usr/bin/pecl install igbinary igbinary-devel redis 
/opt/cpanel/ea-php71/root/usr/bin/php -m | grep redis

yes | /opt/cpanel/ea-php70/root/usr/bin/pecl install igbinary igbinary-devel redis 
/opt/cpanel/ea-php70/root/usr/bin/php -m | grep redis

yes | /opt/cpanel/ea-php56/root/usr/bin/pecl install igbinary igbinary-devel redis 
/opt/cpanel/ea-php56/root/usr/bin/php -m | grep redis
{% endhighlight %}

