# HDMI Touch Screen for 96boards

Recently, I get an 7inch HDMI touch screen and the features of this screen are:
* 800×480 high resolution
* Capacitive touch control
* Supports Raspberry Pi
* Supports Banana Pi / Banana Pro, comes with Lubuntu, Raspbian images
* Supports BB Black, comes with Angstrom image
* For other mini-PCs, driver is required and should be developed by users
* Not only for mini-PCs, it can work as a computer monitor * just like any other general HDMI screen
* HDMI interface for displaying, USB interface for touch control
* Back light control to lower power consumption

The screen use standard HID protocol, so it is very easy to be integrated into Linux system. So the following I will introduce is about how to use it with DragonBoard410c which is one kind of 96boards.

![](../../images/wave.jpg)

The picture below is DragonBoard410c.

![](https://developer.qualcomm.com/sites/default/files/attachments/db410c-top1.png)
## Steps

First, you should install Debian system on your DragonBoard410c,Details you can check [here](https://github.com/96boards/documentation/wiki/Dragonboard-410c-Installation-Guide-for-Linux-and-Android).

### 1. Connect HDMI and USB
* Turn on the Backlight switch.

![](../../images/back.png)
* Take the following picture to connect the touch screen and Dragonboard 410c.
![](../../images/connect.png)

* Then power on the Dragonboard 410c and system can  automatic identify the resolution of screen, you don't need for configuration.
* When the system enters into desktop, hot plug the USB cable, then the touch function can work.
