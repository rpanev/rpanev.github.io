---
id: 508
title: Bash script to create linux and mysql user
date: 2014-03-26T23:42:58+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog//?p=508
permalink: /bash-script-to-create-linux-and-mysql-user.html
tie_views:
  - "204"
image: /wp-content/uploads/2014/02/bash-e1414872639539.png
categories:
  - code
  - Linux
---
<pre>#!/bin/bash
MKP=/usr/bin/mkpasswd
echo "Creating user: $1"
PASSWD=`$MKP`
echo "Password: $PASSWD"
ENPASSWD="$MKP $PASSWD"
useradd $1 -p "$ENPASSWD"
echo "Creating mysql user & database for $1"
mysql -u root --password=your_mysql_password -e "CREATE DATABASE $1; CREATE USER $1 IDENTIFIED BY '$PASSWD'; GRANT ALL ON $1.* TO '$1'"
</pre>

<pre>chmod +x lmuser.sh
./lmuser.sh users
</pre>