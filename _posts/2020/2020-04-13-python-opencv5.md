---
layout: post
title: OpenCV - Face recognition Module을 이용한 얼굴 인식
mathjax: true
tags:
  - python
---

<br/>

## face_recognition 설치

- face_recognition 설치에 앞서 먼저 설치가 필요한 Module들이 있다.
  - cmake
    - pip : `pip install cmake`
    - conda : `conda install -c conda-forge cmake`
  - dlib
    - pip : `pip install dlib`
    - conda : `conda install -c conda-forge dlib`
- face_recognition 설치(pip만 존재하므로 conda 환경에서도 pip 사용)
  - `pip install face_recognition`



## face_recognition 모듈을 사용해 얼굴 인식 / 학습 시키기

```python
import cv2
import face_recognition
import pickle # 학습시킨 데이터를 pickle파일 형태로 저장

# 손흥민과 테디 브루시의 얼굴 사진 각 10장씩
dataset_paths = ['dataset/son/', 'dataset/tedy/']
names = ['Son', 'Tedy']
number_images = 10
image_type = '.jpg'
encoding_file = 'encodings.pickle'

# model의 두 가지가 있다. hog와 cnn
# cnn은 높은 얼굴 인식 정확도를 보이지만 속도가 느리다는 단점(단, GPU환경은 빠르다)
# hog는 낮은 얼굴 인식 정확도를 보이지만 속도가 빠르다는 장점(cnn-gpu와 속도 비슷)
model_method = 'cnn-gpu' # GPU환경 사용, 일반 CPU환경은 cnn


# feature dataset
knownEncodings = []
# label dataset
knownNames = []

# 이미지 경로 반복
for (i, dataset_path) in enumerate(dataset_paths):
    # 사람 이름 추출
    name = names[i]

    for idx in range(number_images):
        file_name = dataset_path + str(idx+1) + image_type

        # 입력 이미지를 load하고 BGR에서 RGB로 변환
        image = cv2.imread(file_name)
        rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

        # 입력 이미지에서 얼굴에 해당하는 box의 (x, y) 좌표 감지
        boxes = face_recognition.face_locations(rgb,
            model=model_method)

        # 얼굴 임베딩 계산
        encodings = face_recognition.face_encodings(rgb, boxes)

        # 인코딩 반복
        for encoding in encodings:
            print(file_name, name, encoding)
            knownEncodings.append(encoding)
            knownNames.append(name)
        
# pickle파일 형태로 데이터 저장
data = {"encodings": knownEncodings, "names": knownNames}
f = open(encoding_file, "wb")
f.write(pickle.dumps(data))
f.close()
```

```python
# 실행 결과
dataset/tedy/1.jpg Tedy [-2.06955492e-01  8.75681862e-02  2.80791949e-02 -1.83639824e-02
 -7.95349106e-02 -1.07647538e-01 -2.05471665e-02 -1.53820992e-01
  1.28729105e-01 -1.90565325e-02  2.01887801e-01 -6.65292367e-02
 -2.32772946e-01 -7.33143538e-02 -5.96043207e-02  1.57312766e-01
 -1.62923768e-01 -7.90786520e-02 -1.34912267e-01 -5.27240485e-02
  7.17194229e-02  6.50360063e-02  5.70355207e-02  7.42899925e-02
 -5.37627190e-02 -3.38073701e-01 -1.15430258e-01 -3.09627149e-02
  6.32061362e-02 -1.04444697e-01 -9.28215962e-03  3.21778394e-02
 -1.94322318e-01 -1.71126440e-01  6.56492785e-02  5.44311553e-02
 ...]
...
```

<br/>



## 학습시킨 데이터로 사진 얼굴 인식

```python
import cv2
import face_recognition
import pickle
import time

image_file = 'image/marathon_01.jpg'
encoding_file = 'encodings.pickle'
unknown_name = 'Unknown'
model_method = 'cnn-gpu'

def detectAndDisplay(image):
    start_time = time.time()
    rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

    # 입력 이미지에서 각 얼굴에 해당하는 box 좌표를 감지하고 얼굴 임베딩 계산
    boxes = face_recognition.face_locations(rgb,
        model=model_method)
    encodings = face_recognition.face_encodings(rgb, boxes)

    # 감지된 각 얼굴의 이름 목록 초기화
    names = []

    # 얼굴 임베딩 반복
    for encoding in encodings:
        # 입력 이미지의 각 얼굴과 학습된 데이터 매치
        matches = face_recognition.compare_faces(data["encodings"],
            encoding)
        name = unknown_name

        # 데이터가 매치된 경우
        if True in matches:
            # 일치하는 모든 얼굴의 인덱스를 찾고, 얼굴 일치 횟수 계산을 위한 초기화
            matchedIdxs = [i for (i, b) in enumerate(matches) if b]
            counts = {}

            # 일치하는 인덱스를 반복하고, 인식된 각 얼굴의 수 유지
            for i in matchedIdxs:
                name = data["names"][i]
                counts[name] = counts.get(name, 0) + 1

            # 가장 많은 표를 얻은 label 선택
            name = max(counts, key=counts.get)
        
        # 이름 목록 업데이트
        names.append(name)

    # 인식된 얼굴 반복
    for ((top, right, bottom, left), name) in zip(boxes, names):
        # 이미지에 인식된 얼굴의 box를 그림
        y = top - 15 if top - 15 > 15 else top + 15
        # 학습된 얼굴(인식한 얼굴)의 경우 녹색 선
        color = (0, 255, 0)
        line = 2
        # 학습되지 않은 얼굴(인식하지 못한 얼굴)의 경우 빨간색 선
        if(name == unknown_name):
            color = (0, 0, 255)
            line = 1
            name = ''
        # 사각형 그리기
        cv2.rectangle(image, (left, top), (right, bottom), color, line)
        y = top - 15 if top - 15 > 15 else top + 15
        # 텍스트 추가
        cv2.putText(image, name, (left, y), cv2.FONT_HERSHEY_SIMPLEX,
            0.75, color, line)
	
    end_time = time.time()
    # 소요시간 체크
    process_time = end_time - start_time
    print("=== A frame took {:.3f} seconds".format(process_time))
    cv2.imshow("Recognition", image)
    
# 학습된 데이터 load
data = pickle.loads(open(encoding_file, "rb").read())

# 테스트할 이미지 load
image = cv2.imread(image_file)
detectAndDisplay(image)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

