---
layout: post
title: R one-day Homework
date: 2020-04-02
summary: 데이터 전처리, 시각화, 트위터 크롤링과 워드클라우드
categories: R
mathjax: true
---

<br/>
### Date : 2020/04/02

<br/>

## Preprocessing

- 사용 데이터 : Kaggle COVID-19 한국 [다운로드 링크](https://www.kaggle.com/kimjihoo/coronavirusdataset)
  - 확진자 데이터
  - 확진자 이동경로 데이터

- Source

  ```R
  library(dplyr)
  library(plyr)
  
  # patient data load
  patient <- read.csv('./dataset/PatientInfo.csv')
  View(patient)
  
  str(patient)
  summary(patient)
  
  # 환자번호, 성별, 연령, 지역(시도), 지역(군구), 전염사유, 확진일 컬럼 추출
  df <- select(patient, patient_id, sex, age, province, city, infection_case, confirmed_date)
  
  # data type 변환
  df$confirmed_date <- as.Date(df$confirmed_date, '%Y-%m-%d')
  
  # 연령대 오류 데이터, 미기입 데이터, 100세이상 데이터 삭제
  df <- df[-(which(df$age == '66s')),]
  df <- df[-(which(df$age == '')),]
  df <- df[-(which(df$age == '100s')),]
  
  # 성별 미기입 row 삭제
  df <- df[-(which(df$sex == '')),]
  # 전염사유 미기입 row 'etc'로 변경
  df$infection_case[df$infection_case == ''] <- 'etc' 
  
  
  df$sex_num[df$sex == 'male'] <- 1
  df$sex_num[df$sex == 'female'] <- 2
  df$age_num <- gsub('s','', df$age)
  df$age_num <- as.numeric(df$age_num)/10
  unique(df$age_num)
  cities <- unique(df$city)
  num <- 1000
  for (i in cities){
    df$city_num[df$city == i] <- num
    num= num+1
  }
  
  # patient route data load
  route <- read.csv('./dataset/PatientRoute.csv')
  str(route)
  
  route <- route[,-2]
  # data type 변환
  route$date <- as.Date(route$date, '%Y-%m-%d')
  summary(route)
  
  # 서울과 경상북도만 추출
  seoul <- df[df$province == 'Seoul',]
  gyeongbuk <- df[df$province == 'Gyeongsangbuk-do',]
  
  # data merge
  seoul_df <- merge(seoul, route, by='patient_id')
  summary(seoul_df)
  View(seoul_df)
  ```

<br/>

## Visualization

- ggplot2

  - Infection case Frequancy(감염경로 빈도)

    - Source

      ```R
      library(ggplot2)
      
      unique(df$infection_case)
      infection_case <- count(df$infection_case)
      infection_case <- infection_case[(infection_case$freq > 50) & (infection_case$x != 'etc'),]
      #                       x freq
      # 4  contact with patient  743
      # 9   Guro-gu Call Center  112
      # 17      overseas inflow  293
      # 21   Shincheonji Church   99
      barplot(infection_case$freq, names.arg=infection_case$x, space=1.2, cex.names=0.9)
      ```

      ![image](https://user-images.githubusercontent.com/52812181/78283958-932d9a80-7559-11ea-8d33-e0e46790dd17.png)

  - 확진자 route 빈도

    - Source

      ```R
      ggplot(data=seoul_df, mapping= aes(date, type)) + geom_count()
      ```

      ![image](https://user-images.githubusercontent.com/52812181/78284098-d25beb80-7559-11ea-9e26-7317a95918a5.png)

  - 서울 연령별 확진일 빈도

    - Source

      ```R
      ggplot(data=seoul, mapping= aes(confirmed_date, age)) + geom_count()
      ```

      ![image](https://user-images.githubusercontent.com/52812181/78284230-0f27e280-755a-11ea-9977-96222ca09e2c.png)

  - 경상북도 연령별 확진일 빈도

    - Source

      ```R
      ggplot(data=gyeongbuk, mapping= aes(confirmed_date, age)) + geom_count()
      ```

      ![image](https://user-images.githubusercontent.com/52812181/78284289-2666d000-755a-11ea-8727-785956018c59.png)

- ggmap

  - 전국 확진자 이동 포인트 지도에 표시

  - Source

    ```R
    library(ggmap)
    
    register_google(key = "your_key")
    
    map <- get_map(location='korea', zoom=7, maptype = 'roadmap', source='google')
    g <- ggmap(map)
    g <- g + geom_point(data=seoul_df, aes(x=longitude, y=latitude), size=1, colour='red')
    print(g)
    ```

    ![image](https://user-images.githubusercontent.com/52812181/78284726-cc1a3f00-755a-11ea-9017-08ebf4021eff.png)

<br/>

## Analysis

- 로지스틱 회귀

  - 지역과 성별,연령대의 상관관계 분석

  - Source

    ```R
    condate.lm <- glm(city_num ~ sex_num + age_num, data= df)
    summary(condate.lm)
    # Call:
    #   glm(formula = city_num ~ sex_num + age_num, data = df)
    # 
    # Deviance Residuals: 
    #   Min      1Q  Median      3Q     Max  
    # -84.19  -34.76   14.12   34.72   73.49  
    # 
    # Coefficients:
    #   Estimate Std. Error t value Pr(>|t|)    
    # (Intercept) 1059.9024     2.8255 375.118  < 2e-16 ***
    #   sex_num        3.6108     1.5419   2.342   0.0193 *  
    #   age_num        2.4381     0.3799   6.418 1.62e-10 ***
    #   ---
    #   Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    # 
    # (Dispersion parameter for gaussian family taken to be 1594.637)
    # 
    # Null deviance: 4467446  on 2753  degrees of freedom
    # Residual deviance: 4386847  on 2751  degrees of freedom
    # AIC: 28130
    # 
    # Number of Fisher Scoring iterations: 2
    ```

<br/>

## Twitter Crawl & Word cloud

- 검색어 : 코로나

- Source

  ```R
  library(twitteR)
  library(wordcloud)
  library(KoNLP)
  
  consumerKey <- "your_consumer_key"
  consumerSecret <- "your_consumer_secret"
  accessToken <- "your_access_token"
  accessTokenSecret <- "your_access_token_secret"
  setup_twitter_oauth(consumerKey, consumerSecret, accessToken, accessTokenSecret)
  
  keyword <- enc2utf8('코로나')
  corona <- searchTwitter(keyword, n=5000, lang='ko')
  length(corona)
  head(corona)
  
  corona_twitter.df <- twListToDF(corona)
  corona_twitter.text <- corona_twitter.df$text
  
  options(max.print = 10000)
  # 특수문자 제거
  tw_txt <- gsub('!@#$%^&*\\(\\)~`.,','',corona_twitter.text)
  # 한글만 추출
  tw_txt <- gsub('[^가-힣]',' ', tw_txt)
  # whitespace 처리
  tw_txt <- gsub('  ', '', tw_txt)
  
  useSejongDic()
  
  # 명사 추출
  nouns <- sapply(tw_txt,extractNoun,USE.NAMES=F)
  nouns <- unlist(nouns)
  
  # 3글자 이상 데이터만 추출
  noun <- list()
  for (i in nouns){
    if(nchar(i) >= 3){
      noun <- append(noun, i)
    }
  }
  noun <- unlist(noun)
  # 명사 빈도 table
  wordcount <- table(noun)
  
  # 워드클라우드
  wordcloud2(wordcount, shape='circle', size=0.5, backgroundColor = 'white')
  ```

  ![image](https://user-images.githubusercontent.com/52812181/78286998-06d0a700-755c-11ea-828f-fe4b307729a1.png)



