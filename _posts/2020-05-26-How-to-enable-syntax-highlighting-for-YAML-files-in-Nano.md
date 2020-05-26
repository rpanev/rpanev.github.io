---
id: 48
title: How to enable syntax highlighting for YAML files in Nano
date: 2020-02-14T14:00:43+00:00
author: Panev
layout: post
categories:
  - linux
  - ansible
tags:
  - Linux
  - ansible
---

# Description
Nano is a simple terminal-based text editor. Though not as powerful as Emacs or Vim, it is easy to learn and use. A lot of developers prefer this editor as it's very simple to use and pretty useful when you only want to edit a single file quickly on your server.

One of those files that you need to change often in this kind of editor are configuration file, like yaml files. Nano offers syntax highlighting for many file types, however not for yaml files. If you want to highlight this kind of files as well, you will need to follow an extra step.

1. List available Nano Syntax Highlight Files

{% highlight shell %}
 [root@ansible]:[~]# ls /usr/share/nano/
asm.nanorc       changelog.nanorc  css.nanorc      elisp.nanorc    go.nanorc     html.nanorc        json.nanorc      man.nanorc   nanohelp.nanorc  objc.nanorc   perl.nanorc  postgresql.nanorc  ruby.nanorc  spec.nanorc     tex.nanorc
autoconf.nanorc  cmake.nanorc      debian.nanorc   fortran.nanorc  groff.nanorc  java.nanorc        lua.nanorc       mgp.nanorc   nanorc.nanorc    ocaml.nanorc  php.nanorc   pov.nanorc         rust.nanorc  tcl.nanorc      xml.nanorc
awk.nanorc       c.nanorc          default.nanorc  gentoo.nanorc   guile.nanorc  javascript.nanorc  makefile.nanorc  mutt.nanorc  nftables.nanorc  patch.nanorc  po.nanorc    python.nanorc      sh.nanorc    texinfo.nanorc  
[root@ansible]:[~]#
{% endhighlight %}

2. Create YAML Nano Syntax Highlighting File
In order to provide syntax highlighting to your file, if the default file doesn't exist, you need to create the syntax highlighting file for this language. This file is the yaml.nanorc file and you need to create it in the mentioned directory. Run nano to create the file:

{% highlight shell %}
nano /usr/share/nano/yaml.nanorc
{% endhighlight %}

and paste the following content:

{% highlight shell %}
# Supports `YAML` files
syntax "YAML" "\.ya?ml$"
header "^(---|===)" "%YAML"

## Keys
color magenta "^\s*[\$A-Za-z0-9_-]+\:"
color brightmagenta "^\s*@[\$A-Za-z0-9_-]+\:"

## Values
color white ":\s.+$"
## Booleans
icolor brightcyan " (y|yes|n|no|true|false|on|off)$"
## Numbers
color brightred " [[:digit:]]+(\.[[:digit:]]+)?"
## Arrays
color red "\[" "\]" ":\s+[|>]" "^\s*- "
## Reserved
color green "(^| )!!(binary|bool|float|int|map|null|omap|seq|set|str) "

## Comments
color brightwhite "#.*$"

## Errors
color ,red ":\w.+$"
color ,red ":'.+$"
color ,red ":".+$"
color ,red "\s+$"

## Non closed quote
color ,red "['\"][^['\"]]*$"

## Closed quotes
color yellow "['\"].*['\"]"

## Equal sign
color brightgreen ":( |$)"
{% endhighlight %}

<a href="https://github.com/serialhex/nano-highlight" rel="noopener noreferrer" target="_blank">Visit the official repository of Nano Highlight </a>, a spiffy collection of nano syntax highlighting files for more information and languages available for nano. This file will be automatically added into nano and will highlight yaml files. Save changes and proceed with the last step.

3. Create Test Yaml File to see results
As final step, you need to test wheter the highlight works or not. Proceed to create a test file with nano and write some YAML on it, for example:

{% highlight yaml %}
- hosts: updates
  gather_facts: False
  tasks:
    - name: Check Dist Version
      shell: cat /etc/os-release
      register: response

    - debug: msg='{{ response.stdout }}'
{% endhighlight %}

Може да заложите конфигурацията директно в ansible.cfg файла:

{% highlight shell %}
grafana_url = "<your_grafana_server_address>:<your_grafana_server_port>/api/annotations"
validate_grafana_certs = 1
http_agent = 'Ansible (grafana_annotations callback)'
grafana_user = <grafana user>
grafana_password = <paasword>
{% endhighlight %}

Save the file, edit it again and you will now see the YAML code highlighted.
