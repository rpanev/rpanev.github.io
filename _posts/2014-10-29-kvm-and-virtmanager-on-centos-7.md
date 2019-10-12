---
id: 784
title: KVM and VirtManager on CentOS 7
date: 2014-10-29T00:31:49+00:00
author: panev
layout: post
categories:
  - linux
  - kvm
  - centos
---

KVM is a kernel-based hypervisor which grows quickly in maturity and popularity in the Linux server market. Red Hat officially dropped Xen in favor of KVM since RHEL. With KVM being officially supported by Red Hat, installing KVM on RedHat-based systems should be a breeze.

In this tutorial, I will describe how to install and configure KVM and VirtManager on CentOS. To use this tutorial, it is not required to have CentOS desktop environment. This tutorial was in fact tested on CentOS 7 server.  


Check Hardware Virtualization Supoort

KVM requires hardware virtualization support such as Intel VT or AMD&#8217;s AMD-V, which are instruction set extensions for hardware-assisted virtualization. Check if hardware virtualization support is available on CentOS host machine:

{% highlight shell %}egrep -i 'vmx|svm' --color=always /proc/cpuinfo
{% endhighlight %}

If CPU flags contain &#8222;vmx&#8220; or &#8222;svm&#8220;, it means hardware virtualization support is available.

**Disable SELinux**

Before installing KVM, be aware that there are several SELinux booleans that can affect the behavior of KVM and libvirt. In this tutorial, I&#8217;m going to set SELinux to &#8222;disable&#8220; for demonstration purpose. If you do not wish to change SELinux mode, refer to the <a href="https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Virtualization/sect-Virtualization-Security_for_virtualization-SELinux_considerations.html" target="_blank">documentation</a> on KVM SELinux booleans.

To disable SELinux on CentOS:

{% highlight shell %}nano /etc/selinux/config
{% endhighlight %}

Edit this line

{% highlight shell %}SELINUX=disabled
{% endhighlight %}

Reboot the machine for the change to take effect.

Install KVM, QEMU and user-space tools

Install KVM and virtinst (a tool to create VMs) as follows:

{% highlight shell %}yum install kvm libvirt python-virtinst qemu-kvm dejavu-lgc-sans-fonts
{% endhighlight %}

Start libvirtd daemon, and set it to auto-start:

{% highlight shell %}service libvirtd start
chkconfig libvirtd on
{% endhighlight %}

Check if KVM has successfully been installed. You should see no error as follows.

{% highlight shell %}virsh -c qemu:///system list
 Id    Name                           State
----------------------------------------------------
{% endhighlight %}

Configure Linux Bridge for VM Networking

Installing KVM alone does not allow VMs to communicate with each other or access external networks. You need to configure VM networking separately. In this tutorial, I am going to set up &#8222;bridged networking&#8220; via Linux bridge.

Install a package needed to create and manage bridge devices:

{% highlight shell %}yum install bridge-utils{% endhighlight %}

Disable Network Manager service if it&#8217;s enabled, and switch to default net manager as follows.

{% highlight shell %}service NetworkManager stop
chkconfig NetworkManager off
chkconfig network on
service network start
{% endhighlight %}

To configure a new bridge, you have to pick an active network interface (e.g., eth0), and enslave it to the bridge. Depending on whether the network interface is assigned an IP address via DHCP or statically, there are two different ways to configure a new bridge.

To configure bridge br0 with a static IP address:

edit /etc/sysconfig/network-scripts/ifcfg-eth0

{% highlight shell %}DEVICE=eth0
TYPE=Ethernet
ONBOOT=yes
BRIDGE=br0
{% endhighlight %}

edit /etc/sysconfig/network-scripts/ifcfg-br0

{% highlight shell %}DEVICE=br0
NM_CONTROLLED=yes
ONBOOT=yes
TYPE=Bridge
NM_CONTROLLED=yes
BOOTPROTO=none
IPADDR=192.168.1.44
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=192.168.1.1
DNS2=8.8.8.8
{% endhighlight %}

Note that the configuration for the enslaved interface (eth0) does not have &#8222;BOOTPROTO&#8220; field, but &#8222;BRIDGE&#8220; field added.

Once configuration files are generated accordingly, run the following to activate the change.

{% highlight shell %}service network restart
{% endhighlight %}

You should now see br0 bridge interface with a proper IP address as follows.

{% highlight shell %}br0: flags=4163&lt;UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.44  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::230:48ff:fef9:9f38  prefixlen 64  scopeid 0x20

<link />
ether 00:00:00:00:00:00  txqueuelen 0  (Ethernet)
        RX packets 419447  bytes 689986593 (658.0 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 385147  bytes 495758281 (472.7 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth0: flags=4163&lt;UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        ether 00:00:00:00:00:00  txqueuelen 1000  (Ethernet)
        RX packets 675144  bytes 737692473 (703.5 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 573506  bytes 510598440 (486.9 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 16  memory 0xd0200000-d0220000  

lo: flags=73&lt;UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10&lt;host>
        loop  txqueuelen 0  (Local Loopback)
        RX packets 375733  bytes 550545800 (525.0 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 375733  bytes 550545800 (525.0 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0: flags=4099&lt;UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 7a:99:87:6b:8b:aa  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1  bytes 90 (90.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vnet0: flags=4163&lt;UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::fc54:ff:feb7:889b  prefixlen 64  scopeid 0x20

<link />
ether fe:54:00:b7:88:9b  txqueuelen 500  (Ethernet)
        RX packets 268  bytes 20742 (20.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 800  bytes 549088 (536.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
{% endhighlight %}

**Install VirtManager**

The final step is to install a desktop UI called VirtManager for managing virtual machines (VMs) through libvirt.

To install VirtManager:

{% highlight shell %}yum install virt-manager libvirt qemu
{% endhighlight %}

If you are using CentOS desktop, you should be able to launch VirtManager locally at this point, by simply running:

{% highlight shell %}virt-manager
{% endhighlight %}

However, if you are using CentOS server without desktop UI, follow these steps to launch VirtManager.

Enable X11 forwarding on SSH server:

{% highlight shell %}yum install xauth
nano /etc/ssh/sshd_config
X11Forwarding yes
service sshd restart
{% endhighlight %}

Next list xauth

{% highlight shell %}xauth list
legolas.smrad.eu/unix:10  MIT-MAGIC-COOKIE-1  7a8b4e69f4de0c5b3da1913f44f15b15
{% endhighlight %}

and finally add the result

{% highlight shell %}xauth add legolas.smrad.eu/unix:10  MIT-MAGIC-COOKIE-1  7a8b4e69f4de0c5b3da1913f44f15b15
{% endhighlight %}

Then connect to your CentOS server from a separate desktop machine, and run the wrapper script vm to launch VirtManager remotely.

{% highlight shell %}ssh -X root@legolas.smrad.eu
{% endhighlight %}

In the end it is necessary to add the following rule :

{% highlight shell %}echo “-I FORWARD -m physdev --physdev-is-bridged -j ACCEPT” > /etc/sysconfig/iptables-forward-bridged
{% endhighlight %}

Install monitoring

{% highlight shell %}yum install virt-top -y
{% endhighlight %}