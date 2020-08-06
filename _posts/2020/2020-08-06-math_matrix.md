---
layout: post
title: 선형대수학 - 행렬과 행렬식
mathjax: true
tags:
  - math


---

<br/>

## 행렬

- 용어 정리

    - 성분 = 항 = 원소 : 행렬 안에 배열된 구성원
    - 행 : 행렬의 가로줄
    - 열 : 행렬의 세로줄
    - $$ m * n $$ 행렬 : m개의 행과 n개의 열로 이루어진 행렬
    - 주대각선 : 행렬의 왼쪽 위에서 오른쪽 아래를 가르는 선
    - 대각성분 : 주대각선에 걸치는 행과 열의 지표수가 같은 성분
    - 영행렬 : 모든 성분이 0인 행렬
    - 전치행렬 : $$ (a_{ij}) $$ 에 대하여 $$ (a_{ij}) $$
    - 대칭행렬 : $$ A = A^t $$ 인 $$ A $$
    - 정사각행렬 : 행, 열의 개수가 같은 행렬
    - 단위행렬 : 모든 대각성분이 1이고, 그 외의 성분은 0인 정사각행렬

- 행렬의 연산

    $$ m * n $$ 행렬 $$ A = (a_{ij}) $$ , $$ B = (b_{ij}) $$ 에 대해

    - 덧셈과 뺄셈 : $$ A \pm B = (a_{ij} \pm b_{ij}) $$
    - 상수배 : 상수 c 에 대해 $$ cA = (ca_{ij}) $$

    $$  m * n $$ 행렬 $$ A = (a_{ij}) $$ 와 $$ n * r $$ 행렬 $$ B = (b_{jk}) $$ 에 대해

    - 곱셈 : $$ AB = (c_{ik}) $$ : $$ m * r $$ 행렬

        단, $$ c_{ik} = \sum\limits_{j = 1}^{n}{(a_{ij}b_{jk})} $$

        - 행렬의 곱셈은 교환법칙이 성립되지 않는다

<br/>

## 연립일차방정식

- 행렬의 표현
    - 예를 들어, $$ x + 2y = 5 $$ 와 $$ 2x + 3y = 8 $$ 은
        - $$ \begin{pmatrix} 1 & 2 & 5 \\ 2 & 3 & 8 \end{pmatrix} $$ 표현 --> 가우스 조던 소거법
        - $$ \begin{pmatrix} 1 & 2 \\ 2 & 3 \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} 5 \\ 8 \end{pmatrix} $$ 표현 --> 역행렬 이용

- 가우스 조던 소거법
    - 다음 세 가지의 기본 행 연산을 통해 연립일차방정식의 첨가행렬을 기약 행 사다리꼴로 변환하여 해를 구한다.
        - 한 행을 상수배한다.
        - 한 행을 상수배하여 다른 행에 더한다.
        - 두 행을 맞바꾼다.
- 역행렬 이용
    - 연립일차방정식 $$ AX = B $$ 에서 $$ A $$의 역행렬 $$ A^-1 $$ 가 존재하면, $$ X = A^-1B $$ 이다.
    - 예를 들어, $$ \begin{pmatrix} 1 & 2 \\ 2 & 3 \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} 5 \\ 8 \end{pmatrix} <=> \begin{pmatrix} x \\ y  \end{pmatrix} = \begin{pmatrix} 1 & 2 \\ 2 & 3 \end{pmatrix}^-1 \begin{pmatrix} 5 \\ 8 \end{pmatrix} $$

<br/>

## 행렬식

- 행렬식이란? 정사각행렬 $$ A $$를 하나의 수로써 대응시키는 특별한 함수. det $$ A = | A | $$

- 이때, $$ A $$가

    1) 0 X 0 -> det ( ) = 0

    2) 1 X 1 -> det ($$a$$) = $$a$$

    3) 2 X 2 -> det $$ \begin{pmatrix} a_11 & a_12 \\ a_21 & a_22 \end{pmatrix} = a_11a_22 - a_12a_21 $$

    4) 3 X 3 -> det $$ \begin{pmatrix} a_11 & a_12 & a_13 \\ a_21 & a_22 & a_23 \\ a_31 & a_32 & a_33 \end{pmatrix} = a_11M_11 - a_12M_12 + a_13M_13 = a_11\begin{matrix} a_22 & a_23 \\ a_32 & a_33 \end{matrix} - a_12\begin{pmatrix} a_21 & a_23 \\ a_31 & a_33 \end{pmatrix} + a_13\begin{pmatrix} a_21 & a_22 \\ a_31 & a_32 \end{pmatrix} = a_11a_22a_33 + a_12a_23a_31 + a_13a_21a_32 - a_13a_22a_31 - a_11a_23a_32 - a_12a_21a_33 $$
