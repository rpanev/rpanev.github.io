---
id: 617
title: 'How to install ownCloud &#8211; Cloud Storage'
date: 2014-04-05T13:03:53+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=617
permalink: /how-to-install-owncloud-cloud-storage.html
tie_views:
  - "181"
image: /wp-content/uploads/2014/04/owncloud.jpg
categories:
  - Linux
---
Напишете в конзола следният ред :

<pre>wget http://download.opensuse.org/repositories/isv:ownCloud:community/CentOS_CentOS-6/isv:ownCloud:community.repo -P /etc/yum.repos.d 
</pre>

След това просто напишете :

<pre>yum -y install owncloud
</pre>

И рестартирате httpd сървиса

<pre>/etc/rc.d/init.d/httpd restart 
</pre>