---
layout: post
title: OpenCV - 캠(Cam)을 이용하 얼굴인식 테스트 / OpenCV Filter Test / Naver 얼굴인식 API
mathjax: true
tags:
  - python
---

<br/>

## Haar Cascade 방식을 이용한 얼굴 인식

```python
import cv2
import numpy as np

detector = cv2.CascadeClassifier('./haarcascade_frontalface_default.xml')
cap = cv2.VideoCapture(0)

while (True):
    ret, img = cap.read()
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    faces = detector.detectMultiScale(gray, 1.3, 5)
    for (x, y, w, h) in faces:
        cv2.rectangle(img, (x,y), (x+w, y+h), (255,0,0), 2)
    
    cv2.imshow('frame', img)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

#### 실제 필자의 실행 결과(늦은 밤이라 머리가 엉망이니 이해부탁..)

![face](https://user-images.githubusercontent.com/52812181/78578912-240eb980-786b-11ea-9d60-98ad8e28dcc4.png)

- 정면은 잘 인식한다, 다만 측면으로 가게되면 전혀 인식하지 못하거나 이상한 부분을 인식하기도 한다

  ![face2](https://user-images.githubusercontent.com/52812181/78579005-46a0d280-786b-11ea-8be9-76f2996ac1dd.png)

<br/>



## openCV 가장자리 검출 Filter

- Canny : cv2.Canny(원본 이미지, 임계값1, 임계값2, 커널 크기, L2그라디언트)

- Laplacian : cv2.Laplacian(그레이스케일 이미지, 정밀도, 커널, 배율, 델타, 픽셀 외삽법)

- Sobel : cv2.Sobel(그레이스케일 이미지, 정밀도, x방향 미분, y방향 미분, 커널, 배율, 델타, 픽셀 외삽법)

  - x와 y를 나누어 설정도 가능하다.

- *왼쪽 위부터 시계방향으로 sobel, laplacian, sobel y축, sobel x축*

  ![filter](https://user-images.githubusercontent.com/52812181/78580031-c7ac9980-786c-11ea-8da8-576d02880130.png)

<br/>



## 공을 찾아 인식하고 공의 이동 경로를 보여주는 예제

- jupyter 환경에서는 불가능하므로, 터미널이용 권장

```python
# import the necessary packages
from collections import deque
from imutils.video import VideoStream
import numpy as np
import argparse
import cv2
import imutils
import time
 
# construct the argument parse and parse the arguments
ap = argparse.ArgumentParser()
ap.add_argument("-v", "--video",
    help="path to the (optional) video file")
ap.add_argument("-b", "--buffer", type=int, default=64,
    help="max buffer size")
args = vars(ap.parse_args())



# define the lower and upper boundaries of the "green"
# ball in the HSV color space, then initialize the
# list of tracked points
greenLower = (29, 86, 6)
greenUpper = (64, 255, 255)
pts = deque(maxlen=args["buffer"])
 
# if a video path was not supplied, grab the reference
# to the webcam
if not args.get("video", False):
    vs = VideoStream(src=0).start()
 
# otherwise, grab a reference to the video file
else:
    vs = cv2.VideoCapture(0)
 
# allow the camera or video file to warm up
time.sleep(2.0)
 
# keep looping
while True:
    # grab the current frame
    frame = vs.read()
 
    # handle the frame from VideoCapture or VideoStream
    frame = frame[1] if args.get("video", False) else frame
 
    # if we are viewing a video and we did not grab a frame,
    # then we have reached the end of the video
    if frame is None:
        break
 
    # resize the frame, blur it, and convert it to the HSV
    # color space
    frame = imutils.resize(frame, width=600)
    blurred = cv2.GaussianBlur(frame, (11, 11), 0)
    hsv = cv2.cvtColor(blurred, cv2.COLOR_BGR2HSV)
 
    # construct a mask for the color "green", then perform
    # a series of dilations and erosions to remove any small
    # blobs left in the mask
    mask = cv2.inRange(hsv, greenLower, greenUpper)
    mask = cv2.erode(mask, None, iterations=2)
    mask = cv2.dilate(mask, None, iterations=2)
 
    # find contours in the mask and initialize the current
    # (x, y) center of the ball
    cnts = cv2.findContours(mask.copy(), cv2.RETR_EXTERNAL,
        cv2.CHAIN_APPROX_SIMPLE)
    cnts = imutils.grab_contours(cnts) 
    center = None
 
    # only proceed if at least one contour was found
    if len(cnts) > 0:
        # find the largest contour in the mask, then use
        # it to compute the minimum enclosing circle and
        # centroid
        c = max(cnts, key=cv2.contourArea)
        ((x, y), radius) = cv2.minEnclosingCircle(c)
        M = cv2.moments(c)
        center = (int(M["m10"] / M["m00"]), int(M["m01"] / M["m00"]))
 
        # only proceed if the radius meets a minimum size
        if radius > 10:
            # draw the circle and centroid on the frame,
            # then update the list of tracked points
            cv2.circle(frame, (int(x), int(y)), int(radius),
                (0, 255, 255), 2)
            cv2.circle(frame, center, 5, (0, 0, 255), -1)
 
    # update the points queue
    pts.appendleft(center)
 
    # loop over the set of tracked points
    for i in range(1, len(pts)):
        # if either of the tracked points are None, ignore
        # them
        if pts[i - 1] is None or pts[i] is None:
            continue
 
        # otherwise, compute the thickness of the line and
        # draw the connecting lines
        thickness = int(np.sqrt(args["buffer"] / float(i + 1)) * 2.5)
        cv2.line(frame, pts[i - 1], pts[i], (0, 0, 255), thickness)
 
    # show the frame to our screen
    cv2.imshow("Frame", frame)
    key = cv2.waitKey(1) & 0xFF
 
    # if the 'q' key is pressed, stop the loop
    if key == ord("q"):
        break
 
# if we are not using a video file, stop the camera video stream
if not args.get("video", False):
    vs.stop()
 
# otherwise, release the camera
else:
    vs.release()
 
# close all windows
cv2.destroyAllWindows()
```

- 실행 결과는 소스제공 사이트를 통해 보는걸로!! [사이트 링크](https://www.pyimagesearch.com/2015/09/14/ball-tracking-with-opencv/)

<br/>



## Naver Clova API를 이용한 얼굴인식

- 소스를 실행하기에 앞서 Clova API 이용을 위해 가입이 필요하다
  - 가입과 API 발급과정은 네이버에서 잘 안내 해 주므로 pass
  - [Naver Developers](https://developers.naver.com/main/)

```python
# 한 명의 얼굴만 있는 사진에서의 Source
import cv2
import requests
import json
client_id = "your_id" # 발급받은 id 입력
client_secret = "your_secret" # 발급받은 secret key 입력
url = "https://openapi.naver.com/v1/vision/face"
files = {'image': open('./face.jpeg', 'rb')}
headers = {'X-Naver-Client-Id': client_id, 'X-Naver-Client-Secret': client_secret }
response = requests.post(url,  files=files, headers=headers)
rescode = response.status_code
if(rescode==200):
    print (response.text)
else:
    print("Error Code:" + rescode)
json_data = json.loads(response.text)
x, y, w, h = json_data['faces'][0]['roi'].values()
img = cv2.imread('face', cv2.IMREAD_COLOR)
img = cv2.rectangle(img, (x, y), (x+w, y+h), (255,0,0), 3)
cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### 출력 결과

![naver_face](https://user-images.githubusercontent.com/52812181/78583176-6b984400-7871-11ea-93c2-aa6f00bb7d56.png)

```python
# 여러 명의 얼굴이 있는 사진에서의 Source
# 위의 소스와 비슷하나 json_data 아래부터 달라진다.(for문 이용)
json_data = json.loads(response.text)
img = cv2.imread('./face_group.jpg', cv2.IMREAD_COLOR)
for i in range(len(json_data['faces'])):
    x, y, w, h = json_data['faces'][i]['roi'].values()
    frame = cv2.rectangle(img, (x, y), (x+w, y+h), (255,0,0), 3)
    cv2.imshow('image', frame)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### 출력 결과

![face_group_detect](https://user-images.githubusercontent.com/52812181/78583349-b31ed000-7871-11ea-8ab9-2957d3fdc702.png)









