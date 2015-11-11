# 制作可启动的 Android SD卡
烧写Android系统镜像需要用专用的烧写工具，Phonenix Card V3.09。如果用Win32DiskImager制作的话，系统是无法启动的。

## 步骤

### 下载
 1. [Android 系统镜像](https://s3.amazonaws.com/pcduino/Images/pcduino8/pcDuino8Uno/Android/pcduino8_android_20150913.img)
 2. [Phonenix Card烧写工具](https://s3.amazonaws.com/pcduino/Tools/PhoenixCard_V309.zip)

### 烧录
1.  插入SD卡，选择系统识别到的磁盘。
2.  选择Android的系统镜像。
3.  写模式，选中“卡启动”模式，点击烧录。
4.  工具最后显示，烧录成功。

注意：**系统镜像的路径不能包含中文**
![android](/images/android.png)


