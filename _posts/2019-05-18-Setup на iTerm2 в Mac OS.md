---
id: 31
title: Setup на iTerm2 в Mac OS
date: 2019-05-18T22:02:39+00:00
author: Panev
layout: post
#guid: https://www.rpanev.pro/?p=31
#permalink: '/apple/macos/setup-%d0%bd%d0%b0-iterm2-%d0%b2-mac-os.html'
image: /wp-content/uploads/2019/08/MACOS-LOGO.png
categories:
  - macos
tags:
  - apple
  - iTerm2
  - MacOS
---
&nbsp;

# Setup на iTerm2 в Mac OS

&nbsp;

Принципно терминла <a href="https://www.iterm2.com/" target="_blank" rel="noopener noreferrer">iTerm2</a> за Mac OS е изключително гъвкъв, функционален и много удобен за работа. Има доста възможности за къстъм конфигурация. Въпреки това реших да разнообразя малко като си направих малко промени, като изнеса част от конфигураця в оделни файлове с цел по-лесно управление. Разделих .bash\_profile на две основни части .bash\_prompt и .aliases. Както показват имената на файловете в единият е конфигурацоята на bash терминала а в другият всички къстъм команди, който съм създал за мое улеснение по време на работа с терминала

<!--more-->

Редактирах първо съществуващият файл **.bash_profile** като кометирах всики линий вътре и добавих следните:

{% highlight bash %}
############# LOAD BASH PROM; ALIASES and FUNCTIONS ###################

for file in ~/.{path,bash_prompt,exports,aliases,functions,extra}; 

do[ -r "$file" ] && source "$file"doneunset file

#################### THE END ####################
{% endhighlight %}

Създадох файла **.bash_prompt** като добавих следните редове код, където изнесох самата конфигурация на bash терминала ми

{% highlight bash %}
if [[ $COLORTERM = gnome-* && $TERM = xterm ]] && infocmp gnome-256color &gt;/dev/null 2&gt;&1; then
export TERM=gnome-256color
elif infocmp xterm-256color &gt;/dev/null 2&gt;&1; then
export TERM=xterm-256color
fi

if tput setaf 1 &&gt; /dev/null; then
tput sgr0
if [[ $(tput colors) -ge 256 ]] 2&gt;/dev/null; then
# Changed these colors to fit Solarized theme
MAGENTA=$(tput setaf 125)
ORANGE=$(tput setaf 166)
GREEN=$(tput setaf 64)
PURPLE=$(tput setaf 61)
WHITE=$(tput setaf 244)
else
MAGENTA=$(tput setaf 5)
ORANGE=$(tput setaf 4)
GREEN=$(tput setaf 2)
PURPLE=$(tput setaf 1)
WHITE=$(tput setaf 7)
fi
BOLD=$(tput bold)
RESET=$(tput sgr0)
else
MAGENTA="\033[1;31m"
ORANGE="\033[1;33m"
GREEN="\033[1;32m"
PURPLE="\033[1;35m"
WHITE="\033[1;37m"
BOLD=""
RESET="\033[m"
fi

export MAGENTA
export ORANGE
export GREEN
export PURPLE
export WHITE
export BOLD
export RESET

function parse_git_dirty() {
[[ $(git status 2&gt; /dev/null | tail -n1) != *"working directory clean"* ]] && echo "*"
}

function parse_git_branch() {
git branch --no-color 2&gt; /dev/null | sed -e '/^[^*]/d' -e "s/* \(.*\)/\1$(parse_git_dirty)/"
}

export PS1="\[${BOLD}${MAGENTA}\]\u \[$WHITE\]at \[$ORANGE\]\h \[$WHITE\]in \[$GREEN\]\w\[$WHITE\]\$([[ -n \$(git branch 2&gt; /dev/null) ]] && echo \" on \")\[$PURPLE\]\$(parse_git_branch)\[$WHITE\]\n\$ \[$RESET\]"
export PS2="\[$ORANGE\]→ \[$RESET\]"

{% endhighlight %}

Създадох и файл **.aliases** като добавих всички мой алиаси

{% highlight bash %}
# Detect which `ls` flavor is in use
if ls --color &gt; /dev/null 2&gt;&1; then # GNU `ls`
colorflag="--color"
else # OS X `ls`
colorflag="-G"
fi&lt;/code>

# List all files colorized in long format
alias ll='ls -lh'

# List all files colorized in long format, including dot files
alias la="ls -lha"

# List only directories
alias lsd='ls -l | grep "^d"'

#lr: Full Recursive Directory Listing
alias lr='ls -R | grep ":$" | sed -e '\''s/:$//'\'' -e '\''s/[^-][^\/]*\//--/g'\'' -e '\''s/^/ /'\'' -e '\''s/-/|/'\'' | less'

#zipf: To create a ZIP archive of a folder
zipf () { zip -r "$1".zip "$1" ; }

#cpuHogs: Find CPU hogs
alias cpu_hogs='ps -o pid,stat,%cpu,time,command | head -10'

# my IP
alias myip="dig +short myip.opendns.com @resolver1.opendns.com"
alias localip="ipconfig getifaddr en0"

# ii: display useful host related informaton
# -------------------------------------------------------------------
ii() {
echo -e "\nYou are logged on ${RED}$HOST"
echo -e "\nAdditionnal information:$NC " ; uname -a
echo -e "\n${RED}Users logged on:$NC " ; w -h
echo -e "\n${RED}Current date :$NC " ; date
echo -e "\n${RED}Machine stats :$NC " ; uptime
echo -e "\n${RED}Current network location :$NC " ; scselect
echo -e "\n${RED}Public facing IP Address :$NC " ;myip
#echo -e "\n${RED}DNS Configuration:$NC " ; scutil --dns
echo
}
# cleanupDS: Recursively delete .DS_Store files
# -------------------------------------------------------------------
alias cleanupDS="find . -type f -name '*.DS_Store' -ls -delete"

# cleanupLS: Clean up LaunchServices to remove duplicates in the "Open With" menu
# -----------------------------------------------------------------------------------
alias cleanupLS="/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -r -domain local -domain system -domain user && killall Finder"

alias nano='vim'
alias vi='vim'
alias l='ls -CF'
alias ps='ps aux'
alias grep='grep --color=auto'

#Sublime
alias subl='/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl'

#pCloud Driver
alias pcloud='cd pCloud\ Drive/'

# Always use color output for `ls`
alias ls="command ls ${colorflag}"
export LS_COLORS='no=00:fi=00:di=01;34:ln=01;36:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arj=01;31:*.taz=01;31:*.lzh=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.gz=01;31:*.bz2=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.jpg=01;35:*.jpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.avi=01;35:*.fli=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.ogg=01;35:*.mp3=01;35:*.wav=01;35:'

# extract: Extract most know archives with one command
# ---------------------------------------------------------
extract () {
if [ -f $1 ] ; then
case $1 in
*.tar.bz2) tar xjf $1 ;;
*.tar.gz) tar xzf $1 ;;
*.bz2) bunzip2 $1 ;;
*.rar) unrar e $1 ;;
*.gz) gunzip $1 ;;
*.tar) tar xf $1 ;;
*.tbz2) tar xjf $1 ;;
*.tgz) tar xzf $1 ;;
*.zip) unzip $1 ;;
*.Z) uncompress $1 ;;
*.7z) 7z x $1 ;;
*) echo "'$1' cannot be extracted via extract()" ;;
esac
else
echo "'$1' is not a valid file"
fi
}
{% endhighlight %}

На фина и файл **.gitconfig**

{% highlight bash %}
[alias]
# Show verbose output about tags, branches or remotes
tags = tag -l
branches = branch -a
remotes = remote -v
# Pretty log output
hist = log --graph --pretty=format:'%Cred%h%Creset %s%C(yellow)%d%Creset %Cgreen(%cr)%Creset [%an]' --abbrev-commit --date=relative&lt;/code>

[color]
# Use colors in Git commands that are capable of colored output when outputting to the terminal
ui = auto
[color "branch"]
current = yellow reverse
local = yellow
remote = green
[color "diff"]
meta = yellow bold
frag = magenta bold
old = red bold
new = green bold
[color "status"]
added = yellow
changed = green
untracked = cyan

# Use `origin` as the default remote on the `master` branch in all cases
[branch "master"]
remote = origin
merge = refs/heads/master

{% endhighlight %}

Използвах също така и темата <a href="https://ethanschoonover.com/solarized/" target="_blank" rel="noopener noreferrer">Solarized</a>, която е по-желание 🙂

Крайният резултат е :<figure id="attachment_32" aria-describedby="caption-attachment-32" style="width: 1180px" class="wp-caption alignnone">

<img class="size-full wp-image-32" src="https://www.rpanev.pro/wp-content/uploads/2019/05/my-iterm2.jpeg" alt="Setup на iTerm2 в Mac OS" width="1180" height="572" srcset="https://www.rpanev.pro/wp-content/uploads/2019/05/my-iterm2.jpeg 1180w, https://www.rpanev.pro/wp-content/uploads/2019/05/my-iterm2-300x145.jpeg 300w, https://www.rpanev.pro/wp-content/uploads/2019/05/my-iterm2-768x372.jpeg 768w, https://www.rpanev.pro/wp-content/uploads/2019/05/my-iterm2-1024x496.jpeg 1024w" sizes="(max-width: 1180px) 100vw, 1180px" /> Setup на iTerm2 в Mac OS