---
layout: post
title: HTML
subtitle : HTML Basic
tags: [HTML]
author: dustinkim86
comments : True
---


#### 용어 정리
- 하이퍼링크 : 인터넷에서 문서 사이를 쉽게 이동할 수 있는 기능
- 플러그인 : 사용자pc에 프로그램을 추가로 설치해 기능을 확장하는 방법
- release note : 소프트웨어 제품과 함께 배포되는 문서
- front end : 눈에 보이는 기술(ex. HTML, CSS, JS 등)
- back end : 눈에 보이지 않는 기술(ex. JSP, PHP 등)
- IoT(Internet of Things) : 물건이 인터넷이 되는 것 => 사물에 인터넷을 활용할 수 있는 칩이 들어가 데이터 축적
- MVS : Modeler(기획자), Viewer(디자이너), Controller(프로그래머)
- 웹은 요청과 응답이라는 간단한 형태로 동작(client(요청자)와 server(제공자))
- Module : 목적에 맞게 개발을 쉽게 할 수 있도록 여러 기능을 모아둔 것
- Framework(뼈대) : 프레임워크를 사용하면 빠르고 쉽게 만들어 낼 수 있음
- 반응형 홈페이지 : 기기 사이즈에 따라서 사이즈가 자동으로 조절되는 것(meta : 웹 페이지에 추가 정보 전달)
- API(Application programming interface) : 앱을 만들기(개발) 위한 문법 참고서
- HTML은 태그, 요소(js에서는 객체), 속성을 이해하면 된다.
  - 요소 : html페이지를 구성하는 각 부품
    - 내용을 가질 수 있는 요소 : ```<h1>```, ```<p>```, ```<audio>``` 등
    - 내용을 가질 수 없는 요소 : ```<img>```, ```<br>```, ```<hr>``` 등
  - 태그 : 요소를 만들 때 사용하는 작성 방법
  - 속성 : 태그에 추가정보를 부여할 때 사용하는 것(src, title 등)



- **HTML 기본 틀**
  ```xml
  <!DOCTYPE html> (doc 문서, type 유형, html hypertext markup language)
  <html> (html 문법으로 쌍 태그 영역에 사용)
  <title>HTML Tutorial</title>
  <body>
    <h1>This is a heading</h1>
    <p>This is a paragraph.</p>
  </body>
  </html>
  ```
- **주석** : 프로그램 실행에는 영향이 없고, 프로그래머가 나중을 위해(다른 사람을 위해) 남겨놓은 설명
  - 주석 단축키 : ctrl + /
  
    ```<!-- -->```
- **더미텍스트(dummy text) 만들어주는 사이트** : Lorem ipsum (영문판과 한글판 홈페이지가 따로 존재)
- **vscode 유용한 플러그인(plug-in)**
  - open in browser : 브라우저를 따로 열지 않고 IDE에서 바로 결과물을 띄우는 것
  - dracula official : 컬러 테마
  - Material Icon Theme : 아이콘 테마
  - 그 밖의 유용한 플러그인 안내 : [출처](https://junhobaik.github.io/vsc-plugin-list/)



- **태그 약어 정리**
  - ```<h>``` tag : heading, 제목
  - ```<p>``` tag : paragraph, 단락
  - ```<br>``` tag : line break, 강제 줄 바꿈
  - ```<hr>``` tag : horizontal rule, 수평 줄 생성
  - ```<a>``` tag : anchor, 다른 웹 페이지나 웹 페이지 내부의 특정 위치로 이동
  - ```<href>``` tag : hyper reference, 웹 페이지나 파일의 위치를 나타내는 경로
  - 글자 태그
    - ```<b>``` tag : bold, 굵은 글자
    - ```<i>``` tag : italic, 기울어진 글자
    - ```<sub>``` tag : subscript, 아래 첨자
    - ```<sup>``` tag : superscript, 위 첨자
    - ```<ins>``` tag : insert, 밑줄 글자
    - ```<del>``` tag : delete, 취소선이 그어진 글자
  - 특수문자 표기
    - ```&nbsp;``` : 공백 (non-breaking space)
    - ```&lt;``` : < (less than)
    - ```&gt;``` : > (greater than)
    - ```&amp;``` : & (ampersand)
  - id를 대체하는 기호 : #(해시) - 중복 불가능
  - class를 대체하는 기호 : .(닷) - 중복 가능
  - 목록 태그
    - ```<ul>``` tag : 순서가 없는 목록(unordered list)
    - ```<ol>``` tag : 순서가 있는 목록(ordered list)
    - ```<li>``` tag : 목록 요소(list item)
  - 표 태그
    - ```<tr>``` tag : table row, 표에 행 삽입
    - ```<th>``` tag : table heading, 표의 제목 셀 생성
    - ```<td>``` tag : table data, 표의 일반 셀 생성
  - 이미지의 대체 문자를 지정할 때 사용하는 속성 : alt
- **CSS 이용하기**
  - 소스에 컬러 등의 디자인적인 요소를 입히는 방법에는 두 가지가 있는데, 소스에 직접 코딩하는 방법과 CSS 파일을 따로 생성하여 불러오는 방법이 있다. 그 중 두 번째 방법을 이용하는 것이 코드도 깔끔하고 스타일시트만 한 눈에 볼 수 있어 주로 이용하는 방법이다.
    1. 아래와 같이 head부분의 link에 불러 올 css파일의 링크를 건다.

      ```xml
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <title>HTML Basic</title>
          <link rel = "stylesheet" href = "Style.css">
      </head>
      <body>
          <h1>Hello world~!</h1>
          <p> 홍길동 </p>
      </body>
      </html>
      ```
    2. css 파일을 생성하고 속성을 부여 할 태그에 스타일을 정의한다.

      ```xml
      th{
          background: #E6E6E6;
      }
      ```
- **Javascript 이용하기**
  - 대세 언어 중 하나, 객체 기반의 언어이다. 이를 이용하여 자동화 등도 비교적 쉽게 구현할 수 있다. 아래에서는 html 구문에서 알림창을 뜨게 하는 소스를 자바스크립트로 구현하였다.
    1. 아래와 같이 head부분의 script에 불러 올 javascript파일의 링크를 건다.

      ```xml
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <title>Document</title>
          <script src="OuterJavaScript.js"> // 외부 자바스크립트 호출
              // 경고창
              // alert('hello javascript ~_~');  내부 자바스크립트 호출

          </script>
      </head>
      <body>

      </body>
      </html>
      ```
    2. javascript 파일을 생성하고 html에서 불러 올 소스를 작성한다.
      ```alert('OuterScript');```



- **시맨틱 태그** : 시맨틱(Semantic)은 "의미의, 의미론적인"이라는 뜻
  ![시맨틱태그](https://user-images.githubusercontent.com/52812181/67070993-6577b380-f1bc-11e9-9144-75c94916cf1c.gif)

- **border-collapse** : 표(table)의 테두리와 셀(td)의 테두리 사이의 간격을 어떻게 처리할 지 정하는 함수

  ```border-collapse: separate | collapse | initial | inherit```
  - separate : table(표)의 테두리와 셀(td)의 테두리 사이에 간력을 둡니다
  - collapse : table의 테두리와 셀의 테두리 사이의 간격을 없앱니다. 겹치는 부분은 한 줄로 나타냅니다.
  - initial : 기본값으로 설정합니다.
  - inherit : 부모 요소의 속성값을 상속받습니다.
  
  속성값이 separate일 때, 간격의 크기는 border-spacing 속성으로 정합니다.


  - password : 비밀번호 입력 양식 생성
  - radio : 라디오 버튼 생성(중복 선택 불가)
  - reset : 초기화 버튼 생성
  - submit : 제출 버튼 생성
  
- 선택 가능한 입력 양식
  ```xml
  <body>
    <select>
      <option>김밥</option>
      <option>떡볶이</option>
      <option>순대</option>
    </select>
  </body>
  ```
- 여러 항목 선택을 하고 싶다면, select부분에 ```multiple="multiple"```을 입력 해 준다.
- 선택 옵션 묶기
  ```xml
  <body>
    <select>
      <optgroup label="Group1">
        <option>group1-A</option>
        <option>group1-B</option>
      </optgroup>
    </select>
  </body>
  ```

- 연관 있는 입력 양식 그룹으로 묶기
  ```xml
  <body>
    <form>
      <fieldset>
        <legend>입력 양식</legend>
        <table>
          <tr>
            <td><label for="name">이름</label></td>
            <td><input id="name" type="text"></td>
          </tr>
        </table>
      </fieldset>
    </form>
  </body>
  ```

- textarea : 왼쪽 들여쓰기까지 모두 출력이 되기 때문에 주의해서 사용
  ```xml
  <body>
    <textarea>
  Textarea 태그
  Textarea 태그
    </textarea>
  </body>
  ```

- 공간 분할 태그
  - div : 블록 형식으로 공간 분할(row 추가방식)
  - span : 인라인 형식으로 공간 분할(col 추가방식)
