# FFMPEG基本命令的使用

在学习FFMPEG中记录的一些命令
ffmpeg命令格式如下：
![]("/images/ffmpeg.png")

预览输出
**ffplay -i input_file ... test_options**

过滤
**-vf**视频过滤，**-af**音频过滤
**ffplay -f lavfi -i testsrc -vf transpose=1**

###  -r 设置帧率
```shell
ffmpeg -i input -r fps output 

ffmpeg -i input.avi -r 30 output.mp4 #将25改成30帧
```

### -b 设置比特率
-b:v 表示视频流；-b:a表示音频流
用来设定每秒存储多少bit的编码流。
```
ffmpeg -i film.avi -b 1.5M film.mp4
```

### -fs 设置输出文件最大值
``` 
ffmpeg -i input.avi -fs 10MB output.mp4
```

## 调整视频大小
### -s 设置
```shell
ffmpeg -i input_file -s 320x240 output_file
```
```shell
#截取1/2高，1/2宽
ffmpeg -i input.avi -vf crop=iw/2:ih/2 output.avi 
```

参考
1. [python bindings of videolan](https://wiki.videolan.org/Python_bindings)



--------
使用ffmpeg，如何下载、安装！

http://www.ffmpeg.org/download.html


**可能遇到的问题：**
Configure and build the x264 libraries.
./configure --enable-static
make
sudo make install

./configure --enable-shared --enable-pic

The -shared and -pic options might also be required when you compile for some other architectures, such as ARM. You know you need these options if you get the following error when compiling OpenCV:
[ 25%] Building CXX object modules/highgui/CMakeFiles/opencv_highgui.dir/src/bitstrm.cpp.o
Linking CXX shared library ../../lib/libopencv_highgui.so
/usr/bin/ld: /usr/local/lib/libavcodec.a(avpacket.o): relocation R_X86_64_32S against `av_destruct_packet' can not be used when making a shared object; recompile with -fPIC
/usr/local/lib/libavcodec.a: could not read symbols: Bad value