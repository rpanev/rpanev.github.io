---
id: 461
title: RAID status check for CentOS
date: 2014-03-01T00:27:13+00:00
author: panev
layout: post
categories:
  - bash
  - Linux
---
За тези, които искат бърз / елегантен начин за проверка на статуса на RAID на вашия диск в CentOS използвате тази команда 

{% highlight shell %}mdadm
{% endhighlight %}

Примерно :

{% highlight shell %}mdadm --detail /dev/md0
{% endhighlight %}

Ще получите нещо подобно 🙂

{% highlight shell %}
┌(eragon)─(12:19 AM Sat Mar 01)
└─(~)─(25 files, 4.3Mb)─> ~mdadm --detail /dev/md0
/dev/md0:
        Version : imsm
     Raid Level : container
  Total Devices : 2

Working Devices : 2


           UUID : 377b86c3:ec64624f:be308076:838b1d4d
  Member Arrays : /dev/md/RAID1

    Number   Major   Minor   RaidDevice

       0       8        0        -        /dev/sda
       1       8       16        -        /dev/sdb

┌(eragon)─(12:26 AM Sat Mar 01)
└─(~)─(25 files, 4.3Mb)─> ~

{% endhighlight %}