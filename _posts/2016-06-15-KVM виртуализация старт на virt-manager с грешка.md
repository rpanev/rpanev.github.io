---
id: 122
title: KVM виртуализация старт на virt-manager с грешка
date: 2016-06-15T17:49:32+00:00
author: Panev
layout: post
#guid: https://www.rpanev.pro/?p=122
#permalink: '/virtualization/kvm-%d0%b2%d0%b8%d1%80%d1%82%d1%83%d0%b0%d0%bb%d0%b8%d0%b7%d0%b0%d1%86%d0%b8%d1%8f-%d1%81%d1%82%d0%b0%d1%80%d1%82-%d0%bd%d0%b0-virt-manager-%d1%81-%d0%b3%d1%80%d0%b5%d1%88%d0%ba%d0%b0.html'
image: /wp-content/uploads/2019/06/kvm2.jpg
categories:
  - kvm
  - cloud
tags:
  - D-Bus
  - dbus-uuidgen
  - KVM
  - machine-id
  - virt-manager
---
## KVM виртуализация старт на virt-manager с грешка

Днес се наложи да инсталирам на CentOS 6.8 KVM виртуализация и при стартиране на virt-manager ми изписа следната грешка 🙂

{% highlight bash %}
[root@localhost r.panev]# virt-manager
process 18532: D-Bus library appears to be incorrectly set up; failed to read machine uuid: Failed to open "/var/lib/dbus/machine-id": No such file or directory
See the manual page for dbus-uuidgen to correct this issue.
  D-Bus not built with -rdynamic so unable to print a backtrace
Aborted
{% endhighlight %}

И абортира 😉 Причината за въпросната грешка се дължи на **Eclipse Memory Analyzer** над сесията на X11 Forwarding през SSH

оказа се, че решението е просто да се изпълни командата 

{% highlight bash %}
dbus-uuidgen > /var/lib/dbus/machine-id
{% endhighlight %}

В случай, че нямате командата dbus-uuidgen е необходимо да я инсталирате посредством пакет менажера yum install dbus.