---
id: 1311
title: 'Малка модификация на скипта за iptables  за Asterisk'
date: 2013-12-11T13:05:57+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=329
permalink: '/%d0%bc%d0%b0%d0%bb%d0%ba%d0%b0-%d0%bc%d0%be%d0%b4%d0%b8%d1%84%d0%b8%d0%ba%d0%b0%d1%86%d0%b8%d1%8f-%d0%bd%d0%b0-%d1%81%d0%ba%d0%b8%d0%bf%d1%82%d0%b0-%d0%b7%d0%b0-iptables-%d0%b7%d0%b0-asterisk.html'
tie_views:
  - "163"
categories:
  - code
  - Linux
---
<pre>#!/bin/bash
###############################################
#chmod +x iptab.sh #
#./iptab.sh IP comment #
#example ./iptab.sh 192.168.1.1 Panev #
#example output ****tcp dpt:22 /* Panev */ #
###############################################
args=("$1") #arguments
args=("$2") #arguments
IPT="/sbin/iptables" #Path to iptables
IPTS="service iptables save"
IF="eth2" #your network interface
Restart="service iptables restart"

#Add your rules
#echo "IPv4" - in development
$IPT -I INPUT -p udp -m udp -s $1 --dport 10000:20000 -j ACCEPT -i $IF -m comment --comment "$2"
$IPT -I INPUT -p udp -m udp -s $1 --dport 5060:5062 -j ACCEPT -i $IF -m comment --comment "$2"
$IPT -I INPUT -p tcp -m tcp -s $1 --dport 22 -j ACCEPT -i $IF -m comment --comment "$2"

#Save your rules
echo "Save you rules"
$IPTS

#Restart iptables
echo "Please wait to restart your iptables"
$Restart
#Show iptables rules and comment
echo "Show iptables rules"
$IPT -L INPUT -n --line-numbers

#View and remove rulz
echo "Please type iptables -L INPUT -n --line-numbers or iptables -D INPUT num to remove rules"


</pre>