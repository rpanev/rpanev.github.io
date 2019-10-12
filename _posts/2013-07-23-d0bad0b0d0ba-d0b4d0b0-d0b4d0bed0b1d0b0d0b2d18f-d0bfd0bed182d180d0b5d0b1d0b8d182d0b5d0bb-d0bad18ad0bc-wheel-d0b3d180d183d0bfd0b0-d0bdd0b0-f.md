---
id: 1308
title: Как да добавя потребител към Wheel група на FreeBSD
date: 2013-07-23T13:32:30+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=63
permalink: '/%d0%ba%d0%b0%d0%ba-%d0%b4%d0%b0-%d0%b4%d0%be%d0%b1%d0%b0%d0%b2%d1%8f-%d0%bf%d0%be%d1%82%d1%80%d0%b5%d0%b1%d0%b8%d1%82%d0%b5%d0%bb-%d0%ba%d1%8a%d0%bc-wheel-%d0%b3%d1%80%d1%83%d0%bf%d0%b0-%d0%bd%d0%b0-f.html'
wp_review_location:
  - bottom
wp_review_desc_title:
  - Summary
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
tie_views:
  - "184"
image: /wp-content/uploads/2016/01/freebsdlogo.jpg
categories:
  - FreeBSD
---
Понякога се налага да добавим даден потребител към групата Wheel, това може да стане директно чрез редактиране на /etc/group или чрез командата **pw** аз лично предпочитам вторият вариант 🙂  
**За да може да добавите потребител в групата Wheel естествено ще трябва да сте root**.

Тази команда ще създаде и добави потребител panev, който ще принадлежи към група Wheel

<pre>pw useradd panev -G wheel</pre>

Използвайте опцията &#8222;G&#8220; за добавяне на потребител към дадена група  
Използвайте следната команда за да зададете парола на потребител panev. 🙂

<pre>passwd panev</pre>

Ако потребителя вече съществува и искате да го добавите в групата просто използвайте **groupmod** .

<pre>pw groupmod wheel -m panev</pre>