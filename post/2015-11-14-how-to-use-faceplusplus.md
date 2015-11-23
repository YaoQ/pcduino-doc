# 如何使用Face++ API进行人脸识别

试想，以后的视频监控不再仅仅是远程监控视频，更有可能进行智能识别，比如移动物体的检测、人脸信息的采集、某样物体特征的提取和识别，等等。除了能用开源而悠久的OpenCV实现特征提取，比如实现人脸识别，但实现的效果尚未满足实际的要求。看到有人[LinkSprite论坛](forum.linksprite.com)上发了一篇帖子，采用了Face++ 提供的API从人脸提取了很多信息，比如人的年龄、性别、种族、是否微笑，并且可以对比人脸是否相似，从一群人的人脸中找到最相似的那个人等等。任何人都可以免费使用[Face++](http://www.faceplusplus.com.cn/)提供的API，无疑对我们这些喜欢折腾的人是一个利好。

![](http://static2.businessinsider.com/image/554268e7ecad048b6c8b456a-480/how-old-microsoft.jpg)

这让我想起，前不久微软推出的how-old.net网站，在朋友圈火了一把。如今，我们也可以用一张人脸照片去估计人的年龄等等信息。当然，除了Face++，还有不下10种API，可以参考下面的文章：

[List of 10+ Face Detection / Recognition APIs, libraries, and software](http://blog.mashape.com/list-of-10-face-detection-recognition-apis/)

话不多说，进入正题。

## 1. 什么是Face++
百度百科如是说：
> Face++是新一代云端视觉服务平台，提供一整套世界领先的人脸检测，人脸识别，面部分析的视觉技术服务。

>Face++旨在提供简单易用，功能强大，平台通用的视觉服务，让广大的Web及移动开发者可以轻松使用最前沿的计算机视觉技术，从而搭建个性化的视觉应用。Face++同时提供云端REST API以及本地API（涵盖Android, iOS, Linux, Windows, Mac OS），并且提供定制化及企业级视觉服务。通过Face++，您可以轻松搭建您自己的云端身份认证，用户兴趣挖掘，移动体感交互，社交娱乐分享等多类型应用。

Face++ API 提供了多种支持，包括：

- Python SDK & 命令行工具  (Python2.7及以上)  
- Objective-C SDK (iOS)  (建议Xcode最新版)
- Java SDK (Android)  (Android2.3及以上)
- Matlab SDK   (Matlab2012a及以上)
- PHP SDK  (PHP5.3及以上)

### 1.1 核心概念
Face++ 人脸识别系统包含五个核心概念：**Image, Face, Person, Faceset 和Group**。这是在运用Face++ API时需要重点区分的。

- Image  指用户或应用程序给Face++ API提供的图片，以供后续检测/识别使用。用户可以通过指定url或在程序中上传（通过HTTP POST提交图片的二进制文件）提供Image。
- Face  指Image中检测出的人脸。一张Image中可能包含多个Face。
- Person 指同一个人的Face集合。Person中的多个Face可能来源于多个Image，但必须是同一个人的多张Face。一个Face可以属于多个不同的Person。Person被用在人脸验证（verify）和人脸识别（identify）中。
- Faceset 指一个或多个Face的集合。Faceset和Person一样，都是Face的集合，但Faceset并不要求Face来源于同一个人。一个Face可以属于多个不同的Person和Faceset。Faceset被用在人脸搜索（search）中。
- Group 指多个Person的集合。在多数Face++人脸识别场景中，用户需指定一个Group来限定在此集合中进行识别。
- ID和Name两套索引系统用来定位和访问上述所有元素， Image，Face，Person，Faceset 和 Group都有系统分配的全局唯一的ID。为便于用户使用有语义信息的名字进行开发，用户也可给Person 和 Group设置一个Name。Name由用户提供，必须在App内全局唯一。

### 1.2 基本工作流程
![工作流程](http://www.faceplusplus.com.cn/wp-content/uploads/2014/01/workflow_cn.jpg)

### 1.3 Face++ API文档
具体请参考如下文档：
[API文档](http://www.faceplusplus.com.cn/api-overview/)

**接下来将介绍如何在pcDuino8 Uno上，通过Face++ python SDK进行人脸识别。**

## 2. 硬件清单
- pcDuino8 Uno

## 3. 软件清单
系统使用Ubuntu14.04，基本的软件包括：
-  python2.7
-  Face++ python SDK
-  git

注意：**Face++是云端的服务，必须要接入网络**

## 4. 具体步骤

首先，你要注册一个账户，下面直接给出Face++官网提供的注册教程。

### 4.1 注册账户，获得API_Key和API_Secret
为了正常运行开发环境，您需要已建立一个App并获得相应的API_Key和API_Secret，具体步骤如下：

1. 点击进入Face++开发者首页，点击右上方的我的应用图标1，您可能需要注册或登录。

2. 点击屏幕右方的创建应用图标2，创建一个新的应用。

![](http://www.faceplusplus.com.cn/wp-content/uploads/2014/01/32.png)
3. 点击提交按键，自动进入应用信息界面，您将看到**API_Key和API_Secret。**

![](http://www.faceplusplus.com.cn/wp-content/uploads/2014/01/42.png)
4. 把API_Key和API_Secret复制到您的API相关文件中，如apikey.cfg（在很多官方下载SDK中都含有此文件），或您自己写的程序里。

注意，如果您注册时选择了阿里云（中国）服务器，请使用如下配置：

	SERVER = 'http://api.cn.faceplusplus.com/'

	API_KEY = 'YOUR_API_KEY'
	API_SECRET = 'YOUR_API_SECRET'

如果您注册时选择了亚马逊（美国）服务器，请使用如下配置：

	SERVER = 'http://api.us.faceplusplus.com/'

	API_KEY = 'YOUR_API_KEY'
	API_SECRET = 'YOUR_API_SECRET'

保存apikey.cfg，应用配置成功。

### 4.2. 试用python SDK示例
#### 4.2.1 下载facepp-python-sdk
```bash
git clone https://github.com/FacePlusPlus/facepp-python-sdk

cd facepp-python-sdk
```

#### 4.2.2 添加自己的API key和API Secret
```bash
vim hello.py
```

找到如下的语句，修改前面申请到的API key和API secret。

	# You need to register your App first, and enter you API key/secret.
	# 您需要先注册一个App，并将得到的API key和API secret写在这里。
	API_KEY = '<your API key here>'
	API_SECRET = '<your API secret here>'

同样，还要修改apikey.cfg这个文件。

#### 4.2.3 运行演示程序

```bash
python hello.py
```

该程序的处理过程如下：
1. 识别网站上提供的3张人脸照片，并提取特征
2. 给每张脸创建一个人
3. 创建一个组，再将这三人加入到这个组中
4. 为了进行识别（identify），必须对组进行训练。
5. 再用第4张人脸去组中找寻最相似的人脸，并输入对比结果：

```bash
facepp-python-sdk-master$ python hello.py
Jim Parsons
  {'face': [{'attribute': {'age': {'range': 7, 'value': 33},
                           'gender': {'confidence': 99.9936,
                                      'value': 'Male'},
                           'race': {'confidence': 97.6082,
                                    'value': 'White'},
                           'smiling': {'value': 2.70393}},
             'face_id': '1f470bc56b3e361d90d87545e8b836af',
             'position': {'center': {'x': 42.19457, 'y': 24.166667},
                          'eye_left': {'x': 37.580317, 'y': 20.741},
                          'eye_right': {'x': 46.056561, 'y': 20.11},
                          'height': 13.333333,
                          'mouth_left': {'x': 39.800679, 'y': 27.785167},
                          'mouth_right': {'x': 46.00181, 'y': 27.373333},
                          'nose': {'x': 41.772398, 'y': 24.882167},
                          'width': 17.873303},
             'tag': ''}],
   'img_height': 1220,
   'img_id': '0cf42f603fc9ddcbf890f7b8d49b40d4',
   'img_width': 900,
   'session_id': '1f6e7ff3102d4a08a11da73dd04f47e8',
   'url': 'http://www.faceplusplus.com.cn/static/resources/python_demo/1.jpg'}
...
============================================================
The person with highest confidence: Jim Parsons
```

## 5. 如何封装Face++ API
如果你想调用Face++ 的API，不得不自己手动封装这些API接口。当然，本人也封装了一些基本的python脚本，已经放到了github上。可以下载下来，供参考：

```bash
$ git clone https://github.com/YaoQ/faceplusplus-demo
```

基本封装命令的介绍：
```bash
python create_group.py  <group_name> #创建一个组
python detection.py <image_path> #提取一张人脸照片的信息
python add_new_face.py  <person_name> <image_path> <group_name> #将某个人的脸加入到组中，并训练模型
python recognition.py <group_name> <face_image_path> #从组中找寻最像的一张人脸。
python dump_info.py    #列出创建的人名和组名
```
其他的命令：
```bash
python del_group.py <group_name> #删除一个组
python del_person.py <person_name> #删除一个人
```

更多API相关的内容，请查看 [Face++ API文档][1].

## 6. 测试人脸检测
根据本人在github上提供的示例，我试着使用**Face++ API:/detection/detect**去检测 Tom Cruise的脸。
![tom](/images/tom.jpg)

```bash
$ cd faceplusplus-demo
$ python detection.py images/tom.jpg
```
检测得到了这张脸的基本信息，内容如下：
```
 Detection result for {}:
   {'face': [{'attribute': {'age': {'range': 10, 'value': 37},
                            'gender': {'confidence': 99.9999,
                                       'value': 'Male'},
                            'race': {'confidence': 98.9731,
                                     'value': 'White'},
                            'smiling': {'value': 67.826}},
              'face_id': '5ca95ae41e7319170bb8bf022ad677f3',
              'position': {'center': {'x': 48.305085, 'y': 54.666667},
                           'eye_left': {'x': 39.201864, 'y': 45.51},
                           'eye_right': {'x': 58.873051, 'y': 47.075833},
                           'height': 44.333333,
                           'mouth_left': {'x': 38.516441, 'y': 65.636333},
                           'mouth_right': {'x': 56.491356,
                                           'y': 66.942833},
                           'nose': {'x': 46.791017, 'y': 58.3435},
                           'width': 45.084746},
              'tag': ''}],
    'img_height': 630,
    'img_id': '6bef5d3f0f1bdb0f3840bdd34a54ad30',
    'img_width': 620,
    'session_id': '78919817e956400d88398e3535f85cb6',
    'url': None}  
```
## 7. 识别一张人脸
这一步，我提取了3张人脸的信息，并针对每张脸创建了一个人，再将这3个人加入到一个叫做**test**的组中。然后再找一张新的人脸，去跟组中所有的脸进行匹配，并找出最匹配的那一个人。测试图片有：奥巴马、汤姆·克鲁斯和姚明：
![obama](/images/obama.jpg)
![tom](/images/tom.jpg)
![yao](/images/yao.jpg)

用于测试的人脸（注意：**这是一张手绘画**）：
![test](/images/test.jpg)

基本的操作过程如下：
```bash
$ python create_group.py test
$ python add_new_face.py tom images/tom.jpg test
$ python add_new_face.py obama images/obama.jpg test
$ python add_new_face.py yao images/yao.jpg test
$ python recognition.py test images/test.jpg
```

**识别的结果**

>The person with highest confidence: obama
Confidence is : 12.066551

图片一对比，就知道检测的结果不尽如人意，当然仅仅是几张图片的对比，并不能说明什么问题。一个人，最多可以添加10000张照片，如果这样大规模的加入一个人人脸的特征，识别的准确性应该会有很大提升。

## 结论
Face++API对图片有一定质量的要求，而且因为是免费使用，不保证调用服务的稳定性，可能出现内部错误的情况。

当然，总的来说，Face++的人脸识别比OpenCV更加实用。

## 可能遇到的问题：

需要注意的一些事情：
1.  图片质量的要求

![](/images/quality.png)
2. 常见的错误
 - INTERL_ERROR，可能的原因如下：

![](/images/error.png)
- 经常出现IndexError，python直接跳出，然后无法重新运行演示程序，很有可能图片中找不到人脸，于是无法打印face字典中相关的词条，所以会报错。解决方案是添加try和except语句，或判断face词条是否为空。

>    rst['face'][0]['candidate'][0]['person_name']
>	IndexError: list index out of range

[1]:http://www.faceplusplus.com.cn/api-overview/