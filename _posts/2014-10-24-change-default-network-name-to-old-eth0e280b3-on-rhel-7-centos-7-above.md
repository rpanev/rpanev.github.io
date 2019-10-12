---
id: 778
title: Change default network name to old “eth0″ on RHEL 7 / CentOS 7 above
date: 2014-10-24T22:50:33+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog/?p=778
permalink: '/change-default-network-name-to-old-eth0%e2%80%b3-on-rhel-7-centos-7-above.html'
wp_review_location:
  - bottom
wp_review_desc_title:
  - Резюме
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
wp_review_user_review_type:
  - star
tie_views:
  - "292"
image: /wp-content/uploads/2014/03/s-5c-a-517-e1414183729413.png
categories:
  - Разни
---
## Change default network name to old “eth0″ on RHEL 7 / CentOS 7 above

Red Hat Enterprise 7 is based on CentOS 7 and upstream of kernel 3.10

Ever wanted to change back to the default network device name like &#8222;ethX&#8220;

This is based on VMware installation i have the default nic name as&#8220;ifcfg-enp13s0&#8243;

enp13s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 1500  
inet 192.168.1.33 netmask 255.255.255.0 broadcast 192.168.1.255  
inet6 fe80::230:48ff:fef9:9f38 prefixlen 64 scopeid 0x20 ether 00:30:48:f9:9f:38 txqueuelen 1000 (Ethernet)  
RX packets 668 bytes 121667 (118.8 KiB)  
RX errors 0 dropped 0 overruns 0 frame 0  
TX packets 807 bytes 102801 (100.3 KiB)  
TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0  
device interrupt 16 memory 0xd0200000-d0220000

enp15s0: flags=4099<UP,BROADCAST,MULTICAST> mtu 1500  
ether 00:30:48:f9:9f:39 txqueuelen 1000 (Ethernet)  
RX packets 0 bytes 0 (0.0 B)  
RX errors 0 dropped 0 overruns 0 frame 0  
TX packets 0 bytes 0 (0.0 B)  
TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0  
device interrupt 17 memory 0xd0300000-d0320000

<pre>[root@server network-scripts]# cat /etc/default/grub
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="rd.lvm.lv=centos_server/usr crashkernel=auto  vconsole.keymap=us rd.lvm.lv=centos_server/swap vconsole.font=latarcyrheb-sun16 rd.lvm.lv=centos_server/root rhgb quiet net.ifnames=0 biosdevname=0"
GRUB_DISABLE_RECOVERY="true"

</pre>

Look for this line “GRUB\_CMDLINE\_LINUX” and add the following: “net.ifnames=0 biosdevname=0″

Should look like this:  
GRUB\_CMDLINE\_LINUX=”rd.lvm.lv=rootvg/usrlv rd.lvm.lv=rootvg/swaplv crashkernel=auto vconsole.keymap=us rd.lvm.lv=rootvg/rootlv vconsole.font=latarcyrheb-sun16 rhgb quiet **net.ifnames=0 biosdevname=0**“

<pre>[root@server network-scripts]# grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.10.0-123.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-123.el7.x86_64.img
Found linux image: /boot/vmlinuz-3.10.0-123.8.1.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-123.8.1.el7.x86_64.img
Found linux image: /boot/vmlinuz-0-rescue-f30bf8eacc1a4c2189599188b85ae68e
Found initrd image: /boot/initramfs-0-rescue-f30bf8eacc1a4c2189599188b85ae68e.img
done

</pre>

If you didn&#8217;t put any names during the installation, you will need to rename the interface files by renaming the file /etc/sysconfig/network-scripts/ifcfg-*.

<pre>mv /etc/sysconfig/network-scripts/ifcfg-enp13s0 /etc/sysconfig/network-scripts/ifcfg-eth0
mv /etc/sysconfig/network-scripts/ifcfg-enp15s0 /etc/sysconfig/network-scripts/ifcfg-eth0
</pre>

run the comand :

<pre>grub2-mkconfig -o /boot/grub2/grub.cfg </pre>

Neprext reboot

<pre>[root@server network-scripts]# reboot
</pre>

After system reboot

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 1500  
inet 192.168.1.33 netmask 255.255.255.0 broadcast 192.168.1.255  
inet6 fe80::230:48ff:fef9:9f38 prefixlen 64 scopeid 0x20 ether 00:30:48:f9:9f:38 txqueuelen 1000 (Ethernet)  
RX packets 724 bytes 126780 (123.8 KiB)  
RX errors 0 dropped 0 overruns 0 frame 0  
TX packets 849 bytes 110115 (107.5 KiB)  
TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0  
device interrupt 16 memory 0xd0200000-d0220000

eth1: flags=4099<UP,BROADCAST,MULTICAST> mtu 1500  
ether 00:30:48:f9:9f:39 txqueuelen 1000 (Ethernet)  
RX packets 0 bytes 0 (0.0 B)  
RX errors 0 dropped 0 overruns 0 frame 0  
TX packets 0 bytes 0 (0.0 B)  
TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0  
device interrupt 17 memory 0xd0300000-d0320000

Прочетете още :

[How to install ownCloud – Cloud](https://panevinfo.eu/blog/how-to-install-owncloud-cloud-storage.html)  
[How to install VideoLan on Fredora, CentOS and Red Het](https://panevinfo.eu/blog/how-to-install-installing-skype-on-fedora.html)

&nbsp;