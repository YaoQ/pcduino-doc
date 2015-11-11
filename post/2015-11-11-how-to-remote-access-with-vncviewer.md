# 如何远程桌面访问

安装在pcDuino8 Uno上的Ubuntu 14.04系统，默认安装了**x11vnc**。用户可以通过vnc客户端进行远程桌面访问。

下面将介绍在Windows下如何远程桌面访问pcDuino8 Uno（Ubuntu14.04）：

### pcDuino8 Uno端（vnc服务器端）

如下图所示，将pcDuino8 Uno与电源、键盘鼠标以及显示器相连，并通过网线连入局域网。
![pcDuino8 Uno](http://cnlearn.linksprite.com/wp-content/uploads/2015/10/pcDuino8-Uno.png)

上电启动，进入桌面系统，打开一个终端，运行如下命令确认是否已经获得IP地址：
```shell
$ ifconfig         #查看RJ45网口的IP地址
```

显示类似如下的信息：

> eth0 Link encap:Ethernet HWaddr 10:00:a4:f3:1d:df
> inet addr:**192.168.1.33** Bcast:192.168.1.255 Mask:255.255.255.0
> inet6 addr: fe80::1200:a4ff:fef3:1ddf/64 Scope:Link
> UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1
> RX packets:26 errors:0 dropped:0 overruns:0 frame:0
> TX packets:66 errors:0 dropped:0 overruns:0 carrier:0
> collisions:0 txqueuelen:1000
> RX bytes:2892 (2.8 KB) TX bytes:9481 (9.4 KB)
> Interrupt:114

如果看不到eth0的IP地址，通过如下命令获取：

```bash
$ sudo dhclient eth0        #通过DHCP获得IP地址
```

如果还不行，还可手动配置静态IP地址：
```bash
$ sudo eth0 192.168.1.33/24  ##需要根据你局域网的IP地址范围配置。
```

系统默认自启动x11vnc，可通过命令查看该服务是否运行：
```bash
ps -A | grep vnc      #打印当前运行的程序，并列出带有vnc的行。
```

## PC（vnc客户端）

1. 在Windows上下载并安装[VNCViewer](http://www.realvnc.com/download/viewer/)软件。

2. 打开VNCViewer，输入刚才所看到的IP地址。
![vnc](http://cnlearn.linksprite.com/wp-content/uploads/2015/10/vnc-300x156.png)

3. 输入密码，默认是：ubuntu
![passwd](http://cnlearn.linksprite.com/wp-content/uploads/2015/10/QQ%E6%88%AA%E5%9B%BE20151029152310.png)

4. 连接成功后，便可以远程桌面访问了。
![desktop](http://cnlearn.linksprite.com/wp-content/uploads/2015/10/desktop-300x223.png)