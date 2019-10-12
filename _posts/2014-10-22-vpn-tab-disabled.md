---
id: 770
title: Network Manager VPN Tab Disabled
date: 2014-10-22T10:10:23+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog/?p=770
permalink: /vpn-tab-disabled.html
wp_review_location:
  - bottom
wp_review_desc_title:
  - Summary
wp_review_color:
  - '#1e73be'
wp_review_fontcolor:
  - '#555555'
wp_review_bgcolor1:
  - '#e7e7e7'
wp_review_bgcolor2:
  - '#ffffff'
wp_review_bordercolor:
  - '#e7e7e7'
tie_views:
  - "399"
image: /wp-content/uploads/2013/11/vpn.png
categories:
  - Linux
---
The NetworkManager can display available network hardware and wireless networks. But, I&#8217;m unable to add VPN support as the Add tab is greyed out. I need to use both PPTP and Cisco vpn clients. How do I fix this problem under Debian or Ubuntu Linux?

[<img src="http://panevinfo.eu/blog/wp-content/uploads/2014/10/Gnome_Network_Connections_Cisco_PPTP_VPN.png" alt="Gnome_Network_Connections_PPTP_VPN" width="482" height="352" class="aligncenter size-full wp-image-771" srcset="https://www.panevinfo.eu/wp-content/uploads/2014/10/Gnome_Network_Connections_Cisco_PPTP_VPN.png 482w, https://www.panevinfo.eu/wp-content/uploads/2014/10/Gnome_Network_Connections_Cisco_PPTP_VPN-300x219.png 300w" sizes="(max-width: 482px) 100vw, 482px" />](http://panevinfo.eu/blog/wp-content/uploads/2014/10/Gnome_Network_Connections_Cisco_PPTP_VPN.png)

How to fix it :).

<!--more-->

The Add tab is greyed out when required plugins are not installed for Gnome NetworkManager. The following plugins should be installed under Debian / Ubuntu Linux:

  * network-manager-openvpn and network-manager-openvpn-gnome &#8211; network management framework OpenVPN plugin GNOME GUI
  * network-manager-pptp and network-manager-pptp-gnome &#8211; network management framework PPTP plugin GNOME GUI
  * network-manager-strongswan &#8211; network management framework strongSwan ipsec vpn plugin
  * network-manager-vpnc and network-manager-vpnc-gnome &#8211; network management framework (VPNC plugin GNOME GUI)

To install all of the above plugins use the apt-get command as follows:

<pre>sudo apt-get install network-manager-openvpn network-manager-pptp network-manager-vpnc network-manager-pptp-gnome

</pre>

The following plugins should be installed under RHEL / Fedora / CentOS / Scientific Linux / Red Hat Enterprise Linux desktop systems:

  * NetworkManager-openvpn : NetworkManager VPN plugin for OpenVPN
  * NetworkManager-pptp : NetworkManager VPN plugin for pptp
  * NetworkManager-vpnc : NetworkManager VPN plugin for vpnc
</u To install all of the above plugins use the yum command as follows: 

<pre>yum install NetworkManager-vpnc NetworkManager-pptp NetworkManager-openvpn
</pre>

Now, you can add vpn connection to your system using NetworkManager itself. You may need to restart the NetworkManager as follows:

<pre>/etc/init.d/network-manager restart
</pre>

[<img src="http://panevinfo.eu/blog/wp-content/uploads/2014/10/Linux_gnome_cisco_pptp_vpn_client.png" alt="Linux_gnome_cisco_pptp_vpn_client" width="508" height="395" class="alignleft size-full wp-image-774" srcset="https://www.panevinfo.eu/wp-content/uploads/2014/10/Linux_gnome_cisco_pptp_vpn_client.png 508w, https://www.panevinfo.eu/wp-content/uploads/2014/10/Linux_gnome_cisco_pptp_vpn_client-300x233.png 300w" sizes="(max-width: 508px) 100vw, 508px" />](http://panevinfo.eu/blog/wp-content/uploads/2014/10/Linux_gnome_cisco_pptp_vpn_client.png)