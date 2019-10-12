---
id: 738
title: How To Tail Multiple Files on UNIX / Linux Console
date: 2014-09-01T10:09:21+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog/?p=738
permalink: /how-to-tail-multiple-files-on-unix-linux-console.html
tie_views:
  - "729"
image: /wp-content/uploads/2014/09/log.png
categories:
  - FreeBSD
  - Linux
---
The tail command is one of the best tool to view log files in a real time using tail -f /path/to/log.file syntax on a Unix-like systems. The program MultiTail lets you view one or multiple files like the original tail program. The difference is that it creates multiple windows on your console (with ncurses). This is one of those dream come true program for UNIX sys admin job. You can browse through several log files at once and do various operations like search for errors and more.  
<!--more-->

Install MultiTail On a Linux

Type the following command under Debian / Ubuntu Linux:

<pre>sudo apt-get update
sudo apt-get install multitail</pre>

RHEL/CentOS/Fedora/ :

<pre>yum install multitail</pre>

If you are using FreeBSD 10, enter:

<pre>pkg upgrade
pkg install multitail
</pre>

How To View Multiple Files 

To view /var/log/messages and /var/log/auth.log, enter:

<pre>multitail /var/log/messages /var/log/auth.log </pre>

How do I display 3 logfiles in 2 columns?

<pre>multitail -s 2 /var/log/secure /var/log/fail2ban.log /var/log/messages </pre>

[<img src="http://panevinfo.eu/blog/wp-content/uploads/2014/09/log.png" alt="log" width="1366" height="738" class="alignleft size-full wp-image-741" srcset="https://www.panevinfo.eu/wp-content/uploads/2014/09/log.png 1366w, https://www.panevinfo.eu/wp-content/uploads/2014/09/log-300x162.png 300w, https://www.panevinfo.eu/wp-content/uploads/2014/09/log-768x415.png 768w, https://www.panevinfo.eu/wp-content/uploads/2014/09/log-1024x553.png 1024w" sizes="(max-width: 1366px) 100vw, 1366px" />](http://panevinfo.eu/blog/wp-content/uploads/2014/09/log.png)

multitail has many other useful options. Please read man page for further details:

<pre>man multitail</pre>

[sz-gplus-post]