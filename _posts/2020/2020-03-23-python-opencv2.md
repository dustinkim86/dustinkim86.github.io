---
layout: post
title: OpenCV(2)
date: 2020-03-23
summary: 이미지 Masking, RGB 색상 조작, Object Detection(Haar 방식)을 이용한 이미지/동영상 얼굴&눈 인식
categories: Python
mathjax: true
---

### Image Masking

- 원본 Image

  ![pengsu](https://user-images.githubusercontent.com/52812181/77333373-ab860400-6d66-11ea-85c1-98e296245aa6.jpg)

- Source

  ```python
  import cv2
  import numpy as np
  
  img = cv2.imread('./pengsu.jpg')
  
  # 이미지 사이즈 살펴보기
  print('width: ', img.shape[1]) # 640
  print('height: ', img.shape[0]) # 405
  print('channels: ', img.shape[2]) # 3
  
  # 이미지 중심점 추출
  (height, width) = img.shape[:2]
  center = (width // 2, height // 2)
  
  cv2.imshow('nomadProgrammer', img)
  
  # 이미지 사이즈(가로/세로)와 동일한 사이즈의 0으로 된 array 생성
  mask = np.zeros(img.shape[:2], dtype="uint8")
  
  # 지난 포스팅에서 배운 cv2.circle을 이용해 mask의 중심점을 기준으로 크기 250, 색상(255,255,255), 전체(-1)을 생성
  cv2.circle(mask, center, 250, (255, 255, 255), -1) 
  cv2.imshow("mask", mask)
  
  # and비트연산(cv2.bitwise_and)을 이용해 img에 mask를 씌움
  masked = cv2.bitwise_and(img, img, mask = mask) # bitwise (and, or, not, xor)
  cv2.imshow('nomadProgrammer with mask', masked)
  
  cv2.waitKey(0)
  cv2.destroyAllWindows()
  ```

- 결과 Image

  ![pengsu_mask](https://user-images.githubusercontent.com/52812181/77334354-04a26780-6d68-11ea-8dc6-f7f5fa1361e5.jpg)

<br>

### Image RGB 다루기

- RGB 색상별 분해(각 색상이 관여된 부분이 없다면 검정색으로 출력)

  ```python
  (Blue, Green, Red) = cv2.split(img)
  cv2.imshow('Red Channel ', Red)
  cv2.imshow('Green Channel ', Green)
  cv2.imshow('Blue Channel ', Blue)
  cv2.waitKey(0)
  ```

  - Red Channel

    ![pengsu_red](https://user-images.githubusercontent.com/52812181/77334910-c35e8780-6d68-11ea-84cc-803b0b31f712.jpg)

  - Green Channel

    ![pengsu_green](https://user-images.githubusercontent.com/52812181/77334937-d2453a00-6d68-11ea-83ff-0447acf93880.jpg)

  - Blue Channel

    ![pengsu_blue](https://user-images.githubusercontent.com/52812181/77334980-df622900-6d68-11ea-80eb-53dc49817b70.jpg)

- 원본 Image를 RGB 색상으로 나누기

  ```python
  zeros = np.zeros(img.shape[:2], dtype="uint8")
  cv2.imshow('Red', cv2.merge([zeros, zeros, Red]))
  cv2.imshow('Green', cv2.merge([zeros, Green, zeros]))
  cv2.imshow('Blue', cv2.merge([Blue, zeros, zeros]))
  cv2.waitKey(0)
  cv2.destroyAllWindows()
  ```

  ![blue](https://user-images.githubusercontent.com/52812181/77335400-70390480-6d69-11ea-942a-c1d86604a40f.jpg)
  ![green](https://user-images.githubusercontent.com/52812181/77335405-716a3180-6d69-11ea-9623-89a59dc1ef61.jpg)
  ![red](https://user-images.githubusercontent.com/52812181/77335407-7202c800-6d69-11ea-9301-e0110cc521d6.jpg)

- cv2.merge를 이용해 RGB 색상 값을 합쳐 원본처럼 만들기

  ```python
  BGR = cv2.merge([Blue, Green, Red])
  cv2.imshow('blue, green adn red', BGR)
  cv2.waitKey(0)
  cv2.destroyAllWindows() # 결과는 원본과 동일하므로 생략
  ```

<br><br>

### Face Detection

#### face detection의 방법은 두 가지로 나뉨

- Object detection

- deep neural network

<br>

#### 1. Object detection

- cascade classifier : Haar 방식(*cnn과는 다른 접근 방식*)
- 장점 : 빠르고 가볍다
- 단점 : CNN보다 정확도가 떨어진다
- 공통점 : CNN과 같이 구역을 나누어 실행한다

- 축구선수 Image를 이용한 실습

  - 원본 Image

    ![soccer_01](https://user-images.githubusercontent.com/52812181/77336203-88f5ea00-6d6a-11ea-8e00-d91cad0c332d.jpg)

  ```python
  import cv2
  import numpy as np
  
  def detectAndDisplay(frame):
      # 정확도를 높이기 위해 그레이 스케일로 변환하여 detection 시작
      frame_gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
      # cv2.equalizeHist() : 히스토그램 평활화
      # 픽셀값들의 누적분포함수를 이용해 어두운 부분을 밝게, 밝은 부분을 어둡게하여 선명한 이미지를 얻음
      frame_gray = cv2.equalizeHist(frame_gray)
      # -- Detect Faces
      # detectMultiScale() : 크기가 다른 object를 검출하는 함수(하단의 xml 파일(트레이닝 데이터)을 불러와 cv2.CascadeClassifier()(다단계 분류)를 통해 어떤 object를 검출할 지 정함)
      # 해당 실습에서는 얼굴과 눈을 검출
      # 얼굴 검출
      faces = face_cascade.detectMultiScale(frame_gray)
      # (x값, y값) : 검출된 얼굴의 좌상단 위치 /  w : 가로크기, h : 세로크기
      for (x, y, w, h) in faces: 
          center = (x + w//2, y + h//2)
          # 얼굴위치 사각형으로 표시
          frame = cv2.rectangle(frame, (x,y), (x+w, y+h), (0, 255,0), 4)
          # 얼굴 위치 좌표 추출
          faceROI = frame_gray[y:y+h, x:x+w]
          # 눈 검출
          # -- In each face, detect eyes
          eyes = eyes_cascade.detectMultiScale(faceROI)
          for (x2, y2, w2, h2) in eyes:
              eye_center = (x + x2 + w2//2, y + y2 + h2//2)
              radius = int(round((w2 + h2)*0.25))
              # 눈위치 원으로 표시
              frame = cv2.circle(frame, eye_center, radius, (255,0.0), 4)
          cv2.imshow('capture - face detection', frame)
  
  
  img = cv2.imread('./soccer.jpg')
  
  # Haar cascade 트레이닝 데이터
  face_cascade_name = './opencv_dnn/data/haarcascades_cuda/haarcascade_frontalface_alt.xml'
  eyes_cascade_name = './opencv_dnn/data/haarcascades_cuda/haarcascade_eye_tree_eyeglasses.xml'
  # 다단계 분류
  face_cascade = cv2.CascadeClassifier()
  eyes_cascade = cv2.CascadeClassifier()
  
  # -- 1. Load the cascades
  if not face_cascade.load(cv2.samples.findFile(face_cascade_name)):
      print('--(!)Error loading face cascade')
      exit(0)
  if not eyes_cascade.load(cv2.samples.findFile(eyes_cascade_name)):
      print('--(!)Error loading eyes cascade')
      exit(0)
  
  detectAndDisplay(img)
  
  cv2.waitKey(0)
  cv2.destroyAllWindows()
  ```

  - 결과 출력 : 얼굴은 다 잘 잡으나 눈은 전혀 잡지 못함

    ![soccer](https://user-images.githubusercontent.com/52812181/77338508-d031aa00-6d6d-11ea-895f-28b1c6445b1b.jpg)

  - **cv2.equalizeHist()** 참고 [**Site**](https://gaussian37.github.io/vision-opencv-equalizeHist/)

- GUI를 이용한 Haar cascade 이미지의 얼굴/눈 검출

  ```python
  import cv2
  import numpy as np
  # tkinter gui Modele
  from tkinter import *
  from tkinter import filedialog
  # Image handling Module
  # conda install -c conda-forge pillow / pip install Pillow
  # 이미지 포맷 변환, 크기 변형, 회줜, transform, filtering등의 프로세싱 작업 가능
  from PIL import Image
  from PIL import ImageTk
  
  face_cascade_name = './opencv_dnn/data/haarcascades_cuda/haarcascade_frontalface_alt.xml'
  eyes_cascade_name = './opencv_dnn/data/haarcascades_cuda/haarcascade_eye_tree_eyeglasses.xml'
  
  face_cascade = cv2.CascadeClassifier()
  eyes_cascade = cv2.CascadeClassifier()
  
  file_name = './soccer.jpg'
  title_name = 'Haar cascade object detection'
  frame_width = 500 # 사이즈를 크게하면 인식하지 못하던 것을 인식하는 경우도 있음(우선 500 기준으로 정함)
  ```

  - **tkinter 강의** [**Site**](https://076923.github.io/posts/Python-tkinter-1/)

  ```python
  # 파일 선택 함수 정의
  def selectFile():
      # 파일 경로 선택 함수(filedialog.askopenfilename())
      # initaldir : 기본 경로 / filetypes : 읽어 올 파일 type 정의
      file_name =  filedialog.askopenfilename(initialdir = "./image",title = "Select file",filetypes = (("jpeg files","*.jpg"),("all files","*.*")))
      # Image read
      read_image = cv2.imread(file_name)
      (height, width) = read_image.shape[:2]
      # sizeSpin : 하단의 Spinbox 참조
      frameSize = int(sizeSpin.get())
      ratio = frameSize / width
      dimension = (frameSize, int(height * ratio))
      read_image = cv2.resize(read_image, dimension, interpolation = cv2.INTER_AREA)
      image = cv2.cvtColor(read_image, cv2.COLOR_BGR2RGB)
      image = Image.fromarray(image)
      imgtk = ImageTk.PhotoImage(image=image)
      detectAndDisplay(read_image) # 재귀함수 방식 호출
  ```
  
  - **tkinter** filedialog.askopenfilename()


  ![selectfile](https://user-images.githubusercontent.com/52812181/77340048-14be4500-6d70-11ea-9f22-eb26e23983c4.jpeg)

  

  ```python
  def detectAndDisplay(frame):
      # 그레이 스케일로 변환
      frame_gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
      frame_gray = cv2.equalizeHist(frame_gray)
      # -- Detect Faces
      faces = face_cascade.detectMultiScale(frame_gray)
      for (x, y, w, h) in faces:
          center = (x + w//2, y + h//2)
          frame = cv2.rectangle(frame, (x,y), (x+w, y+h), (0, 255,0), 4)
          faceROI = frame_gray[y:y+h, x:x+w]
          # -- In each face, detect eyes
          eyes = eyes_cascade.detectMultiScale(faceROI)
          for (x2, y2, w2, h2) in eyes:
              eye_center = (x + x2 + w2//2, y + y2 + h2//2)
              radius = int(round((w2 + h2)*0.25))
              frame = cv2.circle(frame, eye_center, radius, (255,0.0), 4)
          # cv2.imshow('capture - face detection', frame)
          # tkinter의 색상체계는 opencv와 달리 RGB 패턴(opencv는 BGR 패턴)이므로 변환
          image = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
          # numpy배열을 image 객체로 변환
          image = Image.fromarray(image)
          # Tk inter 위에 그림을 보여주기 위해 tkinter와 호환되는 객체로 변환
          imgtk = ImageTk.PhotoImage(image=image)
  		# 이미지 갱신을 위해 image 매개변수 설정
          detection.config(image=imgtk)
          # 가비지 컬렉터(garbage collector)가 이미지를 삭제하지 않도록 image 매개변수에 imgtk를 한번 더 등록
          detection.image = imgtk
  ```

  - **tkinter** opencv 적용하기 참고 [**Site**](https://076923.github.io/posts/Python-tkinter-32/)

  ```python
  # main
  main = Tk()
  main.title(title_name)
  main.geometry()
  
  
  read_image = cv2.imread('./soccer.jpg')
  (height, width) = read_image.shape[:2]
  ratio = frame_width / width
  dimension = (frame_width, int(height * ratio))
  # 이전 포스팅에서 배운 cv2.resize() 함수를 이용해 Image 크기 조절
  read_image = cv2.resize(read_image, dimension, interpolation=cv2.INTER_AREA) 
  
  
  # -- 1. Load the cascades
  if not face_cascade.load(cv2.samples.findFile(face_cascade_name)):
      print('--(!)Error loading face cascade')
      exit(0)
  if not eyes_cascade.load(cv2.samples.findFile(eyes_cascade_name)):
      print('--(!)Error loading eyes cascade')
      exit(0)
  
  
  # Label : 이미지나 도표, 그림 등에 사용되는 주석문 생성
  label=Label(main, text=title_name)
  label.config(font=("Courier", 18))
  # grid : 셀 단위 배치
  # (row/column:해당 구역으로 위젯 이동 / rowspan/columspan:현재 배치된 구역에서 위치 조정
  #  / sticky:현재 배치된 구역 안에서 특정 위치로 이동)
  label.grid(row=0,column=0,columnspan=4)
  sizeLabel=Label(main, text='Frame Width : ')                
  sizeLabel.grid(row=1,column=0)
  sizeVal  = IntVar(value=frame_width)
  # Spinbox() : 수치를 조정하고 입력받는 '수치 조정 기입창'
  # textvariable : 라벨에 표시할 문자열을 가져올 변수
  # from_ : 기입창의 최소값 / to : 기입창의 최대값 / increment : 기입창의 수치 간격
  # justify : 수치 조정 기입창의 문자열이 여러 줄 일 경우 정렬 방법
  sizeSpin = Spinbox(main, textvariable=sizeVal,from_=0, to=2000, increment=100, justify=RIGHT)
  sizeSpin.grid(row=1, column=1)
  # Button : 메소드 또는 함수 등을 실행시키기 위한 단추
  Button(main,text="File Select", height=2,command=lambda:selectFile()).grid(row=1, column=2, columnspan=2, sticky=(W, E))
  detection=Label(main, image=imgtk)
  detection.grid(row=2,column=0,columnspan=4)
  detectAndDisplay(read_image)
  
  # mainloop() : 사용자의 입력을 계속 기다리게 하는 목적으로 프로그램 흐름 마지막단에 위치
  main.mainloop()
  ```

  - **tkinter** Button 참고 [**Site**](https://076923.github.io/posts/Python-tkinter-3/)
  - **tkinter** 위젯 배치 : grid 참고 [**Site**](https://076923.github.io/posts/Python-tkinter-11/)

  - **tkinter** Spinbox 참고 [**Site**](https://076923.github.io/posts/Python-tkinter-25/)

  - Frame width가 500인 경우 : 앞서 실습한 결과와 동일하다

    ![500](https://user-images.githubusercontent.com/52812181/77344327-58b44880-6d76-11ea-998c-3b61bb3b518b.png)

  - Frame width가 1700인 경우 : width가 500일 때 잡지 못했던 눈을 잡을 수 있게 되었지만, 실제 얼굴이 아닌 위치까지 잡는 경우가 생김

  ![soccer_gui](https://user-images.githubusercontent.com/52812181/77344174-1ee34200-6d76-11ea-8d46-2114b7af51b8.png)

<br/>

- 동영상 Object detection

  - 이미지와 크게 다르진 않으나 동영상이기에 `cv2.VideoCapture()`로 비디오를 stream한다.

  ```python
  import cv2
  import numpy as np
  
  face_cascade_name = '../opencv_dnn/data/haarcascades/haarcascade_frontalface_alt.xml'
  eyes_cascade_name = '../opencv_dnn/data/haarcascades/haarcascade_eye_tree_eyeglasses.xml'
  file_name = 'video/india.mp4'
  
  def detectAndDisplay(frame):
      frame = cv2.resize(frame, dsize=(1280, 960), interpolation=cv2.INTER_AREA)
      frame_gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
      frame_gray = cv2.equalizeHist(frame_gray)
      #-- Detect faces
      faces = face_cascade.detectMultiScale(frame_gray)
      
      for (x,y,w,h) in faces:
          center = (x + w//2, y + h//2)
          frame = cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 4)
          faceROI = frame_gray[y:y+h,x:x+w]
          #-- In each face, detect eyes
          eyes = eyes_cascade.detectMultiScale(faceROI)
          for (x2,y2,w2,h2) in eyes:
              eye_center = (x + x2 + w2//2, y + y2 + h2//2)
              radius = int(round((w2 + h2)*0.25))
              frame = cv2.circle(frame, eye_center, radius, (255, 0, 0 ), 4)
          
      cv2.imshow('Capture - Face detection', frame)
  
  face_cascade = cv2.CascadeClassifier()
  eyes_cascade = cv2.CascadeClassifier()
  
  #-- 1. Load the cascades
  if not face_cascade.load(cv2.samples.findFile(face_cascade_name)):
      print('--(!)Error loading face cascade')
      exit(0)
  if not eyes_cascade.load(cv2.samples.findFile(eyes_cascade_name)):
           
      print('--(!)Error loading eyes cascade')
      exit(0)
  
  #-- 2. Read the video stream
  cap = cv2.VideoCapture(file_name)
  if not cap.isOpened:
      print('--(!)Error opening video capture')
      exit(0)
  while True:
      ret, frame = cap.read()
      if frame is None:
          print('--(!) No captured frame -- Break!')
          break
      detectAndDisplay(frame)
      # Hit 'q' on the keyboard to quit!
      if cv2.waitKey(1) & 0xFF == ord('q'):
          break
  
  cv2.destroyAllWindows()
  ```

  ![india_vid_haar](https://user-images.githubusercontent.com/52812181/77937827-d684d080-72ef-11ea-9899-34fb9097eca2.gif)

  - 얼굴을 잘 못잡는 경우가 많다...

### 결론 : Haar 방식은 이미지의 해상도와 크기가 detection에 영향을 줌
