---
layout: post
title: Scikiy-learn
mathjax: true
tags:
  - python
---

<br/>


## 붓꽃 품종 예측하기

### scikit-learn library를 이용해 간단하게 해 보자

- 우선 모듈의 설치가 필요하다 :  `pip install scikit-learn`

```python
# iris dataset load
from sklearn.datasets import load_iris
# 분류 의사결정 나무 모델 load
from sklearn.tree import DecisionTreeClassifier
# dataset의 train set과 test set으로 분리하기 위한 함수 load
from sklearn.model_selection import train_test_split

# dataset load
iris = load_iris() # <class 'sklearn.utils.Bunch'>

type(iris)

# data와 label 분리
iris_data = iris.data
iris_label = iris.target
type(iris_data) # <class 'numpy.ndarray'>
type(iris_label) # <class 'numpy.ndarray'>

# train, test set 분리
X_train, X_test, y_train, y_test = train_test_split(iris_data, iris_label, test_size=.2, random_state=11)

# decisiontreeclassifier 생성
dt_clf = DecisionTreeClassifier(random_state=11)

# 학습 수행
dt_clf.fit(X_train, y_train)
# DecisionTreeClassifier(ccp_alpha=0.0, class_weight=None, criterion='gini',
#                       max_depth=None, max_features=None, max_leaf_nodes=None,
#                       min_impurity_decrease=0.0, min_impurity_split=None,
#                       min_samples_leaf=1, min_samples_split=2,
#                       min_weight_fraction_leaf=0.0, presort='deprecated',
#                       random_state=11, splitter='best')

# 학습이 완료된 decisiontreeclassifier 객체에서 테스트 데이터 세트로 예측 수행
prod = dt_clf.predict(X_test)
prod
# array([2, 2, 1, 1, 2, 0, 1, 0, 0, 1, 1, 1, 1, 2, 2, 0, 2, 1, 2, 2, 1, 0,
#       0, 1, 0, 0, 2, 1, 0, 1])

# 정확도 산출 함수 load
from sklearn.metrics import accuracy_score
print(f'예측 정확도 : {accuracy_score(y_test, prod)}') # 예측 정확도 : 0.9333333333333333
```

--------

### 사이킷런의 주요 모듈

- sklearn.datasets : 사이킷런에 내장되어 예제로 제공하는 데이터 세트
- sklearn.preprocessing : 데이터 전처리에 필요한 다양한 가공 기능 제공(문자열을 숫자형 코드 값으로 인코딩, 정규화, 스케일링 등)
- sklearn.feature_selection : 알고리즘에 큰 영향을 미치는 피처를 우선순위대로 셀렉션 작업을 수행하는 다양한 기능 제공
- sklearn.feature_extraction : 텍스트 데이터나 이미지 데이터의 벡터화된 피처를 추출하는데 사용(count 벡터라이저나 Tf-idf 벡터라이저 생성 기능)
- sklearn.decomposition : 차원 축소와 관련한 알고리즘을 지원, PCA, NMF, Trucated SVD 등
- sklearn.model_selection : 교차 검증을 위한 학습/테스트용 데이터 분리, 그리드 서치로 최적 파라미터 추출 등
- sklearn.metrics : 분류, 회귀, 클러스터링, 페어와이즈에 대한 다양한 성능 측정방법 제공
- sklearn.ensemble : 앙상블 알고리즘. 랜덤 포레스트, 에이다 부스트, 그래디언트 부스팅 등
- sklearn.linear_model : 선형회귀, 릿지/라쏘 및 로지스틱 회귀, SGD 알고리즘 제공
- sklearn.naive_bayes : 나이브 베이즈 알고리즘 제공
- sklearn.neighbors : 최근접 이웃 알고리즘 제공. K-NN 등
- sklearn.svm : 서포트 벡터 머신 알고리즘 제공
- sklearn.tree : 의사결정 나무 알고리즘 제공
- sklearn.cluster : 비지도 클러스터링 알고리즘 제공(k-means, 계층형,  DBSCAN 등)
- sklearn.pipeline : 피처 처리 등의 변환과 머신러닝 알고리즘 학습, 예측 등을 함께 묶어 실행할 수 있는 유틸 제공

-------

### 교차검증(cross-validation)  

- 성능 평가 시 train/test data로 한 번 나누는 것보다 안정적이고 뛰어난 통계적 평가 방법

- 데이터를 여러 번 반복해서 나누고 모델을 학습  

- **K-fold 교차검증**  

  - 가장 보편적인 교차 검증 기법
  - import : `from sklearn.model_selection import KFold`
  - k개의 데이터 폴드 세트를 만들어 k번만큼 각 폴드 세트에 학습과 검증 평가를 반복적으로 수행

  ![스크린샷, 2020-01-30 11-27-38](https://user-images.githubusercontent.com/52812181/73415003-ec0f6580-4353-11ea-8592-4210f10410e1.png)

  - 학습/예측 순서 : 폴드세트 설정 --> for loop반복으로 학습 및 테스트 데이터 인덱스 추출 --> 반복적으로 학습과 예측을 수행하고 예측 성능을 반환

 - **계층별 K-fold 교차검증(Stratified k-fold)**  
    - 불균형한 분포도(특정 레이블 값이 특이하게 많거나 매우 적어서 값의 분포가 한쪽으로 치우치는 것)를 가진 레이블 데이터 집합을 위한 k-fold 방식
    - import : `from sklearn.model_selection import StratifiedKFold`

 - **단순 교차검증(cross_val_score)**  
   	- 위의 K-fold의 학습/예측 순서를 한 번에 수행해주는 모듈
      	- import : `from sklearn.model_selection import cross_val_score, cross_validate` 
   	- scoring= : 예측 성능 평가 지표를 기술 / cv= : 교차 검증 폴드 수
   	- cross_val_score는 내부적으로 StratifiedKFold 를 이용

 - **다중 교차검증(cross_validate)**  
   	- cross_val_score는 단 하나의 평가 지표만 가능하지만, cross_validate는 여러 개의 평가지표를 반환

 - **GridSearchCV**  
    - **최적의 하이퍼 파라미터 튜닝을 수행한 뒤, 별도의 테스트 세트에서 이를 평가하는 것이 일반적인 머신러닝 모델 적용 방법**
    - 하이퍼 파라미터를 순차적으로 입력하면서 편리하게 최적의 파라미터를 도출할 수 있는 방안을 제공
    - import : `from sklearn.model_selection import GridSearchCV`
    - 주요 파라미터
      	- estimator : classifier, regressor, pipeline
      	- param_grid : key + 리스트 값을 가지는 딕셔너리가 주어짐. estimator의 튜닝을 위해 파라미터명과 사용될 여러 파라미터 값을 지정
      	- scoring : 예측 성능을 측정할 평가 방법을 지정. 보통 성능 평가 지표를 지정하는 문자열(ex. 'accuracy')
      	- cv : 교차 검증을 위해 분할되는 학습/테스트 세트의 개수를 지정
      	- refit : default=True, True이면 가장 최적의 하이퍼 파라미터를 찾은 뒤 입력된 estimator 객체를 해당 하이퍼 파라미터로 재학습

   ```python
   from sklearn.tree import DecisionTreeClassifier
   dtree = DecisionTreeClassifier()
   # DecisionTreeClassifier의 중요한 하이퍼 파라미터인 max_depth와 min_samples_split를 key값으로, 하이퍼 파라미터의 값은 리스트 형으로 설정
   parameters = {'max_depth':[1,2,3], 'min_samples_split':[2,3]}
   # param_grid의 하이퍼 파라미터들을 3개의 train, test set fold 로 나누어서 테스트 수행 설정
   grid_dtree = GridSearchCV(dtree, param_grid=parameters, cv=3, refit=True)
   ```

   - `grid_dtree.fit(X_train, y_train)` : cv로 지정한 폴딩 세트로 분할해 param_grid에 기술된 하이퍼 파라미터를 순차적으로 변경하면서 학습/평가를 수행하고 .cv\_result\_는 속성에 기록(.cv\_result\_는 딕셔너리 형태의 결과 세트)
   - `grid_dtree.best_params_` : GridSeachCV를 이용해 나온 최적의 파라미터 추출
   - `grid_dtree.best_score_` : GridSeachCV를 이용해 나온 최고의 정확도 추출
   - `grid_dtree.best_estimator_` : GridSearchCV의 refit으로 이미 학습된  estimator 반환
   - 기 학습된  estimator를 이용해 예측값 반환 : `.predict(X_test)`
   - 반환된 예측값과 실제 test label데이터와 비교 : `accuracy_score(y_test, pred)` 


-----------

### 데이터 전처리  
- 머신러닝 알고리즘은 데이터에 기반하기 때문에 데이터의 상태에 따라 결과가 크게 달라지므로, 결측값 등을 전처리 해야 함
- 사이킷런 머신러닝 알고리즘은 문자열 값을 입력 값으로 허용하지 않음(숫자형으로 반환)

#### 데이터 인코딩  
- 레이블 인코딩 : 문자열 값을 숫자형 카테고리 값으로 변환
  - import : `from sklearn.preprocessing import LabelEncoder`
  - 순서 : LabelEncoder() class호출 --> .fit() --> .transform()

  ```python
  items = ['TV', '냉장고', '전자레인지', '컴퓨터', '선풍기', '선풍기', '믹서', '믹서']
  
  # LabelEncoder를 객체로 생성한 후 , fit() 과 transform() 으로 label 인코딩 수행.
  encoder = LabelEncoder()
  encoder.fit(items)
  labels = encoder.transform(items)
  print('인코딩 변환값:',labels)
  # [0 1 4 5 3 3 2 2]
  print('인코딩 클래스:',encoder.classes_)
  # ['TV' '냉장고' '믹서' '선풍기' '전자레인지' '컴퓨터']
  
  # 디코딩
  print('디코딩 원본 값:',encoder.inverse_transform([4, 5, 2, 0, 1, 1, 3, 3]))
  # ['전자레인지' '컴퓨터' '믹서' 'TV' '냉장고' '냉장고' '선풍기' '선풍기']
  ```  
- 레이블 인코딩의 경우 임의의 숫자값으로 변환이 되는데 부여된 숫자값이 더 큰 데이터가 알고리즘에서 가중치가 더 부여되거나 중요하게 인식할 가능성이 발생할 수 있으므로, 선형회귀와 같은 알고리즘에서는 적용하지 않아야 함
- 트리 계열의 알고리즘은 숫자의 이러한 특성을 반영하지 않으므로 문제 없음

#### 원-핫 인코딩  
- 레이블 인코딩의 문제점을 보완하기 위한 방법
- 피처 값의 유형에 따라 새로운 피처를 추가해 고유 값에 해당하는 칼럼에만 1을 표시하고 나머지 칼럼에는 0을 표시하는 방법
- 원-핫 인코딩을 하기 전 모든 문자열 값이 숫자형 값으로 변환되어야 함
- 입력 값으로 2차원 데이터가 필요함
- import : `from sklearn.preprocessing import OneHotEncoder`

  ```python
  items=['TV','냉장고','전자렌지','컴퓨터','선풍기','선풍기','믹서','믹서']
  
  # 먼저 숫자값으로 변환을 위해 LabelEncoder로 변환합니다.
  encoder = LabelEncoder()
  encoder.fit(items)
  labels = encoder.transform(items)
  # 2차원 데이터로 변환합니다.
  labels = labels.reshape(-1,1)
  
  # 원-핫 인코딩을 적용합니다.
  oh_encoder = OneHotEncoder()
  oh_encoder.fit(labels)
  oh_labels = oh_encoder.transform(labels)
  print('원-핫 인코딩 데이터')
  print(oh_labels.toarray())
  print('원-핫 인코딩 데이터 차원')
  print(oh_labels.shape)
  ```
- 원-핫 인코딩 순서 : 문자열 값을 숫자형 값으로 변환(Label Encoder -> .fit -> .transform -> .reshape(-1,1) --> 원-핫 인코딩(OneHotEncoder -> .fit -> .transform(label)) --> 데이터 확인(.toarray())
- 위와 같이 원-핫 인코딩을 위해 번거로운 작업을 거치지 않고 **pandas library**의 **get_dummies()** 를 이용해 한 번에 가능(pd.get_dummies(df))

------

### 피처 스케일링  
- 서로 다른 변수의 값 범위를 일정한 수준으로 맞추는 작업
- 표준화 : 피처 각각 평균이 0이고 분산이 1인 가우시안 정규 분포를 가진 값으로 변환
- 정규화 : 서로 다른 피처의 크기를 통일하기 위해 크기를 변환   

#### StandardScaler  
- SVM(서포트 벡터 머신), 선형회귀(linear regressor), 로지스틱 회귀(logistic regressor)는 데이터가 가우시간 분포를 가지고 있다고 가정하고 구현됐기 때문에 표준화를 하는 것은 예측 성능 향상에 중요한 요소이다.
- import : `from sklearn.preprocessing import StandardScaler`
- 순서 : StandardScaler() --> .fit(df) --> .transform(df)
- 모든 칼럼 값의 평균이 0에 가까운 값으로, 분산은 1에 가까운 값으로 변환됨

#### MinMaxScaler  
- 데이터 값을 0과 1사이의 범위 값으로 변환(음수 -1은 1로 변환)
- 데이터의 분포가 가우시안 분포가 아닐 경우 사용
- import : `from sklearn.preprocessing import MinMaxScaler`
- standardscaler와 동일한 순서로 진행
