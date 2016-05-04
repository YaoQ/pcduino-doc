# How to install Linux header for pcDuino8 Uno

There is no source of linux-headers for pcDuino8 Uno, so it not easy to compile an kernel module for pcDuino8 uno. But before several days ago, LinkSprite released [kernel source for pcDuino8 Uno](https://github.com/pcduino/pcduino8-uno-kernel), now it can get the Linux headers from this kernel source. 

## Required
- pcDuino8 Uno
- Host PC: Ubuntu 14.04(X86)

## Kernel source
- https://github.com/pcduino/pcduino8-uno-kernel

## Steps
### 1. Prepare source code

Download the source code from github and create a directory called linux-headers-3.4.39 at **/usr/src**.
```
sudo mkdir /usr/src/linux-headers-3.4.39
sudo cp ./linux-3.4 /usr/src/linux-headers-3.4.39 -r
sudo make O=/home/svn/linux-headers-3.4.39 sun8iw6p1smp_defconfig
sudo make O=/home/svn/linux-headers-3.4.39 modules_prepare
```

### 2. Create build soft link
```
cd /lib/modules/3.4.39
sudo ln -s /usr/src/linux-headers-3.4.39 build
```
### 3. Create Module.symvers
This file will be created when compile kernel source.

Go to your host PC Ubunt14.04(x64) and take the following steps to create Module.symvers. 
#### 3.1 Install tools
```
sudo apt-get install libc6:i386 libstdc++6:i386 libncurses5:i386 zlib1g:i386
sudo apt-get install gcc-arm-linux-gnueabihf
sudo apt-get install libncurses5-dev libncursesw5-dev device-tree-compiler u-boot-tools
```
#### 3.2 Install gawk3.1.6
```
wget http://ftp.gnu.org/gnu/gawk/gawk-3.1.6.tar.bz2
tar -xvf gawk-3.1.6.tar.bz2
cd gawk-3.1.6
./configure
make
sudo make install
```
#### 3.3 Build kernel
```
./build.sh config
    Welcome to mkscript setup progress
All available chips:
    0. sun6i
    1. sun8iw6p1
    2. sun9iw1p1
Choice: 1
All available platforms:
    0. android
    1. dragonboard
    2. linux
Choice: 2
not set business, to use default!
LICHEE_BUSINESS=
using kernel 'linux-3.4':
All available boards:
    0. eagle-p1
    1. eagle-p1-secure
    2. eagle-tvd-perf3
    3. pcduino8
    4. pcduino8-linux
Choice: 4
```
The Module.symvers will be created at linux-3.4 directory, and copy it to /usr/src/linux-headers-3.4.39 on pcDuino8 Uno.

### 4. Test
* Create a hello.c file.

```C
#include <linux/init.h>
#include <linux/module.h>

static int pcduino_hello_init(void)
{
    printk("Hello, pcDuino\n");
    return 0;
}

static void pcduino_hello_exit(void)
{
    printk("Bye, pcDuino\n");
}

MODULE_LICENSE("GPL");
MODULE_AUTHOR("latelan");
module_init(pcduino_hello_init);
module_exit(pcduino_hello_exit);
```

* Create Makefile
```
KDIR := /lib/modules/3.4.39/build  # Point to Linux Kernel Headers
PWD := $(shell pwd)

obj-m := hello.o

default:
    make -C $(KDIR) M=$(PWD) modules

clean:
    rm -rf *.o .*cmd *.ko *.mod.c .tmp_versions *.symvers *order
```

* Compile and test
```
make
sudo insmod hello.ko
dmesg | tail -n 1
[ 1957.116615] hello,world.
sudo rmmod hello.ko
dmesg | tail -n 1
[ 1976.438055] goodbye world
```

The installation of Linux headers on pcDuino8 Uno is done!
Thanks to [Latelan](https://github.com/latelan/pcDuino_Study/blob/master/post/2016-04-28-install-linux-headhers-for-pcDuino8-Uno.md).

