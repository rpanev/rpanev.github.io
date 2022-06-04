---
id: 4
title: Как да инсталирате chkrootkit на FreeBSD
date: 2013-07-23T13:58:18+00:00
author: panev
layout: post
categories:
  - FreeBSD
---
С chkrootkit можете да проверявате редовно система за признаци на руткит. chkrootkit търси известни подписи в trojaned системни двоични файлове. Той също така проверява дали интерфейсът е в безразборни режим, за lastlog заличавания, за wtmp заличавания, за wtmpx заличавания, за признаци на LKM троянски коне и за utmp заличавания. Спринт chkrootkit като Cron прави този много полезен инструмент за сигурност. chkrootkit е достъпно на FreeBSD и може да се инсталира през пристанището (ports) директория.

**Задължително трябва да сте root**.

Предвижете се до ports и инсталирате chkrootkit чрез следната команда :

{% highlight shell %}
cd /usr/ports/security/chkrootkit
make install clean; rehash
{% endhighlight %}

След като е инсталиран просто стартирайте приложението в конзола. За най-голяма автоматизация я сложете в crontab примерно проверявайки и входящият/изходящият трафик на mail :

{% highlight shell %}
0 3 * * * (cd /path/to/chkrootkit; ./chkrootkit 2>&1 | mail -s "chkrootkit output" root)
{% endhighlight %}
