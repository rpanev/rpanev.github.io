---
id: 270
title: 'Инсталация на security updates използвайки  yum в CentOS'
date: 2013-11-10T16:35:26+00:00
author: panev
layout: post
categories:
  - bash
  - Linux
---
Инсталирайте плъгина 

{% highlight bash %}yum install yum-plugin-security
{% endhighlight %}

За да проверите за security updates напише те :

{% highlight bash %}yum --security update
{% endhighlight %}

Това е всичко 🙂 Може да ползвате и следните команди :

{% highlight bash %}yum updateinfo summary
yum updateinfo list security
yum updateinfo list available
yum updateinfo list bugzillas{% endhighlight %}