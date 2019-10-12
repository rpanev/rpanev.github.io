---
id: 855
title: 'Как да инсталираме CWP &#8211; CentOS WebPanel'
date: 2016-01-26T06:51:17+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog/?p=855
permalink: '/%d0%ba%d0%b0%d0%ba-%d0%b4%d0%b0-%d0%b8%d0%bd%d1%81%d1%82%d0%b0%d0%bb%d0%b8%d1%80%d0%b0%d0%bc%d0%b5-cwp-centos-webpanel.html'
wp_review_location:
  - bottom
wp_review_desc_title:
  - Резюме
wp_review_color:
  - '#1e73be'
wp_review_fontcolor:
  - '#555555'
wp_review_bgcolor1:
  - '#e7e7e7'
wp_review_bgcolor2:
  - '#ffffff'
wp_review_bordercolor:
  - '#e7e7e7'
post_views_count:
  - "14"
wp_review_user_review_type:
  - star
tie_views:
  - "416"
tie_sidebar_pos:
  - default
tie_post_slider:
  - "1019"
image: /wp-content/uploads/2016/01/centos-web-panel.jpg
categories:
  - Linux
tags:
  - CentOS
  - CWP
  - free
  - hosting panel
---
## Как да инсталираме CWP

Преди да започнете с инсталацията на CWP, трябва да знаете следната информация:

&#8211; Поддържат се само статични IP адреси. Създателите на този панел не поддържат динамични IP адреси или IP адреси от вътрешни мрежи .  
&#8211; CWP не може да бъде деинсталиран. След като инсталирате CWP, трябва да преинсталирате сървъра, за да го премахнете.  
&#8211; Инсталирайте само CWP на прясно инсталирана операционна система, без каквито и да било промени в конфигурацията.  
&#8211; Поддържаните операционни системи са CentOS 6, RedHat 6 и CloudLinux 6, като се препоръчва минимална инсталация.  
&#8211; Системни изсеквания :

32 битова операционна система изисква минимално от 512 MB RAM  
64 битова операционна система изисква минимален, 1024 MB RAM (препоръчва се)  
Препоръчва се да има 4 GB RAM, така че ще имате пълната функционалност, като антивирусни сканиране на имейли

След инсталацията на операционната система е необходимо да :

1. Да инсталиране wget за да може да изтеглите инсталационният скрипт на CWP

<pre>yum -y install wget</pre>

2. Да се обнови системата

<pre>yum -y update</pre>

След като обновяването приключи и инсталирате всички последни и актуални версии на вашата операционна система е необходимо да изтеглим скрипта и да инсталираме самият панел. Това става по-лесният начин.

1. Създателите на CWP препоръчват да изтеглите инсталационният скрипт в директория /usr/local/src

<pre>cd /usr/local/src
wget http://centos-webpanel.com/cwp-latest</pre>

2. Препоръчвам ви да използвате MariaDB за дата бейс сървис

<pre>sh cwp-latest -d mariadb</pre>

за да инсталирате mysql е необходимо просто да изпълните скрипта sh cwp-latest  
3. Рестартирайте система.

<pre>reboot</pre>

За достъп до панел може да използвате IP адреса ви http://SERVER-IP:2030/ или 2031 за SSL връзка. Потребителят е root а паролата е текущата за вашият сървър.

Подобни статии:

[LAMP сървър под CentOS 6](http://panevinfo.eu/blog/lamp-server-on-centos-6.html)  
[Защитете допълнително вашият сървър](http://panevinfo.eu/blog/%d0%b4%d0%be%d0%bf%d1%8a%d0%bb%d0%bd%d0%b8%d1%82%d0%b5%d0%bb%d0%bd%d0%b0-%d0%b7%d0%b0%d1%89%d0%b8%d1%82%d0%b0-%d1%87%d1%80%d0%b5%d0%b7-fail2ban.html)