# How to install Arduino IDE to program ESP8266 WiFi chip

This article describes the steps to install the Arduino IDE which can program ESP8266 WiFi chip.

## Ubuntu 14.04 LTS (Trusty Tahr)

## 1. Install Arduino IDE 1.6.7

```sh
wget https://downloads.arduino.cc/arduino-1.6.7-linux64.tar.xz
tar -xvf arduino-1.6.7-linux64.tar.xz
mv arduino-1.6.7 /opt/
/opt/arduino-1.6.7/install.sh
```
Install the required Java Runtime Environment with:

```
sudo apt-get install openjdk-7-jre
```

Change the USB file mode and add yourself to dialout group, or you will get permmission denied error:
```
sudo chmod a+rw /dev/ttyACM0
sudo addusers <yourname> dialout
```

## 2. Install ESP8266 packages into Arduino IDE
* Go to Arduino directory
* Clone the ESP8266 repository into hardware/esp8266com/esp8266 directory (or clone it elsewhere and create a symlink)
```
cd /opt/arduino-1.6.7/hardware
mkdir esp8266com
cd esp8266com
git clone https://github.com/esp8266/Arduino.git esp8266
```

* Download binary tools (you need Python 2.7)
```sh
cd esp8266/tools
python get.py
```

## 3. Flash Nodemcu firmware of ESP8266
First thing we need is to download the NodeMCU firmware. We do this by downloading one of these BIN files : [Download nodemcu latest firmware](https://github.com/nodemcu/nodemcu-firmware/releases/tag/0.9.6-dev_20150704).


```
python esptool.py -p SERIAL_PORT_NAME write_flash 0x00 firmware-combined.bin
```

Restart Arduino
```



