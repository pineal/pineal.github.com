---
layout: post
title: Start on Fedora
categories:
- Linux
tags:
- Fedora
---

##Installation
Set up Fedora is very easy, and very similiar to the method to install Ubuntu. Here I burned the img file to udisk so that it can be booted as loaded. [I made the bootable Udisk on my Mac.][1]

##Beauty
The default theme of Fedora is too ugly. Especially for the fonts. One of the most popular and beatiful theme I choosed here is Numix, to install it, input the following commands one by one in terminal:

```
sudo yum update
su -c "curl https://satya164.github.io/fedy/fedy-installer -o fedy-installer && chmod +x fedy-installer && ./fedy-installer"
sudo fedy -e numix_themes
sudo yum install gnome-tweak-tool
```

##Fast mirror point
A fast mirror point could be found and set:
```
sudo yum install yum-plugin-fastestmirror
```

##Github
Github is necessary for programmer:
```
sudo yum install git
```

##Dropbox
Here dropbox can be only cloned from terminal[Dropbox on linux install guidance ][2]:

```
cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -
~/.dropbox-dist/dropboxd
```

If you want to keep dropbox running in the background[3]:

```
($HOME/.dropbox-dist/dropboxd &)&
```

*Futher: Run this command once login.*
  
##Adobe
This link is for [Adobe Reader](http://www.if-not-true-then-false.com/2010/install-adobe-acrobat-pdf-reader-on-fedora-centos-red-hat-rhel/). The other link is for installing [Adobe Flash player and plugin in Firefox](https://ask.fedoraproject.org/en/question/10217/how-to-install-adobe-flash-on-fedora/).
  


  [1]: http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-mac-osx
  [2]: https://www.dropbox.com/install?os=lnx
  [3]: http://unix.stackexchange.com/questions/35624/how-to-run-dropbox-daemon-in-background
