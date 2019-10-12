---
id: 508
title: Bash script to create linux and mysql user
date: 2014-03-26T23:42:58+00:00
author: panev
layout: post
categories:
  - bash
  - linux
---
{% highlight bash %}
#!/bin/bash
MKP=/usr/bin/mkpasswd
echo "Creating user: $1"
PASSWD=`$MKP`
echo "Password: $PASSWD"
ENPASSWD="$MKP $PASSWD"
useradd $1 -p "$ENPASSWD"
echo "Creating mysql user & database for $1"
mysql -u root --password=your_mysql_password -e "CREATE DATABASE $1; CREATE USER $1 IDENTIFIED BY '$PASSWD'; GRANT ALL ON $1.* TO '$1'"
{% highlight bash %}


{% highlight shell %}
chmod +x lmuser.sh
./lmuser.sh users
{% highlight bash %}