---
layout: post
title: Data Handling(데이터 핸들링)
subtitle : R language
tags: [R]
author: dustinkim86
comments : True
---


## 데이터 핸들링
### 수집한 원형 데이터를 데이터 분석의 목적에 맞게 가공,처리하는 과정
#### 관련함수 : plyr, dplyr, reshape, reshape2  

#### 1. plyr 패키지 활용  
  - 데이터 병합 : join()
    ```r
    # 병합할 데이터프레임 셋 만들기
    x = data.frame(ID = c(1,2,3,4,5), height = c(160,171,173,162,165))
    y = data.frame(ID = c(5,4,1,3,2), weight = c(55,73,60,57,80))
    # join() : plyr패키지 제공 함수
    z <- join(x,y,by='ID') # ID컬럼으로 조인
    z
    ```
    - 출력 결과
      ```r
      ID height weight
      1 1 160 60
      2 2 171 80
      3 3 173 57
      4 4 162 73
      5 5 165 55
      ```
  
  - Left 조인
    ```r
    x = data.frame(ID = c(1,2,3,4,6), height = c(160,171,173,162,180))
    y = data.frame(ID = c(5,4,1,3,2), weight = c(55,73,60,57,80))
    # left 조인
    z <- join(x,y,by='ID') # ID컬럼으로 left 조인
    z
    ```
    - 출력 결과
      ```r
      ID height weight
      1 1 160 60
      2 2 171 80
      3 3 173 57
      4 4 162 73
      5 6 180 NA <- 결측치이면 NA 처리됨
      ```
  
  - Inner 조인
    ```r
    z <- join(x,y,by='ID', type='inner') # type='inner' : 값이 있는 것만 조인
    z
    ```
    - 출력 결과
      ```r
      ID height weight
      1 1 160 60
      2 2 171 80
      3 3 173 57
      4 4 162 73
      ```
  - Full 조인
    ```r
    z <- join(x,y,by='ID', type='full') # type='full' : 모든 항목 조인
    z
    ```
    - 출력 결과
      ```r
      ID height weight
      1 1 160 60
      2 2 171 80
      3 3 173 57
      4 4 162 73
      5 6 180 NA
      6 5 NA 55
      ```
  - key값으로 조인
    ```r
    x = data.frame(key1 = c(1,1,2,2,3), key2 = c('a','b','c','d','e'), val1 = c(10,20,30,40,50))
    y = data.frame(key1 = c(3,2,2,1,1), key2 = c('e','d','c','b','a'), val2 = c(500,400,300,200,100))
    join(x,y,by=c('key1', 'key2'))
    ```
    - 출력 결과
      ```r
        key1 key2 val1 val2
      1  1     a   10  100
      2  1     b   20  200
      3  2     c   30  300
      4  2     d   40  400
      5  3     e   50  500
      ```

  - tapply() 이용한 집단별(그룹별) 통계치 구하기
    ```r
    예) tapply(iris$Sepal.Length, iris$Species, mean) # 종별 평균치```


#### 2. dplyr 패키지 활용  
  - 데이터를 분석에 필요한 형태로 만드는 데이터 전처리 관련 함수 제공 패키지
  - 주요 함수
    - tbl_df() : 데이터셋 화면창 크기 만큼만 데이터 제공
    - filter() : 지정한 조건식에 맞는 데이터 추출 - subset()
    - select() : 열 추출 - data[, c('ddd', mmm')]
    - mutate() : 열 추가 - transform()
    - arrange() : 정렬 - order(), sort()
    - summarise() : 집계 - aggregate()

  - filter(dataframe, 조건1, 조건2) 함수 이용 데이터 추출
    ```r
    # 1월 1일 데이터 추출
    filter(hflights_df, Month == 1, DayofMonth == 1) # and(, or &)
    # 1월 혹은 2월 데이터 추출
    filter(hflights_df, Month == 1 | Month == 2) # or(|)
    ```
  - arrange()함수를 이용한 오름차순/내림차순 정렬
    ```r
    # 년도,월,도착시간 오름차순
    arrange(hflights_df, Year, Month, ArrTime)
    # 내림차순 - desc(변수) 사용
    ```
  - select()함수를 이용한 열 조작
    ```r
    # Year, Month, DayOfWeek 열을 추출
    select(hflights_df, Year, Month, DayOfWeek)
    # Month기준 내림차순 정렬 -> Year, Month, AirTime, ArrDelay 컬럼 추출
    select(arrange(hflights_df, desc(Month)),Year, Month, AirTime, ArrDelay)
    # Year~DayOfWeek 추출(Year, Month, DayofMonth, DayofWeek)
    select(hflights_df, Year:DayOfWeek)
    # Year부터 DayOfWeek를 제외한 나머지 열 추출
    select(hflights_df, -(Year:DayOfWeek))
    ```
  - mutate()함수를 이용한 열 추가(변형)
    ```r
    # gain 변수 추가 -> gain_per_hour 변수 계산에 사용할 수 있음
    mutate(hflights_df, gain = ArrDelay - DepDelay,
    gain_per_hour = gain/(AirTime/60))
    # 새로 만든 열을 같은 함수 안에서 바로 사용 가능
    ```
  - summarise()함수를 이용한 집계
    ```r
    # n(), sum(), mean(), sd(), var(), median() 등의 함수 사용 - 기초 통계량
    summarise(hflights_df, n()) # n() : dataframe의 row값 리턴 함수
    # 평균 출발지연시간 계산
    summarise(hflights_df, cnt = n(), delay=mean(DepDelay, na.rm = TRUE))
    # Source: local data frame [1 x 1] -> 결과는 data frame
    #      cnt    delay
    # 1 227496 9.444951
    ```
  - group_by(dataframe, 기준변수)함수를 이용한 그룹화
    ```r
    planes <- group_by(hflights_df, TailNum)
    ```


#### 3. reshape2 패키지 활용  
  - 긴 형식 <-> 넓은 형식
  - 주요 함수
    - dcast() : 긴 형식을 넓은 형식으로 모양 변경
    ```r
    예) 아래와 같은 형태를 ~
          Date  Customer_ID Buy
    1  20140101          1   3
    2  20140101          2   4
    3  20140102          1   2
    4  20140102          4   6

        아래와 같은 형태로 변경
        Customer_ID 20140101 20140102
    1             1       3         2 
    2             2       4         0
    3             4       0         6
    ```
    - melt() : 넓은 형식을 긴 형식으로 모양 변경, 위의 예제를 반대로 보면 됨
