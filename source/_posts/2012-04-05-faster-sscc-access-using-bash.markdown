---
layout: post
title: "Faster SSCC  Access using Bash"
date: 2012-04-05 19:08
comments: true
categories: [SSCC, bash, X11, RStudio, R]
---

I use SSH regularly to login remotely to servers for experiments and 
data analysis.  For instance, [Northwestern's Social Sciences Computing 
Cluster](http://www.it.northwestern.edu/research/sscc/) is available with an 
SSH remote login and using [X11](https://en.wikipedia.org/wiki/X_Window_System) 
forwarding, I can access [RStudio](http://www.rstudio.org/) and run analyses 
that require more memory than my office iMac has.  However, logging into the 
SSCC over SSH isn't as quick and launching a program in Spotlight.

While browsing a friend's .bashrc on Github, I realized I could 
use a simple Bash function to speed things up.  Copy and paste the following 
into Terminal:
``` bash Launching RStudio Remotely over SSH
echo "function Rsscc() {
    ssh  -c arcfour,blowfish-cbc \
        -XC netid@hardin.it.northwestern.edu rstudio
    wait $1
    exit 0
}" >> ~/.profile
```
After you restart Terminal.app, you can launch RStudio remotely by typing 
`Rsscc`, or whatever you renamed my function to.  In principle, you could also 
create a simple menu for choosing among multiple servers or programs using a bit of
[read](http://ss64.com/bash/read.html) and 
[case](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_03.html).
``` bash Creating a Simple Command Menu
task_menu () {
cat << EOF
$(tput setaf 5)Remote Login$(tput sgr0)
$(tput setaf 5)============$(tput sgr0)

Please choose one of the following:

1) RStudio
2) Stata

EOF
	read -r choice
	case "$choice" in
		1) task="rstudio" ;;
		2) task="xstata" ;;
		*) echo "Please choose a number!" && task_menu ;;
	esac
fi
ssh -c arcfour,blowfish-cbc \
    -XC netid@hardin.it.northwestern.edu $task
    wait $1
    exit 0
}
```

*Note:*  This works best if you're using an up-to-date version of X11, such as 
[XQuartz](http://xquartz.macosforge.org/trac/wiki) and are accessing the SSCC 
using Ethernet.
