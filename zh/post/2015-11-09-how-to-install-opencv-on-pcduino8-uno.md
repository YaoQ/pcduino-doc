# 在pcDuino8 Uno（ubuntu 14.04 ARM）上安装openCV2.4

网上有很多教程教你如何安装OpenCV 2.×，但不是这出现编译错误，就是那出现依赖关系错误，各种乱象丛生。经过多次的尝试，终于成功地在pcDuino8 Uno(Ubuntu 14.04)上安装了OpenCV 2.4.11。下面就一步一步指导如何安装。

启动pcDuino8 Uno，并连接网络。

## 安装步骤
### 1. 安装cmake和python相关的包
打开Linux终端，运行：
```shell
$ sudo apt-get -y install cmake
$ sudo apt-get -y install python-pygame python-scipy python-numpy
$ sudo apt-get -y install python-pip
```
### 2. 升级 pip
```shell
$ sudo pip install --upgrade pip
```
### 3. 安装一个虚拟环境
````shell
$ sudo pip install virtualenvwrapper
```
### 4. 修改并切换环境变量
```shell
$ vi ~/.bashrc
```
添加如下内容：
> source /usr/local/bin/virtualenvwrapper.sh

### 5. 重新加载bash文件
```shell
$ source ~/.bashrc
```
### 6. 创建一个虚拟环境
```shell
$ mkvirtualenv --system-site-packages env
```
### 7. 安装依赖关系：
```shell
$ sudo apt-get -y install libopencv-dev
$ sudo apt-get -y install build-essential checkinstall cmake pkg-config yasm
$ sudo apt-get -y install libtiff4-dev libjpeg-dev libjasper-dev
$ sudo apt-get -y install libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev $ libxine-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libv4l-dev
$ sudo apt-get -y install python-dev python-numpy
$ sudo apt-get -y install libtbb-dev
$ sudo apt-get -y install libqt4-dev libgtk2.0-dev
$ sudo apt-get -y install libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev
$ sudo apt-get -y install x264 v4l-utils
```
### 8. 下载OpenCV源码
```shell
$ wget -O OpenCV-2.4.11.zip http://sourceforge.net/projects/opencvlibrary/files/opencv-unix/2.4.11/opencv-2.4.11.zip/download  #下载OpenCV源码，或者自行从其他地方下载
```
### 9. 编译并安装代码，需要花费相当长的时间
```shell
$ unzip OpenCV-2.4.11.zip
$ cd opencv-2.4.11
$ mkdir build
$ cd build
$ cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_OPENMP=ON ..
$ make -j4
$ sudo make install
$ sudo sh -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf'
$ sudo ldconfig
$ sh -c 'echo "export PYTHONPATH=/usr/local/lib/python2.7/site-packages:\$PYTHONPATH"  >> ~/.bashrc'
$ source ~/.bashrc
```
### 10. 至此 OpenCV 安装完成。

## 测试
直接用一个开源的项目来测试编译的代码。
```shell
$ git clone https://github.com/shantnu/FaceDetect
$ cd FaceDetect
$ python face_detect.py abba.png haarcascade_frontalface_default.xml
```
测试得到如下的结果，终端还能提示出检测到了几张人脸。
![Detected](http://cnlearn.linksprite.com/wp-content/uploads/2015/10/41.png)

下篇文章将介绍如何使用opencv来识别USB摄像头的人脸。

## 参考

1. [OpenCV安装教程](http://www.instructables.com/id/RasPi-OpenCV-Face-Tracking/?ALLSTEPS)
2. [25行python代码轻松实现人脸识别](https://realpython.com/blog/python/face-recognition-with-python/)
