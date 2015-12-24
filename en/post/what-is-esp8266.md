

Details of connections:
The OLED is a 4-pin I2C device with SDA connecting to GPIO0 (zero) and SCL connecting to GPIO2 (zero 2).
CH_PD is connected to Vcc 3.3V  Some schematics show this connected to Vcc via a small value resistor but that resistor is optional in normal operation.
RST (reset) should be pulled to Vcc using a 10K resistor
Vcc and GND are connected to Vcc and GND of the OLED and then to a suitable, stable 3.3V source (battery, regulator, etc.)  A 100uF capacitor of good quality should be across the Vcc and GND connections very near the ESP8266.
UTXD and URXD are the UART serial connections to the ESP8266, usually 115200bps or 9600 bps depending upon firmware version.

UTXD is an output so it should connect to RXD on the serial-USB module.
URXD is an input, so it should connect to TXD on the serial-USB module.


Programming steps:
The Arduino flavor core seems to manage the ESP8266 well. Using a clean, stable power supply is key. Other things to note:



DO NOT take GPIO0 directly to Gnd to activate program mode! Rather, 

power-off the ESP8266, 
pull-down to Gnd pin GPIO0 using a 330 Ohm resistor, 
power-on, 
program*, 
power-off, 
remove the GPIO0 from the pull-down and connect as sketch required (OLED)
The 330 Ohm resistor will prevent any damage to the uC should the pin be driven accidently to a HIGH state.
* ONLY use a 3.3V serial-USB module. Never use a 5V serial converter.  Do not attempt to power the ESP8266 from the 3.3V output of a serial-USB adapter as the '8266 simply requires too much current.  Batteries or a well designed 3.3V supply is required.
Use a quality electrolytic as close to the ESP8266 board as practical... I'm using 100uF
Do use 1.8K Ohm resistors to 3.3V as I2C pull-ups

https://learn.sparkfun.com/tutorials/esp8266-thing-hookup-guide/installing-the-esp8266-arduino-addon

