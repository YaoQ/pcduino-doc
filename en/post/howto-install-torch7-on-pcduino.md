# Torch7 Installation Guide
### 1. Set time
```
sudo ntpdate -u us.pool.ntp.org
```
### 2. create swap partition
```
free -m
sudo dd if=/dev/zero of=/var/swap.img bs=1M count=1000
sudo mkswap /var/swap.img
sudo swapon /var/swap.img
free -m
```
### 3. hack the system version
`vim /etc/os-release`

>
    NAME="Ubuntu"
    VERSION="14.04"
    ID=ubuntu
    ID_LIKE=debian
    PRETTY_NAME="Ubuntu 14.04"
    VERSION_ID="14.04"
    HOME_URL="http://www.linaro.org/"
    SUPPORT_URL="http://linaro.zendesk.com/"
    BUG_REPORT_URL="http://bugs.launchpad.net/linaro-ubuntu"


`vim /etc/lsb-release`
>
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=14.04
DISTRIB_CODENAME=trusty
DISTRIB_DESCRIPTION="Ubuntu 14.04"


### 4. install rep and deps
```    
sudo echo "deb http://ppa.launchpad.net/jtaylor/ipython/ubuntu trusty main" >> /etc/apt/sources.list.d/jtaylor-ipython-trusty.list
wget --no-check-certificate https://raw.githubusercontent.com/torch/ezinstall/master/install-deps
```

### 5. update install-deps script.file
```
vim install-deps
```
comment out line 153
>        #    sudo add-apt-repository -y ppa:jtaylor/ipython

comment out lines from 170 to 175 
> # if [[ $ubuntu_major_version -lt '14' ]]; then
  #          # Install from source after installing git and build-el
  #          install_openblas || true
  #      else
  #          sudo apt-get install -y libopenblas-dev liblapack-dev
  #      fi
  install_openblas

### 6. install torch
```
git clone https://github.com/torch/distro.git ~/torch --recursive
cd ~/torch; ./install.sh
```

----

## Ref
We provide a simple installation process for Torch on Mac OS X and Ubuntu 12+:

Torch can be installed to your home folder in ~/torch by running these three commands:
```bash
# in a terminal, run the commands
curl -s https://raw.githubusercontent.com/torch/ezinstall/master/install-deps | bash

```

Note: WARNING: Your ipython version is too old.  Type "ipython --version" to see this.  Should be at least version 2
the current ipython is 1.2.1

Then upgrade ipython
`sudo pip install --upgrade ipython -i -i http://pypi.v2ex.com/simple`


```
git clone https://github.com/torch/distro.git ~/torch --recursive
cd ~/torch; ./install.sh
```

The first script installs the basic package dependencies that LuaJIT and Torch require. The second script installs LuaJIT, LuaRocks, and then uses LuaRocks (the lua package manager) to install core packages like torch, nn and paths, as well as a few other packages.

The script adds torch to your PATH variable. You just have to source it once to refresh your env variables

errors：

>Missing dependencies for itorch:  
luacrypto   
uuid   
lzmq >= 0.4.2  

auto install luacrypto and uuid!

>gcc -O2 -fPIC -I/home/linaro/torch/install/include -c src/lzmq.c -o src/lzmq.o -DLUAZMQ_USE_SEND_AS_BUF -DLUAZMQ_USE_TEMP_BUFFERS -DLUe
In file included from src/lzmq.c:13:0:
src/lzutils.h:73:40: error: ��‘ZMQ_EVENT_CONNECTED��’ undeclared here (not in a function)
 #define DEFINE_ZMQ_CONST(NAME) {#NAME, ZMQ_##NAME}
                                        ^
src/lzmq.c:483:3: note: in expansion of macro ��‘DEFINE_ZMQ_CONST��’
   DEFINE_ZMQ_CONST( EVENT_CONNECTED       ),
   ^
src/lzutils.h:73:40: error: ��‘ZMQ_EVENT_CONNECT_DELAYED��’ undeclared here (not in a function)
 #define DEFINE_ZMQ_CONST(NAME) {#NAME, ZMQ_##NAME}


but Error: Failed installing dependency: https://raw.githubusercontent.com/rocks-moonscript-org/moonrocks-mirror/master/lzmq-0.4.3-1.src.ro



try to install libzmq3-dev

linaro@linaro-alip:~/torch$ sudo apt-get install libzmq3-dev


---
stderr: /home/linaro/torch/install/bin/luajit: /home/linaro/torch/install/share/lua/5.1/trepl/init.lua:383: module 'dpnn' not found:Non
        no field package.preload['dpnn']
        no file '/home/linaro/.luarocks/share/lua/5.1/dpnn.lua'
        no file '/home/linaro/.luarocks/share/lua/5.1/dpnn/init.lua'
        no file '/home/linaro/torch/install/share/lua/5.1/dpnn.lua'
        no file '/home/linaro/torch/install/share/lua/5.1/dpnn/init.lua'
        no file './dpnn.lua'
        no file '/home/linaro/torch/install/share/luajit-2.1.0-beta1/dpnn.lua'
        no file '/usr/local/share/lua/5.1/dpnn.lua'
        no file '/usr/local/share/lua/5.1/dpnn/init.lua'
        no file '/home/linaro/.luarocks/lib/lua/5.1/dpnn.so'
        no file '/home/linaro/torch/install/lib/lua/5.1/dpnn.so'
        no file '/home/linaro/torch/install/lib/dpnn.so'
        no file './dpnn.so'
        no file '/usr/local/lib/lua/5.1/dpnn.so'
        no file '/usr/local/lib/lua/5.1/loadall.so'

try to install 

```
luarocks install dpnn
```

---

stderr: /home/linaro/torch/install/bin/luajit: /home/linaro/torch/install/share/lua/5.1/torch/File.lua:317: table index is nil
stack traceback:
        /home/linaro/torch/install/share/lua/5.1/torch/File.lua:317: in function 'readObject'
        /home/linaro/torch/install/share/lua/5.1/nn/Module.lua:154: in function 'read'
        /home/linaro/torch/install/share/lua/5.1/torch/File.lua:298: in function 'readObject'
        /home/linaro/torch/install/share/lua/5.1/torch/File.lua:347: in function 'load'
        /home/linaro/openface/openface/openface_server.lua:45: in main chunk

try to run:
```
luarocks install file
```

