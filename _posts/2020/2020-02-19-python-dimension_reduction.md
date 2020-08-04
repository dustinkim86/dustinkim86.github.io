---
layout: post
title: Dimension reduction(차원축소)
date: 2020-02-19
summary: 차원축소의 특징 / PCA
categories: Python
mathjax: true
---


## 차원 축소(Dimension reduction)
### 차원 축소의 장점
- 학습 데이터 크기를 줄여 시간 절약 가능
- 불필요한 피처들을 줄여 모델 성능 향상에 기여
- 다차원 데이터를 3차원 이하의 차원 축소를 통해 시각적으로 보다 쉽게 데이터 패턴 인지
- **차원 축소 시 원본 데이터의 정보를 최대한 유지한 채로 차원 축소를 수행하는 것이 요점**
- 차원 축소 방법
  - 피처 선택 : 특정 피처에 종속성이 강한 불필요한 피처 제거, 특징을 잘 나타내는 주요 피처만 선택
  - 피처 추출 : 기존 피처를 저차원의 중요 피처로 압축해서 추출, 대표 기법. PCA
- 차원 축소의 의미 : 단순히 압축을 의미하는 것이 아닌 축소를 통해 좀 더 데이터를 잘 설명할 수 있는 잠재적인 요소를 추출하는 데에 있음
- 사용 예 : 추천 엔진, 이미지 분류 및 변환, 문서 토픽 모델링


### PCA(Principal Component Analysis)
- 여러 변수 간에 존재하는 상관관계를 이용해 이를 대표하는 주성분을 추출해 차원을 축소하는 기법으로 차원은 줄이면서 손실은 최소화하는 것이 목적
- 선형에 관련된 기법은 대부분 PCA를 사용
- 주성분 : 가장 높은 분산은 가지는 데이터의 축
- 새롭게 찾아낸 축의 좌표값 : PC_SCORE
- 공분산 : 두 개의 변순간의 변동 / 공분산 행렬 : 여러 변수와 관련된 공분산을 포함하는 정방형 대칭 행렬
- 수행 순서
  1. 입력 데이터 세트의 공분산 행렬 생성
  2. 공분산 행렬의 고유벡터와 고유값 계산
  3. 고유값이 가장 큰 순으로 K개(PCA변환 차수)만큼 고유벡터 추출
  4. 고유값이 가장 큰 순으로 추출된 고유벡터를 이용해 새롭게 입력 데이터를 변환


#### 붓꽃 데이터 세트를 이용한 PCA
```python
# 사이킷런 내장 데이터 셋 API 호출
from sklearn.datasets import load_iris
import pandas as pd
import matplotlib.pyplot as plt

# 넘파이 데이터 셋을 Pandas DataFrame으로 변환
columns = ['sepal_length', 'sepal_width', 'petal_length', 'petal_width']
irisdf = pd.DataFrame(iris.data, columns=columns)
irisdf['target'] = iris.target
irisdf.head()

# setosa는 세모, versicolor는 네모, virginica는 동그라미로 표현
markers = ['^', 's','o']
# setosa의 target 값은 0, versicolor는 1, virginica는 2. 각 target 별로 다른 shape으로 scatter plot 
for i, marker in enumerate(markers):
    x_axis_data = irisdf[irisdf['target']==i]['sepal_length']
    y_axis_data = irisdf[irisdf['target']==i]['sepal_width']
    plt.scatter(x_axis_data, y_axis_data, marker=marker, label=iris.target_names[i])
plt.legend(loc='best')
plt.xlabel('sepal length')
plt.ylabel('sepal width')
plt.show()
```  
![iris](https://user-images.githubusercontent.com/52812181/74825106-b2c38780-534c-11ea-8bc7-2d8f0d399b75.png)
  
```python
from sklearn.preprocessing import StandardScaler
# Target 값을 제외한 모든 속성 값을 StandardScaler를 이용하여 표준 정규 분포를 가지는 값들로 변환
iris_scaled = StandardScaler().fit_transform(irisdf.iloc[:, :-1])

from sklearn.decomposition import PCA

pca = PCA(n_components=2)

#fit( )과 transform( ) 을 호출하여 PCA 변환 데이터 반환
pca.fit(iris_scaled)
iris_pca = pca.transform(iris_scaled)
print(iris_pca.shape)


# PCA 환된 데이터의 컬럼명을 각각 pca_component_1, pca_component_2로 명명
pca_columns = ['pca_component_1', 'pca_component_2']
irisdf_pca = pd.DataFrame(iris_pca, columns=pca_columns)
irisdf_pca['target']=iris.target
irisdf_pca.head()


#setosa를 세모, versicolor를 네모, virginica를 동그라미로 표시
markers=['^', 's', 'o']

#pca_component_1 을 x축, pc_component_2를 y축으로 scatter plot 수행. 
for i, marker in enumerate(markers):
    x_axis_data = irisdf_pca[irisdf_pca['target']==i]['pca_component_1']
    y_axis_data = irisdf_pca[irisdf_pca['target']==i]['pca_component_2']
    plt.scatter(x_axis_data, y_axis_data, marker=marker,label=iris.target_names[i])

plt.legend()
plt.xlabel('pca_component_1')
plt.ylabel('pca_component_2')
plt.show()
```  
![iris2](https://user-images.githubusercontent.com/52812181/74824974-7001af80-534c-11ea-8e3e-42a5cda00c70.png)  

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import cross_val_score
import numpy as np

rcf = RandomForestClassifier(random_state=123)
scores = cross_val_score(rcf, iris.data, iris.target, scoring='accuracy', cv=3)
print('원본 데이터 교차 검증 개별 정확도:', scores)
print('원본 데이터 평균 정확도:', np.mean(scores))
```
- 원본 데이터 교차 검증 개별 정확도: [0.98 0.94 0.96]
- 원본 데이터 평균 정확도: 0.96
```python
pca_X = irisdf_pca[['pca_component_1', 'pca_component_2']]
scores_pca = cross_val_score(rcf, pca_X, iris.target, scoring='accuracy', cv=3 )
print('PCA 변환 데이터 교차 검증 개별 정확도:',scores_pca)
print('PCA 변환 데이터 평균 정확도:', np.mean(scores_pca))
```
- PCA 변환 데이터 교차 검증 개별 정확도: [0.88 0.88 0.88]
- PCA 변환 데이터 평균 정확도: 0.88





