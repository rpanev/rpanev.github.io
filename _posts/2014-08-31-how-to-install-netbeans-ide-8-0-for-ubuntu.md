---
id: 732
title: How to install NetBeans IDE 8.0 for Ubuntu
date: 2014-08-31T00:16:27+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog/?p=732
permalink: /how-to-install-netbeans-ide-8-0-for-ubuntu.html
tie_views:
  - "295"
categories:
  - Linux
---
First you need to download <a href="https://netbeans.org/downloads/" title="NetBeans" target="_blank">NetBeans</a>

Second need <a href="http://www.java.com" title="Java" target="_blank">JAVA</a>

How to &#8230;  
<!--more-->

Installing Java with apt-get is easy. First, update the package index:

<pre>sudo apt-get update</pre>

Then, check if Java is not already installed:

<pre>java -version</pre>

If it returns &#8222;The program java can be found in the following packages&#8220;, Java hasn&#8217;t been installed yet, so execute the following command:

<pre>sudo apt-get install default-jre</pre>

This will install the Java Runtime Environment (JRE). If you instead need the Java Development Kit (JDK), which is usually needed to compile Java applications execute the following command:

<pre>sudo apt-get install default-jdk</pre>

Installing OpenJDK 7 (optional)

To install OpenJDK 7, execute the following command:

<pre>sudo apt-get install openjdk-7-jre</pre>

This will install the Java Runtime Environment (JRE). If you instead need the Java Development Kit (JDK), execute the following command:

<pre>sudo apt-get install openjdk-7-jdk</pre>

That is everything that is needed to install Java.

Now just run the command :

<pre>chmod +x netbeans-8.0-linux.sh</pre>

and run netbeans to install :

<pre>./netbeans-8.0-linux.sh </pre>