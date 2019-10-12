---
id: 314
title: Как да ползваме командаta grep
date: 2013-12-07T19:42:29+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=314
permalink: '/%d0%ba%d0%b0%d0%ba-%d0%b4%d0%b0-%d0%bf%d0%be%d0%bb%d0%b7%d0%b2%d0%b0%d0%bc%d0%b5-%d0%ba%d0%be%d0%bc%d0%b0%d0%bd%d0%b4%d0%b0ta-grep.html'
tie_views:
  - "195"
categories:
  - code
  - FreeBSD
  - Linux
---
Командата **grep** се използва за търсене на текст или претърсва даден файл/ове за зададена дума. Изключително полезна, ето няколко нейни варианта 🙂

<pre>grep 'word' filename
grep 'word' file1 file2 file3
grep 'string1 string2'  filename
grep -i 'Model' /proc/cpuinfo
command option1 | grep 'data'
grep --color 'data' fileName
ps aux | grep {process-name}

</pre>