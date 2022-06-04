---
id: 47
title: Watch your Ansible deployments in Grafana!
date: 2020-02-14T14:00:43+00:00
author: Panev
layout: post
categories:
  - linux
  - monitoring
  - ansible
tags:
  - Linux
  - monitoring
  - ansible
---

# Description
От интерес търсих начин как да мониторирам използването на ansible, и попаднах на [това](https://docs.ansible.com/ansible/latest/plugins/callback/grafana_annotations.html) поразрових се и намерих страхотно поне за мен решемние.

Създаваме директория с име “callback_plugins” за предпочитане в самата директория на ansible скриптовете

{% highlight shell %}
 cd <your_playbook_dir>
 mkdir callback_plugins
{% endhighlight %}

И изтегляме този [плъгин](https://raw.githubusercontent.com/ansible-collections/grafana/master/plugins/callback/grafana_annotations.py). 

{% highlight shell %}
cd /callback_plugins
wget https://raw.githubusercontent.com/ansible-collections/grafana/master/plugins/callback/grafana_annotations.py
{% endhighlight %}

разрешаваме на ansible да изпозлва този плъгин чрез [ansible.cfg](https://docs.ansible.com/ansible/devel/config.html).
{% highlight shell %}
[...]
callback_whitelist = grafana_annotations
{% endhighlight %}

След като конфигурацията е готова ще трябва да зададем няколко параметъра за връзка 

{% highlight shell %}
$ export GRAFANA_SERVER=<your_grafana_server_address>
$ export GRAFANA_PORT=<your_grafana_server_port>
$ export GRAFANA_SECURE=0                           # 0 for HTTP, 1 for HTTPS
$ export GRAFANA_API_TOKEN=<your_grafana_api_token>
{% endhighlight %}

За да си генерираме тоукън трябва да изпълним няколко стъпки:

{% highlight shell %}
curl -XPOST <your_grafana_server_address>:<your_grafana_server_port>/api/auth/keys --user "admin:admin --data '{"name": "ansible-callback", "role": "Editor"}' -H "Content-Type: application/json"
{% endhighlight %}

Може да заложите конфигурацията директно в ansible.cfg файла:

{% highlight shell %}
grafana_url = "<your_grafana_server_address>:<your_grafana_server_port>/api/annotations"
validate_grafana_certs = 1
http_agent = 'Ansible (grafana_annotations callback)'
grafana_user = <grafana user>
grafana_password = <paasword>
{% endhighlight %}

Създаваме си нов панел в графан (Create a dashboard), от настройките избираме подменюто "annotations"

<img src='https://raw.githubusercontent.com/rpanev/rpanev.github.io/master/static/img/_posts/annotations.jpg' alt="Watch your Ansible deployments in Grafana!"/>

Аз лично съм си направим таг за всеки ansible-playbook в разлизен цвят за да си ги следя по-лесно визуално
