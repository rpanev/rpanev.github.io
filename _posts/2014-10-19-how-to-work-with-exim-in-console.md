---
id: 761
title: How to work with exim in console
date: 2014-10-19T13:50:27+00:00
author: panev
layout: post
categories:
  - FreeBSD
  - Linux
---
Hello, I will show basic commands for working with exim in console 🙂

Log file is located in /var/log/exim_mainlog

Email information when sending and receiving :

Real time tracking entries exim_mainlog:

{% highlight shell %} tailf /var/log/exim_mainlog
{% endhighlight %}

Look at the last 500 entries in exim_mainlog:

{% highlight shell %}tail -500 /var/log/exim_mainlog
{% endhighlight %}

To delete messages in queue older than 5 days (sometimes stand there and do not want to go). The figure comes from (86,400 seconds is one day * 5 days = 432,000 seconds):

{% highlight shell %}
exiqgrep -o 432000 -i | xargs exim -Mrm
{% endhighlight %}

To view the headers of the message, sometimes it is useful if you need more information about any sent, received or not sent, not received message: 

{% highlight shell %}exim -Mvh
{% endhighlight %}

To view the contents of the letter: 

{% highlight shell %}exim -Mvb
{% endhighlight %}

To view the logs for the letter:

{% highlight shell %}exim -Mvl
{% endhighlight %}

Everyone goes to delete mails in the queue that have the following string in your content: STRING:

{% highlight shell %}grep -lr 'STRING' /var/spool/exim/input/ | sed -e 's/^.*\/\([a-zA-Z0-9-]*\)-[DH]$/\1/g' | xargs exim -Mrm
{% endhighlight %}

To delete many emails sent from: root@panevinfo.eu. Working with logs (exim_mainlog for received and sent >> / var / log / maillog for me and Razlog pop3, imap):

{% highlight shell %}xiqgrep -i -f '' | xargs exim -Mrm
{% endhighlight %}

To check for sending a letter from a certain IP: 

{% highlight shell %}exigrep '&lt;= .* \[127.0.0.1\] ' /var/log/exim_mainlog
{% endhighlight %}

To check for mails sent to a specific IP address: 

{% highlight shell %}exigrep '=> .* \[127.0.0.1\]' /path/to/exim_log
{% endhighlight %}

How to see all the emails that were returned in response to already sent mail: 

{% highlight shell %}exigrep '=> .*email@email.com' /var/log/exim_mainlog | fgrep '&lt;='
{% endhighlight %}

To see the letters sent and received by email@email.com: 

{% highlight shell %}tail -5000 /var/log/exim_mainlog| grep "email@email.com"
{% endhighlight %}

To see the letters sent by email@email.com: 

{% highlight shell %}tail -5000 /var/log/exim_mainlog| grep "email@email.com" | grep "&lt;="
{% endhighlight %}

To see the letters received from email@emaila.com: 

{% highlight shell %}tail -5000 /var/log/exim_mainlog| grep "email@email.com" | grep "=>"
{% endhighlight %}

You can check how a message is delivered:

{% highlight shell %}exim -bt email@email.com
{% endhighlight %}

for more information : <a href="http://www.exim.org/docs.html" title="exim" target="_blank">exim</a>