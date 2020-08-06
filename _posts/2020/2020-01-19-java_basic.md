---
layout: post
title: Java Basic
mathjax: true
tags:
  - java
---

<br/>


- Eclipse 설치하기
  - *Windows*
    - Oracle은 유료화되어 zulu에서 jdk(개발자도구)와 jre(사용자도구)를 다운로드 한다.
    - [다운로드 링크](https://www.azul.com/downloads/zulu-community/?&architecture=x86-64-bit&package=jdk)
    - ![image](https://user-images.githubusercontent.com/52812181/66741687-04e13180-eeb1-11e9-9910-c4a4482f2220.png)
    - 자바의 버전은 안정적으로 11로 선택하며, 운영체제는 당연히 윈도우, bit는 요즘은 기본적으로 64비트이나 32비트일 경우 변경
    - ![image](https://user-images.githubusercontent.com/52812181/66741836-54bff880-eeb1-11e9-8f64-9422a6a7e580.png)
    - zip파일 다운로드, JDK를 JRE로 변경하고 zip파일 다운로드
    - (default)C드라이브의 ProgramFiles에 java 폴더를 생성하고 다운받은 파일들의 압축을 풀어준다.
    - [이클립스(Eclipse) 다운로드](https://www.eclipse.org/downloads/) 를 한다.
    - 이클립스를 설치할 때 설정해 줄 경로는 방금 전 압축을 풀어 준 폴더의 jre폴더의 exe파일로 연결
  - *Linux - Ubuntu*
    - 터미널 진입
    - sudo apt-get update 를 하여 최신화
    - sudo apt-get install default-jdk (개발자도구 포함, jre만을 원한다면 jdk 대신 jre 입력)
    - 우분투 소프트웨어에서 eclipse 검색하여 설치
- 숫자(정수와 실수)
  - 아래의 코드 참고
  ```java
  public class Number {
    public static void main(String[] args) {
      System.out.println(1+2); // 정수(int type)
      System.out.println(2*5); // 정수(int type)
      System.out.println(2.3-0.8); // 실수(double type)
    }
  }
  ```
  출력 결과
  ```java
  3
  10
  1.5
  ```
- 문자와 문자열
  - 아래의 코드 참고
  ```java
  public class Number {
    public static void main(String[] args) {
      System.out.println('홍'); // 문자(char type) - 한 글자일때만 사용할 수 있는 ' '
      System.out.println("홍길동"); // 문자열(String) - 한 글자 이상이면 모두 가능
      System.out.println("홍길" + "동이"); // 문자의 연산
    }
  }
  ```
  출력 결과
  ```java
  홍
  홍길동
  홍길동이
  ```
  
  

- 입력
  - 가장 먼저 main 함수 선언하기 전에 ```import java.util.Scanner;``` 입력하여 Scanner 객체를 활성화
  - ```Scanner sc = new Scanner(System.in);``` 으로 객체 생성
- 연산자(산술/증감/비교/논리/조건)
  - 산술연산자 ```+(덧셈)```, ```-(뺄셈)```, ```*(곱셈)```, ```/(나눗셈)```, ```%(몫)```
  - 증감연산자 ```++(1씩 증가)```, ```--(1씩 감소)```
  - 비교연산자 ```a<b(a보다 b가 큼)```, ```a<=b(a보다 b가 크거나 같음)```, ```a==b(a와 b가 같음)```, ```a!=b(a와 b가 같지 않음)```
  - 논리연산자 ```&&(and)```, ```||(or)```, ```!(not)```, ```^(xor)```
  - 조건연산자 ```(x>y)?1:-1; x가 y보다 크다면 1, 작다면 -1```
- static : 객체를 생성하지 않아도 사용할 수 있음
- if / switch문
  - if문 : 조건에 따라 else if, else는 생략되어도 무방
  ```java
  if (조건식) {
      ..실행문..
      }
  ```
  ```java
  if (조건식) {
      ..실행문..
      }
  else if (조건식) {
      ..실행문..
      }
  else {
      ..실행문..
      }
  ```
  - *예시*

  ```java
  import java.util.Scanner;

  public class Square {
    public static void main(String[] args) {
      Scanner sc= new Scanner(System.in);
      // 변수 선언
      int width, height;

      System.out.print("가로길이 :");
      width= sc.nextInt();
      System.out.print("세로길이 :");
      height= sc.nextInt();

      int area = (width*height);
      System.out.println("넓이는 "+area);

      sc.close();
    }
  }
  ```

  - switch문
  ```java
  switch (식) {
    case 값1:
      실행문 1;
      break;
    case 값2:
      실행문 2;
      break;
    ...
    default:
      실행문 n;
  }
  ```
  - *예시*

  ```java
  import java.util.Scanner;

  public class Gangi {
    public static void main(String[] args) {
      Scanner sc= new Scanner(System.in);
      System.out.print("정수 입력 :");
      int num = sc.nextInt();
      String gangi;
      switch(num%12) {
        case 1:
          gangi= "닭";
          break;
        case 2:
          gangi= "개";
          break;
        case 3:
          gangi= "돼지";
          break;
        case 4:
          gangi= "쥐";
          break;
        case 5:
          gangi= "소";
          break;
        case 6:
          gangi= "호랑이";
          break;
        case 7:
          gangi= "토끼";
          break;
        case 8:
          gangi= "용";
          break;
        case 9:
          gangi= "뱀";
          break;
        case 10:
          gangi= "말";
          break;
        case 11:
          gangi= "양";
          break;
        default:
          gangi= "원숭이";

      } // switch end

      System.out.println(num+"년생 당신은 "+ gangi +"띠 입니다.");
      sc.close();
    }
  }
  ```


- for문/while문/do-while문
  - for문 순서도
    ![그림1](https://user-images.githubusercontent.com/52812181/67145077-f2079c00-f2b8-11e9-8054-b1746a97ae18.png)
  - for문 예제
    - 1부터 10까지의 합
      ```java
      public class ForSample {
        public static void main(String[] args) {
          int sum=0;

          for(int i=1; i<=10; i++) { // 1~10까지 반복
            sum += i;
            System.out.print(i); // 더하는 수 출력

            if(i<=9) // 1~9까지는 '+' 출력
              System.out.print("+");
            else { // i가 10인 경우 
              System.out.print("="); // '=' 출력하고
              System.out.print(sum); // 덧셈 결과 출력
            }
          }
        }
      }
      ```
      출력결과 : ```1+2+3+4+5+6+7+8+9+10=55```
      
      
  - while문 순서도
    ![그림2](https://user-images.githubusercontent.com/52812181/67145128-9ab5fb80-f2b9-11e9-8572-f817b4a6a686.png)
  - while문 예제
    - -1이 입력될 때까지 입력된 수의 평균
      ```java
      import java.util.Scanner;
      public class WhileSample {
        public static void main(String[] args) {
          int count=0; // count는 입력된 정수의 개수
          int sum=0; // sum은 합
          Scanner scanner = new Scanner(System.in);		
          System.out.println("정수를 입력하고 마지막에 -1을 입력하세요.");

          int n = scanner.nextInt(); // 정수 입력
          while(n != -1) { // -1이 입력되면 while 문 벗어남
            sum += n;
            count++;
            n = scanner.nextInt(); // 정수 입력
          }
          if(count == 0) System.out.println("입력된 수가 없습니다.");
          else {
            System.out.print("정수의 개수는 " + count + "개이며 ");
            System.out.println("평균은 " + (double)sum/count + "입니다.");
          }
          scanner.close();
        }
      }

      ```
      출력결과
      ```java
      정수를 입력하고 마지막에 -1을 입력하세요.
      10 30 –20 40 -1
      정수의 개수는 4개이며 평균은 15.0입니다
      ```
 
  - do-while문 
    ![그림3](https://user-images.githubusercontent.com/52812181/67145157-f08aa380-f2b9-11e9-9308-437497f45340.png)
  - do-while문 예제
    - 'a'부터 'z'까지 출력
      ```java
      public class DoWhileSample {
        public static void main (String[] args) {
          char c = 'a';

          do {
            System.out.print(c);
            c = (char) (c + 1);
          } while (c <= 'z'); 	
        }
      }
      ```
      출력결과 : ```abcdefghijklmnopqrstuvwxyz```



- 2중 for문 예제
  - 구구단 출력하기(출력예시)
  ```java
  1*1=1	1*2=2	1*3=3	1*4=4	1*5=5	1*6=6	1*7=7	1*8=8	1*9=9
  2*1=2	2*2=4	2*3=6	2*4=8	2*5=10	2*6=12	2*7=14	2*8=16	2*9=18
  3*1=3	3*2=6	3*3=9	3*4=12	3*5=15	3*6=18	3*7=21	3*8=24	3*9=27
  4*1=4	4*2=8	4*3=12	4*4=16	4*5=20	4*6=24	4*7=28	4*8=32	4*9=36
  5*1=5	5*2=10	5*3=15	5*4=20	5*5=25	5*6=30	5*7=35	5*8=40	5*9=45
  6*1=6	6*2=12	6*3=18	6*4=24	6*5=30	6*6=36	6*7=42	6*8=48	6*9=54
  7*1=7	7*2=14	7*3=21	7*4=28	7*5=35	7*6=42	7*7=49	7*8=56	7*9=63
  8*1=8	8*2=16	8*3=24	8*4=32	8*5=40	8*6=48	8*7=56	8*8=64	8*9=72
  9*1=9	9*2=18	9*3=27	9*4=36	9*5=45	9*6=54	9*7=63	9*8=72	9*9=81
  ```
  ```java
  public class NestedLoop {
    public static void main(String[] args) {
      for(int i=1; i<10; i++) { // 1단에서 9단
        for(int j=1; j<10; j++) { // 각 단의 구구셈 출력
          System.out.print(i + "*" + j + "=" + i*j); // 구구셈 출력
          System.out.print('\t'); // 하나씩 탭으로 띄기
        }
        System.out.println(); // 한 단이 끝나면 다음 줄로 커서 이동
      }
    }
  }
  ```

- 컴퓨터와 대결하는 가위바위보 게임 만들기/5판3선승(computer vs user, Win three games)


  ```java
  import java.util.*;
  public class GBBGame {
    public static void main(String[] args) {
      String cheol, comStr;
      int com, com_count, cheol_count;

      Scanner sc = new Scanner(System.in);
      Random rand = new Random();
      com_count = 0;
      cheol_count = 0;

      while(true) {
        if (com_count==3 || cheol_count==3) break;

        com = rand.nextInt(2);
        if (com==0) comStr="바위";
        else if (com==1) comStr="가위";
        else comStr="보";

        System.out.print("철수 >>");
        cheol = sc.nextLine();
        cheol = cheol.trim();

        if (cheol.equals("가위")||cheol.equals("바위")||cheol.equals("보")) {
          // 처리-출력
          if(cheol.equals(comStr)) {
            System.out.println("철수="+cheol+", 컴퓨터="+comStr+"- 철수,컴퓨터 비김");
            continue;
          }
          else {
            if(cheol.equals("가위")) {
              if(comStr.equals("보")) {
                System.out.println("철수="+cheol+", 컴퓨터="+comStr+"- 철수 이김");
                cheol_count +=1;
                continue;
              }
              else {
                System.out.println("철수="+cheol+", 컴퓨터="+comStr+"- 컴퓨터 이김");
                com_count +=1;
                continue;
              }
            }
            if(cheol.equals("바위")) {
              if(comStr.equals("보")) {
                System.out.println("철수="+cheol+", 컴퓨터="+comStr+"- 컴퓨터 이김");
                com_count +=1;
                continue;
              }
              else {
                System.out.println("철수="+cheol+", 컴퓨터="+comStr+"- 철수 이김");
                cheol_count +=1;
                continue;
              }
            }
            if(cheol.equals("보")) {
              if(comStr.equals("바위")) {
                System.out.println("철수="+cheol+", 컴퓨터="+comStr+"- 철수 이김");
                cheol_count +=1;
                continue;
              }
              else {
                System.out.println("철수="+cheol+", 컴퓨터="+comStr+"- 컴퓨터 이김");
                com_count +=1;
                continue;
              }
            }
          }
        }
        else { 
          System.out.println("잘못 입력됨, 가위,바위,보 중 고르시오");
          continue;
        }


      sc.close();
      }// while end
      if (com_count==3) System.out.println("컴퓨터 최종 승리!!");
      else System.out.println("철수 최종 승리!!");
    }
  }

  ```
  출력결과
  ```java
  철수 >>바위
  철수=바위, 컴퓨터=가위- 철수 이김
  철수 >>가위
  철수=가위, 컴퓨터=가위- 철수,컴퓨터 비김
  철수 >>가위
  철수=가위, 컴퓨터=바위- 컴퓨터 이김
  철수 >>보
  철수=보, 컴퓨터=바위- 철수 이김
  철수 >>가위
  철수=가위, 컴퓨터=가위- 철수,컴퓨터 비김
  철수 >>가세
  잘못 입력됨, 가위,바위,보 중 고르시오
  철수 >>바위
  철수=바위, 컴퓨터=바위- 철수,컴퓨터 비김
  철수 >>보
  철수=보, 컴퓨터=가위- 컴퓨터 이김
  철수 >>보
  철수=보, 컴퓨터=가위- 컴퓨터 이김
  컴퓨터 최종 승리!!
  ```



- 반복문 연습문제
  1. 문자열 "******"을 아래와 같이 출력
    ```java
    ******
    ******
    ******
    ******
    ******
    ******
    ******
    ```

  2. 문자열 "*"을 아래와 같이 출력
    ```java
    ******
    ******
    ******
    ******
    ******
    ******
    ******
    ```
  
  3. 문자열 "*"을 아래와 같이 출력
    ```java
    *
    **
    ***
    ****
    *****
    ```
  
  4. 문자열 "*"을 아래와 같이 출력
    ```java
    *****
    ****
    ***
    **
    *
    ```
  
  5. 문자열 "*"을 아래와 같이 출력
    ```java
    	   *
     	 ***
     	*****
    	*******
    ```
  

- 연습문제 5번 풀이
  ```java
    String star;
    star = "*";
    for(int i=1; i<5; i++) {
    for(int j=1; j<=4-i; j++) {
      System.out.print(" ");
    }
    for(int j=1; j<=2*i-1; j++) {
      System.out.print(star);
    }System.out.println("");  
  ```


## 예외(Exception)
- 오동작이나 결과에 악영향을 미칠 수 있는 실행 중 발생한 오류

- 예외 처리
  - try ~ catch ~ finally문
    ```java
    try {
      예외가 발생할 가능성이 있는 실행문
    }
    catch (처리할 예외 타입 선언) {
      예외 처리문
    }
    finally {   // 생략 가능
      예외 발생 여부와 상관없이 무조건 실행되는 문장
    }
    ```

- 예외 처리를 이용한 Up & Down 게임 만들기
  [소스 바로가기](https://github.com/dustinkim86/Study-to-daily/tree/master/ApplicationSoftware/java/java_source)
  - 출력예시
    ```java
    up & down 할 숫자를 입력하세요 :50

    입력한 숫자(50)보다 큽니다. 범위 : 50 ~ 100, 시도횟수 : 1
    up & down 할 숫자를 입력하세요 :70

    입력한 숫자(70)보다 작습니다. 범위 : 50 ~ 70, 시도횟수 : 2
    up & down 할 숫자를 입력하세요 :60

    입력한 숫자(60)보다 작습니다. 범위 : 50 ~ 60, 시도횟수 : 3
    up & down 할 숫자를 입력하세요 :54

    입력한 숫자(54)보다 큽니다. 범위 : 54 ~ 60, 시도횟수 : 4
    up & down 할 숫자를 입력하세요 :57
    정답입니다. 게임을 종료합니다.
    ```


## 클래스와 객체

- 객체의 본질적 특징 : 외부의 접근으로부터 객체 보호
- 클래스 : 멤버 함수(method)와 멤버 변수(field)는 모두 클래스 내에서 구현
- 상속 : 상위 객체의 속성이 하위 객체에 물려지며, 하위 객체는 상위 객체늬 속성을 모두 지님
  - 상위 객체 : 수퍼 클래스
  - 하위 객체 : 서브 클래스
- 다형성
  - method overloading : 중복, 같은 이름이지만 다르게 동작
  - method overriding : 수퍼클래스의 method를 서브클래스마다 다르게 구현

- 예시) 원의 둘레와 면적을 구하는 클래스
  ```java
  public class Circle() {
    double r; // 원의 반지름을 저장 할 멤버
    double getCircum() {
      return 2*Math.PI*r;
    }
    double getArea() {
      return Math.PI*Math.pow(r, 2);
    }
    
    double getCircum2(double radius) {
      this.r = radius;
      return 2*Math.PI*r;
    }
    double getArea2(double radius) {
      this.r = radius;
      return Math.PI*Math.pow(r, 2);
    }
  }
  ```
  - 위의 클래스 내 두 개의 함수는 클래스 내에서 선언한 ```double r```을 통해 계산을 하고,  
  아래 두 개의 object는 별도로 object 호출과 동시에 radius를 선언하며 기존의 r에 radius 데이터를 입힌다.
  - 위의 클래스는 단독으로는 사용될 수 없으며 main함수 내에서 호출과 동시에 사용 되어진다.
  ```java
  public class CircleMain {
    public static void main(String[] args) {
      Circle cc = new Circle(); // Circle 함수 호출 및 생성
      cc.r = 11; // Circle 함수 내 double r에 데이터 저장
      System.out.printf("둘레 : %.4f", cc.getCircum()); System.out.println("");
      System.out.printf("넓이 : %.4f", cc.getArea()); System.out.println("");
      System.out.printf("둘레 : %.4f", cc.getCircum2(4)); System.out.println("");
      System.out.printf("넓이 : %.4f", cc.getArea2(4)); System.out.println("");
    }
  }
  ```
  출력 결과
  ```java
  둘레 : 37.6991
  넓이 : 113.0973
  둘레 : 25.1327
  넓이 : 50.2655
  ```
- 클래스 생성 시 무조건 매개변수가 있는 생성자 1개 이상을 생성해야 한다.


## 상속


- 부모클래스에 만들어진 필드 & 메소드를 자식클래스가 물려 받음
- 상속을 통한 자식클래스의 간결화
- 상속 선언
  ```java
  public class Person {
  ...
  }
  public class Student extends Person { // Person을 상속받는 클래스 Student 선언
  ...
  }
  public class StudentWorker extends Student { // Student를 상속받는 StudentWorker 선언
  ...
  }
  ```
- 부모클래스 -> 수퍼클래스(super class) / 자식클래스 -> 서브클래스(sub class)

- 객체 생성
  ![상속_객체생성](https://user-images.githubusercontent.com/52812181/67442708-ec091680-f63c-11e9-8b82-84c386450f49.png)

- 서브클래스에서 수퍼클래스의 멤버 접근
  ![서브에서 수퍼멤버 접근](https://user-images.githubusercontent.com/52812181/67442761-165ad400-f63d-11e9-921c-0bc9a1d0a57a.png)

- 상속의 특징
  - 다중 상속 지원 X
  - 상속 횟수 제한 X
- 접근 지정자 : public, protected, default, private
- 수퍼클래스 멤버
  - private : 다른 모든 클래스에 접근 불허, 클래스 내의 멤버에게만 허용
  - default : 패키지 내 모든 클래스에 접근 허용
  - public : 다른 모든 클래스에 접근 허용
  - protected : 패키지 내 모든 클래스에 접근 허용, 다른 패키지에 있어도 서브클래스는 수퍼클래스의 protected멤버 접근 가능
  ![서브클래스의 수퍼클래스 멤버 접근](https://user-images.githubusercontent.com/52812181/67442936-b284db00-f63d-11e9-82bb-f4a13de30274.png)


### 클래스
- 생성 객체(필드, 메소드) : 메모리에 공간 확보
  - 멤버(필드)
  - 생성자 : 오버로딩으로 생성자를 여러 개 정의(reason. 멤버에 초기값을 주고자 할 때)
  - 메소드 : getter/setter 메소드, 오버로딩(중복) 가능


### 상속
- 메소드 오버라이딩(치환)


### 예제) 은행 계좌 관리
**source folder에 소스 제공**
- 키보드로 계좌 정보를 입력 받아, 계좌를 관리하는 프로그램
- Account, BankApplication 클래스 작성
- Package 설정 : app.bank
- 계좌는 최대 100개
- Account.java
  - 계좌번호, 예금주, 잔고 필드(은닉)
  - setter, getter 이용
  
- BankApplication.java
  - Account 객체 배열
  - Main 메소드 : 메뉴 출력 -> 메소드 호출
  - 계좌생성 메소드
  - 계좌목록 메소드
  - 예금/출금 메소드
  
- 출력 형태
  ![image](https://user-images.githubusercontent.com/52812181/67492938-23fe7100-f6b2-11e9-801b-a58d9d83b28c.png)
