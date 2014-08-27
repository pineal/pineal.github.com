---
layout: post
title: Start on Fedora
categories: 
- Linux
tags： 
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
```
sudo yum install git
```

##Dropbox
[Link of Dropbox on linux install guidance ][2]:
```
cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -
~/.dropbox-dist/dropboxd
```
If you want to keep dropbox running in the background[3]:
```
($HOME/.dropbox-dist/dropboxd &)&
```
*Futher: Run this command once login*
  

  


  [1]: http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-mac-osx
  [2]: https://www.dropbox.com/install?os=lnx
  [3]: http://unix.stackexchange.com/questions/35624/how-to-run-dropbox-daemon-in-background