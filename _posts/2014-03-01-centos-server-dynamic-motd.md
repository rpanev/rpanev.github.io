---
id: 456
title: centos server dynamic motd
date: 2014-03-01T00:00:28+00:00
author: panev
layout: post
categories:
  - bash
  - linux
---
Скрипта може да се използва както за MOTD (message of the day) така и като bash скрипт с който да изкарвате моментна статистика на машината си 🙂

{% highlight bash %}
#!/bin/bash
#Server Status Script
#Last updated: March - 2014
#http://rpanev.pro
#CPU info
cpumodel=`cat /proc/cpuinfo | head -20 | grep "model name"| awk '{print $4, $5, $6, $7, $8, $10 }'`
core0=`sensors -u | head -11 |grep "temp2_input"| awk '{print $2 }' |awk '{printf("%d\n",$1 + 0.5);}'`
core1=`sensors -u | head -20 |grep "temp3_input"| awk '{print $2 }' |awk '{printf("%d\n",$1 + 0.5);}'`
core2=`sensors -u | head -30 |grep "temp4_input"| awk '{print $2 }' |awk '{printf("%d\n",$1 + 0.5);}'`
core3=`sensors -u | head -40 |grep "temp4_input"| awk '{print $2 }' |awk '{printf("%d\n",$1 + 0.5);}'`
Load1=`cat /proc/loadavg | awk {'print $1'}`
Load5=`cat /proc/loadavg | awk {'print $2'}`
Load15=`cat /proc/loadavg | awk {'print $3'}`
#Users info
user=`whoami`
psa=`ps -Afl | wc -l`
psu=`ps U $user h | wc -l`
#System update need
yum=`yum list updates -q | grep -vc "Updated Packages"`
#Uptime
btime=`last -x | grep reboot | tail -n 1 | awk '{print $1, $2, $6, $7, $8}'`
uptime=`cat /proc/uptime | cut -f1 -d.`
upDays=$((uptime/60/60/24))
upHours=$((uptime/60/60%24))
upMins=$((uptime/60%60))
upSecs=$((uptime%60))
#RAM
mfr=`free -m | head -n 2 | tail -n 1 | awk {'print $4'}`
mfc=`free -m | head -n 3 | tail -n 1 | awk {'print $3'}`
swap=`free -m | tail -n 1 | awk {'print $3'}`
#Disk
raid=`cat /proc/mdstat |grep Personalities`
disk=`df -h | awk '{ a = $4 } END { print a }'`
echo -e "
                       \e[96mSystem Server Status
					   
\e[39m- \e[31mServer Name                  \e[35m= \e[92m`hostname`
\e[39m- \e[31mPublic IP                    \e[35m= \e[92m`dig +short myip.opendns.com @resolver1.opendns.com`
\e[39m- \e[31mOS Version                   \e[35m= \e[92m`cat /etc/redhat-release`
\e[39m- \e[31mPlatform                     \e[35m= \e[92m`uname -orpi`
\e[39m- \e[31mCPU model                    \e[35m= \e[92m$cpumodel
\e[39m- \e[31mCPU Usage                    \e[35m= \e[92m$Load1 1 min \e[93m>> \e[92m$Load5 5 min \e[93m>> \e[92m$Load15 15 min
\e[39m- \e[31mUsers logged                 \e[35m= \e[92mCurrently `users | wc -w` users logged on
\e[39m- \e[31mCurrent user                 \e[35m= \e[92m$user
\e[39m- \e[31mSystem boot                  \e[35m= \e[92m$btime
\e[39m- \e[31mSystem Uptime                \e[35m= \e[92m$upDays days $upHours hours $upMins minutes $upSecs seconds
\e[39m- \e[31mSystem Update                \e[35m= \e[31m$yum \e[92mpackages can be updated
\e[39m- \e[31mMemory free (real)           \e[35m= \e[92m$mfr Mb
\e[39m- \e[31mMemory free (cache)          \e[35m= \e[92m$mfc Mb
\e[39m- \e[31mSwap in use                  \e[35m= \e[92m$swap Mb
\e[39m- \e[31mProcesses                    \e[35m= \e[92mYou are running $psu of $psa processes
\e[39m- \e[31mRAID                         \e[35m= \e[92m$raid
\e[39m- \e[31mDisk Space Used              \e[35m= \e[92m$disk
\e[39m- \e[31mCPU Temperature              \e[35m= \e[92mCore1 $core0 °C ; Core2 $core1 °C ; Core3 $core2 °C ; Core4 $core3 °C ;
\e[36m 
" > /etc/motd
#exit 0

{% endhighlight %}

За да ползвате скрипта като обикновен bash сложете коментар пред 

{% highlight bash %}> /etc/motd{% endhighlight %}

и махнете коментара пред 

{% highlight bash %}exit 0{% endhighlight %}