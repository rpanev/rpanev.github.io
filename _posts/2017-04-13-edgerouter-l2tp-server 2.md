---
id: 976
title: 'EdgeRouter &#8211; L2TP Server'
date: 2017-04-13T21:10:48+00:00
author: panev
layout: post
guid: https://www.panevinfo.eu/blog/?p=976
permalink: /edgerouter-l2tp-server.html
wp_review_color:
  - '#1e73be'
wp_review_fontcolor:
  - '#555555'
wp_review_desc_title:
  - Резюме
wp_review_location:
  - bottom
tie_sidebar_pos:
  - default
wp_review_user_review_type:
  - star
wp_review_bordercolor:
  - '#e7e7e7'
wp_review_bgcolor2:
  - '#ffffff'
wp_review_bgcolor1:
  - '#e7e7e7'
tie_views:
  - "1809"
tie_post_slider:
  - "1019"
image: /wp-content/uploads/2017/04/edgerouter-x.jpg
categories:
  - Linux
---
## EdgeRouter &#8211; L2TP Server

<figure id="attachment_981" aria-describedby="caption-attachment-981" style="width: 150px" class="wp-caption alignleft"><img src="https://www.panevinfo.eu/wp-content/uploads/2017/04/edgerouter-x-150x150.jpg" alt="edgerouter x" width="150" height="150" class="size-thumbnail wp-image-981" /><figcaption id="caption-attachment-981" class="wp-caption-text">edgerouter-x</figcaption></figure>  
Здравейте,

EdgeRouter &#8211; L2TP Server &#8211; Купих си тия дни ново рутерче <a href="https://www.ubnt.com/edgemax/edgerouter-x/" target="_blank">EdgeRouter X</a>. Понеже съм macbook pro и ми се налага да ползвам vpn се наложи да конфигурирам рутерчето да работи с <a href="https://en.wikipedia.org/wiki/Layer_2_Tunneling_Protocol" target="_blank">VPN L2TP</a>.

Това което направих да се логна чрез SSH в рутера :

`<br />
ssh panev@192.168.99.1<br />
Welcome to EdgeOS`

след като се логнах в рутера изпълних следните команди:

`configure`  
влизаме в конфигурационне режим на рутера

Има няколко възможности за конфигурация според това на кои порт е интернета и каква е конфигурацията му DHCP, STATIC или PPPoE. В моя случай използвам eth0 и STATIC IP и ще споделя как съм го конфигурирал

`set vpn l2tp remote-access outside-address STATIC_IP`  
* заменяте STATIC_IP с вашият ip адрес, който може да проверите като посетите сайта http://ping.eu

Задавам си рейндж от кой до кой IP адрес да раздава ip адреси  
`<br />
set vpn l2tp remote-access client-ip-pool start 192.168.100.101<br />
set vpn l2tp remote-access client-ip-pool stop 192.168.100.110<br />
` 

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
`<br />
set vpn l2tp remote-access dns-servers server-1 8.8.8.8<br />
set vpn l2tp remote-access dns-servers server-2 8.8.4.4<br />
` 

Прилагане на промените  
`commit`

Може да разгледаме конфигурацията която направихме, чрез командата  
`show vpn l2tp remote-access`

Записваме настройките  
`save`

След тези настройки е необходимо да достъпим уеб интерфейса на приложението за да конфигурираме и Firewall.  
`Firewall/NAT таба > Firewall Policies > WAN_LOCAL > Actions > Edit`

Трябва да създадем две паравила L2TP и ESP  
`<br />
Basic:<br />
  • Description:  Allow L2TP<br />
  • Check Enable.<br />
  • Action:  Accept.<br />
  • Protocol:  UDP<br />
Destination:<br />
  • Port:  500,1701,4500<br />
`  
Записваме настройките, чрез бутона save

`<br />
Basic Tab:<br />
  • Description:  Allow ESP<br />
  • Check Enable.<br />
  • Action:  Accept.<br />
  • Protocol: Choose a protocol by name:  esp<br />
`  
Записваме настройките, чрез бутона save

Това е, вече имаме конфигуриран 

## EdgeRouter &#8211; L2TP Server

на нашият рутер. Остава да конфигурирате вашият компютър да изпозлва VPN сървъра.