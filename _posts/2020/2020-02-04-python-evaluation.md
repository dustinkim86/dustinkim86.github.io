---
layout: post
title: Evaluation
mathjax: true
tags:
  - python
---

<br/>

# **평가(Evaluation)**



### 성능 평가 지표
- 정확도(Accuracy) : 예측 결과와 실제 값이 동일한 건수 / 전체 데이터 수
- 오차 행렬(Confusion Matrix)
- import numpy as np
  import cudf정밀도(Precision)
- 재현율(Recall)
- F1 스코어(F1 score)
- ROC AUC

-------

#### 회귀 모델 : MSE(평균 제곱 오차)를 주로 사용

#### 분류 모델 : 일반적으로는 정확도

- 정확도만을 보면 잘못된 평가를 하게 될 수 있음. 특히 2진 분류
- 불균형한 레이블 값 분포에서 정확도는 부적합

-----

#### 오차 행렬(Confusion Matrix)

- 이진 분류에서 성능 지표로 활용

- 이진 분류의 예측 오류가 얼마인지와 더불어 어떠한 유형의 예측 오류가 발생하고 있는지를 함께 나타내는 지표

  ![IUrBHiD](https://user-images.githubusercontent.com/52812181/73509831-d7020780-4423-11ea-90f9-240b18916895.png)

- import : `from sklearn.metrics import confusion_matrix`

- 정확도(accuracy) : (TN + TP) / (TN + FN + FP + TP)

- 정밀도(precision) : TP / (FP + TP) : 실제  negative 음성인 데이터 예측을 positive로 잘못 판단하면 큰 영향이 발생하는 경우(스팸 메일)

- 재현율(recall) : TP / (FN + TP) : 실제 positive 양성인 데이터 예측을 negative로 잘못 판단하면 큰 영향이 발생하는 경우(암 진단, 금융사기 판별)

- precision / recall trade-off

  - 분류하려는 업무 특성상 정밀도 또는 재현율이 특별히 강조돼야 할 경우 분류의 결정 임곗값을 조정해 정밀도 또는 재현율의 수치를 높일 수 있음
  - 정밀도와 재현율은 상호 보완적인 평가 지표이므로 한 쪽을 높이면 다른 한 쪽은 떨어짐
  - predict_proba() : 개별 데이터별로 예측 확률을 반환하는 함수.  테스트 피처 레코드의 개별 클래스를 예측 확률로 반환
    - 함수 실행 결과의 0번 칼럼은 negative / 1번 칼럼은 positive를 나타냄
  - Binarizer class
    - import : `from sklearn.preprocessing import Binarizer`
    - threshold : 분류 결정 임곗값, 낮출수록 positive로 예측을 더 너그럽게 함. 즉, True값이 많아지게 됨 --> 재현율(recall)은 올라가고 정밀도(precision)은 떨어짐
    - .fit_transform : 지정한 threshold값보다 작거나 같으면 0, 크면 1로 변환
  - precision_recall_curve()
    - 임곗값 변화에 따른 평가 지표 값 반환
    - import : `from sklearn.metrics import precision_recall_curve`

#### F1 스코어(F1 score)

- 정밀도와 재현율을 결합한 지표로 이진분류인 데이터를 확인할 때 사용
- 정밀도와 재현율이 어느 한 쪽으로 치우치지 않는 수치를 나타낼 때 상대적으로 높은 값을 지님

  ​								 F1 = 2 * precision * recall / recision + recall
- import : `from sklearn.metrics import f1_score `

#### ROC곡선과 AUC스코어

- 이진 분류의 예측 성능 측정에서 중요하게 사용되는 지표

- ROC곡선은 FPR(False Positive Rate)이 변할 때 TPR(True Positive Rate, 민감도)이 어떻게 변하는지 나타내는 곡선

- TPR = TP / (FN + TP)

- TPR 대응 지표 : TNR(True Negative Rate) --> TNR = TN / (FP + TN)

- FPR은 FP / (FP + TN) 이므로 1 - TNR 또는 1 - 특이성으로 표현

  ​	<img width="449" alt="roc곡선" src="https://user-images.githubusercontent.com/52812181/73702699-d671b580-4730-11ea-9bff-246652e3b512.png">

   - ROC곡선이 점선에 멀어지고 AOC 면적이 1에 가까워질수록 좋은 성능

   - import : `from sklearn.metrics import roc_curve`

      - parameter

        	- y_true :  실제 클래스 값
        	- y_score : predict_prob()의 반환 값
        	- thresholds(임곗값) : threshold값 array

        ```python
        # 레이블 값이 1일 때의 예측 확률 추출
        predict_proba_class = lr_clf.predict_proba(X_test)[:, 1]
        fprs, tprs, thresholds = roc_curve(y_test, predict_proba_class)
        # 반환된 임곗값 배열 로우가 47건이므로 샘플로 10건만 추출하되, 임곗값을 5 step으로 추출
        thr_index = np.arange(0, thresholds.shape[0], 5)
        print('샘플 추출을 위한 임곗값 배열의 index 10개 : ', thr_index)
        print('샘플용 10개의 임곗값 :', np.round(thresholds[thr_index], 2))
        # 5 step 단위로 추출된 임곗값에 따른 FPR, TPR 값
        print('샘플 임곗값별 FPR :', np.round(fprs[thr_index], 3))
        print('샘플 임곗값별 TPR :', np.round(tprs[thr_index], 3))
        ```

  - ROC AUC값을 구할 수 있는 함수 : `from sklearn.metrics import roc_auc_socre`
    - roc_auc_score(y_real_values, y_predict_values)






