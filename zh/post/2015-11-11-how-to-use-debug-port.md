# 如何使用调试口，快速进入系统
pcDuino8 Uno跟其他系列的pcDuino板卡一样，提供了一个调试串口，方便用户的调试。用户可以在不外接键盘、鼠标、显示器的情况下，通过该调试串口，在命令行下直接访问Ubuntu系统。

下面将介绍如何使用该调试串口访问Ubuntu系统。

### 1.  准备USB串口线

![](http://linksprite.com/wiki/images/thumb/f/f6/RPI_UARTCABLE_1.jpg/400px-RPI_UARTCABLE_1.jpg)

首先需要一根USB转串口的数据线，本人使用了一根USB串口数据线[3]。如上图所示。一端是USB口，一端引出了4根不同颜色的接头，不同颜色分别代表不同的含义：

<table>
   <tr>
      <td>颜色</td>
      <td>描述</td>
   </tr>
   <tr>
      <td>红色</td>
      <td>VCC</td>
   </tr>
   <tr>
      <td>黑色</td>
      <td>GND</td>
   </tr>
   <tr>
      <td>绿色</td>
      <td>TXD</td>
   </tr>
   <tr>
      <td>白色</td>
      <td>RXD</td>
   </tr>
</table>

### 2. 安装驱动（以Windows为例）

从下面地址可以下载并安装 [PL2303 Windows驱动程序](http://www.prolific.com.tw/UserFiles/files/PL2303_Prolific_DriverInstaller_v1_10_0_20140925.zip)。

### 3. 串口线与调试口互联

![debug](http://cnlearn.linksprite.com/wp-content/uploads/2015/10/debug-300x210.png)

根据上图将串口线的3个接口与调试口互联。

> 调试口(RX) <—>USB串口线 RXD(白线)
> 调试口(GND) <—USB串口线GND(黑线)
> 调试口(TX) <—USB 串口线TXD(绿线）

### 4. USB接电脑

从设备管理器中确认电脑所识别出的COM口。
![com](https://wt-prj.oss.aliyuncs.com/14aadb635e6448e19ecee7a67cf9de05/a0944464-05ef-4041-ab6b-0e6d39b84661.png)

### 5. 配置串口调试软件（Putty）

给pcDuino8 Uno上电，使用一个5V 2A的直流电源。上电后，板上会有一个LED电源指示灯亮起。

![putty](https://wt-prj.oss.aliyuncs.com/14aadb635e6448e19ecee7a67cf9de05/79e4b160-60c4-4118-a114-7f6786eda23c.png)

串口选用**“COM11”**，而波特率选择**“115200”**，然后点击 **“Open”**。

![terminal](https://wt-prj.oss.aliyuncs.com/14aadb635e6448e19ecee7a67cf9de05/63b20d26-1ad3-4f40-a0d9-e19849b4df50.png)

然后就像在Linux Terminal终端一样可以访问Ubuntu了。

如果外接网络，获得IP地址，就能远程桌面登陆等，这样就省去了接键盘、鼠标、显示器等的必要，加快了开发的速度。
