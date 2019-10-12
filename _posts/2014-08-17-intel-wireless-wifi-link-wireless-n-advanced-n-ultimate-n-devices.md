---
id: 701
title: Intel Wireless WiFi Link, Wireless-N, Advanced-N, Ultimate-N devices
date: 2014-08-17T00:14:36+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog/?p=701
permalink: /intel-wireless-wifi-link-wireless-n-advanced-n-ultimate-n-devices.html
tie_views:
  - "312"
image: /wp-content/uploads/2014/08/debian-1-logo-primary.jpg
categories:
  - Linux
---
How to install intel wirless for Debian 🙂

The iwlwifi module (iwlagn prior to Linux 3.2) is produced by the iwlwifi Linux kernel driver, which supports several Intel wireless LAN adapters: 

Intel Wireless WiFi 4965AGN

In Debian 7 &#8222;Wheezy&#8220;, this device is now supported by the iwlegacy driver (iwl4965 module).  
<!--more-->

Intel Wireless WiFi 5100AGN, 5300AGN, 5350AGN

Intel Wireless WiFi 5150AGN

Intel WiFi Link 1000BGN

Intel 6000 Series WiFi Adapters (6200AGN and 6300AGN)

Intel Wireless WiFi Link 6250AGN Adapter

Intel 6005 Series WiFi Adapters

Intel 6030 Series WiFi Adapters

Intel Wireless WiFi Link 6150BGN 2 Adapter

Intel 100 Series WiFi Adapters (100BGN and 130BGN)

Intel 2000 Series WiFi Adapters 

Add a &#8222;non-free&#8220; component to /etc/apt/sources.list, for example: 

`<br />
# Debian 7 "Wheezy"<br />
deb http://http.debian.net/debian/ wheezy main contrib non-free` 

Update the list of available packages and install the <a href="https://packages.debian.org/firmware-iwlwifi" title="firmware-iwlwifi" target="_blank">firmware-iwlwifi</a> package: 

`<br />
root@debian:/home/panev# apt-get update && apt-get install firmware-iwlwifi<br />
` 

As the iwlwifi module is automatically loaded for supported devices, reinsert this module to access installed firmware: 

`<br />
root@debian:/home/panev# modprobe -r iwlwifi ; modprobe iwlwifi` 

Configure your wireless interface as appropriate for KDE :

`<br />
root@debian:/home/panev#  aptitude install plasma-widget-networkmanagement`