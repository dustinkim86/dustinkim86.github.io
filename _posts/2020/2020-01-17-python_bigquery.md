---
layout: post
title: Google Bigquery & pymysql
mathjax: true
tags:
  - python
---

<br/>


## Google Bigquery
- DB의 위치에 제한적이지 않은 GCP의 Bigquery를 이용한 python DB활용을 해 보고자 함

- 준비 단계
  1. 가상 환경 생성
      - 나의 경우엔 굳이 가상 환경까지 이용할 필요가 없어 적용하지 않았으나, 필요하다면 하단 링크 참조  
        [Google Cloud virtualenv](https://cloud.google.com/python/setup?hl=ko)
  
  2. 패키지 설치
      - ```pip install google-cloud-bigquery```
  
  3. GCP 서비스 계정 키 만들기
      1. GCP Console에서 서비스 계정 키 만들기 페이지로 이동
      2. 서비스 계정 목록에서 새 서비스 계정을 선택
      3. 서비스 계정 이름 필드에 이름을 입력
      4. 역할 목록에서 프로젝트 > 소유자를 선택
      5. 만들기를 클릭, 컴퓨터에 키가 포함된 JSON파일 다운로드 됨
  
  4. 환경 변수 설정하요 사용자 인증 정보 제공
      - Windows PowerShell
        - ```$env:GOOGLE_APPLICATION_CREDENTIALS="[PATH]"```
        - ```set GOOGLE_APPLICATION_CREDENTIALS=[PATH]```
          [PATH] == 다운로드 된 JSON파일의 경로 : 예시) ```D:/bigquerytest-c642.json```

- Python 에서 Bigquery 시작하기
  ```python
  from google.cloud import bigquery
  client = bigquery.Client.from_service_account_json('[PATH]', project='[GCP Project Name]')
  
  QUERY = ('SELECT * FROM `[GCP Project Name]` LIMIT 1000')
  query_job = client.query(QUERY)
  rows = query_job.result()
  
  for row in rows:
    print(row)
  ```
  - 출력 결과
  ```python
  Row(('이순신', 40), {'name': 0, 'age': 1})
  Row(('홍길동', 30), {'name': 0, 'age': 1})
  Row(('유관순', 28), {'name': 0, 'age': 1})
  ```
  - data row 추가
  ```python
  QUERY = ('INSERT INTO `[GCP Project Name]` VALUES ("유재석", 48)')
  query_job = client.query(QUERY)  # API request
  rows = query_job.result()
  ```
  - 다시 select로 출력한 결과
  ```python
  Row(('이순신', 40), {'name': 0, 'age': 1})
  Row(('홍길동', 30), {'name': 0, 'age': 1})
  Row(('유재석', 48), {'name': 0, 'age': 1})
  Row(('유관순', 28), {'name': 0, 'age': 1})
  ```

**구글 클라우드 Guide** [사이트 바로가기](https://cloud.google.com/bigquery/docs/reference/libraries?hl=ko)


----------------

# pymysql
- Bigquery는 qeury 제한이 있어 대량의 데이터를 한꺼번에 업로드하기 어렵고, 속도에도 제한이 많음
- 오프라인 database 서버를 두고 이용하는 방법도 추가
- python file [바로가기](https://github.com/dustinkim86/study_to_daily/blob/master/python/Database/pymysql.py)  
  - 필요한 Library
    - pymysql(전체적인 SQL 이용)
    - pandas(데이터베이스로 쓰기 위한 csv데이터 load)
  - 순서
    1. localhost server 접속
    2. table 생성
    3. csv data load
    4. 생성한 table에 data insert
    5. insert한 database check
