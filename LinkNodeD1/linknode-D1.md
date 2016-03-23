# LinkNode D1 wiki

## Introduction
LinkNode D1 is Arduino-compatible WiFi development board which is powered by the high integrated WiFi chip [ESP-8266EX](http://espressif.com/en/products/hardware/esp8266ex/overview).

Thanks for the contribution from open source community who have developed Arduino core for ESP8266, this let Aduino IDE program LinkNode D1 without any change. At the same time, LinkNode D1 has Arduino-compatible pin out which make it very easy to connect to Arduino shield.
![](../images/linknoded1.jpg)

## Features
* Powered by ESP-8266EX
* 11 Digital I/O pins
* 1 Analog Input pin
* 1 micro USB port for power/configure
* Power jack for 9-24V power input
* Compatible with [Arduino programming](https://github.com/pcduino/LinkNodeD1)
* Compatible with [NodeMCU](http://www.nodemcu.com/)
* OTA -- Wireless Upload(Program)

## Tutorials

### Get started in Arduino

If you have used Arduino before, you will feel that the LinkNode D1 is as same as Arduino, and there is no difference between their programming. The only limitation of LinkNode D1 is that it only has 11 digital ports and 1 analog input port.  

#### 1. Requirements
* [Arduino IDE](https://www.arduino.cc/en/Main/Software), (the latest version is **1.6.8** )
* The [Arduino core](https://github.com/pcduino/LinkNodeD1) for LinkNode D1

#### 2. Manually install hardware package(Option 1)
* Install Arduino IDE
* Manually download hardware package from [github](https://github.com/pcduino/LinkNodeD1)
* Click the button **Download ZIP** to download
* Go to installation directory of Arduino IDE, and create  a **esp8266com** directory
* Move the unzipped LinkNodeD1 file to **ESP8266com**

#### 3. Use git to install (Option 2)
* You can use `git` command to download the hardware package like:
```
cd <ArduinoIDE path>/hardware
mkdir esp8266com
cd esp8266com
git clone https://github.com/pcduino/LinkNodeD1
```

#### 4. Check the configuration of Board

* Open Arduino IDE and go to **Tools --> Board --> LinkNode D1**
* Choose your own configuration  

**Upload Using**
* Serial – Use USB port on board to upload flash
* OTA – Use OTA to upload flash

**CPU Frequency**
* 80MHz
* 160MHz

**Flash Size**
* 4M (3M SPIFFS) – 3M File system size
* 4M (1M SPIFFS) – 1M File system size

**Upload Speed**
* **921600 bps** – recommend

#### 5. Create a Arduino Project
* Connect LinkNode D1 to your PC
* Check your serial port which your PC recognize
![](../images/board-config.png)

* Enter the following source code and click the **Upload** buttion

```c
void setup() {
  // initialize digital pin GPIO2/D9 as an output.
  pinMode(BUILTIN_LED, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(BUILTIN_LED, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(1000);              // wait for a second
  digitalWrite(BUILTIN_LED, LOW);    // turn the LED off by making the voltage LOW
  delay(1000);              // wait for a second
}
```
* After finish uploading, please check the BLUE LED on the ESP-8266EX chip, is it blinking?

###  Hello World
* Take the steps above and run the following code:
```c
void setup() {
  Serial.begin(115200);
}

// the loop function runs over and over again forever
void loop() {
  Serial.println("Hello world!");
  delay(1000);              // wait for a second
}
```
* After finish uploading, and open the **Serial Monitor** in Arduino IDE
* The serial will print **Hello world!** in every second.

### 3. Expand


### 4. WiFi Manager

### 5. Connect to LinkSprite IO

## Reference
