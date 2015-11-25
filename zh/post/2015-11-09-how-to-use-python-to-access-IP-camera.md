# 如何使用python和opencv获取IP摄像头视频

### 使用的IP摄像头参数
<table>
   <tr>
      <td>Image Sensor</td>
      <td> 1/4″CMOS</td>
   </tr>
   <tr>
      <td> Resolution</td>
      <td> 1280×720</td>
   </tr>
   <tr>
      <td> Pixel</td>
      <td> 1.0MP</td>
   </tr>
   <tr>
      <td> Compression</td>
      <td> H.264/JPEG</td>
   </tr>
   <tr>
      <td> Frame Rate</td>
      <td> 25fps</td>
   </tr>
   <tr>
      <td> Bit Rate</td>
      <td> 64Kbps～8Mbps</td>
   </tr>
   <tr>
      <td> Min.Illumination</td>
      <td> 0Lux(IR ON)</td>
   </tr>
</table>


```python
vcap = cv.VideoCapture("rtsp://192.168.1.2:8080/out.h264")

while(1):

    ret, frame = vcap.read()
    cv.imshow('VIDEO', frame)
    cv.waitKey(1)
```

```C

int main(int argc, char **argv){

  IplImage *pFrame = NULL, *srcImage=NULL;

 CvCapture *pCapture = NULL;

 //pCapture = cvCaptureFromFile("rtsp://admin:12345@192.168.7.45:554/h264/ch1/main/av_stream");
 pCapture = cvCreateFileCapture("rtsp://admin:default@10.1.10.173/h264");
 //pCapture = cvCreateCameraCapture(1);
 if(!pCapture){
  printf("Can not get the video stream from the camera!\n");
  return NULL;
 }

 //read the video by frame
 //while(1)
 while(1){
  //pFrame = cvQueryFrame(pCapture);
  if (srcImage==NULL)
  {
   pFrame = cvQueryFrame(pCapture);
   srcImage=cvCloneImage(pFrame);
   cvShowImage("123234",srcImage);
   //cout<<pFrame->width<<","<<pFrame->height<<endl;
   cvWaitKey(10);
   cvReleaseImage(&srcImage);
   srcImage=NULL;
  }

 }
 cvReleaseCapture(&pCapture);
 cvReleaseImage(&pFrame);


 return 0;
}
```
