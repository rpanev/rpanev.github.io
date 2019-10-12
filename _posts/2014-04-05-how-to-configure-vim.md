---
id: 621
title: How to Configure VIM
date: 2014-04-05T14:26:00+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=621
permalink: /how-to-configure-vim.html
tie_views:
  - "246"
image: /wp-content/uploads/2014/04/vim-editor_logo.png
categories:
  - FreeBSD
  - Linux
---
Инсталирайте vim

<pre>yum -y install vim
</pre>

Задайте alias ако искате за всички потребители ако ли не направете го на вашият профил:

<pre>vi /etc/profile
</pre>

добавете следният ред :

<pre>alias vi='vim'
</pre>

Ако искате само вашият потребител да се възползва от това добавете горният ред в :

<pre>vi ~/.bashrc
</pre>

За настройки може да ползвате :

<pre>set number
syntax on
highlight Comment ctermfg=LightCyan
</pre>