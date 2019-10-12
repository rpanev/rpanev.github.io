---
id: 828
title: HP Smart Storage Administrator
date: 2015-12-18T12:34:34+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog/?p=828
permalink: /who-to-install-hpssa.html
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
  - "0"
wp_review_user_review_type:
  - star
tie_views:
  - "267"
tie_sidebar_pos:
  - default
tie_post_slider:
  - "1019"
image: /wp-content/uploads/2015/12/centos_logo.jpg
categories:
  - Linux
---
## HP Smart Storage Administrator How to install

За да инсталирате HPSSA (HP Smart Storage Administrator) просто изпълнете следните 4 стъпки :

1) Създавате ново хранилище :

nano /etc/yum.repos.d/HP_hpssa.repo

и залагате следният код :

[hp-mcp]  
name=HP Management Component Pack  
baseurl=http://downloads.linux.hp.com/repo/spp/RedHat/$releasever/$basearch/current/  
enabled=1  
gpgcheck=0

2) Обновявате

yum update

3) Инсталираме

yum install kmod-hpsa.x86\_64 hpssa.x86\_64 hpssacli.x86_64 (hpssacli не е задължително то е управление под конзола)

4) Стартирате конзолата :

hpssa &#8211; local (през X) и те се стартира автоматично браузъра подразбиране със зареденият адрес

Тествано е на CentoS 6.x и CentOS 7.X  
<img class="alignleft size-medium wp-image-829" src="http://panevinfo.eu/blog/wp-content/uploads/2015/12/HPSSA-300x225.png" alt="HPSSA" width="300" height="225" srcset="https://www.panevinfo.eu/wp-content/uploads/2015/12/HPSSA-300x225.png 300w, https://www.panevinfo.eu/wp-content/uploads/2015/12/HPSSA-768x576.png 768w, https://www.panevinfo.eu/wp-content/uploads/2015/12/HPSSA.png 1024w" sizes="(max-width: 300px) 100vw, 300px" /> 

Вижте още!  
[Смяна на името подразбиран не мрежата](http://panevinfo.eu/blog/change-default-network-name-to-old-eth0%e2%80%b3-on-rhel-7-centos-7-above.html)  
[KVM виртуализация под CentOS 7](http://panevinfo.eu/blog/how-to-install-and-configure-kvm-and-virtmanager-on-centos-7.html)  
[CentOS WebPanel](http://panevinfo.eu/blog/%d0%ba%d0%b0%d0%ba-%d0%b4%d0%b0-%d0%b8%d0%bd%d1%81%d1%82%d0%b0%d0%bb%d0%b8%d1%80%d0%b0%d0%bc%d0%b5-cwp-centos-webpanel.html)