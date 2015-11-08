# 在pcDuino8 Uno上如何使用python访问GPIO

python是一种非常简单易学，但又非常强大的编程语言，拥有高效的高级数据结构，且能够用简单而又高效的方式进行面向对象编程。

接下来将介绍如何使用python访问pcDuino8 Uno上的GPIO接口。具体步骤如下：

## 1. 安装基本的python包
进入Ubuntu系统，打开Linux终端，系统默认安装了python，但还需要安装如下的一些包：

 `sudo apt-get install python-requests`  #安装Request:
`sudo apt-get install python-dev IPython` #安装python-dev 和IPython 
`sudo apt-get install python-pip  ` #安装python-pip
`sudo pip install pip --upgrade`  #升级pip
`sudo pip install flask`  #安装Flask

## 2. 下载支持pcDuino的python测试环境
`git clone https://github.com/pcduino/python-pcduino.git`  #下载python测试环境。

## 3. 测试GPIO
`cd python-pcduino`
`python python-pcduino/Samples/blink_led/blink_led.py` # 测试 pcDuino8 Uno上LED7。


![pcDuino8 Uno GPIO](/images/pcduino8-gpio.JPG/)
**查看图中 LED7  是否在闪烁！**

### blink_led的python代码
```
#!/usr/bin/env python
# blink_led.py
# gpio test code for pcduino ( http://www.pcduino.com )
#
import gpio
import time

led_pin = "gpio18"

#定义ms延时
def delay(ms):
    time.sleep(1.0*ms/1000)

#设置GPIO为输出
def setup():
    gpio.pinMode(led_pin, gpio.OUTPUT)

#循环运行
def loop():
    while(1):
        gpio.digitalWrite(led_pin, gpio.HIGH)
        delay(200)
        gpio.digitalWrite(led_pin, gpio.LOW)
        delay(100)

def main():
    setup()
    loop()

main()

```

