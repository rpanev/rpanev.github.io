---
id: 153
title: Как да инсталираме и конфигурираме Prometheus на Linux VM
date: 2019-09-11T18:57:43+00:00
author: Panev
layout: post
categories:
  - centos
  - devops
  - linux
  - Monitoring
  - Prometheus
tags:
  - prometheus
  - systemd
---

<center>
<img src="https://raw.githubusercontent.com/rpanev/rpanev.github.io/master/static/img/_posts/Prometheus-Logo.jpg" alt="Prometheus " />
</center>

Как да инсталираме и конфигурираме Prometheus на Linux VM


# Инсталация от source 

1. Ъпдейт на репозиторито

{% highlight bash %}yum update -y
{% endhighlight %}

2. Отваряме страницата на проекта <a href="https://prometheus.io/download/" rel="noopener noreferrer" target="_blank">Prometheus</a> и сваляме последната катуална версия 

3. Сваляме нужнитя ни пакет

{% highlight bash %}cd /opt/
curl -LO https://github.com/prometheus/prometheus/releases/download/v2.12.0/prometheus-2.12.0.linux-amd64.tar.gz
tar -xvf prometheus-2.3.2.linux-amd64.tar.gz
mv prometheus-2.3.2.linux-amd64 prometheus-files
{% endhighlight %}

4. Създаваме потребител prometheus, нужните директории и техните права

{% highlight bash %}useradd --no-create-home --shell /bin/false prometheus
mkdir /etc/prometheus
mkdir /var/lib/prometheus
chown prometheus:prometheus /etc/prometheus
chown prometheus:prometheus /var/lib/prometheus
{% endhighlight %}

5. Копираме бинарните файлове от директория prometheus-files във /usr/bin/

{% highlight bash %}cp prometheus-files/prometheus /usr/local/bin/
cp prometheus-files/promtool /usr/local/bin/
chown prometheus:prometheus /usr/local/bin/prometheus
chown prometheus:prometheus /usr/local/bin/promtool
{% endhighlight %}

6. Преместваме директориите consoles и console_libraries в /etc/prometheus

{% highlight bash %}cp -r prometheus-files/consoles /etc/prometheus
cp -r prometheus-files/console_libraries /etc/prometheus
chown -R prometheus:prometheus /etc/prometheus/consoles
chown -R prometheus:prometheus /etc/prometheus/console_libraries
{% endhighlight %}

# Конфигурация на prometheus

Необходимата конфигурация се намира във файла /etc/prometheus/prometheus.yml.

1. Създаваме фйала prometheus.yml

{% highlight bash %}touch /etc/prometheus/prometheus.yml
{% endhighlight %}

2. Копирайте следните редове във вашият prometheus.yml файл.

{% highlight bash %}global:
  scrape_interval: 10s
 
scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']
{% endhighlight %}

3. Оправяме правата на файла:

{% highlight bash %}chown prometheus:prometheus /etc/prometheus/prometheus.yml
{% endhighlight %}

# Създаваме Prometheus Service файл

1. Създаваме фйала /etc/systemd/system/prometheus.service

{% highlight bash %}touch /etc/systemd/system/prometheus.service
{% endhighlight %}

2. Копирайте следните редове във вашият prometheus.service файл.

{% highlight bash %}[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target
 
[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries
 
[Install]
WantedBy=multi-user.target
{% endhighlight %}

3. Презареждаме systemd service и стартираме prometheus

{% highlight bash %}systemctl daemon-reload
systemctl start prometheus
{% endhighlight %}

# Access Prometheus Web UI

Достъп до Web UI може да направите на вашият IP и порт 9090

{% highlight bash %}http://&lt;prometheus-ip>:9090/graph
{% endhighlight %}