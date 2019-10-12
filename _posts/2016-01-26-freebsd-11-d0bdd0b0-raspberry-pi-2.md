---
id: 847
title: FreeBSD 11 на Raspberry Pi 2?
date: 2016-01-26T03:25:00+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog/?p=847
permalink: '/freebsd-11-%d0%bd%d0%b0-raspberry-pi-2.html'
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
wp_review_user_reviews:
  - "0"
wp_review_review_count:
  - "0"
post_views_count:
  - "1"
wp_review_user_review_type:
  - star
tie_views:
  - "4372"
tie_sidebar_pos:
  - default
tie_post_slider:
  - "1019"
image: /wp-content/uploads/2016/01/freebsd_logo.jpg
categories:
  - FreeBSD
---
## FreeBSD 11 на Raspberry Pi 2 &#8211; как ?

Можете лесно да инсталирате FreeBSD 10 или FreeBSD 11 на Raspberry Pi 2 Model B. Можете да се изгради хубав и лесен за използване Unix сървър посредством FreeBSD операционна система. FreeBSD-CURRENT подкрепи Raspberry Pi от ноември 2012 г. и Raspberry Pi 2 от март, 2015 г. В този бърз урок аз ще обясня как да инсталирате FreeBSD 11 текущата версия на RPI2.

1.Изтегляне на FreeBSD за arm

Може да видите пълният списък на поддържани arm системи от <a href="ftp://ftp.freebsd.org/pub/FreeBSD/snapshots/arm/armv6/ISO-IMAGES/11.0" target="_blank">тук</a>. Или да използвате командите wget и curl за да свалите изображението на FreeBSD 11.

<pre>wget ftp://ftp.freebsd.org/pub/FreeBSD/snapshots/arm/armv6/ISO-IMAGES/11.0/FreeBSD-11.0-CURRENT-arm-armv6-RPI2-20151016-r289420.img.xz</pre>

или

<pre>curl -O ftp://ftp.freebsd.org/pub/FreeBSD/snapshots/arm/armv6/ISO-IMAGES/11.0/FreeBSD-11.0-CURRENT-arm-armv6-RPI2-20151016-r289420.img.xz</pre>

2. Разархивирате архива

<pre>unxz FreeBSD-11.0-CURRENT-arm-armv6-RPI2-20151016-r289420.img.xz</pre>

3. Инсталиране на SD карта

<pre>dd if=FreeBSD-11.0-CURRENT-arm-armv6-RPI2-20151016-r289420.img of=/dev/disk2 bs=64k</pre>

4. Зареждане на FreeBSD

Поставете SD картата във вашата Raspberry Pi 2 Model B. Трябва да се свържете клавиатура, мишка и монитор дисплей.

5. Потребителско име и парола за RPi2

Паролите по подразбиране за изображенията са freebsd / freebsd и root / root.

Това е, вече имате FreeBSD сървър базиран на Raspberry Pi 2 Model B!

Обвързани статии :

[How to upgrade FreeBSD 10 from RELEASE to STABLE](http://panevinfo.eu/blog/how-to-upgrade-freebsd-10-from-release-to-stable/)

[How To Tail Multiple Files on UNIX / Linux Console](http://panevinfo.eu/blog/how-to-tail-multiple-files-on-unix-linux-console/)

[How to install FreeBSD 10 Step by Step](http://panevinfo.eu/blog/how-to-upgrade-freebsd-10-from-release-to-stable/)