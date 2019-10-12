---
id: 114
title: 'EdgeRouter X L2TP Server'
date: 2017-04-13T17:26:57+00:00
author: Panev
layout: post
image: /static/img/_posts/edgerouter-x.jpg
categories:
  - linux
  - debian
tags:
  - Accept
  - EdgeRouter
  - Firewall
  - l2tp
  - ubnt
  - UDP
---
<center>
<img src="https://raw.githubusercontent.com/rpanev/rpanev.github.io/master/static/img/_posts/edgerouter-x.jpg" alt="EdgeRouter X" />
</center>

EdgeRouter X L2TP Server. Купих си тия дни ново рутерче <a href="https://www.ubnt.com/edgemax/edgerouter-x/" target="_blank" rel="noopener noreferrer">EdgeRouter X</a>. Понеже съм с macbook pro и ми се налага да ползвам vpn се наложи да конфигурирам рутерчето да работи с <a href="https://en.wikipedia.org/wiki/Layer_2_Tunneling_Protocol" target="_blank" rel="noopener noreferrer">VPN L2TP</a>.

Това което направих е да се логна чрез SSH в рутера :  
{% highlight shell %}
ssh user@192.168.99.1
Welcome to EdgeOS
{% endhighlight %}

след като се логнах в рутера изпълних следните команди:

`configure`  
влизаме в конфигурационне режим на рутера

Има няколко възможности за конфигурация според това на кои порт е интернета и каква е конфигурацията му DHCP, STATIC или PPPoE. В моя случай използвам eth0 и STATIC IP и ще споделя как съм го конфигурирал

`set vpn l2tp remote-access outside-address STATIC_IP`  
* заменяте STATIC_IP с вашият ip адрес, който може да проверите като посетите сайта http://ping.eu

Задавам си рейндж от кой до кой IP адрес да раздава ip адреси  

`set vpn l2tp remote-access client-ip-pool start 192.168.100.101`
`set vpn l2tp remote-access client-ip-pool stop 192.168.100.110` 

Избираме интерфейса на който е конфигуриран интернета  
`set vpn ipsec ipsec-interfaces interface eth0`

Задаване на режим на удостоверяване IPsec  
`set vpn l2tp remote-access ipsec-settings authentication mode pre-shared-secret`

Задаване на secret phrase.  
`set vpn l2tp remote-access ipsec-settings authentication pre-shared-secret "secret phrase"`

`set vpn l2tp remote-access authentication mode local`

Конфигурация на потребител и парола за L2TP сървъра  
`set vpn l2tp remote-access authentication local-users username USER password USERPASSWORD`

`set vpn l2tp remote-access mtu 1492`

Незабравяме да заложим и DNS аз изпозлвам тези на google  

`set vpn l2tp remote-access dns-servers server-1 8.8.8.8`
`set vpn l2tp remote-access dns-servers server-2 8.8.4.4`
 
Прилагане на промените  
`commit`

Може да разгледаме конфигурацията която направихме, чрез командата  
`show vpn l2tp remote-access`

Записваме настройките  
`save`

След тези настройки е необходимо да достъпим уеб интерфейса на приложението за да конфигурираме и Firewall.  
`Firewall/NAT таба > Firewall Policies > WAN_LOCAL > Actions > Edit`

Трябва да създадем две паравила L2TP и ESP  

{% highlight shell %}
Basic:
  • Description:  Allow L2TP
  • Check Enable.
  • Action:  Accept.
  • Protocol:  UDP
Destination:
  • Port:  500,1701,4500
{% endhighlight %}

Записваме настройките, чрез бутона save

{% highlight shell %}
Basic Tab:
  • Description:  Allow ESP
  • Check Enable.
  • Action:  Accept.
  • Protocol: Choose a protocol by name:  esp
{% endhighlight %}
Записваме настройките, чрез бутона save

Това е, вече имаме конфигуриран 

## EdgeRouter X L2TP Server

на нашият рутер. Остава да конфигурирате вашият компютър да изпозлва VPN сървъра.