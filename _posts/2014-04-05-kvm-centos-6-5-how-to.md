---
id: 21
title: 'KVM CentOS 6.5 how to'
date: 2014-04-05T23:56:23+00:00
author: panev
layout: post
categories:
  - Linux
---
KVM (Kernel-базирани Virtual Machine) + QEMU. Това означава, че процесора на компютъра ви трябва да поддържаIntel VT или AMD-V.  
Първа стъпка е да инсталираме KVM. Това става като напишете в конзола :

{% highlight shell %}yum groupinstall "Virtualisation Tools" "Virtualization Platform"
yum install kvm qemu-kvm python-virtinst libvirt libvirt-python virt-manager libguestfs-tools bridge-utils 
{% endhighlight %}

Проверка на модула дали е зареден :

{% highlight bshellash %}┌(eragon)─(12:38 AM Sun Apr 06)
└─(~)─(32 files, 4.4Mb)─> ~
 #  lsmod | grep kvm
kvm_intel              54285  3
kvm                   332980  1 kvm_intel
{% endhighlight %}

Стартирайте KVM

{% highlight shell %}┌(eragon)─(12:40 AM Sun Apr 06)
└─(~)─(32 files, 4.4Mb)─> ~
# etc/rc.d/init.d/libvirtd start 
Starting libvirtd daemon:      [ОК]
chkconfig libvirtd on
{% endhighlight %}

{% highlight shell %}┌(eragon)─(12:45 AM Sun Apr 06)
└─(~)─(32 files, 4.4Mb)─> ~
 #  cd /etc/sysconfig/network-scripts
{% endhighlight %}

Създайте интерфейс ifcfg-br0.

{% highlight shell %}cp ifcfg-eth0 ifcfg-br0 
{% endhighlight %}

Редактирайте първо ifcfg-br0.

{% highlight shell %}DEVICE=br0             #change
ONBOOT=yes
BOOTPROTO=none
IPADDR=192.168.1.11
NETMASK=255.255.255.0
TYPE=Bridge            #change
GATEWAY=192.168.1.1
DNS1=192.168.1.1
DNS2=8.8.8.8
IPV6INIT=no
USERCTL=no
{% endhighlight %}

След това ifcfg-eth0.

{% highlight shell %}DEVICE=eth0
ONBOOT=yes
BOOTPROTO=none
IPADDR=192.168.1.11
NETMASK=255.255.255.0
TYPE=Ethernet
GATEWAY=192.168.1.1
DNS1=192.168.1.1
DNS2=8.8.8.8
IPV6INIT=no
USERCTL=no
BRIDGE=br0             # add
{% endhighlight %}

Рестарт на network service 

{% highlight shell %}/etc/rc.d/init.d/network restart {% endhighlight %}

Вижте мрежовите настройки:

{% highlight shell %}┌(eragon)─(12:48 AM Sun Apr 06)
└─(~)─(32 files, 4.4Mb)─> ~
 # ifconfig
br0       Link encap:Ethernet  HWaddr 00:24:1D:CA:8D:AB
          inet addr:192.168.1.11  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::224:1dff:feca:8dab/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:684600 errors:0 dropped:0 overruns:0 frame:0
          TX packets:744001 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:169815222 (161.9 MiB)  TX bytes:877486790 (836.8 MiB)

eth0      Link encap:Ethernet  HWaddr 00:24:1D:CA:8D:AB
          inet6 addr: fe80::224:1dff:feca:8dab/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:121369689 errors:0 dropped:0 overruns:0 frame:0
          TX packets:103565596 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:75420646329 (70.2 GiB)  TX bytes:22827277783 (21.2 GiB)
          Interrupt:20 Memory:f3100000-f3120000

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:318630588 errors:0 dropped:0 overruns:0 frame:0
          TX packets:318630588 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:32937557613 (30.6 GiB)  TX bytes:32937557613 (30.6 GiB)

ppp0      Link encap:Point-to-Point Protocol
          inet addr:10.0.0.1  P-t-P:10.0.0.100  Mask:255.255.255.255
          UP POINTOPOINT RUNNING NOARP MULTICAST  MTU:1396  Metric:1
          RX packets:3844 errors:0 dropped:0 overruns:0 frame:0
          TX packets:4659 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:3
          RX bytes:477487 (466.2 KiB)  TX bytes:1243627 (1.1 MiB)

vnet0     Link encap:Ethernet  HWaddr FE:54:00:C0:30:F7
          inet6 addr: fe80::fc54:ff:fec0:30f7/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:51857 errors:0 dropped:0 overruns:0 frame:0
          TX packets:100001 errors:0 dropped:0 overruns:258 carrier:0
          collisions:0 txqueuelen:500
          RX bytes:4028806 (3.8 MiB)  TX bytes:138216299 (131.8 MiB)
{% endhighlight %}

Задължително добавете и:

{% highlight shell %}echo “-I FORWARD -m physdev --physdev-is-bridged -j ACCEPT” > /etc/sysconfig/iptables-forward-bridged{% endhighlight %}

иначе има вероятност да се чудите защо нямате мрежа на виртуалните машинки. :)))