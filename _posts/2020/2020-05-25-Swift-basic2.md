---
layout: post
title: Swift 기초(2)
date: 2020-05-25
summary: 구조체,
categories: Swift
mathjax: true

---

<br/>

### 구조체

- 구조체는 **값(value) 타입**이며 대문자 카멜케이스를 사용해 정의

- 문법

    ```swift
    struct 이름 {
    	/* 구현부 */
    }
    ```

- 구조체 프로퍼티(변수) 및 메서드(함수) 구현

    ```swift
    struct Sample {
    	// 가변 프로퍼티(값 변경 가능)
        var mutableProperty: Int = 100
        // 불변 프로퍼티(값 변경 불가능)
        let immutableProperty: Int = 100
        // 타입 프로퍼티(타입 자체가 사용하는 프로퍼티)
        static var typeProperty: Int = 100
        // 인스턴스 메서드(인스턴스가 사용하는 메서드)
        func intanceMethod() {
            print("instance method")
        }
        // 타입 메서드(타입 자체가 사용하는 메서드)
        static func typeMethod() {
            print("type method")
        }
    }
    
    // 가변 인스턴스 생성
    var mutable: Sample = Sample()
    mutable.mutableProperty = 200
    // 불변 프로퍼티는 인스턴스 생성 후 수정할 수 없어 컴파일 오류 발생
    // mutable.immutableProperty = 200
    
    // 불변 인스턴스 생성
    let immutable: Sample = Sample()
    // 불변 인스턴스는 아무리 가변 프로퍼티라도 인스턴스 생성 후에 수정할 수 없음
    // immutable.mutableProperty = 200
    
    // 타입 프로퍼티 및 메서드
    Sample.typeProperty = 300
    Sample.typeMethod()
    // 인스턴스에서는 타입 프로퍼티나 타입 메서드를 사용할 수 없음
    // mutable.typeProperty = 400
    // mutable.typeMethod()
    ```

- 학생 구조체 만들어보기

    ```swift
    struct Student {
        // 가변 프로퍼티
        var name: String = "unknown"
        
        // 키워드도 `로 묶어주면 이름으로 사용할 수 있음
        var `class`:String = "Swift"
        
        // 타입 메서드
        static func selfIntroduce() {
            print("학생타입입니다")
        }
        
        // 인스턴스 메서드
        // self는 인스턴스 자신을 지칭하며, 몇몇 경우를 제외하고 사용은 선택사항
        func selfIntroduce() {
            print("저는 \(self.class)반 \(name)입니다")
        }
    }
    
    // 타입 메서드 사용
    Student.selfIntroduce() // 학생타입입니다
    
    // 가변 인스턴스 생성
    var hoya: Student = Student()
    hoya.name = "hoya"
    hoya.class = "스위프트"
    hoya.selfIntroduce() // 저는 스위프트반 hoya입니다
    
    // 불변 인스턴스 생성
    let jina: Student = Student()
    // 불변 인스턴스이므로 프로퍼티 값 변경 불가
    // jina.name = "jina"
    jina.selfIntroduce() // 저는 Swift반 unknown입니다
    ```

<br/>

### 클래스(Class)

- 클래스는 **참조 타입** (구조체와의 가장 큰 차이이다)

- 다중상속이 되지 않음

- 객체의 설계도에 해당하는 것을 정의 해두고, 이를 바탕으로 개체를 만듦

- 클래스에서 생성된 객체 : 인스턴스

- 클래스에 제공되는 변수 : 속성(property) / 클래스에서 제공되는 함수 : 메서드(method)

- 클래스의 정의

    ```swift
    class 클래스명 { // 클래스명은 upper camel case 사용
    	/* 속성과 메서드 작성 */
    }
    
    // 예시
    class Shape {
        var numberOfSides = 0
        func simpleDescription() -> String {
            return "A shape with \(numberOfSides) sides."
        }
    }
    
    var shape = Shape()
    shape.numberOfSides = 7
    var shapeDescription = shape.simpleDescription()
    print(shapeDescription) // A shape with 7 sides.
    
    ```

- 이니셜라이저

    - 인스턴스를 만들 때 필요한 값 등을 인수로 전달하기 위해 사용
    - 인스턴스를 만들 때 자동으로 호출되는 초기화 처리 전용 메서드

    ```swift
    init (인수) {
    	/* 초기화 처리 */
    }
    
    // 예시
    class NamedShape {
        var numberOfSides: Int = 0
        var name: String
        
        init(name: String) {
            self.name = name
        }
        
        func simpleDescription() -> String {
            return "A shape with \(numberOfSides) sides."
    	}
    }
    ```

- 클래스 상속

    - 클래스의 재사용을 크게 확장하는 기능
    - 유용한 클래스가 있으면, 그것을 계승하여 자신의 고유한 기능을 덧붙여 새로운 클래스를 만드는 것

- 메소드 오버라이드

    - 상속을 하면 이미 있는 클래스에 새로운 기능을 덧붙일 수 있지만 이미 있는 클래스의 기능을 변경할 수 없다. 이것을 가능하게 하는 기능이 오버라이드이다
    - 슈퍼클래스에 있는 메서드를 재정의 하는 기술
    - 오버라이드 하는 메서드를 재정의 할때는 메서드명 뿐만 아니라 인수와 반환 값도 슈퍼클래스의 메서드와 동일하게 작성해야 함

```swift
class 클래스명: 슈퍼클래스명 {
	/* 클래스 내용 */
    
    override func 메서드명() { ... }
}

// 예시
class Square: NamedShape {
    var sideLength: Double
    
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }
    
    func area() -> Double {
        return sideLength * sideLength
    }
    
    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)"
    }
}

let test = Square(sideLenth: 5.2, name: "my test square")
print(test.area()) // 27.040000000000003
print(test.simpleDescription()) // A square with sides of length 5.2
```

- Computed 속성

    - 속성은 클래스에 값을 보관할 변수인데, 어떤 값이 될지 모르기 때문에 값의 입출력을 프로그래밍으로 제어할 수 있도록 하는 것

    - 작성법

        ```swift
        var 속성: 유형 {
	        get {
                /* 처리 */
                return 값
            }
    	    set {
                /* 처리 */
            }
        }
        ```

    - get만 작성하고 set을 작성하지 않으면 값을 얻기만 가능하고 수정할 수 없는 속성, set만 작성하면 값 변경만 가능하고 얻을 수 없는 속성이 됨

    ```swift
    class EquilateralTriangle: NamedShape {
        var sideLength: Double = 0.0
        
        init(sideLength: Double, name: String) {
            self.sideLength = sideLength
            super.init(name: name)
            numberOfSides = 3
        }
        
        var perimeter: Double {
            get {
                return 3.0 * sideLength
            }
            set {
                sideLength = newValue / 3.0
            }
        }
        
        override func simpleDescription() -> String {
            return "An equilateral triangle with sides of length \(sideLength)"
        }
    }
    
    var triangle = EquilateralTriangle(sideLength: 3.1, name:"a triangle")
    print(triangle.perimeter) // 9.3
    triangle.perimeter = 9.9
    print(triangle.sideLength) // 3.3000000000000003
    print(triangle.simpleDescription()) // An equilateral triangle with sides of length 3.3000000000000003
    ```

    - 서브클래스 이니셜라이저 3단계
        1. 서브 클래스가 선언하는 프로퍼티의 값을 설정
        2. 수퍼 클래스의 이니셜 라이저를 호출
        3. 수퍼 클래스에 의해 정의 된 속성 값 변경 이 시점에서 메소드, 게터 또는 세터를 사용하는 추가 설정 작업도 수행 가능

- didSet & willSet

    - 프로퍼티의 변경되기 직전, 직후를 감지하는 역할
    - 프로퍼티 옵저버를 사용하기 위해서는 값이 반드시 초기화가 되어 있어야 함

    ```swift
    var myProperty: Int = 10 {
    	didSet { /* 처리 */ } // myProperty의 값이 변경된 직후에 호출
        willSet { /* 처리 */ } // myProperty의 값이 변경되기 직전에 호출
    }
    
    // 예시
    class TriangleAndSquare {
        var triangle: EquilateralTriangle {
            willSet {
                square.sideLength = newValue.sideLength
            }
        }
        var square: Square {
            willSet {
                triangle.sideLength = newValue.sideLength
            }
        }
        init(size: Double, name: String) {
    		square = Square(sideLength: size, name: name)
            triangle = EquilateralTriangle(sideLength: size, name: name)
        }
    }
    var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
    print(triangleAndSquare.square.sideLength) // 10.0
    print(triangleAndSquare.triangle.sideLength) // 10.0
    triangleAndSquare.saure = Square(sideLength: 50, name: "larger square")
    print(triangleAndSquare.triangle.sideLength) // 50.0
    ```

- 프로퍼티 및 메서드 구현

    ```swift
    class Sample {
        // 가변 프로퍼티
        var mutableProperty: Int = 100
        // 불변 프로퍼티
        let immutableProperty: Int = 100
        // 타입 프로퍼티
        static var typeProperty: Int = 100
        // 인스턴스 메서드
       	func instanceMethod() {
    		print("instance method")
        }
        // 타입 메서드
        // 상속시 재정의 불가 타입 메서드 - static
        static func typeMethod() {
            print("type method - static")
        }
        // 상속시 재정의 가능 타입 메서드 - class
        class func classMethod() {
    		print("type method - class")
        }
    }
    
    // 인스턴스 생성 - 참조정보 수정 가능
    var mutableReference: Sample = Sample()
    mutableReference.mutableProperty = 200
    // 불변 프로퍼티는 인스턴스 생성 후 수정할 수 없음
    // mutableReference.immutableProperty = 200
    
    // 인스턴스 생성 - 참조정보 수정 불가
    let immutableReference: Sample = Sample()
    // 클래스의 인스턴스는 참조 타입이므로 let으로 선언되었더라도 인스턴스 프로퍼티의 값 변경이 가능
    immutableReference.mutableProperty = 200
    // 다만 참조정보를 변경할 수는 없음
    // immutableReference = mutableReference
    // 참조 타입이라도 불변 인스턴스는 인스턴스 생성 후에 수정할 수 없음
    // immutableReference.immutableProperty = 200
    
    // 타입 프로퍼티 및 메서드
    Sample.typeProperty = 300
    Sample.typeMethod()
    // 인스턴스에서는 타입 프로퍼티나 타입 메서드를 사용할 수 없음
    // mutableReference.typeProperty = 400
    // immutableReference.typeMethod()
    ```

- 학생 클래스 만들어보기

    ```swift
    class Student {
        // 가변 프로퍼티
        var name: String = "unknown"
        var `class`: String = "Swift"
        class func selfIntroduce() {
            print("학생타입입니다")
        }
        func selfIntroduce() {
            print("저는 \(self.class)반 \(name)입니다")
        }
    }
    
    Student.selfIntroduce() // 학생타입입니다
    // 인스턴스 생성
    var hoya: Student = Student()
    hoya.name = "hoya"
    hoya.class = "스위프트"
    hoya.selfIntroduce() // 저는 스위프트반 hoya입니다
    // 인스턴스 생성
    let jina: Student = Student()
    jina.name = "jina" // 구조체와는 달리 let으로 선언했더라도 재정의가 가능함
    jina.selfIntroduce() // 저는 Swift반 jina입니다
    ```

<br/>

### 열거형

- 유사항 종류의 여러 값을 한 곳에 모아서 정의한 것(요일, 월, 계절 등)

- enum 자체가 하나의 데이터 타입으로, 대문자 카멜케이스를 사용하여 이름을 정의

- 각 case는 소문자 카멜케이스로 정의

- 각 case는 그 자체가 고유의 값(각 case에 자동으로 정수값이 할당되지 않음)

- 각 case는 한 줄에 개별로도, 한 줄에 여러 개도 정의할 수 있음

- 문법

    ```swift
    enum 이름 {
        case 이름1
        case 이름2
        case 이름3, 이름4, 이름5
        // ...
    }
    
    // 예제
    enum BoostCamp {
        case iosCamp
        case androidCamp
        case webCamp
    }
    ```

- 열거형 사용

    - 타입이 명확할 경우, 열거형의 이름을 생략할 수 있음
    - switch 구문에서 사용하면 좋음

    ```swift
    enum Weekday {
        case mon
        case tue
        case wed
        case thu, fri, sat, sun
    }
    // 열거형 타입과 케이스 모두 사용 가능
    var day: Weekday = Weekday.mon
    // 타입이 명확하다면 .케이스 처럼 표현해도 무방
    day = .tue
    print(day) // tue
    
    // switch의 비교값에 열거형 타입이 위치할 때 모든 열거형 케이스를 포함한다면 default를 작성할 필요가 없음
    switch day {
    case .mon, .tue, .wed, .thu:
        print("평일입니다")
    case Weekday.fri:
        print("불금 파티!!!")
    case .sat, .sun:
        print("신나는 주말!!")
    }
    ```

 - rawValue(원시값)

    - C언어의 enum처럼 정수값을 가질 수 있음
        - rawValue는 case별로 각각 다른 값을 가지고 있어야 함
            - 자동으로 1이 증가된 값이 할당 됨
                - rawValue를 반드시 지닐 필요가 없다면 굳이 만들지 않아도 됨

    ```swift
    enum Fruit: Int {
        case apple = 0
        case grape = 1
        case peach
        
        // case mango = 0 // mango와 apple의 원시갑싱 같으므로 mango 케이스의 원시값을 0으로 정의할 수 없음
    }
    print("Fruit.peach.rawValue == \(Fruit.peach.rawValue)") // .. == 2
    ```

- 정수 타입 뿐만 아니라, Hashable 프로토콜을 따르는 모든 타입을 원시값의 타입으로 지정할 수 있음

    ```swift
    enum School: String {
        case elementary = "초등"
        case middle = "중등"
        case high = "고등"
        case university
    }
    print("School.middle.rawValue == \(School.middle.rawValue)") // .. == 중등
    // 열거형의 원시값 타입이 String일 때, 원시값이 지정되지 않았다면 case의 이름을 원시값으로 사용
    print(School.university.rawValue) // university
    ```

- 원시값을 통한 초기화

    - rawValue를 통해 초기화 할 수 있음
    - rawValue가 case에 해당하지 않을 수 있으므로, rawValue를 통해 초기화 한 인스턴스는 옵셔널 타입

    ```swift
    // rawValue를 통해 초기화 한 열거형 값은 옵셔널 타입이므로 Fruit 타입이 아님
    // let apple: Fruit = Fruit(rawValue: 0)
    let apple: Fruit? = Fruit(rawValue: 0)
    
    // if let 구문을 사용하면 rawValue에 해당하는 케이스를 곧바로 사용할 수 있음
    if let orange: Fruit = Fruit(rawValue: 5) {
        print("rawValue 5에 해당하는 케이스는 \(orange)입니다")
    } else {
        print("rawValue 5에 해당하는 케이스가 없습니다")
    } // rawValue 5에 해당하는 케이스가 없습니다
    ```

- 메서드

    - 열거형에서 메서드도 추가가 가능함

    ```swift
    enum Month {
        case dec, jan, feb
        case mar, apr, may
        case jun, jul, aug
        case sep, oct, nov
        
        // Method
        func printMassage() {
            switch self {
            case .mar, .apr, .may:
                print("따스한 봄")
            case .jun, .jul, .aug:
                print("신나는 여름")
            case .sep, .oct, .nov:
                print("독서의 계절, 가을")
            case .dec, .jan, .feb:
                print("눈의 계절, 겨울")
            }
        }
    }
    Month.mar.printMassage() // 따스한 봄
    ```
