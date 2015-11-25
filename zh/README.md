# pcDuino8 Uno快速入手指导手册

## 介绍
介绍pcDuino8 Uno快速入门的相关内容，包括：

### 1. 快速入门
- pcDuino8 Uno介绍
- 制作可启动SD卡
- 远程访问pcDuino8 Uno
- Arduino开发快速入门
- 使用Python控制GPIO
- 如何使用调试口

### 2. 视频图像处理
- 在pcDuino8 Uno上如何安装FFMPEG，并获得IP Camara视频流
- 如何安装OpenCV，并实现人脸识别
- 如何使用motion实现网络视频监控
- 在pcDuino8 Uno上如何使用Face++ API实现人脸识别

### 3. **ROS系列**
- 如何在Ubuntu 14.04 安装 ROS Indigo

## 用gitbook生成电子书
本手册可通过gitbook生成电子书。
以 Ubuntu 为例：

### 安装软件
```bash
$ sudo apt-get install -y retext git nodejs npm
$ sudo ln -fs /usr/bin/nodejs /usr/bin/node
$ sudo apt-get install -y calibre fonts-arphic-gbsn00lp
$ sudo npm install gitbook-cli -g
```
### 下载
```bash
$ git clone https://github.com/YaoQ/pcDuino_Doc.git
$ cd pcDuino_Doc/zh
```
### 编译
```bash
$ gitbook build  // 编译成网页
$ gitbook pdf    // 编译成 pdf
```

## 版权

本书采用 ![CC BY NC ND 4.0](http://i.creativecommons.org/l/by-nc-nd/4.0/88x31.png) 协议发布，详细版权信息请参考 [CC BY NC ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)。

## 联系方式

由于时间有限，难免会出现一些错误，如果发现有任何问题，请联系（**youkee2015 # foxmail.com**），或者扫一扫本人的微信二维码：

![](images/mmcode.png)
