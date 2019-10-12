---
id: 1316
title: How to Install Google Chrome on Fedora, CentOS and Red Het Linux
date: 2014-04-01T17:53:32+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=602
permalink: /how-to-install-google-chrome-on-fedora-centos-and-red-het-linux.html
tie_views:
  - "188"
image: /wp-content/uploads/2014/03/s-5c-a-517-e1414183729413.png
categories:
  - code
  - Linux
---
Първо трябва да създаден YUM хранилище:

<pre>vi /etc/yum.repos.d/google-chrome.repo </pre>

Напишете следното:

<pre>[google]
name=Google - $basearch
baseurl=http://dl.google.com/linux/rpm/stable/$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub

[google-testing]
name=Google Testing - $basearch
baseurl=http://dl.google.com/linux/rpm/testing/$basearch
enabled=0
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub

[google-earth]
name=Google Earth $basearch
baseurl=http://dl.google.com/linux/earth/rpm/stable/$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub

[google-chrome]
name=Google Chrome $basearch
baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_sign
</pre>

След което просто изпълняваме стандартната команда за инсталация

<pre>yum install google-chrome
</pre>