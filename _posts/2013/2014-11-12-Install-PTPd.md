---
layout: post
title: Install PTPd
categories:
-Linux
Tags:
-PTPd
---
This blog is going to introduce the on the Raspberry Pi.
PTPd is an open source implementation of the Precision Time Protocol for Unix-like computers. You can find sources and documentation on this website(or you can google it by yourself): [http://ptpd.sourceforge.net/](http://ptpd.sourceforge.net/).

Now we want to synchronising two Raspberry Pi with PTPd.

1. Using your computer as a master, and set your two Pis as slave
2. Using one of Pi as master and set the other as slave
This blog gives a demo that how to setup PTPd on linux(link).

Here is a method to test the synchronisation.The main idea is to let two Pi's do something that can be detected in the same time.Here is a pice of code written by python can display the system time in a specified time point.

```
&gt;&gt;&gt; import time
&gt;&gt;&gt; print time.time()
1326459121.68
```