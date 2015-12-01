# How to install openCV2.4.11 on pcDuino8 Uno

OpenCV (Open Source Computer Vision) is a library of programming functions mainly aimed at real-time computer vision. The library is cross-platform and free for use under the open-source BSD license.

This tutorial will tell how to install OpenCV 2.4.11.

## Steps
### 1. Install Cmake and Python related package 
Open Linux terminal and run ：
```shell
$ sudo apt-get -y install cmake
$ sudo apt-get -y install python-pygame python-scipy python-numpy
$ sudo apt-get -y install python-pip
```
### 2. Upgrade pip
```shell
$ sudo pip install --upgrade pip
```
### 3. Install virtual environment 
```shell
$ sudo pip install virtualenvwrapper
```
### 4.  Modify .bashrc file
```shell
$ vi ~/.bashrc 
```
add :
> source /usr/local/bin/virtualenvwrapper.sh

### 5. Reload .bashrc file
```shell
$ source ~/.bashrc
```
### 6. Create virtual environment
```shell
$ mkvirtualenv --system-site-packages env
```
### 7. Install dependencies：
```shell
$ sudo apt-get -y install libopencv-dev
$ sudo apt-get -y install build-essential checkinstall cmake pkg-config yasm
$ sudo apt-get -y install libtiff4-dev libjpeg-dev libjasper-dev
$ sudo apt-get -y install libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev libxine-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libv4l-dev
$ sudo apt-get -y install python-dev python-numpy
$ sudo apt-get -y install libtbb-dev
$ sudo apt-get -y install libqt4-dev libgtk2.0-dev
$ sudo apt-get -y install libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev
$ sudo apt-get -y install x264 v4l-utils
```
### 8. Download OpenCV source code 
```shell
$ wget -O OpenCV-2.4.11.zip http://sourceforge.net/projects/opencvlibrary/files/opencv-unix/2.4.11/opencv-2.4.11.zip/download  
```
### 9. Configure, compile and install 
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
### 10. Now the installation is done。

## test
Download a face detection project from github to test the compiled OpenCV 。
```shell
$ git clone https://github.com/shantnu/FaceDetect  
$ cd FaceDetect
$ python face_detect.py abba.png haarcascade_frontalface_default.xml 
```
The detection result is showed in the following picture.
![Detected](http://cnlearn.linksprite.com/wp-content/uploads/2015/10/41.png)

## Reference

1. [OpenCV installation steps](http://www.instructables.com/id/RasPi-OpenCV-Face-Tracking/?ALLSTEPS)
2. [Face recognition with python](https://realpython.com/blog/python/face-recognition-with-python/)
