---
id: 263
title: server status motd
date: 2013-11-10T15:51:32+00:00
author: panev
layout: post
categories:
  - bash
  - FreeBSD
  - Linux
---
Ще направим прост bash скрипт който ще показва статуса на сървъра в motd (message of the day)

Създайте следният файл :

{% highlight shell %}
vim /usr/local/bin/systemstats.sh
{% endhighlight %}


Поставете следният код в него :

{% highlight bash %}
#!/bin/bash
#
# Server Status Script

CPUTIME=$(ps -eo pcpu | awk 'NR>1' | awk '{tot=tot+$1} END {print tot}')
CPUCORES=$(cat /proc/cpuinfo | grep -c processor)
UP=$(echo `uptime` | awk '{ print $3 " " $4 }')
echo "
System Status
Updated: `date`

- Server Name               = `hostname`
- Public IP                 = `dig +short myip.opendns.com @resolver1.opendns.com`
- OS Version                = `cat /etc/redhat-release`
- Load Averages             = `cat /proc/loadavg`
- System Uptime             = `echo $UP`
- Platform Data             = `uname -orpi`
- CPU Usage (average)       = `echo $CPUTIME / $CPUCORES | bc`%
- Memory free (real)        = `free -m | head -n 2 | tail -n 1 | awk {'print $4'}` Mb
- Memory free (cache)       = `free -m | head -n 3 | tail -n 1 | awk {'print $3'}` Mb
- Swap in use               = `free -m | tail -n 1 | awk {'print $3'}` Mb
- Disk Space Used           = `df -h | awk '{ a = $4 } END { print a }'`
" > /etc/motd
# End of script
{% endhighlight %}

Направете скрипта изпълним със следната команда :

{% highlight bash %}chmod +x /usr/local/bin/systemstats.sh
{% endhighlight %}

Добавете скрипта в crontab така, че на всеки 5 минути да се изпълнява:

{% highlight shell %}
vim  /etc/crontab
{% endhighlight %}

Добавете следният ред:

{% highlight bash %}
# Status Script
*/5 * * * * root /usr/local/bin/systemstats.sh
{% endhighlight %}

Ако не ви се чака 5 мин. изпълнете скрипта :

{% highlight bash %}
/usr/local/bin/systemstats.sh
{% endhighlight %}

Влезте на машината си от ново и би следвало да получите следното :

{% highlight bash %}
System Status
Updated: Sun Nov 10 15:45:01 EET 2013

- Server Name               = eragon
- Public IP                 = вашето IP от което излизате
- OS Version                = CentOS release 6.4 (Final)
- Load Averages             = 0.00 0.00 0.00 2/248 12910
- System Uptime             = 2:16, 2
- Platform Data             = версия на ядрото GNU/Linux
- CPU Usage (average)       = 0%
- Memory free (real)        = 4136 Mb
- Memory free (cache)       = 276 Mb
- Swap in use               = 0 Mb
- Disk Space Used           = 47G

{% endhighlight %}