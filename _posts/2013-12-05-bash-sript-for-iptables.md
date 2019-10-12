---
id: 281
title: bash sript for iptables
date: 2013-12-05T13:33:58+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=281
permalink: /bash-sript-for-iptables.html
tie_views:
  - "196"
categories:
  - code
  - Linux
---
В работа се наложи да пусна <a href="http://www.asterisk.org/" title="asterisk" target="_blank">asterisk</a> и ми се наложи да добавям много IP, реших да си съкратя малко повторенията 🙂 Скрипта не е кой, знае какво но ми спести доста време 🙂  
<!--more-->

<pre>#!/bin/bash
args=("$@")
IPT="/sbin/iptables"                                    #Path to iptables
IPTS="service iptables save"
IF="eth2"                                               #your network interface
Restart="service iptables restart"

#Add your rules
$IPT -I INPUT -p udp -m udp -s $@ --dport 10000:20000 -j ACCEPT -i $IF
$IPT -I INPUT -p udp -m udp -s $@ --dport 5062 -j ACCEPT -i $IF
$IPT -I INPUT -p tcp -m tcp -s $@ --dport 22 -j ACCEPT -i $IF

#Save you rules
echo "Save you rules"
$IPTS

#Restart iptables
echo "Please wait to restart your iptables"
$Restart

exit 0

</pre>

За да пуснете скрипта напишете следната команда :

<pre>chmod +x iptab.sh</pre>

<pre>./iptab.sh IP </pre>