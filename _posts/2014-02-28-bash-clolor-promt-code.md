---
id: 451
title: bash color prompt code
date: 2014-02-28T14:59:02+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=451
permalink: /bash-clolor-promt-code.html
wpgplus_message:
  - ""
wpgplus_publish:
  - 'no'
wp_review_location:
  - bottom
wp_review_desc_title:
  - Summary
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
tie_views:
  - "223"
image: /wp-content/uploads/2014/02/bash-e1414872639539.png
categories:
  - code
---
Добавете следните редове в .bashrc 

<pre>vim .bashrc

</pre>

За потребител:

<pre>PROMPT_COMMAND='history -a;echo -en "\033[m\033[38;5;2m"$(( $(sed -nu "s/MemFree:[\t ]\+\([0-9]\+\) kB/\1/p" /proc/meminfo)/1024))"\033[38;5;22m/"$(($(sed -nu "s/MemTotal:[\t ]\+\([0-9]\+\) kB/\1/Ip" /proc/meminfo)/1024 ))MB"\t\033[m\033[38;5;55m$(&lt; /proc/loadavg)\033[m"'
PS1='\n\e[1;30m[\j:\!\e[1;30m]\e[0;36m \T \d \e[1;30m[\e[1;34m\u@\H\e[1;30m:\e[0;37m`tty 2>/dev/null` \e[0;32m+${SHLVL}\e[1;30m] \e[1;37m\w\e[0;37m\[\033]0;[ ${H1}... ] \w - \u@\H +$SHLVL @`tty 2>/dev/null` - [ `uptime` ]\007\]\n\[\]\$ '
</pre>

Демо :  
[<img src="http://panevinfo.eu/blog//wp-content/uploads/2014/02/root-eragon1.png" alt="root-eragon" width="835" height="112" class="alignleft size-full wp-image-538" srcset="https://www.panevinfo.eu/wp-content/uploads/2014/02/root-eragon1.png 835w, https://www.panevinfo.eu/wp-content/uploads/2014/02/root-eragon1-300x40.png 300w, https://www.panevinfo.eu/wp-content/uploads/2014/02/root-eragon1-768x103.png 768w" sizes="(max-width: 835px) 100vw, 835px" />](http://panevinfo.eu/blog//wp-content/uploads/2014/02/root-eragon1.png)

Друг хубав вариант за root потребителят е този :

<pre>PS1="\n\[\033[1;37m\]\342\224\214($(if [[ ${EUID} == 0 ]]; then echo '\[\033[01;31m\]\h'; else echo '\[\033[01;34m\]\u@\h'; fi)\[\033[1;37m\])\$([[ \$? != 0 ]] && echo \"\342\224\200(\[\033[0;31m\]\342\234\227\[\033[1;37m\])\")\342\224\200(\[\033[1;34m\]\@ \d\[\033[1;37m\])\[\033[1;37m\]\n\342\224\224\342\224\200(\[\033[1;32m\]\w\[\033[1;37m\])\342\224\200(\[\033[1;32m\]\$(ls -1 | wc -l | sed 's: ::g') files, \$(ls -sh | head -n1 | sed 's/total //')b\[\033[1;37m\])\342\224\200> \[\033[0m\]\e[1;37m\w\e[0;37m\[\033]0;[ ${H1}... ] \w - \u@\H +$SHLVL @`tty 2>/dev/null` - [ `uptime` ]\007\]"
</pre>

Демо :

[<img src="http://panevinfo.eu/blog//wp-content/uploads/2014/02/root-eragon3.png" alt="root-eragon" width="835" height="112" class="alignleft size-full wp-image-540" srcset="https://www.panevinfo.eu/wp-content/uploads/2014/02/root-eragon3.png 835w, https://www.panevinfo.eu/wp-content/uploads/2014/02/root-eragon3-300x40.png 300w, https://www.panevinfo.eu/wp-content/uploads/2014/02/root-eragon3-768x103.png 768w" sizes="(max-width: 835px) 100vw, 835px" />](http://panevinfo.eu/blog//wp-content/uploads/2014/02/root-eragon3.png)