---
layout: post
title: Swift Basic(1)
mathjax: true
tags:
  - swift
---

<br/>

### 변수와 상수

- 변수

    - 한 번 정해져도 다른 데이터로 대체할 수 있는 형식

    - `var`로 선언

        - 기본적인 선언 방식 : `var 이름: 타입 = 값`
        - 값의 타입이 명확하다면 타입은 생략 가능
        
        ```swift
        var myVariable = 42
        myVariable = 50
        ```

- 상수

    - 한 번 정해지면 불변하여 다른 데이터로 대체할 수 없는 형식

    -  `let`으로 선언

        - 기본적인 선언 방식 : `let 이름: 타입 = 값`
        - 값의 타입이 명확하다면 타입은 생략 가능
        
        ```swift
        let myConstant = 42
        myConstant = 50 // 에러 발생
        ```

- 값의 타입이 명확하지 않은 타입 예시

    ```swift
    let explicitDouble: Double = 70 // Double type
    ```

- 명시적 값 변환

    - 숫자를 문자로 인식하게 하는 등의 명시적인 값의 변환은 변환하려는 type을 앞에 붙여 사용할 수 있다

    ```swift
    let label = "The width is "
    let width = 94
    let widthLabel = label + String(width)
    ```

- 문자열 안에 다른 속성의 값을 표기하는 방법은 또 있다 (뿐만 아니라 사칙연산도 가능하다)

    ```swift
    let apples = 3
    let oranges = 5
    let appleSummary = "I have \(apples) apples."
    let fruitSummary = "I have \(apples + oranges) pieces of fruit."
    ```

    - 여러 줄의 문장을 적을 땐 `"""`를 사용한다

        ```swift
        let quotation = """
        I said "I have \(apples) apples."
        And then I said "I have \(apples + oranges) pieces of fruit."
        """
        ```

<br/>

### 기본 데이터 타입

- Bool : true, false
- Int(정수), UInt(양의 정수) : 둘 다 64비트
- Float(32비트 실수), Double(64비트 실수)
- Character(문자), String(문자열) : 유니코드 사용, 큰따옴표 사용

<br/>

### Any, AnyObject, nil

- Any : 스위프트의 모든 타입을 지칭하는 키워드
- AnyObject : 모든 클래스 타입을 지칭하는 프로토콜
- nil : 없음을 의미하는 키워드

```swift
var someAny: Any = 100
someAny = "어떤 타입도 수용 가능"
someAny = 123.12
let someDouble: Double = someAny // 에러 발생

class SomeClass {}
var someAnyObject: AnyObject = SomeClass()
someAnyObject = 123.12 // 에러 발생

someAny = nil // 에러 발생, 어떤 데이터 타입도 들어갈 수 있지만 아무런 값을 할당하지 않을 수 는 없다
```

<br/>

### 컬렉션(Collection) - 배열(Array)과 사전(Dictionary), 세트(Set)

- 배열(Array)

    - 순서가 있는 리스트 컬렉션

    - 빈 Array 생성 : 아래와 같이 여러 형태로 선언이 가능하다

        ```swift
        var integers: Array<Int> = Array<Int>()
        var doubles: Array<Double> = [Double]()
        var strings: [String] = [String]()
        var characters: [Character] = []
        ```

    - 하나의 변수(또는 상수)에 여러 개의 데이터를 묶어서 저장하는 형태

    - `[ ]`의 형태로 선언하며 Python의 List와 상당히 비슷하다(인덱스 접근도 똑같다)

    ```swift
    var shoppingList = ["catfish", "water", "tulips"]
    shoppingList[1] = "bottle of water" // 데이터 변경
    shoppingList.append("blue paint") // 값 추가
    shoppingList.contains("catfish") // 값을 가지고 있는지 확인(true, false로 반환)
    shoppingList.count // 값의 갯수
    shoppingList.remove(at: 1) // 해당 인덱스의 값 삭제
    shoppingList.removeLast() // 마지막 값 삭제
    shoppingList.removeFirst() // 첫 번째 값 삭제
    shoppingList.removeAll()
    shoppingList.append("blue paint")
    shoppingList = [] // 초기화(선언은 되어 있어야 함)
    ```

- 사전(Dictionary)

    - 한 쌍의 Key와 Value로 이루어진 데이터의 형태

    - 빈 Dictionary 생성 : 아래와 같이 여러 형태로 선언이 가능하다

        ```swift
        var anyDictionary: Dictionary<String, Any> = [String: Any]()
        let emptyDictionary1: [String: String] = [:]
        let emptyDictionary2 = [String: Float]()
        ```

    - 배열과 마찬가지로 `[ ]`의형태로 사용하지만 키와 값을 동시에 명시해 주어야 함

    ```swift
    var occupations = [
    	"Malcolm": "Captain",
    	"Keylee": "Mechanic",
    ]
    occupations["Jayne"] = "Public Relations" // 데이터 추가
    occupations["Malcolm"] = "Barista" // 값 재할당
    occupations.removeValue(forKey: "Keylee") // 해당하는 키와 값을 삭제
    occupations["Jayne"] = nil // 위와 같이 해당하는 키와 값을 삭제
    occupations = [:] // 초기화(선언은 되어 있어야 함)
    ```


- 세트(Set)

    - 순서가 없고, 멤버(값)이 유일한 컬렉션

    - 동일한 값을 여러 개 넣어도 한 개만 보존하고 나머지는 버린다

    - 빈 Set 생성 : 아래와 같이 여러 형태로 선언이 가능하다

        ```swift
        var integerSet1: Set<Int> = Set<Int>()
        var integerSet2: Set<Int> = []
        ```

    - 사용 예시

    ```swift
    var integerSet: Set<Int> = []
    integerSet.insert(1) // 값 추가
    integerSet.insert(99)
    integerSet.contains(1) // 값을 가지고 있는지 확인(true, false로 반환)
    integerSet.remove(99) // 값 삭제
    integerSet.count // 데이터 갯수
    ```

<br/>

### 집합과 정렬

- 간단히 예시로 어떻게 사용되는 지에 대해서만 보자

    ```swift
    let setA: Set<Int> = [1,2,3,4,5]
    let setB: Set<Int> = [3,4,4,5,6]
    let union = setA.union(setB) // 합집합
    let sortedUnion = union.sorted() // 정렬
    let intersection = setA.intersection(setB) // 교집합
    let subtracting = setA.subtracting(setB) // 차집합
    ```

<br/>

### 조건문과 반복문

- 조건문
    - 조건문의 조건은 반드시 true, false로 반환되어야 하며 소괄호는 생략 가능(단, 중괄호는 필수)
    - 사용 형태
        - if - else 구문

            ```swift
            if 조건 {
              /* 실행 구문 */
            } else if 조건 {
              /* 실행 구문 */
            } else {
              /* 실행 구문 */
            }
            ```

        - switch - case - default 구문

            - 다른 언어에 비해 사용이 광범위 함
            - 정수타입의 값만 비교하는 것이 아닌 대부분의 스위프트 기본 타입을 지원하며, 다양한 패턴과 응용이 가능
            - 명시적으로 break를 적어주지 않아도 한 개의 case당 자동으로 걸림

            ```swift
            switch 비교값 {
            case 패턴:
              /* 실행 구문 */
            default:
              /* 실행 구문 */
            }
              
            // 예시
            switch someInteger {
            case 0:
              print("Zero")
            case 1..<100: // 범위 연산자, 1이상 100미만
              print("1~99")
            case 100:
              print("100")
            case 101...Int.max: // 범위 연산자, 101이상 Int.max이하
              print("over 100")
            default:
              print("Unknown")
            }
              
            switch "hoya" {
            case "jake", "mina": // 한 개의 케이스에 속하게 만들기 위해 두 개를 동시에 적음
              print("jake n mina")
              fallthrough // break 하지 않고 아래의 케이스까지 진행되게 만들어 줌
            case "hoya":
              print("hoya!")
            default:
              print("unknown")
            }
            
            // 예시2
            let vegetable = "red pepper"
            switch vegetable {
            case "celery":
                print("Add some rainsins and make ants on a log.")
            case "cucumber", "watercress":
                print("That would make a good tea sandwich.")
            case let x where x.hasSuffix("pepper"):
                print("Is it a spicy \(x)?")
            default:
                print("Everything tastes good in soup.")
            } // Is it a spicy red pepper?
            ```

            

- 반복문
    - 명시한 반복 횟수(또는 데이터의 양)만큼 구문 안에서 같은 작업을 반복함

    - 조건문과 같이 소괄호는 생략 가능하나 중괄호는 필수

    - 사용 형태

        - for - in 사용 (dictionary의 경우 이터레이션 아이템으로 튜플이 들어옴)

            ```swift
            for item in items {
              /* 실행 구문 */
            }
              
            // 예시
            var instegers = [1,2,3]
            let people = ["hoya": 10, "eric": 15, "mike": 12]
              
            for integer in integers {
              print(integer)
            }
            for (key, value) in people {
              print(\(key): \(value))
            }
            
            // 예시2
            let interestingNumbers = [
                "Prime": [2, 3, 5, 7, 11, 13],
                "FIbonacci": [1, 1, 2, 3, 5, 8],
                "Square": [1, 4, 9, 16, 25],
            ]
            var largest = 0
            for (kind, numbers) in interestingNumbers {
                for number in numbers {
                    if number > largest {
                        largest = number
                    }
                }
            }
            print(largest) // 25
            ```

        - while 사용

            ```swift
            while 조건 {
              /* 실행 구문 */
            }
              
            // 예시
            while integers.count > 1 {
              integers.removeLast()
            }
            // 예시2
            var n = 2
            while n < 100 {
                n *= 2
            }
            print(n) // 128
            ```

        - repeat - while 사용 (do-while 구문와 형태와 동작이 유사함)

            ```swift
            repeat {
              /* 실행 구문 */
            } while 조건
              
            // 예시
            repeat {
              integers.removeLast()
            } while integers.count > 0
            // 예시2
            var m = 2
            repeat {
                m *= 2
            } while m < 100
            print(m) // 128
            ```

- 조건문과 반복문을 사용한 예시

```swift
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
for score in individualSocres {
	if score > 50 {
  	teamScore += 3
	} else {
		teamScore += 1
	}
}
print(teamScore) // 11
```

- 범위 연산자(`..<` & `...`)

    - 반복문을 사용할 때(혹은 더 있을 수 있는 상황에서) 범위 연산자를 이용해 범위를 지정할 수 있다

    ```swift
    var total = 0
    for i in 0..<4 { // 0 이상 4 미만
    	total += i
    }
    print(total) // 6
    
    total = 0
    for i in 0...4 { // 0 이상 4 이하
        total += i
    }
    print(total) // 10
    ```

<br/>

### 옵셔널(optional)

- 값이 있을 수도, 없을 수도 있음을 표현
- nil이 할당될 수 있는지, 없는지 표현
- 옵셔널을 쓰는 이유
    - 명시적 표현
        - nil의 가능성을 코드만으로 표현 가능
        - 문서 / 주석 작성 시간 절약
    - 안전한 사용
        - 전달받은 값이 옵셔널이 아니라면 nil 체크를 하지 않고 사용 가능
        - 예외 상황을 최소화 하는 안전한 코딩
        - 효율적 코딩

```swift
var optionalString: String? = "Hello"
print(optionalString == nil) // false

var optionalName: String? = "John"
var greeting = "Hello!"
if let name = optionalName {
greeting += ", \(name)"
}
print(greeting) // Hello, John
```

- 선택적인 값이 nil이면 조건이 false이고 중괄호 안의 코드는 건너 뜀. 그렇지 않으면 선택적 값이 래핑되지 않고 let에 할당되므로 래핑되지 않은 값을 코드 블록 내에서 사용할 수 있음

```swift
let nickName: String? = nil
let fullName: String = "John Appleseed"
let infomalGreeting = "Hi \(nickName ?? fullName)"
print(infomalGreeting) // Hi John Appleseed
```

- 선택적 값을 처리하는 다른 방법은 `??`를 사용해 앞의 값이 nil이면 뒤의 값을 사용할 수 있다

<br/>

### 옵셔널 추출(optional unwrapping)

- 옵셔널에 들어있는 값을 사용하기 위해 꺼내오는 것

- 옵셔널 방식

  - 옵셔널 바인딩

    - nil 체크 + 안전한 추출
    - 옵셔널 안에 값이 들어있는지 확인하고 값이 있으면 꺼내 옴
    - if - let 방식 사용

```swift
func printName(_ name: String) {
  print(name)
}
var myName: String? = nil

// printName(myName) // 전달되는 값의 타입이 다르기 때문에 컴파일 에러 발생

if let name: String = myName {
  printName(name)
} else {
  print("myName == nil")
}

var yourName: String! = nil

if let name: String = yourName {
  printName(name)
} else {
  print("yourName == nil")
}

// name 상수는 if-let 구문 내에서만 사용 가능
// printName(name) // 상수 사용범위를 벗어났기 때문에 컴파일 에러 발생

// ,(콤마)를 사용해 한 번에 여러 옵셔널을 바인딩 하 수 있음
// 모든 옵셔널에 값이 있을 때만 동작
myName = "hoya"
yourName = nil
if let name = myName, let friend = yourName {
  print("\(name) and \(friend)")
} // yourName이 nil이기 때문에 실행되지 않음

yourName = "haha"
if let name = myName, let friend = yourName {
  print("\(name) and \(friend)")
} // hoya and haha
```

  - 강제 추출

    - 옵셔널에 값이 들어있는지 아닌지 확인하지 않고 강제로 값을 꺼내는 방식
    - 값이 없을 경우(nil) 런타임 에러가 발생하기 때문에 추천되지는 않음

    ```swift
    var myName: String? = hoya
    var yourName: String! = nil
    
    printName(myName!) // hoya
    myName = nil
    
    // print(myName!) // 강제추출시 값이 없으므로 런타임 에러 발생
    yourName = nil
    // printName(yourName) // nil값이 전달되기 때문에 런타임 에러 발생
    ```

<br/>

### 함수(Function)

- 함수 선언의 기본 형태

    ```swift
    func 함수이름(매개변수1이름: 매개변수1타입, 매개변수2이름: 매개변수2타입 ...) -> 반환타입 {
    	/* 함수 구현부 */
    	return 반환값
    }
      
    // 예시
    // sum이라는 이름을 가지고 a와 b라는 int타입의 매개변수를 가지며 int 타입의 값을 반환하는 함수
    func sum(a: Int, b: Int) -> Int {
      return a + b
    }
    // 예시2
    func greet(person: String, day: String) -> String {
    	return "Hello \(person), today is \(day)"
    }
    greet(person: "Bob", day: "Tuesday") // Hello Bob, today is Tuesday
    
    // 기본적으로 함수는 매개 변수 이름을 인수의 레이블로 사용, 매개 변수 이름 앞에 맞춤 인수 레이블을 쓰거나 인수 레이블을 사용하지 않으려면 _를 사용
    func greet(_ person: String, on day: String) -> String {
        return "Hello \(person), today is \(day)."
    }
    greet("John", on: "Wednesday") // Hello John, today is Wednesday
    ```

- 반환 값이 없는 함수

    ```swift
    func 함수이름(매개변수1이름: 매개변수1타입 ...) -> Void {
      /* 함수 구현부 */
      return
    }
      
    // 예시
    func printMyName(name: Stirng) -> Void {
      print(name)
    }
    // Void는 생략 할 수 있음
    func printYourName(name: String) {
      print(name)
    }
    ```

- 매개 변수가 없는 함수

    ```swift
    func 함수이름() -> 반환타입 {
      /* 함수 구현부 */
      return 반환값
    }
      
    // 예시
    func maximumIntegerValue() -> Int {
      return Int.max
    }
    ```

- 매개 변수와 반환값이 없는 함수

    ```swift
    func 함수이름() -> Void {
      /* 함수 구현부 */
      return
    }
      
    // 함수 구현이 짧은 경우 가독성을 해치지 않는 범위에서 줄바꿈을 하지 않고 한 줄에 표현 가능
    func hello() -> Void { print("Hello") }
      
    // 반환 값이 없는 경우, Void 생략 가능
    func bye() { print("Bye") }
    ```

- 함수의 호출

    ```swift
    sum(a:3, b: 5) // 8
    printMyName(name: "hoya") // hoya
    printYourName(name: "haha") // haha
    maximumIntegerValue() // Int의 최댓값
    hello() // Hello
    bye() // Bye
    ```

- 함수에서 여러 값 반환을 이용한 튜플 복합 값 만들기

    - 튜플의 요소는 이름이나 숫자로 나타낼 수 있음

    ```swift
    func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    	var min = scores[0]
        var max = scores[0]
        var sum = 0
      	
        for score in scores {
            if score > max {
                max = score
            } else if score < min {
                min = score
            }
            sum += score
        }
        return (min, max, sum)
    }
    let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])
    print(statistics) // (min: 3, max: 100, sum: 120)
    print(statistics.sum) // 120
    print(statistics.2) // 120
    ```

- 중첩 함수

    ```swift
    func returnFifteen() -> Int {
        var y = 10
        func add() {
            y += 5
        }
        add()
        return y
    }
    returnFifteen() // 15
    
    // 함수가 다른 함수의 값으로 반환하는 형태
    func makeIncrementer() -> ((Int) -> Int) {
        func addOne(number: Int) -> Int {
            return 1 + number
        }
        return addOne
    }
    var increment = makeIncrementer()
    increment(7) // 8
    ```

<br/>

### 클로저(Closure)

- 클로저는 사용자의 코드 안에서 전달되어 사용할 수 있는 로직을 가진 중괄호(`{ }`)로 구분된 코드의 블럭이며, 일급 객체의 역할을 할 수 있다

- 일급 객체는 전달 인자로 보낼 수 있고, 변수/상수 등으로 저장하거나 전달할 수 있으며, 함수의 반환 값이 될 수도 있다

- 참조 타입이다

- 함수는 클로저의 한 형태로, 이름이 있는 클로저이다

- 클로저 표현방식

    ```swift
    { (인자들) -> 반환타입 in
    	로직 구현
    }
    ```

- Inline Closure : 함수로 따로 정의된 형태가 아닌 인자로 들어가 있는 형태의 클로저

    ```swift
    let reverseNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
    })
    
    // 다른 함수를 인수 중 하나로 사용
    func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    	for item in list {
            if condition(item) {
                return true
            }
        }
        return false
    }
    func lessThanTen(number: Int) -> Bool {
        return number < 10
    }
    var numbers = [20, 19, 7, 12]
    hasAnyMatches(list: numbers, condition: lessThanTen) // true
    
    numbers.map({ (number: Int) -> Int in
                 let result = 3 * number
                 return result
    })
    
    // 클로저의 유형을 이미 알고 있는 경우, 매개변수 유형과 반환 유형을 생략할 수 있다
    let mappedNumbers = numbers.map({
        number in 3 * number })
    print(mappedNumbers) // [60, 57, 21, 36]
    ```

- 클로저의 축약

    - 타입 생략

        - 위의 예제에서 sorted(by:)의 경우 이미 (String, String) - > Bool 타입의 인자가 들어와야 하는지 알고 있기 때문에 클로저에서 타입을 명시하는 것을 생략할 수 있다
        - 하지만 코드의 모호성을 피하기 위해 타입을 명시하는 것이 좋을 때도 있다

        ```swift
        let reverseNames = names.sorted(by: {s1, s2 in return s1 > s2})
        ```

    - 반환타입 생략 : 반환 키워드의 생략

        ```swift
        let reverseNames = names.sorted(by: {s1, s2 in s1 > s2})
        ```

    - 인자 이름 생략(인자의 표기는 $0부터 순서대로 한다)

        ```swift
        let reverseNames = names.sorted(by: {$0 > $1})
        
        // 예시
        let sortedNumbers = numbers.sorted { $0 > $1 }
        print(sortedNumbers) // [20, 19, 12, 7]
        ```

    - 연산자 메소드 : 연산자를 사용할 수 있는 타입의 경우 연산자만 남길 수 있다

        ```swift
        let reverseNames = names.sorted(by: > )
        ```

- 후행 클로저

    - 인자로 클로저를 넣기가 길다면 후행 클로저를 사용해 함수의 뒤에 표현할 수 있다

    ```swift
    let reverseNames = names.sorted() { $0 > $1 }
    ```

    - 함수의 마지막 인자가 클로저이고, 후행 클로저를 사용하면 소괄호를 생략할 수 있다

        ```swift
        let reverseNames = names.sorted { $0 > $1 }
        let reverseNames = names.sorted { (s1: String, s2: String) -> Bool in return s1 > s2 }
        ```

- 값 캡쳐(Capturing Values)

    - 클로저는 특정 문맥의 상수나 변수의 값을 캡쳐할 수 있다 즉, 원본 값이 사라져도 클로저의 body 안에서 그 값을 활용할 수 있다

    - 가장 단순한 형태는 중첩 함수이며, 아래는 그 예시이다

        ```swift
        func makeIncrementer(forIncrement amount: Int) -> () -> Int {
        	var runningTotal = 0
        	func incrementer() -> Int {
                runningTotal += amount
                return runningTotal
            }
            return incrementer
        }
        
        let plusTen = makeIncrementer(forIncrement: 10)
        let plusSeven = makeIncrementer(forIncrement: 7)
        
        // 함수가 각각 실행되어도 실제로는 변수 runningTotal과 amount가 캡쳐되서 그 변수를 공유하기 때문에 누적된 결과를 가진다
        let plusedTen = plusTen() // 10
        let plusedTen2 = plusTen() // 20
        // 다른 클로저이기 때문에 고유의 저장소에 runningTotal과 amount를 캡쳐해서 사용한다
        let plusedSeven = plusSeven() // 7
        let plusedSeven2 = plusSeven() // 14
        ```

        - plusTen, plusSeven이 상수이지만 runningTotal을 증가시킬 수 있었던 이유는 클로저가 참조타입이기 때문이다

    - 함수와 클로저를 상수나 변수에 할당할 때 실제로는 상수와 변수에 해당 함수나 클로저의 참조가 할당 된다. 만약 한 클로저를 두 상수나 변수에 할당하면 그 두 상수나 변수는 같은 클로저를 참조하고 있게 된다

        - 최적화를 이유로 스위프트는 그 값이 클로저에 의해 변경되지 않고, 클로저가 생성된 후 값이 변경되지 않는 경우 값의 복사본을 캡쳐하여 저장함
        - 클로저가 어떤 클래스 인스턴스의 프로퍼티로 할당하고 그 클로저가 그 인스턴스를 캡쳐하면 강한 순환 참조에 빠지게 되기 때문에 이를 해결하기 위해 스위프트에서는 캡쳐 리스트를 사용한다
