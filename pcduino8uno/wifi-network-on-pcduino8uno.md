# Automatically connect a pcDuin8 Uno to a Wifi network

In this post I'll quickly cover how you can set up your pcDuino8 uno to automatically connect to your wireless network. All you need is a [WiFi-dongle](http://store.linksprite.com/pcduino1-an-minipc-with-arduino-headers-ubuntu-android/).

Because the pcDuino8 uno only has one USB-port, you have to use an USB hub to expander USB port.

## Requried
* pcDuino8 Uno
* USB Hub
* USB keyboard and mouse
* [USB WiFi Dongle](http://store.linksprite.com/pcduino1-an-minipc-with-arduino-headers-ubuntu-android/)
* Screen with HDMI port and cable

## Setting up WiFi connection

Start by booting the pcDuino8 uno, connected to a display and a keyboard. Open up the terminal and edit the network interfaces file:

    sudo vim /etc/network/interfaces

This file contains all known network interfaces, it'll probably have a line or two in there already.

Change the first line (or add it if it's not there) to:

    auto wlan0

Then at the bottom of the file, add these lines telling the pcDuino8 uno to allow wlan as a network connection method and use the **/etc/wpa_supplicant/wpa_supplicant.conf** as your configuration file.

    allow-hotplug wlan0
    iface wlan0 inet dhcp
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
    iface default inet dhcp

The next step is to create this configuration file.

## Configuring WiFi connection

Open up the wpa_supplicant.conf file in the editor.

    sudo vim /etc/wpa_supplicant/wpa_supplicant.conf

Again, some lines might already be present, just add the following.
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
    ssid="YOUR_NETWORK_NAME"
    psk="YOUR_NETWORK_PASSWORD"
    proto=RSN
    key_mgmt=WPA-PSK
    pairwise=CCMP TKIP
    group=CCMP TKIP
    auth_alg=OPEN
}

```

* **proto** could be either RSN (WPA2) or WPA (WPA1).
* **key_mgmt** could be either WPA-PSK (most probably) or WPA-EAP (enterprise networks)
* **pairwise** could be either CCMP (WPA2) or TKIP (WPA1)
* **auth_alg** is most probably OPEN, other options are LEAP and SHARED

## Make sure it works

Reboot the pcDuino8 uno and it should connect to the wireless network. If it doesn't, repeat above steps or get help from an adult.

A big thanks to [Automatically connect a Raspberry Pi to a Wifi network](http://weworkweplay.com/play/automatically-connect-a-raspberry-pi-to-a-wifi-network/) and [pcDuino8 Uno section on LinkSprite's Forum](http://forum.linksprite.com/index.php?/topic/4624-wifi-success-finally/)
