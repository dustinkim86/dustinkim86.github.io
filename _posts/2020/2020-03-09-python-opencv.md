---
layout: post
title: OpenCV - 모듈 설치와 이미지 조작하기
mathjax: true
tags:
  - python
---

<br/>

### OpenCV(Open Source Computer Vision)

- 오픈 소스 컴퓨터 비전 라이브러리
- 객체, 얼굴,  행동 인식 등에서 사용

<br>

#### OpenCV 설치

- pip : `pip install opencv-python`
- conda : `conda install -c conda-forge opencv`

- 설치 후 확인

  ```python
  import cv2
  print(cv2.__version__)
  ```

<br>

#### MNIST 이미지 출력 해보기

- 사전 준비 : Keras module 설치
  - Anaconda : `conda install -c conda-forge keras` 
  - Anaconda gpu : `conda install -c anaconda keras-gpu`
  - Pip : `pip install keras`

```python
# n번째 이미지 출력
selected_image = 1

# Keras 내장 데이터셋의 MNIST load
from keras.datasets import mnist
(x_train, y_train), (x_test, y_test) = mnist.load_data()

# Draw Digit Image
import numpy as np
import matplotlib.pyplot as plt

fig = plt.figure(figsize=(10,10)) # 출력 이미지 사이즈
ax = fig.add_subplot(1, 1, 1)

# 눈금선
# 주 눈금선은 5, 보조 눈금선은 1 간격 설정
major_ticks = np.arange(0, 29, 5)
minor_ticks = np.arange(0, 29, 1)

ax.set_xticks(major_ticks)
ax.set_xticks(minor_ticks, minor=True)
ax.set_yticks(major_ticks)
ax.set_yticks(minor_ticks, minor=True)

# And a corresponding grid
ax.grid(which='both')

# 격자선
# Or if you want different settings for the grids:
ax.grid(which='minor', alpha=0.2)
ax.grid(which='major', alpha=0.5)

ax.imshow(x_test[selected_image], cmap=plt.cm.binary)
plt.show()

print(y_test[selected_image])
print(x_test[selected_image])
```

![2](https://user-images.githubusercontent.com/52812181/76229477-9b99fa80-6265-11ea-874a-0540b71b9ad3.png)



<br><br>



#### Image 그레이스케일로 변경 해보기

- 사용 Image

  ![pengsu](https://user-images.githubusercontent.com/52812181/76230140-9b4e2f00-6266-11ea-8a92-739434b45a67.jpg)

```python
import cv2

# Image load
img= cv2.imread('pengsu.jpg')
# 그레이스케일로 변환
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

# 결과 확인
cv2.imshow('photo', img)
cv2.imshow('photo - gray', gray)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![image](https://user-images.githubusercontent.com/52812181/76230297-d9e3e980-6266-11ea-8be4-d376c48106fc.png)

<br>

<br>



#### Image 저장하기

```python
# 이미지 불러오기
img = cv2.imread('load_image.png')
# (일련의 수정 과정을 거친 후) 이미지 저장하기
cv2.write('saving.jpg', img)
```

<br>

#### Image Pixel 하나의 RGB 값 가져오기

```python
img = cv2.imread('load_image.png')
# img 0행 0열의 rgb값
(b, g, r) = img[0,0] # 괄호 없어도 무방 / RGB 순서가 아닌 BGR 순서
# 결과 확인
print("Pixel at (0, 0) - Red: {}, Green: {}, Blue: {}".format(r,g, b))
# Pixel at (0, 0) - Red: 239, Green: 240, Blue: 244
```

- Slicing으로 특정 구간 pixel 이미지 가져오기

  ```python
  img[50:100, 50:100]
  ```

<br>

#### 사각형 / 원 / 선 / 글씨를 그려주는 함수

- 사각형(cv2.rectangle())

  ```python
  # cv2.rectangle(적용할 이미지, 좌상단 위치, 우하단 위치, RGB값, 선 두께)
  cv2.rectangle(img, (150,50), (200,100), (0, 255, 0), 5)
  ```

- 원(cv2.circle())

  ```python
  # cv2.circle(적용할 이미지, 원의 중심 위치, 반지름 크기, RGB값, 선 두께)
  cv2.circle(img, (275,75), 25, (0, 255, 255), -1) # 선 두께 -1은 전체를 의미
  ```

- 선(cv2.line())

  ```python
  # cv2.line(적용할 이미지, 시작점 위치, 끝점 위치, RGB값, 선 두께)
  cv2.line(img, (350,100), (400,100), (0,0,255), 5)
  ```

- 글씨(cv2.putText())

  ```python
  # cv2.putText(적용할 이미지, 삽입할 글자, 위치, 글꼴, 글자 크기, RGB값, 글자 두께)
  cv2.putText(img, 'OpenCV', (200, 250), cv2.FONT_HERSHEY_SIMPLEX, 1, (0,0,0), 4)
  ```

![image](https://user-images.githubusercontent.com/52812181/76232615-490f0d00-626a-11ea-88b9-2df30d01a626.png)

<br>

<br>

#### Image 이동 / 회전 / 크기 조정 / 대칭

```python
import cv2
import numpy as np

img = cv2.imread('load_image.jpg')

# Image 높이, 너비 추출
(height, width) = img.shape[:2]
# 중앙 위치 추출
center = (width // 2, height // 2)
```

- Image 이동

  ```python
  # x 방향으로 100, y 방향으로 100 이동하는 matrix 생성
  move = np.float32([[1, 0, 100], [0, 1, 100]])
  # cv2.warpAffine(이동 대상 이미지, 좌표, (출력될 이미지의 너비, 높이))
  moved = cv2.warpAffine(img, move, (width, height))
  ```

  ![image](https://user-images.githubusercontent.com/52812181/76232836-a4d99600-626a-11ea-822e-cec434ae0bb4.png)

  <br>

- Image 회전

  ```python
  # 우측으로 90도 회전(위에 정의한 center(이미지 중앙위치)를 중심으로)
  rotate = cv2.getRotationMatrix2D(center, -90, 1.0)
  rotated = cv2.warpAffine(img, rotate, (width, height))
  ```

  ![image](https://user-images.githubusercontent.com/52812181/76233831-241b9980-626c-11ea-859a-0b782ab6c783.png)

<br>

- Image 크기 조절

  ```python
  # 너비 비율 설정
  ratio = 200.0 / width
  # 너비, 너비 비율에 맞는 높이 설정
  dimension = (200, int(height * ration))
  # cv2.resize(적용할 이미지, 너비, 높이, 보간법)
  # 보간법
  	# - 이미지 축소 : 보통 cv2.INTER_AREA 사용
      # - 이미지 확대 : 보통 cv2.INTER_CUBIC + cv2.INTER_LINEAR 사용
      # INTER_AREA : 픽셀 영역 관계를 이용한 resampling 방법
  resized = cv2.resize(img, dimension, interpolation = cv2.INTER_AREA)
  
  ```

  ![image](https://user-images.githubusercontent.com/52812181/76235178-17984080-626e-11ea-8bcc-34a266dc98a9.png)

<br>

- Image 대칭

  ```python
  # cv2.flip(적용할 이미지, 대칭방법)
  # 1 : 좌우대칭, 0 : 상하대칭, -1 : 상하좌우대칭
  flipped = cv2.flip(img, -1)
  ```

  ![image](https://user-images.githubusercontent.com/52812181/76235428-6b0a8e80-626e-11ea-8596-e9650b4f1437.png)

