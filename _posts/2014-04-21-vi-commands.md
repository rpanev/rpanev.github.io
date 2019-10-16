---
id: 24
title: vi commands
date: 2014-04-21T12:20:23+00:00
author: panev
layout: post

categories:
  - FreeBSD
  - Linux
---
%vi + name &#8230; at end  
%vi -r list saved files  
%vi -r name recover file name  
%vi name &#8230; edit first;rest via :n  
%vi -t tag start at tag  
%vi +/pat name search for pat  
% view name read only mode  
ZZ save and exit from vi  
CTRL-Z stop vi, don&#8217;t exit 

File Manipulation  
&#8211;&#8211;&#8211;&#8211;&#8211;&#8211;  
:w write back changes  
:wq write and quit  
:q quit  
:q! quit, discard changes  
:e name edit file name  
:e! reedit discard changes  
:e + name edit starting at end  
:e +n name edit starting at line n  
:e # edit alternate file  
CTRl-^ synonym for :e #  
:r(name) paste file name starting at current position.  
:w(name) write file name  
:w! name overwrite file name  
:sh run shell, then return  
:!cmd run cmd, then return  
:n edit next file in arglist  
:n args specify new arglist  
:f show current file and line.  
CTRL-G synonym for :f  
:ta tag to tag file entry tag  
CTRL-] :ta, following word tag

The Display  
&#8211;&#8211;&#8211;&#8211;  
Last line Error mesg, echoing input to :/? and !, feedback about i/o and large changes  
@ lines On screen only, not in file  
~lines Lines past end of file  
CTRL-x Control characters, DEL is delete.  
tabs Expand to spaces, cursor at last.

Vi Modes  
&#8211;&#8211;&#8211;  
Command Normal and initial state. Others return here. Esc (escape) cancels partial command. 

Insert Entered by a i A I O o c C s S R. Arbitrary test then terminates with ESC character, or abnormally with interrupt

Last line Reading input for :/? or !; terminate with ESC or CR to execute. interrupt to

m cancel. 

Positioning within File  
&#8211;&#8211;&#8211;&#8211;&#8211;&#8211;&#8211;&#8211;  
CTRL-F forward screenfull  
CTRL-B backward screenfull  
CTRL-D scroll down half screen  
CTRL-U scroll up half screen  
G goto line (end default)  
/pattern next line matching pattern  
?pattern prev line matching pattern  
n repeat last / or ?  
N reverse last / or ?  
/pat/+n n&#8217;th line after pat  
?pat?-n n&#8217;th line befor pat  
]] next section/function  
[[ previous section/function  
% find matching () { or }  
