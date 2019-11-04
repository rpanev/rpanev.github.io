---
id: 45
title:  Инсталация на Command-line interface for Hetzner Cloud
date: 2019-11-04T14:20:43+00:00
author: Panev
layout: post
categories:
  - devops
tags:
  - devops
---

# Създаваме API Token 

1) За целта е необходимо да посетим следният сайт <a href="https://console.hetzner.cloud">Hetzner</a>
2) Създаваме/Избираме проект
3) Отиваме на Access > API tokens и големият червен бутон `GENERATE API TOKEN`
4) Слагаме описание и копираме тоукъна

# Инсталация на hcloud: Command-line interface for Hetzner Cloud

1) За MacOS и Linux или разбира може да свалим сорса от <a href="https://github.com/hetznercloud/cli/releases/tag/v1.13.0">Github.

{% highlight shell %}

$ brew install hcloud
Updating Homebrew...
==> Auto-updated Homebrew!
Updated 2 taps (homebrew/core and homebrew/cask).
==> New Formulae
manticoresearch
==> Updated Formulae
git ✔          ack            cmake          dcmtk          deark          gmt            harfbuzz       hydra          jsvc           libssh         offlineimap    php-cs-fixer   qt             siril          sourcedocs     tile38         ucloud         yafc

==> Downloading https://homebrew.bintray.com/bottles/hcloud-1.13.0.catalina.bottle.tar.gz
==> Downloading from https://akamai.bintray.com/45/45162c76dbf09c5ccf2d8be86382b5c1c81d90740b1b42b3d4ef4e0cb8a421d1?__gda__=exp=1572869911~hmac=cc20d712b07ed3f460a1b4e86aad1a3fda98358bb3f48b90b665f1f4617a1a14&response-content-disposition=attachment%3Bfilename%3D%22hcloud-
######################################################################## 100.0%
==> Pouring hcloud-1.13.0.catalina.bottle.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d

zsh completions have been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/hcloud/1.13.0: 8 files, 9.4MB

{% endhighlight %}

- може да проверим версията
{% highlight shell %}
$ hcloud version
hcloud v1.13.0
{% endhighlight %}

- HELP командата
{% highlight shell %}
$ hcloud context
Usage:
  hcloud context [FLAGS]
  hcloud context [command]

Available Commands:
  active      Show active context
  create      Create a new context
  delete      Delete a context
  list        List contexts
  use         Use a context
{% endhighlight %}

- създаме context и въвеждаме тоукъна
{% highlight shell %}
$ hcloud context create r.panev
Token:    <<<<<TOKEN>>>>>
Context r.panev created and activated
{% endhighlight %}

# Създвана на клауд сървър

- Проверяваме кой от предоставяните сървъри ще ни трябва
{% highlight shell %}
$ hcloud server-type list
ID   NAME        CORES   MEMORY     DISK     STORAGE TYPE
1    cx11        1       2.0 GB     20 GB    local
2    cx11-ceph   1       2.0 GB     20 GB    network
3    cx21        2       4.0 GB     40 GB    local
4    cx21-ceph   2       4.0 GB     40 GB    network
5    cx31        2       8.0 GB     80 GB    local
6    cx31-ceph   2       8.0 GB     80 GB    network
7    cx41        4       16.0 GB    160 GB   local
8    cx41-ceph   4       16.0 GB    160 GB   network
9    cx51        8       32.0 GB    240 GB   local
10   cx51-ceph   8       32.0 GB    240 GB   network
11   ccx11       2       8.0 GB     80 GB    local
12   ccx21       4       16.0 GB    160 GB   local
13   ccx31       8       32.0 GB    240 GB   local
14   ccx41       16      64.0 GB    360 GB   local
15   ccx51       32      128.0 GB   600 GB   local
{% endhighlight %}

- Вдигаме сървъра 
{% highlight shell %}
$ hcloud server create --image centos-8 --type cx11 --name panev-hcloud
   2s [====================================================================] 100%
Waiting for server 3578313 to have started... done
Server 3578313 created
IPv4: 78.47.127.86
Root password: AUkgv44dmqaFAX7K4Xbr
{% endhighlight %}

- И вече си имаме сървърче :)
{% highlight shell %}
$ hcloud server list
ID        NAME                 STATUS    IPV4           IPV6                     DATACENTER
3578313   panev-hcloud         running   78.47.127.86   2a01:4f8:c2c:cfb8::/64   nbg1-dc3
{% endhighlight %}

