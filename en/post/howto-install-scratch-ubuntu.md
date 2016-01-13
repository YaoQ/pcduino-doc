# How to Install Scratch on Ubuntu 14.04

![](http://programmingexplorer.weebly.com/uploads/5/5/7/0/55703841/3144136_orig.png)

While Mac and Windows have relatively simple Scratch installs, the install via Ubuntu is more complicated, particularly if it is a 64 bit system.  While the older version of Scratch is available via the Ubuntu package manager, Scratch 2 is not.  Since Scratch 2 has some important new features, such as code blocks, you may want to install it instead, or in addition to, the older Scratch version.  Here is a step-by-step instruction guide to install Scratch 2 on an Ubuntu 14.04 including i386 and X64:

## Download the relatively software
Download the Adobe Air and Scratch files from:
- [scratch2 for Linux](https://scratch.mit.edu/scratchr2/static/sa/Scratch-442.air)
- [Adobe Air for Linux](http://airdownload.adobe.com/air/lin/download/2.6/AdobeAIRInstaller.bin)

Note: Other version please check this [Scratch2download website](https://scratch.mit.edu/scratch2download/)

## Install on Ubuntu 14.04 i386

### 1. Install AdobeAIR
You have to create a symbolic link to your gnome keyring:
```bash
sudo ln -s /usr/lib/i386-linux-gnu/libgnome-keyring.so.0 /usr/lib/libgnome-keyring.so.0
sudo ln -s /usr/lib/i386-linux-gnu/libgnome-keyring.so.0.2.0 /usr/lib/libgnome-keyring.so.0.2.0
```
Then run AdobeAIRInstaller.bin file:
```
chmod +x AdobeAIRInstaller.bin
sudo ./AdobeAIRInstaller.bin
```
### 2. Install scratch2
```bash
sudo "Adobe AIR Application Installer" /path/to/scratch2/installer/Scratch-442.air
```
Done!

## Install on Ubuntu 14.04 X64
### 1. Install extra libraries
If you want to install AdobeAIR on Ubuntu 14.04 X64 system, the extra libraries are needed to be installed.
```bash
sudo apt-get install libxt6:i386 libnspr4-0d:i386 libgtk2.0-0:i386 libstdc++6:i386 libnss3-1d:i386 lib32nss-mdns libxml2:i386 libxslt1.1:i386 libcanberra-gtk-module:i386 gtk2-engines-murrine:i386
```
**Note:** Many people have installed these libraries successfully but as to me, I got many package dependency errors which make me crazy, even I install these libraries on a new Ubuntu 14.04 x64 system, there are also many dependency errors. I give up and if you make it, please tell me! 

### 2. install AdobeAIR
Now you have to create a symbolic link to your gnome keyring:
```bash
sudo ln -s /usr/lib/x86_64-linux-gnu/libgnome-keyring.so.0 /usr/lib/libgnome-keyring.so.0
sudo ln -s /usr/lib/x86_64-linux-gnu/libgnome-keyring.so.0.2.0 /usr/lib/libgnome-keyring.so.0.2.0
```

`cd` into the directory with the AdobeAIRInstaller.bin
```
chmod +x AdobeAIRInstaller.bin
sudo ./AdobeAIRInstaller.bin
```
### 3. Install Scratch 2

```bash
sudo "Adobe AIR Application Installer" /path/to/scratch2/installer/Scratch-437.air 
```
Done!

Good luck!
