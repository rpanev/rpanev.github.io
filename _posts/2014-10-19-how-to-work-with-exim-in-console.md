---
id: 761
title: How to work with exim in console
date: 2014-10-19T13:50:27+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog/?p=761
permalink: /how-to-work-with-exim-in-console.html
wp_review_location:
  - bottom
wp_review_desc_title:
  - Summary
wp_review_color:
  - '#1e73be'
wp_review_fontcolor:
  - '#555555'
wp_review_bgcolor1:
  - '#e7e7e7'
wp_review_bgcolor2:
  - '#ffffff'
wp_review_bordercolor:
  - '#e7e7e7'
tie_views:
  - "4745"
image: /wp-content/uploads/2014/10/exim.png
categories:
  - FreeBSD
  - Linux
---
Hello, I will show basic commands for working with exim in console 🙂

Log file is located in /var/log/exim_mainlog

Email information when sending and receiving :

Real time tracking entries exim_mainlog:

<pre>tailf /var/log/exim_mainlog
</pre>

Look at the last 500 entries in exim_mainlog:

<pre>tail -500 /var/log/exim_mainlog
</pre>

To delete messages in queue older than 5 days (sometimes stand there and do not want to go). The figure comes from (86,400 seconds is one day * 5 days = 432,000 seconds):

<pre
>exiqgrep -o 432000 -i | xargs exim -Mrm
</pre>

To view the headers of the message, sometimes it is useful if you need more information about any sent, received or not sent, not received message: 

<pre>exim -Mvh
</pre>

To view the contents of the letter: 

<pre>exim -Mvb
</pre>

To view the logs for the letter:

<pre>exim -Mvl
</pre>

Everyone goes to delete mails in the queue that have the following string in your content: STRING:

<pre>grep -lr 'STRING' /var/spool/exim/input/ | sed -e 's/^.*\/\([a-zA-Z0-9-]*\)-[DH]$/\1/g' | xargs exim -Mrm
</pre>

To delete many emails sent from: root@panevinfo.eu. Working with logs (exim_mainlog for received and sent >> / var / log / maillog for me and Razlog pop3, imap):

<pre>xiqgrep -i -f '' | xargs exim -Mrm
</pre>

To check for sending a letter from a certain IP: 

<pre>exigrep '&lt;= .* \[127.0.0.1\] ' /var/log/exim_mainlog
</pre>

To check for mails sent to a specific IP address: 

<pre>exigrep '=> .* \[127.0.0.1\]' /path/to/exim_log
</pre>

How to see all the emails that were returned in response to already sent mail: 

<pre>exigrep '=> .*email@email.com' /var/log/exim_mainlog | fgrep '&lt;='
</pre>

To see the letters sent and received by email@email.com: 

<pre>tail -5000 /var/log/exim_mainlog| grep "email@email.com"
</pre>

To see the letters sent by email@email.com: 

<pre>tail -5000 /var/log/exim_mainlog| grep "email@email.com" | grep "&lt;="
</pre>

To see the letters received from email@emaila.com: 

<pre>tail -5000 /var/log/exim_mainlog| grep "email@email.com" | grep "=>"
</pre>

You can check how a message is delivered:

<pre>exim -bt email@email.com
</pre>

for more information : <a href="http://www.exim.org/docs.html" title="exim" target="_blank">exim</a>