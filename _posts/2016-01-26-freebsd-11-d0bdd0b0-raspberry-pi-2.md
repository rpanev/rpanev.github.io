---
id: 847
title: FreeBSD 11 на Raspberry Pi 2?
date: 2016-01-26T03:25:00+00:00
author: panev
layout: post
categories:
  - FreeBSD
---
Можете лесно да инсталирате FreeBSD 10 или FreeBSD 11 на Raspberry Pi 2 Model B. Можете да се изгради хубав и лесен за използване Unix сървър посредством FreeBSD операционна система. FreeBSD-CURRENT подкрепи Raspberry Pi от ноември 2012 г. и Raspberry Pi 2 от март, 2015 г. В този бърз урок аз ще обясня как да инсталирате FreeBSD 11 текущата версия на RPI2.

1.Изтегляне на FreeBSD за arm

Може да видите пълният списък на поддържани arm системи от <a href="ftp://ftp.freebsd.org/pub/FreeBSD/snapshots/arm/armv6/ISO-IMAGES/11.0" target="_blank">тук</a>. Или да използвате командите wget и curl за да свалите изображението на FreeBSD 11.

{% highlight shell %}wget ftp://ftp.freebsd.org/pub/FreeBSD/snapshots/arm/armv6/ISO-IMAGES/11.0/FreeBSD-11.0-CURRENT-arm-armv6-RPI2-20151016-r289420.img.xz{% endhighlight %}

или

{% highlight shell %}curl -O ftp://ftp.freebsd.org/pub/FreeBSD/snapshots/arm/armv6/ISO-IMAGES/11.0/FreeBSD-11.0-CURRENT-arm-armv6-RPI2-20151016-r289420.img.xz{% endhighlight %}

2. Разархивирате архива

{% highlight shell %}unxz FreeBSD-11.0-CURRENT-arm-armv6-RPI2-20151016-r289420.img.xz{% endhighlight %}

3. Инсталиране на SD карта

{% highlight shell %}dd if=FreeBSD-11.0-CURRENT-arm-armv6-RPI2-20151016-r289420.img of=/dev/disk2 bs=64k{% endhighlight %}

4. Зареждане на FreeBSD

Поставете SD картата във вашата Raspberry Pi 2 Model B. Трябва да се свържете клавиатура, мишка и монитор дисплей.

5. Потребителско име и парола за RPi2

Паролите по подразбиране за изображенията са freebsd / freebsd и root / root.

Това е, вече имате FreeBSD сървър базиран на Raspberry Pi 2 Model B!