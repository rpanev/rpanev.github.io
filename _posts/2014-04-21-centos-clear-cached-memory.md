---
id: 670
title: 'CentOS: Clear cached memory'
date: 2014-04-21T12:38:14+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=670
permalink: /centos-clear-cached-memory.html
tie_views:
  - "167"
categories:
  - Linux
---
Use the following command to clear you cached memory:

root@linux [~]# echo 1 > /proc/sys/vm/drop_caches  
<img src="https://mirkov.info/wp-content/uploads/2014/04/cache.png" width="750" height="277" class="alignnone" /> 

by <a href="https://mirkov.info/centos-clear-cached-memory/" target="_blank">Miroslav Mirkov</a>