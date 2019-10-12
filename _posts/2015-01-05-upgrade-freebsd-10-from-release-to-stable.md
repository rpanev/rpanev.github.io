---
id: 797
title: Upgrade FreeBSD 10 from RELEASE to STABLE
date: 2015-01-05T01:20:17+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog/?p=797
permalink: /upgrade-freebsd-10-from-release-to-stable.html
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
wp_review_user_review_type:
  - star
tie_views:
  - "225"
image: /wp-content/uploads/2016/01/freebsdlogo.jpg
categories:
  - Разни
---
## Upgrade FreeBSD 10 from RELEASE to STABLE

What is STABLE ?

The name &#8222;-STABLE&#8220; is frequently misunderstood. It does not mean solid or steady. -STABLE means that while code can change, the ABI (Application Binary Interface) will remain stable and not change. Programs compiled to run on FreeBSD 9.0-RELEASE, or 9.1-RELEASE, or 9.2-RELEASE will continue to work on FreeBSD 9-STABLE. Effectively, -STABLE is the latest version of FreeBSD you can get without breaking installed software.  
<!--more-->

Procedure  
Move the current source code to a backup folder to be sure to get only -STABLE code:

<pre>mv /usr/src /usr/src-RELEASE
</pre>

Do the same thing for the ports tree:

<pre>mv /usr/ports /usr/ports-RELEASE
</pre>

When I did not moved the **/usr/src** folder and continued this article, everytime I would be back in 10.0-RELEASE&#8230;

Check out the STABLE source code:

<pre>svn checkout https://svn0.us-west.freebsd.org/base/stable/10 /usr/src
</pre>

Also for the ports tree:

<pre>svn checkout https://svn0.us-west.freebsd.org/ports/head /usr/ports
</pre>

Then cd in to the correct folder

<pre>cd /usr/src
</pre>

Build the world:

<pre>make buildworld -j4
</pre>

The -j 4 part means that it should run 4 jobs at once. I have a quad core CPU so all the cores will be used.

Build the kernel:

<pre>make buildkernel KERNCONF=MYKERNEL
make installkernel KERNCONF=MYKERNEL
</pre>

Now reboot in to the new kernel:

<pre>shutdown -r now
</pre>

Next, it was time to install the world. However, **make installworld** complained:

<pre>ERROR: Required unbound user is missing, see /usr/src/UPDATING.
*** Error code 1

Stop.
make[1]: stopped in /usr/src
*** Error code 1

Stop.
make: stopped in /usr/src
</pre>

**/usr/src/UPDATING** to the rescue:

<pre>With the addition of unbound(8), a new unbound user is now
        required during installworld.  "mergemaster -p" can be used to
        add the user prior to installworld, as documented in the handbook.
</pre>

However, **mergemaster -p** did not create the user:

<pre>mergemaster -p

*** Creating the temporary root environment in /var/tmp/temproot
 *** /var/tmp/temproot ready for use
 *** Creating and populating directory structure in /var/tmp/temproot



*** Beginning comparison

 *** Temp ./etc/group and installed have the same Id, deleting
 *** Temp ./etc/master.passwd and installed have the same Id, deleting

*** Comparison complete

*** /var/tmp/temproot is empty, deleting
</pre>

I already am on FreeBSD 10, but this box is updated from 8 to 9 to 10, so maybe that didn&#8217;t work out quite well. Installing unbound via pkg did work:

<pre>pkg install unbound
</pre>

It seemed that it was half done:

<pre>Proceed with installing packages [y/N]: y
ldns-1.6.17.txz 
unbound-1.4.22.txz
Checking integrity... done
[1/2] Installing ldns-1.6.17... done
[2/2] Installing unbound-1.4.22...===> Creating users and/or groups.
Using existing group 'unbound'.
Creating user 'unbound' with uid '59'.
 done
</pre>

Oh wel&#8230; Now the **make installworld** continues;

<pre>make installworld
</pre>

After that finished we can do another mergemaster:

<pre>mergemaster -Ui 
</pre>

Time to reboot:

<pre>shutdown -r now
</pre>

Remove old files and libraries:

<pre>cd /usr/src
make check-old
>>> Checking for old files
>>> Checking for old libraries
>>> Checking for old directories
To remove old files and directories run 'make delete-old'.
To remove old libraries run 'make delete-old-libs'.

make delete-old
>>> Removing old files (only deletes safe to delete libs)
remove /usr/include/clang/3.3/__wmmintrin_aes.h? y
remove /usr/include/clang/3.3/__wmmintrin_pclmul.h? y
remove /usr/include/clang/3.3/altivec.h? y
remove /usr/include/clang/3.3/ammintrin.h? y
[...]
>>> Old files removed
>>> Removing old directories
>>> Old directories removed
To remove old libraries run 'make delete-old-libs'.



make delete-old-libs
>>> Removing old libraries
Please be sure no application still uses those libraries, else you
can not start such an application. Consult UPDATING for more
information regarding how to cope with the removal/revision bump
of a specific library.
>>> Old libraries removed
</pre>

Note that freebsd-update does not work with the STABLE branch. Therefore this process is required&#8230;

That&#8217;s it. Run **freebsd-version** to see that you are now on stable:

<pre>freebsd-version
10.0-STABLE
</pre>

Обвързани статии :

[How to upgrade FreeBSD 10 from RELEASE to STABLE](http://panevinfo.eu/blog/how-to-upgrade-freebsd-10-from-release-to-stable/)

[How To Tail Multiple Files on UNIX / Linux Console](http://panevinfo.eu/blog/how-to-tail-multiple-files-on-unix-linux-console/)

[How to install FreeBSD 10 Step by Step](http://panevinfo.eu/blog/how-to-upgrade-freebsd-10-from-release-to-stable/)