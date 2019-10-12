---
id: 476
title: 'Samba 4 &#8211; Active Directory Domain Controller'
date: 2014-03-02T21:18:23+00:00
author: panev
layout: post
categories:
  - Linux
---
Инсталация на [](https://www.samba.org/ "samba ") под линукс CentOS 6.

{% highlight bash %}
#!/bin/bash
#Install SAMBA 4 DC
#Last updated: March - 2014
#http://panevifo.eu
DIR="/root/samba"
#Install the following packages required for installing and building Samba4:
yum -y install ntp git-core gcc libacl-devel libblkid-devel gnutls-devel readline-devel python-devel gdb pkgconfig krb5-workstation zlib-devel setroubleshoot-server libaio-devel setroubleshoot-plugins policycoreutils-python libsemanage-python setools-libs-python setools-libs popt-devel libpcap-devel sqlite-devel libidn-devel libxml2-devel libacl-devel libsepol-devel libattr-devel keyutils-libs-devel cyrus-sasl-devel cups-devel bind-utils
#Create dir for download
mkdir $DIR
cd $DIR
#Use a directory of your choice and download the latest version of samba from git:
git clone git://git.samba.org/samba.git samba-master
#Build samba:
cd samba-master
./configure --enable-debug --enable-selftest && make && make install
#Setting up your Samba4 server in its own domain:
/usr/local/samba/bin/samba-tool domain provision
#Start SAMBA server and NTP (Network Time Protocol)
/usr/local/samba/sbin/samba
/etc/init.d/ntpd start
chkconfig ntpd on
#Add your samba to startup on boot:
echo "/usr/local/samba/sbin/samba" >> /etc/rc.d/rc.local
#Testing Samba as an Active Directory DC:
/usr/local/samba/sbin/samba -V
#Verify you are running the correct samba-client version:
/usr/local/samba/bin/smbclient --version
#List the shares on your Samba4 server:
/usr/local/samba/bin/smbclient -L localhost -U%
echo - e "\e[31mReboot your system!!!"
init 6
exit 0
{% endhighlight %}

При проблем конфигурирането на домейна или грешка от рода на :

{% highlight error %}ERROR(&lt;class 'samba.provision.ProvisioningError'>): Provision failed - ProvisioningError: Your filesystem or build does not support posix ACLs, which s3fs requires.  Try the mounting the filesystem with the 'acl' option.{% endhighlight %}

изтрийте 

{% highlight shell %}rm  /usr/local/samba/etc/smb.conf{% endhighlight %}

и изпълнете наново командата :

{% highlight shell %}/usr/local/samba/bin/samba-tool domain provision --use-rfc2307 --interactive --use-ntvfs{% endhighlight %}

При правилна конфигурация ще получите следното :

{% highlight shell %}
┌(eragon)─(06:21 PM Sun Mar 02)
└─(~/samba-master)─(55 files, 336Kb)─> ~/samba-master
 #/usr/local/samba/bin/samba-tool domain provision -
Realm: smrad.lan
 Domain [smrad]:
 Server Role (dc, member, standalone) [dc]:
 DNS backend (SAMBA_INTERNAL, BIND9_FLATFILE, BIND9_DLZ, NONE) [SAMBA_INTERNAL]:
 DNS forwarder IP address (write 'none' to disable forwarding) [192.168.1.11]:
Administrator password:
Retype password:
Looking up IPv4 addresses
Looking up IPv6 addresses
No IPv6 address will be assigned
Setting up secrets.ldb
Setting up the registry
Setting up the privileges database
Setting up idmap db
Setting up SAM db
Setting up sam.ldb partitions and settings
Setting up sam.ldb rootDSE
Pre-loading the Samba 4 and AD schema
Adding DomainDN: DC=smrad,DC=lan
Adding configuration container
Setting up sam.ldb schema
Setting up sam.ldb configuration data
Setting up display specifiers
Modifying display specifiers
Adding users container
Modifying users container
Adding computers container
Modifying computers container
Setting up sam.ldb data
Setting up well known security principals
Setting up sam.ldb users and groups
Setting up self join
Adding DNS accounts
Creating CN=MicrosoftDNS,CN=System,DC=smrad,DC=lan
Creating DomainDnsZones and ForestDnsZones partitions
Populating DomainDnsZones and ForestDnsZones partitions
Setting up sam.ldb rootDSE marking as synchronized
Fixing provision GUIDs
A Kerberos configuration suitable for Samba 4 has been generated at /usr/local/samba/private/krb5.conf
Setting up fake yp server settings
Once the above files are installed, your Samba4 server will be ready to use
Server Role:           active directory domain controller
Hostname:              eragon
NetBIOS Domain:        SMRAD
DNS Domain:            smrad.lan
DOMAIN SID:            S-1-5-21-1061100771-1359248561-2410260555
{% endhighlight %}

Конфигурация на DNS :  
Примерен конфиг файл за smb.conf

{% highlight shell %}
┌(eragon)─(09:26 PM Sun Mar 02)
└─(/usr/local/samba/bin)─(52 files, 18Mb)─> /usr/local/samba/bin
 #cat /usr/local/samba/etc/smb.conf
# Global parameters
[global]
        workgroup = SMRAD
        realm = SMRAD.LAN
        netbios name = ERAGON
        server role = active directory domain controller
        dns forwarder = 192.168.1.11
        server services = rpc, nbt, wrepl, ldap, cldap, kdc, drepl, winbind, ntp_signd, kcc, dnsupdate, dns, smb
        dcerpc endpoint servers = epmapper, wkssvc, rpcecho, samr, netlogon, lsarpc, spoolss, drsuapi, dssetup, unixinfo, browser, eventlog6, backupkey, dnsserver, winreg, srvsvc
        idmap_ldb:use rfc2307 = yes

[netlogon]
        path = /usr/local/samba/var/locks/sysvol/smrad.lan/scripts
        read only = No

[sysvol]
        path = /usr/local/samba/var/locks/sysvol
        read only = No
{% endhighlight %}

Редактирайте 

{% highlight shell %}vim /etc/resolv.conf{% endhighlight %}

и добавете следните редове:

{% highlight shell %}# Generated by NetworkManager
domain mydomain.com
nameserver 192.168.1.11
{% endhighlight %}

Следващата стъпка е редакцията на :

{% highlight shell %}vim /etc/sysconfig/network-scripts/ifcfg-eth0
{% endhighlight %}

Намерете реда на който пише **DNS1**.

{% highlight shell %}DNS1="192.168.1.11" #И го сменете с вашето IP!!
{% endhighlight %}

След като настройте DNS задължително го тествайте :

{% highlight shell %}# host -t SRV _ldap._tcp.smrad.lan.
_ldap._tcp.smrad.lan has SRV record 0 100 389 eragon.smrad.lan.

# host -t SRV _kerberos._udp.smrad.lan.
_kerberos._udp.smrad.lan has SRV record 0 100 88 eragon.smrad.lan.

# host -t A eragon.smrad.lan.
eragon.smrad.lan has address 192.168.1.11
{% endhighlight %}

Конфигурация на Kerberos:  
Копирайте конфиг файла в /etc/

{% highlight shell %}cp /usr/local/samba/share/setup/krb5.conf /etc/krb5.conf
{% endhighlight %}

Той трябва да има следният вид :

{% highlight shell %}┌(eragon)─(09:41 PM Sun Mar 02)
└─(/home/unix)─(16 files, 126Mb)─> /home/unix
 #cat /etc/krb5.conf
[libdefaults]
        default_realm = ${REALM}
        dns_lookup_realm = false
        dns_lookup_kdc = true
{% endhighlight %}

Сложете administratorska парола за domeina

{% highlight shell %}kinit administrator@SMRAD.LAN
Password for administrator@SMRAD.LAN:
Warning: Your password will expire in 41 days on Sun Feb 3 14:21:51 2013
{% endhighlight %}

След което трябва да се тестват:

{% highlight shell %}┌(eragon)─(09:44 PM Sun Mar 02)
└─(/home/unix)─(16 files, 126Mb)─> /home/unix
 #klist
Ticket cache: FILE:/tmp/krb5cc_0
Default principal: administrator@SMRAD.LAN

Valid starting     Expires            Service principal
03/02/14 18:46:53  03/03/14 04:46:53  krbtgt/SMRAD.LAN@SMRAD.LAN
        renew until 03/03/14 18:46:44
{% endhighlight %}

Остава да отворите следните портове в защитната стена

{% highlight shell %}
53, TCP & UDP (DNS)
88, TCP & UDP (Kerberos authentication)
135, TCP (MS RPC)
137, UDP (NetBIOS name service)
138, UDP (NetBIOS datagram service)
139, TCP (NetBIOS session service)
389, TCP & UDP (LDAP)
445, TCP (MS-DS AD)
464, TCP & UDP (Kerberos change/set password)
1024, TCP (AD)
{% endhighlight %}

Инсталирайте и Remote Server Administration Tools за да може да управлявате домейна и потребителите в него.