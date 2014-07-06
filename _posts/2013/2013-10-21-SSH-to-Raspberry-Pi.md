---
layout: post
title: Using SSH to access Raspberry Pi
categories:
- Raspberry Pi
tags:
- Raspberry Pi
- SSH
---

We will using SSH to control pI remotely without a monitor or keyboard connected to Pi. Enable SSH function from the pi's config.Then read the inet IP address from ifconfig. Using the command to SSH your pi.

```
$ ssh username@IP address
```

"Host key verification failed" means that the host key of the remote host was changed.

```
ssh-keygen -R hostname
```

Xserver Windows remote connection:

```
ssh -X <ip address of Rpi> -l <username on Rpi>
```

If you want to open image on the terminal, please install the software "eog" on your raspberry pi:

```
sudo apt-get install eog
eog hello.jpg
```

Using SCP to copy a folder from host to remote:
```
scp -r username@remoteIP:remotedirection/ /localdirection/
```