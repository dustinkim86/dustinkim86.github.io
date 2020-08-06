---
layout: post
title: Decision Tree
mathjax: true
tags:
  - python
---

<br/>

## 결정 트리(Decision Tree)
- 모델의 시각화가 직관적이고 가독성이 높아 이해하기 쉬운 알고리즘
- 데이터에 있는 규칙을 학습을 통해 자동으로 찾아내 트리 기반의 분류 규칙을 만드는 것
- 변수의 스케일 조정 필요 없음
- 이진과 연속 특징이 혼합되어 있어도 잘 동작하며 많은 특징(입력변수)를 갖는 데이터 셋은 부적합
- 깊이(depth)가 깊어질수록 예측 성능 저하
- 과대적합이 발생할 우려가 있는 단점
	- 과적합을 해결하기 위한 방안
		- 각 규칙이 포함한 관측값의 최소 갯수 제한
		- 가지치기(cp=0.01)
- 데이터 균일도 측정 방법
	- 엔트로피(Entropy) 지수 : 주어진 데이터 집합의 혼잡도
		$$
		Entropy = -sum(p * log(p))
		$$
	- 정보 이득(Information Gain) 지수 : 지수가 높은 속성을 기준으로 결정 트리 분할, 1에서 엔트로피 지수를 뺀  값(1 - 엔트로피 지수)
	- 지니 계수(Gini Impurity) : 확률 변수 간의 불확실성을 나타내는 수치, 1로 갈수록 균일도가 높으므로 지니 계수가 높은 속성을 기준으로 분할
		$$
		Gini impurity = sum(p * (1-p))
		$$
- CART(Classification And Regression Trees) 기반 알고리즘
- Parameter
	- min_samples_split
		- 노드를 분할하기 위한 최소한의 샘플 데이터 수(과적합 제어)
	- min_samples_leaf
		- 말단 노드가 되기 위한 최소한의 샘플 데이터 수(과적합 제어의 용도는 비슷하나 비대칭적)
	- max_features
		- 최적의 분항르 위해 고려할 최대 피처 개수(default = None)
	- max_depth
		- 트리의 최대 깊이를 규정
		- 적절한 값으로 제어 필요
	- max_leaf_nodes
		- 말단 노드 최대 개수
- 모델의 시각화 : Graphviz library
	- 윈도우의 경우 설치 후 환경변수 설정 필요
	- 사이킷런의 export_graphviz()로 데이터 전송
		- import : `from sklearn.tree import export_graphviz`
		- source
			```python
			export_graphviz(dt_clf, out_file="tree.dot", class_names=iris_data.target_names, feature_names = iris_data.feature_names, impurity=True, filled=True)
	  ```
- 피처의 중요한 역할 지표 제공
	 - `classification_model.feature_importances_`

#### 예제1. 사이킷런 내장 데이터셋 'wine'을 이용한 Decision Tree
```python
import numpy as np
import pandas as pd
from sklearn.datasets import load_wine
from sklearn.tree import DecisionTreeClassifier # Y변수 범주형
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# 1. dataset load
wine = load_wine()
X = wine.data
y = wine.target
print(X.shape) # (178, 13)
print(y.shape) # (178,)

# 2. train / test split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.3, random_state = 123)
print(X_train.shape) # (124, 13)
print(y_test.shape) # (54,)

# 3. model 생성
model = DecisionTreeClassifier().fit(X_train, y_train)
pred = model.predict(X_test)
y_true = y_test

# 4. model 평가 - confusion matrix
tab = pd.crosstab(y_true, pred)
print(tab)
acc = (tab.iloc[0,0] + tab.iloc[1,1] + tab.iloc[2,2]) / len(y_true)
print(acc) # 0.9444444444444444
# metrics.accuracy_score 이용
acc2 = accuracy_score(y_true, pred)
print(acc2) # 0.9444444444444444
```

#### 예제2. 사용자 행동 인식 데이터 Decision Tree
- 데이터셋 다운로드 [링크](https://archive.ics.uci.edu/ml/datasets/human+activity+recognition+using+smartphones)  

```python
# features.txt 파일에는 피처 이름 index와 피처명이 공백으로 분리되어 있음. 이를 DataFrame으로 로드.
feature_name_df = pd.read_csv('./human_activity/features.txt',sep='\s+', header=None,names = ['column_index','column_name'])

# 피처명 index를 제거하고, 피처명만 리스트 객체로 생성한 뒤 샘플로 10개만 추출
feature_name = feature_name_df.iloc[:, 1].values.tolist()
print('전체 피처명에서 10개만 추출:', feature_name[:10])
feature_name_df.head(20)

def get_new_feature_name_df(old_feature_name_df):
	feature_dup_df = pd.DataFrame(data=old_feature_name_df.groupby('column_name').cumcount(), columns=['dup_cnt'])
	feature_dup_df = def get_human_dataset( ):
	# 각 데이터 파일들은 공백으로 분리되어 있으므로 read_csv에서 공백 문자를 sep으로 할당.
	feature_name_df = pd.read_csv('./human_activity/features.txt',sep='\s+', header=None,names=['column_index','column_name'])
	# 중복된 feature명을 새롭게 수정하는 get_new_feature_name_df()를 이용하여 새로운 feature명 DataFrame생성
	new_feature_name_df = get_new_feature_name_df(feature_name_df)
	# DataFrame에 피처명을 컬럼으로 부여하기 위해 리스트 객체로 다시 변환
	feature_name = new_feature_name_df.iloc[:, 1].values.tolist()
	# 학습 피처 데이터 셋과 테스트 피처 데이터을 DataFrame으로 로딩. 컬럼명은 feature_name 적용
	X_train = pd.read_csv('./human_activity/X_train.txt',sep='\s+', names=feature_name )
	X_test = pd.read_csv('./human_activity/X_test.txt',sep='\s+', names=feature_name)
	# 학습 레이블과 테스트 레이블 데이터을 DataFrame으로 로딩하고 컬럼명은 action으로 부여
	y_train = pd.read_csv('./human_activity/y_train.txt',sep='\s+',header=None,names=['action'])
	y_test = pd.read_csv('./human_activity/y_test.txt',sep='\s+',header=None,names=['action'])
	# 로드된 학습/테스트용 DataFrame을 모두 반환
	return X_train, X_test, y_train, y_test

X_train, X_test, y_train, y_test = get_human_dataset() if x[1] >0  else x[0] , axis=1)
	new_feature_name_df = new_feature_name_df.drop(['index'], axis=1)
	return new_feature_name_df

pd.options.display.max_rows =  999
new_feature_name_df = get_new_feature_name_df(feature_name_df)
new_feature_name_df[new_feature_name_df['dup_cnt'] >  0]

def get_human_dataset( ):
	# 각 데이터 파일들은 공백으로 분리되어 있으므로 read_csv에서 공백 문자를 sep으로 할당
	feature_name_df = pd.read_csv('./human_activity/features.txt',sep='\s+', header=None,names=['column_index','column_name'])
	# 중복된 feature명을 새롭게 수정하는 get_new_feature_name_df()를 이용하여 새로운 feature명 DataFrame생성
	new_feature_name_df = get_new_feature_name_df(feature_name_df)
	# DataFrame에 피처명을 컬럼으로 부여하기 위해 리스트 객체로 다시 변환
	feature_name = new_feature_name_df.iloc[:, 1].values.tolist()
	# 학습 피처 데이터 셋과 테스트 피처 데이터을 DataFrame으로 로딩. 컬럼명은 feature_name 적용
	X_train = pd.read_csv('./human_activity/X_train.txt',sep='\s+', names=feature_name )
	X_test = pd.read_csv('./human_activity/X_test.txt',sep='\s+', names=feature_name)
	# 학습 레이블과 테스트 레이블 데이터을 DataFrame으로 로딩하고 컬럼명은 action으로 부여
	y_train = pd.read_csv('./human_activity/y_train.txt',sep='\s+',header=None,names=['action'])
	y_test = pd.read_csv('./human_activity/y_test.txt',sep='\s+',header=None,names=['action'])
	# 로드된 학습/테스트용 DataFrame을 모두 반환
	return X_train, X_test, y_train, y_test

X_train, X_test, y_train, y_test = get_human_dataset()

print(y_train['action'].value_counts())
X_train.isna().sum().sum()
 

from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score

# 예제 반복 시 마다 동일한 예측 결과 도출을 위해 random_state 설정
dt_clf = DecisionTreeClassifier(random_state=156)
dt_clf.fit(X_train , y_train)
pred = dt_clf.predict(X_test)
accuracy = accuracy_score(y_test , pred)
print('결정 트리 예측 정확도: {0:.4f}'.format(accuracy))


# DecisionTreeClassifier의 하이퍼 파라미터 추출
print('DecisionTreeClassifier 기본 하이퍼 파라미터:\n', dt_clf.get_params())
 
 
from sklearn.model_selection import GridSearchCV
params = {
	'max_depth' : [ 6, 8 ,10, 12, 16 ,20, 24]
}


grid_cv = GridSearchCV(dt_clf, param_grid=params, scoring='accuracy', cv=5, verbose=1 )
grid_cv.fit(X_train , y_train)
print('GridSearchCV 최고 평균 정확도 수치:{0:.4f}'.format(grid_cv.best_score_))
print('GridSearchCV 최적 하이퍼 파라미터:', grid_cv.best_params_)


max_depths = [ 6, 8 ,10, 12, 16 ,20, 24]
# max_depth 값을 변화 시키면서 그때마다 학습과 테스트 셋에서의 예측 성능 측정
for depth in max_depths:
	dt_clf = DecisionTreeClassifier(max_depth=depth, random_state=156)
	dt_clf.fit(X_train , y_train)
	pred = dt_clf.predict(X_test)
	accuracy = accuracy_score(y_test , pred)
	print('max_depth = {0} 정확도: {1:.4f}'.format(depth , accuracy))


params = {
	'max_depth' : [ 8 , 12, 16 ,20],
	'min_samples_split' : [16,24],
}


grid_cv = GridSearchCV(dt_clf, param_grid=params, scoring='accuracy', cv=5, verbose=1 )
grid_cv.fit(X_train , y_train)
print('GridSearchCV 최고 평균 정확도 수치: {0:.4f}'.format(grid_cv.best_score_))
print('GridSearchCV 최적 하이퍼 파라미터:', grid_cv.best_params_)

best_df_clf = grid_cv.best_estimator_

pred1 = best_df_clf.predict(X_test)
accuracy = accuracy_score(y_test , pred1)
print('결정 트리 예측 정확도:{0:.4f}'.format(accuracy))

  
import seaborn as sns
ftr_importances_values = best_df_clf.feature_importances_
# Top 중요도로 정렬을 쉽게 하고, 시본(Seaborn)의 막대그래프로 쉽게 표현하기 위해 Series변환
ftr_importances = pd.Series(ftr_importances_values, index=X_train.columns )

# 중요도값 순으로 Series를 정렬
ftr_top20 = ftr_importances.sort_values(ascending=False)[:20]
plt.figure(figsize=(8,6))
plt.title('Feature importances Top 20')
sns.barplot(x=ftr_top20 , y  = ftr_top20.index)
plt.show()import pandas as pd
```
