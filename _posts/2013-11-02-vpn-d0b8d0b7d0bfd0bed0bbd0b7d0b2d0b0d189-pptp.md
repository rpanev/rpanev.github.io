---
id: 236
title: VPN използващ PPTP
date: 2013-11-02T16:18:00+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=236
permalink: '/vpn-%d0%b8%d0%b7%d0%bf%d0%be%d0%bb%d0%b7%d0%b2%d0%b0%d1%89-pptp.html'
tie_views:
  - "207"
categories:
  - Linux
---
[<img src="http://panevinfo.eu/blog//wp-content/uploads/2013/11/vpn.png" alt="vpn" width="256" height="256" class="alignleft size-full wp-image-448" srcset="https://www.panevinfo.eu/wp-content/uploads/2013/11/vpn.png 256w, https://www.panevinfo.eu/wp-content/uploads/2013/11/vpn-150x150.png 150w" sizes="(max-width: 256px) 100vw, 256px" />](http://panevinfo.eu/blog//wp-content/uploads/2013/11/vpn.png)  
&#8211; Тествана на CentOS 6.4  
Първа стъпка е да инсталираме сървиса. Ако пробвате с командата yum install pptpd ще останете леко разочаровани от факта, че я няма : 

<pre>[root@eragon ~]# yum install pptpd
Loaded plugins: fastestmirror, security
Loading mirror speeds from cached hostfile
 * base: mirrors.neterra.net
 * epel: mirrors.neterra.net
 * extras: mirrors.neterra.net
 * updates: mirrors.neterra.net
Setting up Install Process
No package pptpd available.
Error: Nothing to do
</pre>

<!--more-->

  
За това просто напишете в конзолата :

<pre>rpm -i http://poptop.sourceforge.net/yum/stable/rhel6/pptp-release-current.noarch.rpm</pre>

След което пишем :

<pre>yum -y install pptpd</pre>

_Забележка :-y слагаме с идеята да не ти пита дали искаме да инсталираме. Така автоматично инсталира желаният пакет без въпроси 🙂_

След като се инсталира отворете с текстов редактор файла **pptpd.conf**, който се намира в **/etc/pptpd.conf**.

<pre>[root@eragon ~]# vim /etc/pptpd.conf</pre>

Добавете най-долу следните редове :

<pre># (Recommended)
#localip 192.168.0.1
#remoteip 192.168.0.234-238,192.168.0.245
# or
#localip 192.168.0.234-238,192.168.0.245
#remoteip 192.168.1.234-238,192.168.1.245
localip 10.0.0.1
remoteip 10.0.0.100-200
</pre>

_Забележка : 10.0.0.1 и 10.0.0.100-200 са дадени за пример, може да ги смените както ви харесва на вас_

След като сме описали мрежата трябва да направим потребител и парола за връзка с VPN сървъра ни. Отворете следният файл /etc/ppp/chap-secrets и добавете желаните потребители:

<pre># Secrets for authentication using CHAP
# client        server  secret                  IP addresses
   panev        pptpd   12345                    *
</pre>

client &#8211; потребителско име;  
server – pptpd оказваме името на сървиса;  
secret – парола;  
IP addresses – ако поставим * (звездичка) ще има достът от всяко IP, ако зададем IP достъп до VPN сървъра ни ще има само от описаното IP.

Трябва да добавим DNS Server в следният файл /etc/ppp/pptpd-options.

<pre>ms-dns 8.8.8.8
ms-dns 8.8.4.4
</pre>

_Забележка : За примера съм сложил DNS на Google_

Рестартирайте ppptd сървиса :

<pre>[root@eragon ~]# service pptpd restart</pre>

Ще получите следният резултат в конзолата:

<pre>Shutting down pptpd:                                       [  OK  ]
Starting pptpd:                                            [  OK  ]
</pre>

За да се уверите, че вашия VPN сървър е пуснат и работи напишете следната команда:

<pre>[root@eragon ~]# netstat -alpn | grep :1723
tcp        0      0 0.0.0.0:1723                0.0.0.0:*                   LISTEN      10723/pptpd
</pre>

Важно е да се направи forwarding на PPTP сървъра. Отворете /etc/sysctl.conf и добавете:

<pre>[root@eragon ~]# vi /etc/sysctl.conf</pre>

<pre>net.ipv4.ip_forward = 1</pre>

Обновете настройките на sysctl.conf 

<pre>[root@eragon ~]# sysctl -p</pre>

Добавим и няколко реда в iptables :

<pre>iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE && iptables-save </pre>