---
id: 289
title: Обновяване на CentOS 6.4 до 6.5
date: 2013-12-06T23:22:21+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=289
permalink: '/%d0%be%d0%b1%d0%bd%d0%be%d0%b2%d1%8f%d0%b2%d0%b0%d0%bd%d0%b5-%d0%bd%d0%b0-centos-6-4-%d0%b4%d0%be-6-5.html'
tie_views:
  - "185"
categories:
  - Linux
---
Проверете текущата версия

<pre>cat /etc/redhat-release
</pre>

Примерно е:

<pre>CentOS release 6.4 (Final)
</pre>

За да се появят всички налични пакети и обновления въведете следните команди:

<pre>yum clean all
yum check-update

</pre>

След като ви покаже, че има обновления и нови пакети напишете :

<pre>yum update -y
</pre>

По желание може да reboot машината, аз лично и двете не съм 🙂 За да проверите дали всичко е наред след update ползвайте следните команди :

<pre>netstat -tulpn
tail -f /var/log/messages
tail -f /path/to/log/files
ps aux | less
ps aux | egrep 'httpd|mysql'
pgrep 'my_app'

</pre>