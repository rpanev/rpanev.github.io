---
id: 23
title: 'CentOS: Clear cached memory'
date: 2014-04-21T12:38:14+00:00
author: panev
layout: post
categories:
  - Linux
---
Use the following command to clear you cached memory:

{% highlight bash %}
root@linux [~]# echo 1 > /proc/sys/vm/drop_caches 
{% endhighlight %}
<img src="https://mirkov.info/wp-content/uploads/2014/04/cache.png" width="750" height="277" class="alignnone" /> 

by <a href="https://mirkov.info/centos-clear-cached-memory/" target="_blank">Miroslav Mirkov</a>