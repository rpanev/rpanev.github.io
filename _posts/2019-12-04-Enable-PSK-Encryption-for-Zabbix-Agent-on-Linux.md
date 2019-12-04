---
id: 46
title: Enable PSK Encryption for Zabbix Agent on Linux
date: 2019-12-04T12:37:43+00:00
author: Panev
layout: post
categories:
  - linux
  - monitoring
tags:
  - Linux
  - monitoring
---

# Description
By default, agent communication is done in clear text.
For encryption we have an option to use PSK-based encryption.
PSK means pre shared key.
The PSK option consists of two important values, the PSK identity and the PSK Secret.
The secret should be minimum a 128-bit (16-byte PSK, entered as 32 hexadecimal digits) up to 2048-bit (256-byte PSK, entered as 512 hexadecimal digits)

You can generate a 256 bit PSK secret with openssl using the command:

{% highlight shell %}
openssl rand -hex 32
{% endhighlight %}

I will save it directly to the zabbix directory

{% highlight shell %}
cd /etc/zabbix
{% endhighlight %}

I then run,
{% highlight shell %}
openssl rand -hex 32 > secret.psk
{% endhighlight %}

I also make sure that only the Zabbix user can read the file.

{% highlight shell %}
chown zabbix:zabbix secret.psk && chmod 640 secret.psk
{% endhighlight %}

I then edit the Zabbix agent configuration file.

{% highlight shell %}
nano /etc/zabbix/zabbix_agentd.conf
{% endhighlight %}

and change the options near the bottom,

{% highlight shell %}
TLSConnect=psk
TLSAccept=psk
TLSPSKFile=secret.psk
TLSPSKIdentity=[my secret pass]
{% endhighlight %}

<aside class="warning">
The TLSPSKIdentity value you decide will not be encrypted on transport, so don't use anything sensitive.
</aside>

I then restart the agent
{% highlight shell %}
service zabbix-agent restart
{% endhighlight %}

# The configuration of the zabbix server is as follows

Configuration > Host > we choose the host for which to put psk and we click on it > Encryption > I select the :

'Connections to host' = PSK

'Connections from host' = PSK

'PSK Identity' = [what ever you used in the Zabbix agent config]

'PSK' = [the long hex string generated from the OpenSSL command above]

After a minute or two, the Zabbix Server and Agent will successfully communicate using PSK encryption.
