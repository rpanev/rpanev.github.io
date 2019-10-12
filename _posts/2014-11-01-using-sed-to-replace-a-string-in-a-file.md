---
id: 789
title: Using sed to replace a string in a file
date: 2014-11-01T22:11:19+00:00
author: panev
layout: post
guid: http://panevinfo.eu/blog/?p=789
permalink: /using-sed-to-replace-a-string-in-a-file.html
wp_review_desc_title:
  - Summary
wp_review_location:
  - bottom
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
tie_views:
  - "203"
image: /wp-content/uploads/2014/02/bash.jpg
categories:
  - Разни
---
The ‘sed’ command can be used to easily replace all occurrences of one string in a text file, with another string.

<pre>sed -i "s/string/replacestring/g" /path/to/file.txt

</pre>

This command will find all occurrences of “string” and replace them on the fly with “replacestring” in the file “/path/to/file.txt”.

The “-i” command switch specifies to edit the file in place, otherwise it will just display the output to the standard output. Run it without the “-i” switch to ensure it runs correctly, and makes the change you want, to prevent accidentally messing up your file.

You can also replace the whole line depending on your regular expression.

<pre>sed -i "s/^myhostname.*/myhostname = your.hostname.here/g" /etc/postfix/main.cf


</pre>

Assuming you have root permissions, this will replace the whole “myhostname” line from /etc/postfix/main.cf, and replace it with “myhostname = your.hostname.here”. You could have simply matched against the existing value, but if you wanted to script something to run on many machines, you are better off matching the beginning of the config line, as you wont always know the exact value you want to change.