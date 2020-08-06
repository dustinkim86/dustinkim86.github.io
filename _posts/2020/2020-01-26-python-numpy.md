---
layout: post
title: Numpy
mathjax: true
tags:
  - python
---

<br/>


## Numpy

- 성능 : 파이썬 리스트보다 빠름

- 메모리 사이즈 : 파이썬 리스트보다 적은 메모리 사용

- 빌트인 함수 : 선형대수, 통계관련 여러 함수 내장



#### ndarray

- numpy에서 사용되는 다차원 리스트를 표현할 때 사용되는 데이터 타입

- list의 경우 연속되지 않은 메모리를 사용하나 ndarray는 연속된 메모리의 vectorization 사용

	- vectorization : 여러 데이터를 한 덩어리로 인식하여 한 번에 연산할 수 있게 하는 것

- array

	- numpy.array([1,2,3,4])

	- numpy.array([2,3,4],[1,2,5])

- arange 함수(range 함수와 동일)

	- 예) numpy.arange(1,10,2)

- ones, zeros

	- 모든 원소를 1로 생성 / 0으로 생성

	- type을 튜플로 생성

	- 예) numpy.ones((4,5))

	- 예) numpy.zeros((2,3,4)) - 면,행,열

- empty, full

	- empty : 초기화가 되지 않음(비어있는 공간의 배열 생성)

	- 예) numpy.empty((2,3))

	- full : 원하는 값으로 배열 생성

	- 예) numpy.full((2,3), 8) - 모든 원소를 8로 생성

- eye

	- 단위 행렬 생성

	- 대각선의 모든 원소가 1이고, 나머지는 0

	- numpy.eye(5) - 5행5열의 단위행렬

- linspace

	- 원하는 수로 등분한 원소 생성

	- numpy.linspace(1,10,3) - 시작값1, 끝값10, 등분학고자 하는 수3

- reshape

	- 기존의 차원을 원하는 차원으로 바꾸기 위해 사용
- 바꾸고자 하는 차원 수의 곱이 원래 차원 수의 곱과 같아야 함
	- 사용 예) 이미지 데이터 벡터화 - 이미지는 기본적으로 2차원 혹은 3차원이나 트레이닝을 위해 1차원으로 변경하여 사용 됨
	
	```python
	array.reshape(-1, 6) : 열이 6개인 shape으로 변환
	array.reshape(6, -1) : 행이 6개인 shape으로 변환
	```



- random 서브모듈

  - rand  : 0,1 사이의 분포로 랜덤한 ndarray 생성

    - numpy.random.rand(2, 3) - 2행3열 0~1사이의 random 값

  - randn  : n(normal distribution, 정규분포)

    - numpy.random.randn(3, 4, 2) - 3면 4행 2열 정규분포를 따르는 random 값

  - randint  : 특정 정수 사이에서 랜덤하게 샘플링

    - numpy.random.randint(1, 100, size=(3,5)) - 3행5열 1~99사이의 random값

  - seed  : 랜덤한 값을 동일하게 다시 생성하고자 할 때 사용

    - numpy.random.seed(42) : 42와 연관된 값으로만 설정(고정된 값)

  - choice 

    - 주어진 1차원 ndarray로부터 랜덤으로 샘플링

    - 정수가 주어진 경우, numpy.arange(해당숫자)로 간주

    - numpy.random.choice(100, size=(3,4)) - 0~99까지의 정수 를 3행4열로 생성

      ```python
      x = numpy.array([1,2,3,1.5,2.6,4.9])
      numpy.random.choice(x, size=(2,2), replace=False)
      ```

  - 확률분포에 따른 ndarray 생성
    - uniform
      - numpy.random.uniform(low=1.0, high=3.0, size=(4, 5))
    - normal
      - numpy.random.normal(size=(3,4))

- indexing

  - 1차원 벡터 인덱싱

    ```python
    x = numpy.arange(1, 10)
    print(x[-1])
    # 9
    print(x[4:])
    # 4,5,6,7,8,9
    ```

  - 2차원 행렬 인덱싱

    ```python
    x = x.reshape(3,3)
    print(x[2,1])
    # 8
    print(x[[0,1], :2])
    # [[1,2],
    #	[4,5]]
    print(x[x < 6])
    # [1,2,3,4,5]
    ```

  - 3차원 텐서 인덱싱

    ```python
    x = numpy.arange(36).reshape(3,4,3)
    print(x[0,2,1])
    # 7
    ```

- slicing

  - 1차원 벡터 슬라이싱

    ```python
    x = numpy.arange(10)
    print(x[7:])
    # [6,7,8,9]
    ```

  - 2차원 행렬 슬라이싱

    ```python
    x = numpy.arange(10).reshape(2, 5)
    print(x[1,1:4])
    # [6,7,8] - 인덱싱
    print(x[:, :2])
    # [[0,1], 
    #	[5,6]] - 슬라이싱
    ```

    - 인덱싱을 하면 차원이 축소되지만, 슬라이싱을 하면 차원이 축소되지 않음
    - 곧, 인덱스 번호를 직접 지명하면 차원 축소, 콜론(:)을 이용하여 슬라이싱 하면 차원 축소 없음

  - 3차원 텐서 슬라이싱

    ```python
    x = numpy.arange(12).reshape(2, 3, 2)
    print(x[:1, :2, :])
    # [[[0, 1],
    #	[2, 3]]]
    ```

- ravel

  - 다차원 배열을 1차원으로 변경

  - 내부데이터를 유지하기 때문에 

  - 'order' 파라미터

    - 'C' - row 우선 변경
    - 'F' - column 우선 변경

    ```python
    x = numpy.arange(10).reshape(2, 5)
    print(x.ravel())
    # [0,1,2,3,4,5,6,7,8,9]
    temp = x.ravel()
    temp[0] = 100
    print(temp)
    # [100,1,2,3,4,5,6,7,8,9]
    print(x)
    # [100,1,2,3,4,5,6,7,8,9]
    print(x.ravel(order='C')) - C style
    # [[0,1,2,3,4],
    #   [5,6,7,8,9]]
    print(x.ravel(order='F')) - Fotran style
    # [[0,2,4,6,8],
    #   [1,3,5,7,9]]
    ```

    - ravel 함수는 내부데이터를 유지하기 때문에 temp 변수로 데이터를 받더라도 주소값은 x와 같음
    - 곧, temp 변수의 일부 데이터를 변경하면 x 변수의 데이터도 같이 변경 됨

- flatten

  - 다차원 배열을 1차원으로 변경
  - ravel과의 차이점 : copy를 생성하여 변경함(즉 원본 데이터가 아닌 복사본을 반환)
  - ravel 함수와 같이 order 파라미터를 지님

  ```python
  x = numpy.arange(10).reshape(2, 5)
  print(x.flatten())
  # [0,1,2,3,4,5,6,7,8,9]
  temp = x.flatten()
  temp[0] = 100
  print(temp)
  # [100, 1,2,3,4,5,6,7,8,9]
  print(x)
  # [0,1,2,3,4,5,6,7,8,9]
  ```

  - ravel과는 달리 flatten은 주소값이 다른 전혀 다른 데이터를 생성하기 때문에 temp 변수 데이터를 변경해도 x 변수에 영향을 미치지 않음

#### Numpy 공식 문서 링크 : [Numpy Documentation](https://numpy.org/doc/)

- 연산 함수
  - add : `numpy.add(x, y)` - 같은 shape의 x 행렬과 y 행렬의 각자의 같은 위치의 값을 더함
  - substract : `numpy.substract(x, y)` - 같은 shape의 x 행렬과 y 행렬의 각자의 같은 위치의 값을 뺌
  - multiply : `numpy.multiply(x, y)` - 같은 shape의 x 행렬과 y 행렬의 각자의 같은 위치의 값을 곱함
  - divide : `numpy.divide(x, y)` - 같은 shape의 x 행렬과 y 행렬의 각자의 같은 위치의 값은 나눔

- 통계 함수 : 평균, 분산, 중앙, 최대, 최소값 등 통계 관련된 함수가 내장
  - 평균 : `numpy.mean(x) or x.mean()`
  - 최대값 : `numpy.max(x) ` : 값을 반환 / `x.argmax()` : 인덱스를 반환(flatten한 형태로 탐색 후 반환함)
  - 분산 : `numpy.var(x)`
  - 중앙값 : `numpy.median(x)`
  - 표준편차 : `numpy.std(x)`
- 집계 함수 : 합계(sum), 누적합계(cumsum) 등 계산 가능
  - `numpy.sum(x)` : scala값 반환
  - `numpy.cumsum(x)` : array값 반환

- any : 특정 조건을 만족하는 것이 하나라도 있으면 True, 아니면 False

- all : 모든 원소가 특정 조건을 만족한다면 True, 아니면 False

  ```python
  x = numpy.random.randn(3)
  # [-0.99252354, 1.17432562, -0.02342682]
  numpy.any(x > 0)
  # True
  numpy.all(x > 0)
  # False
  ```

- where : 조건에 따라 선별적으로 값을 선택 가능
	- 예) 음수인 경우는 0, 나머지는 그대로 값을 쓰는 경우
	- numpy.where(x > 0, x, 0)


- axis 이해하기
	- 몇몇 함수에는 axis keyword 파라미터가 존재
	- axis값이 없는 경우에는 전체 데이터에 대해 적용
	- axis값이 있는 경우에는 해당 axis를 따라서 연산 적용
	- axis를 파라미터로 갖는 함수를 이용하기
		- 거의 대부분의 연산 함수들이 axis 파라미터를 사용
		- 이 경우, 해당 값이 주어졌을 때 해당 axis를 따라서 연산이 적용(따라서 결과는 axis가 제외된 나머지 차원의 데이터만 남게 됨)

	- axis의 값이 튜플일 경우 해당 튜플에 명시된 모든 axis에 대해서 연산

	```python
	np.sum(a, axis=(0,1))
	# [198 210 222]
	```
- boolean indexing
	- ndarray 인덱싱 시, bool 리스트를 전달하여 True인 경우만 필터링
	- 브로드캐스팅을 활용하여 ndarray로부터 bool list 얻기(예. 짝수인 경우만 찾아보기)
	- 다중조건 사용하기
		- 파이썬 논리 연산자인 and, or, not 키워드 사용 불가
		- `&` -> and / `|` -> or

- 브로드캐스팅
	- shape이 같은 두 ndarray에 대한 연산은 각 원소별로 진행
	- 연산되는 두 ndarray가 다른 shape을 갖는 경우 브로드캐스팅(shape을 맞춤) 후 진행
	- rule : 뒷 차원에서부터 비교하여 shape이 같거나, 차원 중 값이 1인 것이 존재하면 가능





