---
layout: post
title: OpenCV(3)
date: 2020-03-30
summary: DNN, caffe, YOLO
categories: Python
mathjax: true
---

### Face Detection

#### 2. Deep Neural Network(DNN)

- Object detection 보다 높은 정확도를 보이나 좋은 컴퓨터 성능을 요구함(특히 동영상에서 많은 차이를 보임)
- 사용자가 confidence(신뢰도)를 설정하면 그 이상의 신뢰도를 지닌 face detection 결과만을 보여준다

- 마라톤 Image를 이용한 실습

  - caffe 모델을 사용함 [다운로드 링크](https://github.com/thegopieffect/computer_vision/blob/master/CAFFE_DNN/res10_300x300_ssd_iter_140000.caffemodel)

  - 원본 Image

    ![marathon_01](https://user-images.githubusercontent.com/52812181/77939163-b0f8c680-72f1-11ea-8dc9-01208bbf12d1.jpg)

  ```python
  import cv2
  import numpy as np
  
  model_name = 'res10_300x300_ssd_iter_140000.caffemodel' # caffe 학습된 모델 파일
  prototxt_name = 'deploy.prototxt.txt' # 학습이 완료된 모델에 임의의 입력을 다룰 때 사용함
  min_confidence = 0.15 # 최소 신뢰도
  file_name = "image/marathon_01.jpg"
  
  def detectAndDisplay(frame):
      # pass the blob through the model and obtain the detections
      model = cv2.dnn.readNetFromCaffe(prototxt_name, model_name) # opencv4.0 부터는 readNet()으로 통합하여 하는 것이 좋다
  
      # Resizing to a fixed 300x300 pixels and then normalizing it
      # cv2.dnn.blobFromImage()는 4차원 BLOB을 만듦
      # cv2.dnn.blobFromImage(image, scalefactor, size, Scalar&Mean, swapRB)
          # image : blob을 통해 사전 처리하기를 원하는 이미지
          # scalefactor : 평균빼기를 시전한 후 스케일할 값 (R -ur)/a 에서 a 값
          # size : 공간 크기
          # mean : 평균 빼기 값, 튜플이거나 단일 값일 수도 있습니다.
          # swapRB : RB의 순서를 변경
      blob = cv2.dnn.blobFromImage(cv2.resize(frame, (300, 300)), 1.0,
              (300, 300), (104.0, 177.0, 123.0))
  
      model.setInput(blob)
      detections = model.forward()
  
      # loop over the detections
      for i in range(0, detections.shape[2]):
              # extract the confidence (i.e., probability) associated with the
              # prediction
              confidence = detections[0, 0, i, 2]
  
              # filter out weak detections by ensuring the `confidence` is
              # greater than the minimum confidence
              if confidence > min_confidence:
                  # compute the (x, y)-coordinates of the bounding box for the
                  # object
                  box = detections[0, 0, i, 3:7] * np.array([width, height, width, height])
                  (startX, startY, endX, endY) = box.astype("int")
                  print(confidence, startX, startY, endX, endY)
  
                  # draw the bounding box of the face along with the associated
                  # probability
                  text = "{:.2f}%".format(confidence * 100)
                  y = startY - 10 if startY - 10 > 10 else startY + 10
                  cv2.rectangle(frame, (startX, startY), (endX, endY),
                          (0, 255, 0), 2)
                  cv2.putText(frame, text, (startX, y),
                          cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 0), 1)
  
      # show the output image
      cv2.imshow("Face Detection by dnn", frame)
      
  
  img = cv2.imread(file_name)
  cv2.imshow("Original Image", img)
  
  detectAndDisplay(img)
  
  cv2.waitKey(0)
  cv2.destroyAllWindows()
  ```

  ![image](https://user-images.githubusercontent.com/52812181/77940278-6bd59400-72f3-11ea-8a1d-c4b42ec0c39a.png)



- 동영상 DNN 실습

  - 이 전의 Object detection과 상이한 부분은 caffemodel과 deploy.prototxt를 사용한다는 것외엔 없음

  ```python
  import cv2
  import numpy as np
  from tkinter import *
  from PIL import Image
  from PIL import ImageTk
  from tkinter import filedialog
  
  model_name = 'res10_300x300_ssd_iter_140000.caffemodel'
  prototxt_name = 'deploy.prototxt.txt'
  min_confidence = 0.5
  file_name = 'video/india.mp4'
  title_name = 'dnn Deep Learnig object detection Video'
  frame_width = 300
  frame_height = 300
  cap = cv2.VideoCapture() # 전역변수 사용을 위한 정의
  
  def selectFile():
      file_name =  filedialog.askopenfilename(initialdir = "./video",title = "Select file",filetypes = (("MP4 files","*.mp4"),("all files","*.*")))
      print('File name : ', file_name)
      global cap # 전역변수 호출
      cap = cv2.VideoCapture(file_name)
      detectAndDisplay() # 재귀함수
      
  def detectAndDisplay():
      _, frame = cap.read()
      (h, w) = frame.shape[:2]
      # pass the blob through the model and obtain the detections 
      model = cv2.dnn.readNetFromCaffe(prototxt_name, model_name)
  
      # Resizing to a fixed 300x300 pixels and then normalizing it
      blob = cv2.dnn.blobFromImage(cv2.resize(frame, (300, 300)), 1.0,
              (300, 300), (104.0, 177.0, 123.0))
  
      model.setInput(blob)
      detections = model.forward()
  
      min_confidence = float(sizeSpin.get()) # 사용자가 sizeSpin에 설정한 신뢰도값을 받아 적용
      
      # loop over the detections
      for i in range(0, detections.shape[2]):
              # extract the confidence (i.e., probability) associated with the
              # prediction
              confidence = detections[0, 0, i, 2]
  
              # filter out weak detections by ensuring the `confidence` is
              # greater than the minimum confidence
              if confidence > min_confidence:
                      (height, width) = frame.shape[:2]
                      # compute the (x, y)-coordinates of the bounding box for the
                      # object
                      box = detections[0, 0, i, 3:7] * np.array([w, h, w, h])
                      (startX, startY, endX, endY) = box.astype("int")
       
                      # draw the bounding box of the face along with the associated
                      # probability
                      text = "{:.2f}%".format(confidence * 100)
                      y = startY - 10 if startY - 10 > 10 else startY + 10
                      cv2.rectangle(frame, (startX, startY), (endX, endY),
                              (0, 255, 0), 2)
                      cv2.putText(frame, text, (startX, y),
                              cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 0), 1)
  
      cv2image = cv2.cvtColor(frame, cv2.COLOR_BGR2RGBA) # 항상 기억하자, opencv는 BGR로 구성되어있음을...(tk는 RGB)
      img = Image.fromarray(cv2image)
      imgtk = ImageTk.PhotoImage(image=img)
      lmain.imgtk = imgtk
      lmain.configure(image=imgtk)
      lmain.after(10, detectAndDisplay) 
      
      
  #main
  main = Tk()
  main.title(title_name)
  main.geometry()
  
  #Graphics window
  label=Label(main, text=title_name)
  label.config(font=("Courier", 18))
  label.grid(row=0,column=0,columnspan=4)
  sizeLabel=Label(main, text='Min Confidence : ')
  sizeLabel.grid(row=1,column=0)
  sizeVal  = IntVar(value=min_confidence)
  sizeSpin = Spinbox(main, textvariable=sizeVal,from_=0, to=1, increment=0.05, justify=RIGHT)
  sizeSpin.grid(row=1, column=1)
  
  Button(main,text="File Select", height=2,command=lambda:selectFile()).grid(row=1, column=2, columnspan=2, sticky=(W, E))
  imageFrame = Frame(main)
  imageFrame.grid(row=2,column=0,columnspan=4)
    
  #Capture video frames
  lmain = Label(imageFrame)
  lmain.grid(row=0, column=0)
  
  main.mainloop()  #Starts GUI
  ```



​	![image](https://user-images.githubusercontent.com/52812181/77941222-c8857e80-72f4-11ea-9889-1e6e86b49c49.png)

​	![india_vid_dnn](https://user-images.githubusercontent.com/52812181/77941685-87da3500-72f5-11ea-805b-8658a28a65b4.gif)



![image](https://user-images.githubusercontent.com/52812181/77941748-9e808c00-72f5-11ea-8ff2-ceb54802c59c.png)

![india_vid_dnn2](https://user-images.githubusercontent.com/52812181/77942293-5dd54280-72f6-11ea-9d10-eb143cbe7721.gif)



- 최소 신뢰도를 낮출수록 많은 얼굴을 포착하지만 얼굴이 아닌 다른 곳까지 얼굴로 인식할 오류를 범할 수 있다
