# How to Use Face++ API on pcDuino8 Uno
Face++ provides the free API and SDK of face detection, recognition and analysis, along with custom cloud services, for companies that want to use its facial recognition. The service can detect 83 points of the face and analyze age, gender, race and so on. 

This post will tell you how to use Face++ python API on pcDuino8 Uno to implement facial recognition.

## Prepare
###  Hardware
- pcDuino8 Uno

### Software
- python

## Steps
 
### 1. Register Face++ dev Account and Create a new App
For proper operation of the development environment, you need to have created an App and got the corresponding API Key and API Secret, follow these steps:

1. Click into the Dec center page, click on “My App”, you need to regist or login in first.

2. Click the icon ![create_new_app_2](http://www.faceplusplus.com/wp-content/uploads/2013/11/create_new_app_2.png)on the center of the screen to create a new App.
![1](http://www.faceplusplus.com/wp-content/uploads/2013/11/1.png)

3. Click SUBMIT, it turns into App information interface automatically.You will see API Key and API Secret.

![2](http://www.faceplusplus.com/wp-content/uploads/2013/11/21.png)
4. Copy the API Key and API Secret to your API-related documents such as apikey.cfg (many official downloaded SDK contains this file ) , or your own program.

 
**PAY ATTENTION**: If you choose Aliyun(China) server, please use the following configuration:
```python
SERVER = 'http://api.cn.faceplusplus.com/'

API_KEY = 'YOUR_API_KEY' API_SECRET = 'YOUR_API_SECRET'
If you choose Amazon(US) server,please use the following configuration:

SERVER = 'http://api.us.faceplusplus.com/'

API_KEY = 'YOUR_API_KEY'
API_SECRET = 'YOUR_API_SECRET'

Save apikey.cfg, application configuration success!
```
### 2. Download facepp-python-sdk and add API key and secret
```bash
$ git clone https://github.com/FacePlusPlus/facepp-python-sdk
```
Modify apikey.cfg. Add your API key and API secret , and switch to US server:
```bash
$ vim apikey.cfg
```
Like:
```python
# SERVER = 'http://api.cn.faceplusplus.com/'
# uncomment the following line to switch to US server
SERVER = 'http://api.us.faceplusplus.com/'

API_KEY = '<your API key here>'
API_SECRET = '<your API secret here>'
```

Modify hello.py, and Add your API key and API secret.
```bash
$ vim hello.py 
```

### 3. Run demo
This demo run as the following steps:
1. Detect faces in the 3 pictures which are on Internet, and find out their positions and attributes.
2. Create persons using the face_id for each face.
3. Create a new group and add those persons in it.
4. Train the model.
5. Recognize face in a new image.

And this is the recognition result: 
```
recognition result
  {'face': [{'candidate': [{'confidence': 12.613228,
                            'person_id': '660ff5c03b80968b8556e1b91c304a3d',
                            'person_name': 'Jim Parsons',
                            'tag': ''},
                           {'confidence': 2.212482,
                            'person_id': '670a30aec37967f238253ed29afe8557',
                            'person_name': 'Leonardo DiCaprio',
                            'tag': ''},
                           {'confidence': 0.0,
                            'person_id': '69f3024b47251887914c6634fe3c20c5',
                            'person_name': 'Andy Liu',
                            'tag': ''}],
             'face_id': '20f06a8387b6e98062ba8aa66203040a',
             'position': {'center': {'x': 54.522613, 'y': 26.333333},
                          'eye_left': {'x': 47.511307, 'y': 23.069},
                          'eye_right': {'x': 60.246985, 'y': 22.802},
                          'height': 19.0,
                          'mouth_left': {'x': 48.529648, 'y': 32.322333},
                          'mouth_right': {'x': 59.838191,
                                          'y': 32.016167},
                          'nose': {'x': 55.24196, 'y': 27.1255},
                          'width': 28.643216}}],
   'session_id': '17112916268e4471967d5c25234dba47'}
============================================================
The person with highest confidence: Jim Parsons
```
### 4. How to wrap face++ API
If you want to call face++ APIs, you have to wrap them by yourself. Also I wrapped some face++ python API and upload these codes to github, you can download them using the following command as a template.

```bash
$ git clone https://github.com/YaoQ/faceplusplus-demo
```

More APIs' information please check [Face++ API Overview][1].

### 5. Face Detection test
I use **Face++ API:/detection/detect** to detect Tom Cruise' face.
![tom](/images/tom.jpg)
```bash
$ cd faceplusplus-demo
$ python detection.py images/tom.jpg
```
And this is the result, and I think it's not bad!
>
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

### 6. Recognize a new face
In this step, I will detect 3 face images and add them to a new group, called **test**. Then I will take a new face and try to find which face in the group is most match.
Obama, Tom and Yao:
![obama](/images/obama.jpg)
![tom](/images/tom.jpg)
![yao](/images/yao.jpg)

This is the new face to test:
![test](/images/test.jpg)
```bash
$ python create_group.py test
$ python add_new_face.py tom images/tom.jpg test
$ python add_new_face.py obama images/obama.jpg test
$ python add_new_face.py yao images/yao.jpg test
$ python recognition.py test images/test.jpg
```

**Recognition result:**

>The person with highest confidence: obama
Confidence is : 12.066551

## Conclusion
Face++ API requires the high quality of face image. If you give a blurry image, the API will return error, said no face detected. And because it is free, so it can't make sure service is stable.

Anyway, this face detection is more impractical than OpenCV.

[1]:http://www.faceplusplus.com/api-overview/
