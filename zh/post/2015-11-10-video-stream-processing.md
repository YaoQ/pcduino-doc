# python+vlc获取rstp视频流
python vlc的API

地址：https://github.com/mikemccand/pylive555

* First, download and compile/install the Live555 library from
    http://www.live555.com/liveMedia/public, and unzip/tar it and run:

    * ./genMakefiles linux
    * export CPPFLAGS=-fPIC CFLAGS=-fPIC
    * make
    * [optional: make install]

  * If you unzip/tar'd Live555 in this directory (the pylive555 checkout), to the sub-directory "live", then you can skip this step; otherwise, edit INSTALL_DIR in setup.py to point the live headers and libraries.

`sudo apt-get install python3-dev`

  * Build the python bindings: python3 setup.py build; make sure there are no errors.

  * Copy the resulting .so (from build/lib/*.so) to somewhere onto your PYTHONPATH.

  * Run the example 
`python3 example.py 10.17.4.118 1 10 out.264`
    
    That will record 10 seconds of H264 video from the camera at
    10.17.4.118, channel 1, saving it to the file out.264.


文章内容：
After buying a few of these cameras I needed a simple way to pull the raw H264 video from them, and with some digging I discovered the cameras speak RTSP and RTP which are standard protocols for streaming video and audio from IP cameras. Many IP cameras have adopted these standards. 

Both VLC and MPlayer can play RTSP/RTP video streams; for the Lorex cameras the default URL is: 

  rtsp://admin:000000@<hostname>/PSIA/Streaming/channels/1. 

After more digging I found the nice open-source (LGPL license) Live555 project, which is a C++ library for all sorts of media related protocols, including RTSP, RTP and RTCP. VLC and MPlayer use this library for their RTSP support. Perfect! 

My C++ is a bit rusty, and I really don't understand all of Live555's numerous APIs, but I managed to cobble together a simple Python extension module, derived from Live555's testRTSPClient.cpp example, that seems to work well. 

I've posted my current source code in a new Google code project named pylive555. It provides a very simple API (only 3 functions!) to pull frames from a remote camera via RTSP/RTP; Live555 has many, many other APIs that I haven't exposed. 

The code is thread-friendly (releases the global interpreter lock when invoking the Live555 APIs).

I've included a simple example.py Python program, that shows how to load H264 video frames from the camera and save them to a local file. You could start from this example and modify it to do other things, for example use the ffmpeg H264 codec to decode individual frames, use a motion detection library to trigger recording, parse each frame's metadata to find the keyframes, etc. Here's the current example.py: 




参考
1. [python bindings of videolan](https://wiki.videolan.org/Python_bindings)



--------
使用ffmpeg，如何下载、安装！

http://www.ffmpeg.org/download.html