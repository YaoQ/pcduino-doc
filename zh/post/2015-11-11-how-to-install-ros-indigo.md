# 如何在Ubuntu 14.04 安装 ROS Indigo
![](http://www.ros.org/wp-content/uploads/2014/07/indigoigloo_600.png)

## 简介

ROS（机器人操作系统，Robot Operating System），是专为机器人软件开发所设计出来的一套电脑操作系统架构。它是一个开源的元级操作系统（后操作系统），提供类似于操作系统的服务，包括硬件抽象描述、底层驱动程序管理、共用功能的执行、程序间消息传递、程序发行包管理，它也提供一些工具和库用于获取、建立、编写和执行多机融合的程序。

ROS的运行架构是一种使用ROS通信模块实现模块间P2P的松耦合的网络连接的处理架构，它执行若干种类型的通讯，包括基于服务的同步RPC（远程过程调用）通讯、基于Topic的异步数据流通讯，还有参数服务器上的数据存储。

 **ROS的特点：**

* **“轻便”**：ROS是设计得尽可能方便简易。您不必替换主框架与系统，因为ROS编写的代码可以用于其他机器人软件框架中。毫无疑问的，ROS更易于集成与其他机器人软件框架。事实上ROS已完成与OpenRAVE、Orocos和Player的整合。
*  **ROS-agnostic库**：建议的开发模型是使用clear的函数接口书写ROS-agnostic库。
* **语言独立性**：ROS框架很容易在任何编程语言中执行。我们已经能在Python和C++中顺利运行，同时添加有Lisp、Octave和Java语言库。
* **测试简单**：ROS有一个内建的单元/组合集测试框架，称为“rostest”。这使得集成调试和分解调试很容易。
* **扩展性**：ROS适合于大型实时系统与大型的系统开发项目。

ROS在很多机器人上得到了广泛的应用。很多公司和学校在ROS的基础上推出了不同特色的机器人。例如，带有2个运行Ubuntu和ROS电脑的PR2；基于云端、拥有情感的Softbank Pepper机器人，等等。

目前，ROS最稳定的发行版Indigo，支持Ubuntu14.04 ARM，但是并不是所有的软件包均支持该系统。

下面将介绍如何在pcDuino8 Uno（Ubuntu 14.04 ARM）上安装ROS Indigo。

## 安装

### 1. 对Ubuntu进行区域设置

配置你的 Ubuntu 软件仓库(repositories) 以允许 “restricted”、”universe” 和 “multiverse”这三种安装模式。 你可以按照ubuntu中的配置指南来完成配置。

由于ROS工具要求系统进行区域设置，所以运行如下命令：

    sudo update-locale LANG=C LANGUAGE=C LC_ALL=C LC_MESSAGES=POSIX
### 2. 添加sources.list

配置你的电脑使其能够安装来自 packages.ros.org的软件。 ROS Indigo Trusty armhf(14.04)。

    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

### 3. 设置keys

    wget https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -O - | sudo apt-key add -

### 4. 安装

首先，确保你的Debian软件包索引是最新的：

	sudo apt-get update

ROS中有很多不同的库和工具，但是并没有全部在ARM编译过。你可以独立安装ROS包。
安装ROS-Base（Bare Bones）：ROS包、编译和通信库，不带GUI工具。

	sudo apt-get install ros-indigo-ros-base

#### 4.1 添加独立的安装包

你可以安装一个特定的ROS包（用包名取代下面“PACKAGE”）：

	sudo apt-get install ros-indigo-PACKAGE

例如：

	sudo apt-get install ros-indigo-navigation

可通过如下命令查询有哪些包可用：

	apt-cache search ros-indigo

### 5. 初始化rosdep

在开始使用ROS之前你还需要初始化rosdep。rosdep可以方便在你需要编译某些源码的时候为其安装一些系统依赖，同时也是某些ROS核心功能组件所必需用到的工具。

	sudo apt-get install python-rosdep
	sudo rosdep init
	rosdep update

### 6. 环境设置

如果每次打开一个新的终端时ROS环境变量都可以自动配置（即添加到bash会话中），那将会方便很多：

	echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
	source ~/.bashrc

如果你只想改变当前终端下的环境变量，你可以执行命令：

	source /opt/ros/indigo/setup.bash

### 7. 安装 rosinstall

rosinstall 是ROS中一个独立分开的常用命令行工具，它可以方便让你通过一条命令就可以给某个ROS软件包下载很多源码树。

要在ubuntu上安装这个工具，请运行：

	sudo apt-get install python-rosinstall

### 8. Build farm 状态

你所安装的各种软件包都是通过ROS build farm来编译构建的。你可以在这里查看各种独立软件包的编译状态。

## 测试

此时，运行roscore就能看到ROS master正常运行输出的信息：

```bash
linaro@linaro-alip:~$ roscore
… logging to /home/linaro/.ros/log/6fe7a434-1dea-11b2-8e0e-1000a4f31ddf/roslaunch-linaro-alip-1217.log
Checking log directory for disk usage. This may take awhile.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

started roslaunch server http://linaro-alip:36980/
ros_comm version 1.11.13

SUMMARY

========

PARAMETERS
* /rosdistro: indigo
* /rosversion: 1.11.13

NODES

auto-starting new master
process[master]: started with pid [1228]
ROS_MASTER_URI=http://linaro-alip:11311/

setting /run_id to 6fe7a434-1dea-11b2-8e0e-1000a4f31ddf
process[rosout-1]: started with pid [1241]
started core service [/rosout]
```

以上是ROS indigo在pcDuino8 Uno上的安装。

## 参考链接

1. [Ubuntu ARM install of ROS Indigo](http://wiki.ros.org/indigo/Installation/UbuntuARM)
