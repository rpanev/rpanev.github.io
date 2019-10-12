---
id: 835
title: Raspberry pi Nginx
date: 2016-01-19T02:06:53+00:00
author: panev
layout: post
categories:
  - Linux
---
## Raspberry pi Nginx php5

**NGINX**: споделя файловете на уеб сайта от локалната директория в интернет. Върши същата работа като Apache2, но има статии и бенчмаркове, където показват и обясняват неговото по високо бързодействие от Apache2. Идеално е както за натоварени така и за малки сървъри и е доста “лек” като процес на действие а Apache2 по-тромаво, затова съм се спрял на него.  
**PHP5-FPM**: FPM (FastCGI Process Manager) е алтернативно PHP FastCGI изпълнение при обработката/смилането на PHP кода на сайта (например WordPress или Joomla) с допълнителни функции които са полезни за натоварени сайтове. При обикновения PHP, когато потребител зареди сайта, PHP процеса се включва, минава през процесора и се зарежда в RAM-та, където обработва PHP кода. Докато PHP-FPM е постоянно включен/работещ процес (daemon), който е постоянно тече през процесора и стои натоварен в RAM-та в изчакване на заявката за обработка на PHP кода на сайта. Затова и обработва кода по-бързо от обикновеното PHP понеже е постоянно включен дори и без заявки, докато нормалното PHP чака заявка, че да се включи след това като процес.  
**PHP-APC**: APC (Alternative PHP Cache) това е алтернативен PHP фреймуорк, който кешира в споделената памет за кратко време вече обработен PHP код и в случай че последно заредената PHP страница от заявка получи нова такава, то не се налага обработването на кода отново за кратък интервал от време а направо се изпраща вече готово заредения/смлян код. Това оптимизира бързината на PHP страницата/сайта допълнително и спестява процесорни и дискови ресурси. PHP-APC е бил използван от Facebook през 2014 което е спомогнало за функционалността на сайта натоварен от множеството потребители в пъти повече.  
**Varnish** – Varnish е HTTP ускорител, за разлика от PHP-APC (който кешира динамичните елементи на сайта) – Varnish е съсредоточен върху кеширането на статичните елементи в сайта, така една и съща заявка няма да бъде изпълнявана отново и отново а просто заявена веднъж – следващият път ще бъде предоставена директно от споделената памет, която всички знаем, че има по-добро бързодействие от диска/картата.  
Та това беше накратко за комбинацията от софтуер, който ще използваме за Уеб Сървър на Берито.  
Нека да започнем със същинската част.

1. Инсталация на софтуера.  
За да инсталираме целия тоя софтуер накуп изпълняваме следната команда:

{% highlight shell %}apt-get install nginx php5-fpm php-apc varnish -y{% endhighlight %}

2. Настройка на Nginx.  
Като за начало отваряте nginx.conf намиращ се в /etc/nginx/ с любимия ви текстов редактор, моя е nano:

{% highlight shell %}nano /etc/nginx/sites-enabled/default{% endhighlight %}

Намирате следния ред:

{% highlight shell %}# Add index.php to the list if you are using PHP
 index index.html index.htm index.nginx-debian.html;{% endhighlight %}

След което замените index.nginx-debian.html (или да го премахнете и добавите) index.php накрая.  
Трябва да изглежда така:

{% highlight shell %}# Add index.php to the list if you are using PHP
 index index.html index.htm index.php;{% endhighlight %}

След което намирате следните редове:

{% highlight shell %}# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
 #
 #location ~ \.php$ {
 # include snippets/fastcgi-php.conf;
 #
 # # With php5-cgi alone:
 # fastcgi_pass 127.0.0.1:9000;
 # # With php5-fpm:
 # fastcgi_pass unix:/var/run/php5-fpm.sock;
 #}
{% endhighlight %}

и точно след тях добавяте тези по-долу

{% highlight shell %}location ~\.php$ {
         fastcgi_pass unix:/var/run/php5-fpm.sock;
         fastcgi_split_path_info ^(.+\.php)(/.*)$;
         fastcgi_index index.php;
         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         fastcgi_param HTTPS off;
         try_files $uri =404;
         include fastcgi_params;
 }
}
{% endhighlight %}

а крайния резултат трябва да изглежда така:

{% highlight shell %}# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
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
{% endhighlight %}

За да тестваме конфигурацията ще трябва да изтрием всичко от /var/www/html и да направим index.php файл със съдържание <!--?php phpinfo(); ?-->.

{% highlight shell %}rm -rf /var/www/html/*
echo '<!--?php phpinfo(); ?-->' &gt;&gt; /var/www/html/index.php
service nginx start
service php5-fpm start{% endhighlight %}

3. Настройка на Varnish:

Отваряте /lib/systemd/system/varnish.service

{% highlight shell %}nano /lib/systemd/system/varnish.service{% endhighlight %}

и редактирате порта на който да листва Varnish от 6081 на 80, който е по подразбиране за уеб като цяло

{% highlight shell %}ExecStart=/usr/sbin/varnishd -a :6081 -T localhost:6082 -f /etc/varnish/default.vcl -S /etc/varnish/secret -s malloc,256m{% endhighlight %}

да изглежда така

{% highlight shell %}ExecStart=/usr/sbin/varnishd -a :80 -T localhost:6082 -f /etc/varnish/default.vcl -S /etc/varnish/secret -s malloc,256m{% endhighlight %}

Активирате промените да влязат в сила

{% highlight shell %}systemctl daemon-reload{% endhighlight %}

След което спирате Nginx, който вече листва на порт 80 за да не стане конфликт при стартирането на Varnish

{% highlight shell %}service nginx stop{% endhighlight %}

отваряте файла /etc/nginx/sites-enabled/default

{% highlight shell %}nano /etc/nginx/sites-enabled/default{% endhighlight %}

и редактирате следните два реда

{% highlight shell %}# Default server configuration
#
server {
 listen 80 default_server;
 listen [::]:80 default_server;{% endhighlight %}

на

{% highlight shell %}# Default server configuration
#
server {
 listen 8080 default_server;
 listen [::]:8080 default_server;{% endhighlight %}

след което пускате Varnish и Nginx отново. Последно редактирате

{% highlight shell %}service nginx start
service varnish start{% endhighlight %}

И накрая нагласяте Nginx, Varnish и PHP-FPM да стартират при зареждането на системата автоматично всеки път:

{% highlight shell %}chkconfig nginx on
chkconfig varnish on
chkconfig php5-fpm on{% endhighlight %}

За да проверите дали Varnish функционира, може да направите това през конзолата като изпълнете:

{% highlight shell %}curl -I localhost
HTTP/1.1 200 OK
Server: nginx/1.6.2
Date: Tue, 05 Jan 2016 20:28:37 GMT
Content-Type: text/html; charset=UTF-8
X-Varnish: 12
Age: 0
Via: 1.1 varnish-v4
Connection: keep-alive
{% endhighlight %}

Ако нямате curl пакет, може да го инсталирате лесно със

{% highlight shell %}apt-get install curl -y{% endhighlight %}

И все пак ако не запали от първия път, рестартирайте Varnish със

{% highlight shell %}service varnish restart{% endhighlight %}

Инсталиране на MySQL и PhpMyAdmin

Стартиране на инсталацията:

{% highlight shell %}apt-get install mariadb-server php5-mysql phpmyadmin -y{% endhighlight %}

По време на инсталацията ще бъдете подканени да въведете root парола за MySQL два пъти.  
След което ще бъдете подканени да въведете отново два пъти паролата за root на MySQL, но този път за конфигурацията на PhpMyAdmin.

На въпроса:

{% highlight shell %}[ ] apache2
[ ] lighttpd{% endhighlight %}

Не избирате нищо, просто си продължавате нататък.

След като завърши инсталацията стартирате MySQL и го настройвате да се пуска при всяко зареждане на системата с Nginx, Php-Fpm и Varnish:

{% highlight shell %}service mysql start
chkconfig mysql on
{% endhighlight %}

Това е!