---
id: 27
title: How To Tail Multiple Files on UNIX / Linux Console
date: 2014-09-01T10:09:21+00:00
author: panev
layout: post
categories:
  - FreeBSD
  - Linux
---
The tail command is one of the best tool to view log files in a real time using tail -f /path/to/log.file syntax on a Unix-like systems. The program MultiTail lets you view one or multiple files like the original tail program. The difference is that it creates multiple windows on your console (with ncurses). This is one of those dream come true program for UNIX sys admin job. You can browse through several log files at once and do various operations like search for errors and more.  

Install MultiTail On a Linux

Type the following command under Debian / Ubuntu Linux:

{% highlight shell %}sudo apt-get update
sudo apt-get install multitail{% endhighlight %}

RHEL/CentOS/Fedora/ :

{% highlight shell %}yum install multitail{% endhighlight %}

If you are using FreeBSD 10, enter:

{% highlight shell %}pkg upgrade
pkg install multitail
{% endhighlight %}

How To View Multiple Files 

To view /var/log/messages and /var/log/auth.log, enter:

{% highlight shell %}multitail /var/log/messages /var/log/auth.log {% endhighlight %}

How do I display 3 logfiles in 2 columns?

{% highlight shell %}multitail -s 2 /var/log/secure /var/log/fail2ban.log /var/log/messages {% endhighlight %}

[<img src="http://panevinfo.eu/blog/wp-content/uploads/2014/09/log.png" alt="log" width="1366" height="738" class="alignleft size-full wp-image-741" srcset="https://www.panevinfo.eu/wp-content/uploads/2014/09/log.png 1366w, https://www.panevinfo.eu/wp-content/uploads/2014/09/log-300x162.png 300w, https://www.panevinfo.eu/wp-content/uploads/2014/09/log-768x415.png 768w, https://www.panevinfo.eu/wp-content/uploads/2014/09/log-1024x553.png 1024w" sizes="(max-width: 1366px) 100vw, 1366px" />](http://panevinfo.eu/blog/wp-content/uploads/2014/09/log.png)

multitail has many other useful options. Please read man page for further details:

{% highlight shell %}man multitail{% endhighlight %}
