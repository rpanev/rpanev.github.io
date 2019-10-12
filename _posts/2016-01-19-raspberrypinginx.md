---
id: 835
title: Raspberry pi Nginx
date: 2016-01-19T02:06:53+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog/?p=835
permalink: /raspberrypinginx.html
wp_review_location:
  - bottom
wp_review_desc_title:
  - Резюме
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
post_views_count:
  - "1"
wp_review_user_review_type:
  - star
tie_views:
  - "4067"
image: /wp-content/uploads/2016/01/Raspberry-Pi-Logo1.jpg
categories:
  - Linux
---
## Raspberry pi Nginx php5

<img class="alignleft size-full wp-image-843" src="https://www.panevinfo.eu/wp-content/uploads/2016/01/raspbian-logo.jpg" alt="raspbian-logo" width="428" height="109" srcset="https://www.panevinfo.eu/wp-content/uploads/2016/01/raspbian-logo.jpg 428w, https://www.panevinfo.eu/wp-content/uploads/2016/01/raspbian-logo-300x76.jpg 300w" sizes="(max-width: 428px) 100vw, 428px" />  
**NGINX**: споделя файловете на уеб сайта от локалната директория в интернет. Върши същата работа като Apache2, но има статии и бенчмаркове, където показват и обясняват неговото по високо бързодействие от Apache2. Идеално е както за натоварени така и за малки сървъри и е доста “лек” като процес на действие а Apache2 по-тромаво, затова съм се спрял на него.  
**PHP5-FPM**: FPM (FastCGI Process Manager) е алтернативно PHP FastCGI изпълнение при обработката/смилането на PHP кода на сайта (например WordPress или Joomla) с допълнителни функции които са полезни за натоварени сайтове. При обикновения PHP, когато потребител зареди сайта, PHP процеса се включва, минава през процесора и се зарежда в RAM-та, където обработва PHP кода. Докато PHP-FPM е постоянно включен/работещ процес (daemon), който е постоянно тече през процесора и стои натоварен в RAM-та в изчакване на заявката за обработка на PHP кода на сайта. Затова и обработва кода по-бързо от обикновеното PHP понеже е постоянно включен дори и без заявки, докато нормалното PHP чака заявка, че да се включи след това като процес.  
**PHP-APC**: APC (Alternative PHP Cache) това е алтернативен PHP фреймуорк, който кешира в споделената памет за кратко време вече обработен PHP код и в случай че последно заредената PHP страница от заявка получи нова такава, то не се налага обработването на кода отново за кратък интервал от време а направо се изпраща вече готово заредения/смлян код. Това оптимизира бързината на PHP страницата/сайта допълнително и спестява процесорни и дискови ресурси. PHP-APC е бил използван от Facebook през 2014 което е спомогнало за функционалността на сайта натоварен от множеството потребители в пъти повече.  
**Varnish** – Varnish е HTTP ускорител, за разлика от PHP-APC (който кешира динамичните елементи на сайта) – Varnish е съсредоточен върху кеширането на статичните елементи в сайта, така една и съща заявка няма да бъде изпълнявана отново и отново а просто заявена веднъж – следващият път ще бъде предоставена директно от споделената памет, която всички знаем, че има по-добро бързодействие от диска/картата.  
Та това беше накратко за комбинацията от софтуер, който ще използваме за Уеб Сървър на Берито.  
Нека да започнем със същинската част.

1. Инсталация на софтуера.  
За да инсталираме целия тоя софтуер накуп изпълняваме следната команда:

<pre>apt-get install nginx php5-fpm php-apc varnish -y</pre>

2. Настройка на Nginx.  
Като за начало отваряте nginx.conf намиращ се в /etc/nginx/ с любимия ви текстов редактор, моя е nano:

<pre>nano /etc/nginx/sites-enabled/default</pre>

Намирате следния ред:

<pre># Add index.php to the list if you are using PHP
 index index.html index.htm index.nginx-debian.html;</pre>

След което замените index.nginx-debian.html (или да го премахнете и добавите) index.php накрая.  
Трябва да изглежда така:

<pre># Add index.php to the list if you are using PHP
 index index.html index.htm index.php;</pre>

След което намирате следните редове:

<pre># pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
 #
 #location ~ \.php$ {
 # include snippets/fastcgi-php.conf;
 #
 # # With php5-cgi alone:
 # fastcgi_pass 127.0.0.1:9000;
 # # With php5-fpm:
 # fastcgi_pass unix:/var/run/php5-fpm.sock;
 #}
</pre>

и точно след тях добавяте тези по-долу

<pre>location ~\.php$ {
         fastcgi_pass unix:/var/run/php5-fpm.sock;
         fastcgi_split_path_info ^(.+\.php)(/.*)$;
         fastcgi_index index.php;
         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         fastcgi_param HTTPS off;
         try_files $uri =404;
         include fastcgi_params;
 }
}
</pre>

а крайния резултат трябва да изглежда така:

<pre># pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #       include snippets/fastcgi-php.conf;
        #
        #       # With php5-cgi alone:
        #       fastcgi_pass 127.0.0.1:9000;
        #       # With php5-fpm:
        #       fastcgi_pass unix:/var/run/php5-fpm.sock;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #       deny all;
        #}

         location ~\.php$ {
         fastcgi_pass unix:/var/run/php5-fpm.sock;
         fastcgi_split_path_info ^(.+\.php)(/.*)$;
         fastcgi_index index.php;
         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         fastcgi_param HTTPS off;
         try_files $uri =404;
         include fastcgi_params;
 }
}
</pre>

За да тестваме конфигурацията ще трябва да изтрием всичко от /var/www/html и да направим index.php файл със съдържание <!--?php phpinfo(); ?-->.

<pre>rm -rf /var/www/html/*
echo '<!--?php phpinfo(); ?-->' &gt;&gt; /var/www/html/index.php
service nginx start
service php5-fpm start</pre>

3. Настройка на Varnish:

Отваряте /lib/systemd/system/varnish.service

<pre>nano /lib/systemd/system/varnish.service</pre>

и редактирате порта на който да листва Varnish от 6081 на 80, който е по подразбиране за уеб като цяло

<pre>ExecStart=/usr/sbin/varnishd -a :6081 -T localhost:6082 -f /etc/varnish/default.vcl -S /etc/varnish/secret -s malloc,256m</pre>

да изглежда така

<pre>ExecStart=/usr/sbin/varnishd -a :80 -T localhost:6082 -f /etc/varnish/default.vcl -S /etc/varnish/secret -s malloc,256m</pre>

Активирате промените да влязат в сила

<pre>systemctl daemon-reload</pre>

След което спирате Nginx, който вече листва на порт 80 за да не стане конфликт при стартирането на Varnish

<pre>service nginx stop</pre>

отваряте файла /etc/nginx/sites-enabled/default

<pre>nano /etc/nginx/sites-enabled/default</pre>

и редактирате следните два реда

<pre># Default server configuration
#
server {
 listen 80 default_server;
 listen [::]:80 default_server;</pre>

на

<pre># Default server configuration
#
server {
 listen 8080 default_server;
 listen [::]:8080 default_server;</pre>

след което пускате Varnish и Nginx отново. Последно редактирате

<pre>service nginx start
service varnish start</pre>

И накрая нагласяте Nginx, Varnish и PHP-FPM да стартират при зареждането на системата автоматично всеки път:

<pre>chkconfig nginx on
chkconfig varnish on
chkconfig php5-fpm on</pre>

За да проверите дали Varnish функционира, може да направите това през конзолата като изпълнете:

<pre>curl -I localhost
HTTP/1.1 200 OK
Server: nginx/1.6.2
Date: Tue, 05 Jan 2016 20:28:37 GMT
Content-Type: text/html; charset=UTF-8
X-Varnish: 12
Age: 0
Via: 1.1 varnish-v4
Connection: keep-alive
</pre>

Ако нямате curl пакет, може да го инсталирате лесно със

<pre>apt-get install curl -y</pre>

И все пак ако не запали от първия път, рестартирайте Varnish със

<pre>service varnish restart</pre>

Инсталиране на MySQL и PhpMyAdmin

Стартиране на инсталацията:

<pre>apt-get install mariadb-server php5-mysql phpmyadmin -y</pre>

По време на инсталацията ще бъдете подканени да въведете root парола за MySQL два пъти.  
След което ще бъдете подканени да въведете отново два пъти паролата за root на MySQL, но този път за конфигурацията на PhpMyAdmin.

На въпроса:

<pre>[ ] apache2
[ ] lighttpd</pre>

Не избирате нищо, просто си продължавате нататък.

След като завърши инсталацията стартирате MySQL и го настройвате да се пуска при всяко зареждане на системата с Nginx, Php-Fpm и Varnish:

<pre>service mysql start
chkconfig mysql on
</pre>

Това е!

Вижте още  
[FreeBSD под Raspberry Pi](http://panevinfo.eu/blog/%d0%ba%d0%b0%d0%ba-%d0%b4%d0%b0-%d0%b8%d0%bd%d1%81%d1%82%d0%b0%d0%bb%d0%b8%d1%80%d0%b0%d0%bc-freebsd-11-%d0%bd%d0%b0-raspberry-pi-2-model-b.html)  
[NAS сървър](http://panevinfo.eu/blog/%d0%ba%d0%b0%d0%ba-%d0%b4%d0%b0-%d1%81%d0%b8-%d0%bd%d0%b0%d0%bf%d1%80%d0%b0%d0%b2%d0%b8%d0%bc-nas-%d1%81%d1%8a%d1%80%d0%b2%d1%8a%d1%80.html)