---
id: 461
title: RAID status check for CentOS
date: 2014-03-01T00:27:13+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=461
permalink: /raid-status-check-for-centos.html
tie_views:
  - "216"
image: /wp-content/uploads/2014/03/1361040118_4_FT113_raid-level-1.gif
categories:
  - code
  - Linux
---
За тези, които искат бърз / елегантен начин за проверка на статуса на RAID на вашия диск в CentOS използвате тази команда 

<pre>mdadm
</pre>

Примерно :

<pre>mdadm --detail /dev/md0
</pre>

Ще получите нещо подобно 🙂

<pre>┌(eragon)─(12:19 AM Sat Mar 01)
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

</pre>