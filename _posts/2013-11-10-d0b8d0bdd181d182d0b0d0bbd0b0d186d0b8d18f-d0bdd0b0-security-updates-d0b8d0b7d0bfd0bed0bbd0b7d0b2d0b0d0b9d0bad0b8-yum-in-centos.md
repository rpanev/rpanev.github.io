---
id: 270
title: 'Инсталация на security updates използвайки  yum в CentOS'
date: 2013-11-10T16:35:26+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=270
permalink: '/%d0%b8%d0%bd%d1%81%d1%82%d0%b0%d0%bb%d0%b0%d1%86%d0%b8%d1%8f-%d0%bd%d0%b0-security-updates-%d0%b8%d0%b7%d0%bf%d0%be%d0%bb%d0%b7%d0%b2%d0%b0%d0%b9%d0%ba%d0%b8-yum-in-centos.html'
tie_views:
  - "167"
categories:
  - code
  - Linux
---
Инсталирайте плъгина 

<pre>yum install yum-plugin-security
</pre>

За да проверите за security updates напише те :

<pre>yum --security update
</pre>

Това е всичко 🙂 Може да ползвате и следните команди :

<pre>yum updateinfo summary
yum updateinfo list security
yum updateinfo list available
yum updateinfo list bugzillas</pre>