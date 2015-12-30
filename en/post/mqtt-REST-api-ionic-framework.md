# Mobile App Controlled LED through ESP8266 via MQTT+HTTP
The system is based on pcDuino8 Uno and run Node.js REST API server, MQTT Broker locally.The Android Mobile App is built using Ionic Framework utilizing AngularJs. 

![](../images/iot-pcduino.png)

##Requirements
### 1.Hardware
* pcDuino8 Uno
* 5V Power Supply ( at least 2A )
* WiFI Router
* USB WiFi(or using RJ45 to connect Router)

### 2. Software
* Node.js
* Ionic Framework
* python for pcduino

## Install MQTT Server on pcDuino8 Uno
### 1. Install node and npm
```bash
sudo ntpdate us.pool.ntp.org
sudo apt-get install curl
curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo ln -s /usr/bin/nodejs /usr/bin/node
sudo apt-get install -y npm
echo "export NODE_PATH=/usr/local/lib/node_modules" >> ~/.bashrc
source ~/.bashrc
```
### 2. Install MQTT node module
```
sudo npm install mosca -g
sudo npm install mqtt -g

```

### 3. Code for MQTT server
```js
console.log(process.pid);
//require('daemon')();

var moscaSettings = {
  port: 1883,
  host: "localhost"
};

var server = new mosca.Server(moscaSettings);
server.on('ready', setup);

server.on('clientConnected', function(client) {
    console.log('client connected', client.id);     
});

server.on('published', function(packet, client) {
  console.log('Published', packet.payload);
});

function setup() {
  console.log('Mosca server is up and running')
}
console.log(process.pid);
```


## RESE API Server + MQTT Client(Node.js)
```bash
sudo npm install hapi -g
```

### 4. Write JavaScript code
```bash
vim index.js
```
**index.js: REST API Server simple demo**
```js
var Hapi = require('hapi');

var server = new Hapi.Server();
var port = Number(process.env.PORT || 4444);

server.connection({ port: port, routes: { cors: true } });

server.route([
  {
    method: 'POST',
    path: '/device/control',
    handler: function (request, reply) {
      reply("Hi");
     console.log(request.payload.message);
    }
  }
]);

server.start();

```
>/usr/local/lib/node_modules/hapi/lib/index.js:5
const Server = require('./server');
^^^^^
SyntaxError: Use of const in strict mode.
    at Module._compile (module.js:439:25)
    at Object.Module._extensions..js (module.js:474:10)
    at Module.load (module.js:356:32)
    at Function.Module._load (module.js:312:12)
    at Module.require (module.js:364:17)
    at require (module.js:380:17)
    at Object.<anonymous> (/home/linaro/iot/mqtt-pub-rest-api/index.js:1:74)
    at Module._compile (module.js:456:26)
    at Object.Module._extensions..js (module.js:474:10)
    at Module.load (module.js:356:32)

If you get this error, it means node version is lower than 4.0, and you should update the node, also update the npm using:
```
sudo npm install npm -g
```

## MQTT Client for Subscriber(Python)
```
sudo apt-get install python-pip
sudo pip install paho-mqtt
git clone https://github.com/YaoQ/iot-pcduino
cd mqtt-sub-on-pcduino
python demo.py
```

MQTT Client to control LED
```python
#!/usr/bin/env python
import paho.mqtt.client as mqtt
import json
import gpio
dev1_pin = "gpio19"
dev2_pin = "gpio18"
ip_address= "192.168.1.106"

def setup():
    gpio.pinMode(dev1_pin, gpio.OUTPUT)
    gpio.digitalWrite(dev1_pin, gpio.LOW)

def destroy():
    gpio.digitalWrite(dev1_pin,gpio.LOW)

def on_connect(client, userdata, flags, rc):
    print("Connected with result code " + str(rc))
    client.subscribe("device/control")

def on_message(client, userdata, msg):
    gpio_msc = str(msg.payload)
    print(msg.topic+" "+gpio_msc)
    
    if gpio_msc == 'dev1-on':
        gpio.digitalWrite(dev1_pin, gpio.HIGH)
    elif gpio_msc == 'dev1-off':
        gpio.digitalWrite(dev1_pin, gpio.LOW)
    elif gpio_msc == 'dev2-on':
	gpio.digitalWrite(dev2_pin, gpio.HIGH)
    elif gpio_msc == 'dev2-off': 
        gpio.digitalWrite(dev2_pin, gpio.LOW)

client = mqtt.Client()
client.on_connect = on_connect
client.on_message = on_message
setup()

try:
    client.connect(ip_address, 1883, 60)
    client.loop_forever()

except KeyboardInterrupt:
    client.disconnect()
    destroy()

```



## Develop APP using Ionic Framework(on Ubuntu 14.04 X64 PC)


### Install node and npm on PC
```
sudo apt-get install curl
curl -sL https://deb.nodesource.com/setup | sudo -E bash -
sudo apt-get install -y nodejs
sudo ln -s /usr/bin/nodejs /usr/bin/node
sudo apt-get install -y npm
echo "export NODE_PATH=/usr/local/lib/node_modules" >> ~/.bashrc
source ~/.bashrc
```

Require:

* Ionic 1.7.2
* cordova 5.4.1
* Android 5.1.1 (API 22)
* Android SDK Tools 24.4.1
* Android SDK Platform-tools 23.1
* Android SDK Build-tools 23.0.2

### Install JDK and android SDK
Install and update your environment in .bashrc file!

### Install cordova and ionic
```
sudo npm install -g cordova
sudo npm intall -g ionic
git clone https://github.com/YaoQ/iot-ionic
cd iot-ionic
vim www/js/services.js
```
```js
angular.module('starter.services', [])

.factory('Devices', function($http) {
  // Might use a resource here that returns a JSON array

  var ipServer = 'http://192.168.1.106:4444';

  return {
    deviceCommand: function(data) {
      console.log(data.deviceNum);
      console.log(data.command);
      return $http.post(ipServer + '/device/control', data);
    }
  };
});
```

### Compile and gennerate APK file
```
ionic platform add android
ionic serve
ionic build android
```

If your app can not access network please take the following commands:
```
ionic plugin add https://github.com/apache/cordova-plugin-whitelist.git
```






