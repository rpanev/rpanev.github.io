---
id: 604
title: How to install Installing Skype on Fedora, CentOS and Red Het
date: 2014-04-01T18:33:05+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=604
permalink: /how-to-install-installing-skype-on-fedora.html
tie_views:
  - "200"
image: /wp-content/uploads/2014/04/skype.jpg
categories:
  - Linux
---
Инсталиране на skype на Fedora, CentOS и Red Het 32 или 64 бита чрез YUM

Първо трябва да създаден YUM хранилище:

<pre>vi /etc/yum.repos.d/skype.repo</pre>

Напишете следното:

<pre>[skype]
name=Skype Repository
baseurl=http://download.skype.com/linux/repos/fedora/updates/i586/
gpgkey=http://www.skype.com/products/skype/linux/rpm-public-key.asc
enabled=1
gpgcheck=0
</pre>

След което просто изпълняваме стандартната команда за инсталация

<pre>yum install skype</pre>