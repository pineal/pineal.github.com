---
layout: post
title: WIFI Connection on Raspberry Pi
categories:
- Raspberry Pi
source: http://www.raspberrypi-tutorials.co.uk/set-raspberry-pi-wireless-network/
tags:
- Raspberry Pi 
---

##General Connection
Using lsusb to look up your wifi adapter device name:

```
$ lsusb
```
You will see your device name such as Wlan0, but the IP address is still empty.Using the commend to modify the configuration file:

```
$ sudo nano /etc/network/interfaces
```
The file should contain such context:

```
auto lo
iface lo inet loopback
iface eth0 inet dhcp

auto wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-ssid "SSID"
wpa-psk "password"
```
After this, the pi will take a DHCP auto connection. Make sure type the right SSID and password. The IP address will be allocated automatically.Using the command to restart the network. 

```
$ sudo /etc/init.d/networking restart
```
If the terminal says done, you can enjoy wifi with your PI now.

##Connect to University endroam
In University of Liverpool, the wireless network called "eduroam" is applied. Actually this wireless network is applied in most universities in Europe. So here is the method to config "eduroam" for Raspberry Pi.
Firstly, find the information about your university network. For example: 

*http://www.liv.ac.uk/csd/mobiles-and-tablets/other-devices/*

Download the certificate, copy or move the certificate file: e.g. :

```
$ sudo mount /dev/sda2 /mnt/usb/  #mount the udisk to your file
$ cd /mnt/usb/
$ sudo cp /your direction/certificate.cer /etc/wpa_supplicant
```
Then, you can use either GUI WifiConfig in the desktop, or edit the wpa_supplicant.conf:

```
$ sudo gedit /etc/wpa_supplicant/wpa_supplicant.conf
```
Add the code below to the file:

```
network={
        ssid="eduroam"
        proto=RSN
        key_mgmt=WPA-EAP
        pairwise=CCMP
        auth_alg=OPEN
        identity="your university login name"
        password="your password"
        ca_cart="/etc/wpa_supplicant/certificate.cer"
}
```