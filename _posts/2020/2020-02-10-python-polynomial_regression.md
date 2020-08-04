---
layout: post
title: Regression(회귀)
date: 2020-02-10
summary: 다중 회귀 분석과 규제 선형 모델
categories: Python
mathjax: true
---


## 다중 회귀
- 설명변수(독립변수)가 2개 이상인 회귀 모형
- 다중 회귀 모형
  
$$
Y_i = \beta_0 + \beta_1X_{1i} + \beta_2X_{2i} + ... + \beta_pX_i + e_i, i = 1,2, ..., n
$$
  
### 다항 회귀(Polynomial Regression)
- 동일 설명변수의 1차항과 2차항이 동시에 있는 모형
- 다중공선성 문제가 발생하므로 설명변수를 표준화해야 함
- 단항식 피처를 다항식으로 변환
	- import : `from sklearn.preprocessing import PolynomialFeatures`
	- Features
		- **degree : 차수**
		- interaction_only : True면 2차항에서 상호작용항만 출력
		- include_bias : 상수항 생성 여부
  
- 과소적합(Under fitting)과 과대적합(Over fitting)
	- 과소적합
		- 원인
			1. 부적절한 분석 모형
			2. 학습 데이터의 부족
			3. 표준 집합 부족
		- 대응 방안
			1. 분석 모델의 유연성 확보
			2. 충분한 학습 데이터 확보
			3. Cross Validation
	- 과대적합
		- 원인
			1. 편중된 학습 데이터
			2. 과하게 많은 Features
			3. 무분별한 노이즈의 수용
		- 대응 방안
			1. 다양한 훈련 데이터 확보
			2. 정규화와 표준화
			3. Dropout(일부 뉴런 생략)
	- 과소/과대적합 방지를 위해서는..
		- 노이즈를 고려한 적절한 분포도를 지닌 충분한 훈련 데이터의 확보
		- 대상 별 적절한 Feature수 선정과 원하는 분석모형을 고려한 일반화
		- degree(차수) 계수 설정에 따라 과대적합 또는 과소적합에 이를 수 있기 때문에 적절한 계수 설정이 필요
	![unver_overfitr](https://user-images.githubusercontent.com/52812181/74119034-2bb23900-4c01-11ea-8848-c524bb7b2946.png)
  
  
- 편향-분산 트레이드 오프(Bias-Variance Trade off)
	- 지도 학습 알고리즘이 트레이닝 셋의 범위를 넘어 지나치게 일반화 하는 것을 예방하기 위해 두 종류의 오차(편향, 분산)를 최소화 할 때 겪는 문제
	- 편향 : 학습 알고리즘에서 잘못된 가정을 했을 때 발생하는 오차(높은 편향값은 과소적합 문제 발생)
	- 분산 : 훈련 셋에 내재된 작은 변동때문에 발생하는 오차(높은 분산값은 과대적합 문제 발생)
  
  
### 규제 선형 모델
- 회귀 계수의 크기를 제어해 과적합을 개선하기 위해 비용 함수 목표를 최소화하는 것
	$$
	비용함수 목표 = Min(RSS(W) + alpha * ||W||^2_2
	$$
  
	- alpha : 학습 데이터 적합 정도와 회귀 계수 값의 크기 제어를 수행하는 튜닝 파라미터
		- alpha 값을 크게 하면 비용 함수는 회귀 계수 W의 값으 ㄹ작게 해 과적합을 개선
		- alpha 값을 작게 하면 회귀 계수 W의 값이 커져도 어느 정도 상쇄가 가능해 학습 데이터 적합을 개선
  
1. Lidge(L2 규제)
	- 회귀 계수(W)의 제곱에 대해 패널티를 부여하는 방식
	- 제곱을 하기 때문에 0에 수렴하지만 0이 되지는 않음
	- 대부분의 Feature의 영향력이 비슷할 때 사용
2. Lasso(L1 규제)
	- 회귀 계수(W)의 절댓값에 대해 패널티를 부여하는 방식
	- 절댓값을 하기 때문에 0이 될 수 있음
	- 어떤 Feature의 영향력이 너무 커서 나머지 Feature들이 힘을 발휘하지 못하고 차이가 날 때 사용
	- Feature를 지워 0에 가깝도록 만드는 방법
3. Elastic Net
	- 릿지와 라쏘를 결합하여 상관관계가 얽혀 있을 때 효과적이라고 하나, 검증하기 어려움
	- 결합된 규제로 수행시간이 긺
  
- 선형 회귀 모델을 위한 데이터 변환
	- 일반적으로 중요 피처들이나 타깃값의 분포도가 심하게 왜곡됐을 경우 수행
	- 피처 데이터 셋 변환 작업
		- StandardScaler, MinMaxScaler 등 이용
		- 스케일한 데이터에 다항 특성을 적용(스케일링 성능 향상이 없을 경우)
		-   원 데이터에 로그 변환(많이 사용되는 방법)
  

### 로지스틱 회귀
- 분류에 사용되는 선형 회귀
- 가중치(weight) 변수가 선형인지 아닌지를 따름
- 선형 회귀와의 차이점 : 학습을 통해 회귀 최적선을 찾는 것이 아니라 **시그모이드 함수 최적선**을 찾고 함수의 반환 값을 확률로 간주해 **확률에 따라** 분류를 결정
- 계산식
$$
y = {1 \over 1 + e^{-x}}
$$
  
- 항상 0과 1사이 값을 반환
- 오즈비(Odds ratio) : 0(실패)에 대한 1(성공)의 비율(0 : no, 1 : yes)
	$$
	odds = {p(success) \over 1-p(fail)} 
	$$ 
	- p : y = 1이 나올 확률
	- 1-p : y = 1의 여사건

- 로그 변환
	$$
	logit = log({p \over 1-p})
	$$
	  
- 장점
	- 가볍고 빠름
	- 이진 분류 예측 성능이 뛰어남
	- 희소한 데이터 세트 분류에도 뛰어난 성능을 보여 텍스트 분류에서도 자주 사용됨