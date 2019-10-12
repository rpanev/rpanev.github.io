---
id: 828
title: HP Smart Storage Administrator
date: 2015-12-18T12:34:34+00:00
author: panev
layout: post
categories:
  - Linux
---
## How to install

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
