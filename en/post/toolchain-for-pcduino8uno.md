# How to get toolchain for pcDuino

If you want to compile source code for pcDuino(ARM) on other architecture PC like X86,  you should get a cross-toolchain. 

You can easily install arm cross-compiler on Ubuntu 14.04 as follows:

```bash
sudo apt-get install libc6:i386 libstdc++6:i386 libncurses5:i386 zlib1g:i386
sudo apt-get arm-linux-gnueabihf
sudo apt-get install libncurses5-dev libncursesw5-dev device-tree-compiler u-boot-tools

```

This will install Linaro cross-toolchain version 4.8 on Ubuntu 14.04, but if you want to install other version like 4.7, you can take the following commands:
```bash
sudo add-apt-repository ppa:linaro-maintainers/toolchain
sudo apt-get update
sudo apt-get install gcc-4.7-arm-linux-gnueabi
```

You might want additional tools for building a Linux system that are not related to the cross-compiler:
```bash
apt-get install build-essential git debootstrap u-boot-tools device-tree-compiler
```

## Another way to get Linaro toolchain
It lists the following toolchains which are tar file.

1. [gcc-linaro 4.7 (4.7-2014.06)](http://releases.linaro.org/14.06/components/toolchain/gcc-linaro/4.7)

2. [gcc-linaro 4.8 (4.8-2014.04)](http://releases.linaro.org/14.04/components/toolchain/gcc-linaro/4.8)

3. [gcc-linaro 4.9 (4.9-2014.07)](http://releases.linaro.org/14.07/components/toolchain/gcc-linaro/4.9)

**When in doubt, try 4.7 first.**

**WARNING**: Do not use the 4.8 gcc versions of the linaro toolchain to build legacy kernels (Linux kernel 3.4.39 etc.), those seem to have issues building the kernel. Use an earlier version instead. (TODO: Verify that this is still true today).

More information you can take from the following website:

1. [Linaro toolchain Wiki](https://wiki.linaro.org/WorkingGroups/ToolChain)

2. [Sunxi Linux Wiki](http://linux-sunxi.org/Toolchain)

3. [Buildup Linux 4.x & Ubuntu System on pcDuino3 Nano](http://learn.linksprite.com/pcduino/buildup-linux-4-x-ubuntu-system-on-pcduino3-nano-2/)

