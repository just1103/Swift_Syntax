# Swift syntax

Created: January 24, 2021 1:43 PM
Last Edited Time: September 28, 2021 12:31 AM
Property: Yagom
Type: 언어

- Contents
- xcode>file>new>playground>black에서 코드 입력

# 1. Swift 특징

- Rules
    - Naming rule
        - Lower Camel Case : function, method, variable, constant
        - Upper Camel Case : type (class, structure, enum, extension...)

    - console log
        - print : 단순 문자열 출력
        - dump : 인스턴스의 자세한 설명 (description property)까지 출력

            ```swift
            struct BasicInfo {
                let name: String
                var age: Int
            }
            var yagomInfo = BasicInfo(name: "ya", age: 9)

            print(yagomInfo)  // BasicInfo(name: "ya", age: 9)
            print(yagomInfo.name)  // ya

            dump(yagomInfo)
            // ▿ __lldb_expr_5.BasicInfo
            //    - name: "ya"
            //    - age: 9

            -
            class Person {
                var height: Float = 0.0
                var weight: Float = 0.0
            }
            let yagom = Person()
            yagom.height = 180
            yagom.weight = 80

            print(yagom)  // Person
            print(yagom.height)  // 180.0

            dump(yagom)
            // ▿ __lldb_expr_5.Person #0
            //    - height: 180.0
            //    - weight: 80.0
            ```

            - [ ]  struct, class → print(인스턴스명) 결과가 다르다 ??

    - 문자열 보간법 (String interpolation)
        - 프로그램 실행 중 긴 문자열 내에 변수/상수의 실질적인 값을 끼워넣기 위해 사용
        - Swift uses `string interpolation` to include the name of a constant/variable as a placeholder in a longer string, and to prompt Swift to replace it with the current value of that constant/variable. 
        Wrap the name in `parentheses()` and escape it with a `backslash\` before the opening parenthesis:
        - \()
    - ;
        - Unlike many other languages, Swift doesn’t require you to write a semicolon (;) after each statement in your code, although you can do so if you wish.
        - However, semicolons *are* required if you want to write multiple separate statements on a single line.

            ```swift
            let cat = "🐱"; print(cat)   // Prints "🐱"
            ```

- 다중 패러다임 프로그래밍 언어

    명령형, 객체지향 (자료 추상화, 상속, 다형성, 동적 바인딩) 프로그래밍 패러다임을 기반으로 
    함수형 프로그래밍 (함수가 일급객체, 대규모 병렬처리용) 패러다임 및 프로토콜 지향 프로그래밍 패러다임을 지향한다.
    (Notion-Swift/iOS History & CS의 객체지향 프로그래밍 패러다임 참고)
    "Swift supports Multi-paradigm: protocol-oriented, object-oriented, functional, imperative, block structured, declarative"

- 표현력 풍부

    ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled.png)

- MarkUp Syntax
    - 특정 기능에 대해 MarkUp 주석을 작성하면 Custom Quick Help를 통해 확인이 가능함
    - 참고 - [https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497-CH2-SW1](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497-CH2-SW1)

# 2. 상수/변수 선언

- 프로그램에서 사용하는 데이터를 메모리에 임시 저장함
- let, var
    - Once you’ve declared a constant or variable of a certain type, you can’t declare it again with the same name, or change it to store values of a different type. 
    Nor can you change a constant into a variable or a variable into a constant.

        ```swift
        상수 선언 (고정값을 나타내므로 가독성 있음)
        let constantName: type = value
        let constantName = value  // 값의 타입이 명확한 경우 타입 생략 가능
        // 상수는 차후에 값 변경 불가함 (immutable)

        변수 선언
        var variableName: type = value
        var variableName = value  // 값의 타입이 명확한 경우 타입 생략 가능
        // 변수는 차후에 값 변경 가능함 (mutable)

        let constant1: String = "변경 불가"
        var variable1: String = "변경 가능"

        variable1 = "변경"
        //constant1 = "변경"  // 상수는 다른 값 할당이 불가하므로 오류 발생

        -
        var x = 0.0, y = 0.0, z = 0.0   // You can declare multiple constants or multiple variables on a single line, separated by commas
        ```

    - 상수/변수 선언 후 나중에 값을 할당할 수 있음

        ```swift
        let sum: Int
        sum = 10

        // 참고
        let sumError: Int
        // print(sum) // 값 할당 전에 사용하면 컴파일 에러 발생 - initialize 전에 사용했다는 오류 메세지가 뜸 (Constant 'sum' used before being initialized)
        ```

    - [x]  전역 변수, 지역 변수
        - 유효범위에 따라 구분
            - 전역변수, global variable (메모리의 global 영역에 저장) : 클래스/구조체/열거형 등의 타입이나 클로저/함수 "외부"에서 선언된 변수로, 프로그램 전체에서 접근할 수 있는 변수이다.
            - 지역변수, local variable (메모리의 stack 영역에 저장) : 클래스/구조체/열거형 등의 타입이나 클로저/함수 "내부"에서 선언된 변수로, 만약 함수 내부에서 선언된 경우 함수가 실행될 때 메모리에 만들어지고 함수가 종료될 때 메모리에서 소멸되는 변수이다. 함수 외부에서 접근 불가하다.

# 3. Data Type

- Swift는 Data Type에 엄격함. Type은 Upper Camel Case로 표현함
- 기본 Data Type 모두 Struct을 기반으로 하며, 추가 기능 (Extension, Generic 등)을 사용함
- Type conversion (type이 다른 값 연산)
    - 한번 선언된 변수는 타입을 바꿀 수 없지만, conversion 은 가능하다.
    To convert one specific number type to another, you `initialize` a new number of the desired type with the existing value.

        ```swift
        var intA = 100  // Int type
        var doubleB = 100.1  // Double type

        doubleB = doubleB + Double(intA)  // 200.1 <- Int를 Double로 convertion 해줌 (initialize)

        intA = Int(doubleB)  // 200 <- 소수점 삭제 (0.1 이든 0.9 이든 상관 없이) *truncated
        ```

- Bool, Int, UInt, Float, Double, Character, String & 특수문자
    - 설명

        ```swift
        // Bool 
        var someBool: Bool = true
        someBool = false
        // 0,1 은 사용 불가

        // Int (정수)
        var someInt: Int = -100
        // 64 Bit 
        // *On a 32-bit platform, UInt is the same size as UInt32. / On a 64-bit platform, UInt is the same size as UInt64.

        // UInt (양의 정수)  * Int is preferred, even when the values to be stored are known to be nonnegative.
        var someUInt: UInt = 100
        // Unsigned Integer (부호가 없는 정수. 즉 양의 정수 또는 0) 
        // *signed (positive, zero, or negative) or unsigned (positive or zero)

        *Floating-point numbers are numbers with a fractional component. Swift provides two signed floating-point number types: Float, Double
        // Float (32 Bit 부동소수형)
        var someFloat: Float = 3.14
        someFloat = 3  // 정수도 할당 가능함
        // 대부분의 소수를 표현할 때?

        // Double (64 Bit 부동소수형)  *In situations where either type would be appropriate, Double is preferred.
        var someDouble: Double = 3.14
        someDouble = 3  // 정수도 할당 가능함
        // 정밀한 소수를 표현할 때?

        // Character
        var someCharacter: Character = "A"
        someCharacter = "가"
        // someCharacter = "하하하" // 한글자가 아니라서 오류 발생
        // 한글자 문자를 표현, 유니코드 사용가능 (이모티콘 등)

        // String
        var someString: String = "하하하 웃으면 복이 와요"
        someString = someString + "추가 문구를 넣어주자"  // String type 끼리 바로 연결 가능
        ```

        - [x]  실수
        - 실수 표현방식은 고정소수점, 부동소수점 (떠돌이 소수점)으로 나뉜다.
        [https://gsmesie692.tistory.com/94](https://gsmesie692.tistory.com/94)
- String 처리

    ```swift
    var someString: String = "초기값"
    someString.append("추가 문구-2")  // append 메서드를 통해 뒤로 문자열을 추가 가능
    someString.count // 문자의 수
    someString.isEmpty // 빈 문자열인 경우 true

    var someBool: Bool = false  // 연산자를 통한 문자열 비교
    someBool = hello == "hello" // 이것의 결과값을 someBool에 할당
    print(someBool) // true
    ```

    - String은 a collection of Character이다.

        ```swift
        let str = "Hello"
        for c in str {
            print("element : \(c), type: \(type(of: c))")
        }

        // element : H, type: Character
        // element : e, type: Character
        // element : l, type: Character
        // element : l, type: Character
        // element : o, type: Character
        ```

    - 자주쓰는 메서드

        ```swift
        // (Collection O, String X).joined() 
        // Returns a new string by concatenating the elements of the sequence, adding the given separator between each element.
        set1.map{ String($0) }.joined(separator: "|") // Collection의 element를 [string]으로 변환한 뒤에 사용할 수도 있다. 3|2|1|4|5

        print([2,3,4,5,11,3].map{ String($0) }) // ["2", "3", "4", "5", "11", "3"]
        print(["2","3","4","5","11","3"].joined(separator: " ")) // 2 3 4 5 11 3 - Array에 든 String을 풀어서 하나의 String으로 만든다.

        // (Collection O, String O).split()
        inputString.split(separator: " ") 
        ```

        - `func joined(separator: String = "") -> String`  // Returns a new string by concatenating the elements of the sequence, adding the given separator between each element.
        - `func components(separatedByseparator: CharacterSet) -> [String]`
        - `func split(separator: Self.Element, maxSplits: Int = Int.max, omittingEmptySubsequences: Bool = true) -> [Substring]` // Returns the longest possible subsequences of the collection, in order, around elements equal to the given element.

            ```swift
            let line = "BLANCHE:   I don't want realism. I want magic!"
            print(line.split(separator: " ")) // Prints "["BLANCHE:", "I", "don\'t", "want", "realism.", "I", "want", "magic!"]"
            print(line.split(separator: " ", maxSplits: 1)) // Prints "["BLANCHE:", "  I don\'t want realism. I want magic!"]" - 1번만 분리한다.
            print(line.split(separator: " ", omittingEmptySubsequences: false)) // Prints "["BLANCHE:", "", "", "I", "don\'t", "want", "realism.", "I", "want", "magic!"]" - contains empty strings where spaces were repeated.
            ```

    - <문자열 다루기> 관련 내용은 아래 참고

        [https://www.notion.so/Apple-Developer-Documentation-3b4983e7a71941fb8bbc531effdc16d9#ec0151b021f74383a90a59babfba3f9f](https://www.notion.so/Apple-Developer-Documentation-3b4983e7a71941fb8bbc531effdc16d9)

- #특수문자# (Special String for Escape sequence)

    \n  줄바꿈(line feed) - 커서를 현재 행의 다음 행으로 내리기
    \\  String 내 백슬래쉬를 표현 
    \"  String 내부의 "를 표현 
    \t  탭 문자
    \0  String 종료를 나타내는 null 문자(null character)
    \r carriage return - 커서를 현재 행의 맨 좌측으로 옮기기 (현재 컴파일러는 \n으로 \r,\n 기능을 대체하므로 \r를 사용할 일이 없다.)

    ```swift
    // String 내부에 위의 특수문자를 사용하지 않는 방법
    // print(#""#)

    print("문자열 내부에\n 이런 \"특수문자\"를\t사용하면 \\이런 놀라운 결과를 볼 수 있습니다")
    print(#"문자열 내부에 "나 /를 출력하고 싶지만, 특수문자를 사용하기 싫다면 문자열 앞뒤에 #을 붙여주세요"#) // 백슬래쉬가 없어도 특수문자가 그대로 출력된다.
    let number: Int = 100
    print(#"특수문자를 사용하지 않을 때도 문자열 보간법을 사용하고 싶다면 이렇게 \#(number) 해보세요"#)

    //출력
    문자열 내부에
     이런 "특수문자"를	사용하면 \이런 놀라운 결과를 볼 수 있습니다
    문자열 내부에 "나 /를 출력하고 싶지만, 특수문자를 사용하기 싫다면 문자열 앞뒤에 #을 붙여주세요
    특수문자를 사용하지 않을 때도 문자열 보간법을 사용하고 싶다면 이렇게 100 해보세요
    ```

- Any, AnyObject, nil (+L/R)
    - Data Type을 명시하는 것이 유리하므로 Any, AnyObject 사용은 지양하는 것이 좋다.
    - Any

        Swift의 모든 Data Type을 할당 가능하다. (A class, structure, or enumeration / A metatype, such as `Int.self` / A tuple / A Closure type 모두 가능하다.) 
        *Metatype Type : Notion 3. Data Type-L/R 참고

        ```swift
        let mixed: [Any] = ["one", 2, true, (4, 5.3), { () -> Int in return 6 }]
        ```

        When you use Any type as a concrete type (구체적인 type) for an instance, you need to cast the instance to a known type before you can access its properties or methods. Instances with a concrete type of Any maintain their original dynamic type and can be cast to that type using one of the type-cast operators—as, as?, or as!.

        - [ ]  Array의 element를 instance라고 부르나???
        - [ ]  dynamic type?

        ```swift
        if let first = mixed.first as? String { // Use as? to conditionally downcast the first object in a heterogeneous array to a String
            print("The first item, '\(first)', is a string.")
        }
        // Prints "The first item, 'one', is a string."
        ```

    - AnyObject

        클래스의 인스턴스를 할당 가능하다. (모든 클래스 타입을 지칭하는 프로토콜)

        AnyObject는 프로토콜이다.

        - Swift Language Guide>Reference Manual - [https://docs.swift.org/swift-book/ReferenceManual/Types.html](https://docs.swift.org/swift-book/ReferenceManual/Types.html)

            The AnyObject protocol is similar to the Any type. All classes implicitly conform to AnyObject. 
            Unlike Any, which is defined by the language, AnyObject is defined by the Swift standard library. → 이게 왜 중요???

        - Swift Language Guide - [https://docs.swift.org/swift-book/LanguageGuide/Protocols.html#ID281](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html#ID281)
            - You can limit protocol adoption to class types (and not structures or enumerations) by adding the AnyObject protocol to a protocol’s inheritance list. (AnyObject를 상속 목록에 추가하면, Protocol Adoption을 할 수 있는 대상을 Class type으로 제한한다.) (L/G - Class-Only Protocols)

                ```swift
                protocol SomeClassOnlyProtocol: AnyObject, SomeInheritedProtocol {
                    // class-only protocol definition goes here
                }
                ```

        - Swift Documentation, AnyObject로 구글 검색 - [https://developer.apple.com/documentation/swift/anyobject](https://developer.apple.com/documentation/swift/anyobject)

            You use AnyObject when you need the flexibility of an untyped object (type이 정해지지 않은 객체) or when you use bridged Objective-C methods and properties that return an untyped result. 

            AnyObject can be used as the concrete type for 1) an instance of any class, 2) class type, 3) class-only protocol. 

            AnyObject can also be used as the concrete type for an instance of a type that bridges to an Objective-C class. Many value types in Swift bridge to Objective-C counterparts (대응), like String and Int. (Objective-C class로 연결되는 타입의 인스턴스를 할당 가능하다. String, Int 등 Swift의 값 타입은 Objective-C에 대응하는 부분???이 있고 그것에 연결 가능하다.)
            - The flexible behavior of the AnyObject protocol is similar to Objective-C’s id type. For this reason, imported Objective-C types frequently use AnyObject as the type for properties, method parameters, and return values. ???

            - 타입 확인방법 
            - type(of:) : type을 반환한다.
            - is : 일치 여부를 Bool type을 반환한다.
            - [ ]  bridged Objective-C methods and properties?
            - [ ]  let z: AnyObject = FloatRef.self // ???
            - [ ]  concrete type? (구글: A concrete class is a class that has an implementation for all of its methods.)

            ```swift
            class FloatRef {
                let value: Float
                init(_ value: Float) {
                    self.value = value
                }
            }

            let x = FloatRef(2.3) // Class 인스턴스 생성
            let y: AnyObject = x  // AnyObject에 할당 가능하다.

            let z: AnyObject = FloatRef.self // ??? Class type???

            //

            let s: AnyObject = "This is a bridged string." as NSString // Objective-C .......? 를 할당 가능하다.
            print(s is NSString) // Prints "true" - NSString type이다. AnyObject가 아니다. (type이 아니라 protocol이므로)

            let v: AnyObject = 100 as NSNumber
            print(type(of: v)) // Prints "__NSCFNumber"
            ```

            Objects with a concrete type of AnyObject maintain a specific dynamic type and can be cast to that type using one of the type-cast operators (as, as?, or as!).

            - [ ]  String type의 인스턴스???

            ```swift
            // Casting AnyObject Instances ??? to a Known Type

            let s: AnyObject = "This is a bridged string." as NSString

            if let message = s as? String {  // as?를 사용하여 상수 s를 Swift String type의 인스턴스로 Casting 했다.
                print("Successful cast to String: \(message)")
            }
            // Prints "Successful cast to String: This is a bridged string."
            ```

    - nil

        Data Type이 아니며, "없음"을 의미하는 키워드이다. (옵셔널 type에만 할당 가능하다. Any에도 할당 불가하다.)

        단, Empty와는 다르다. 예를 들어 `var array1: [Int] = []` 는 변수 array1에 메모리가 할당되고, Empty Array가 들어있는 상태이다. 반면 nil은 메모리 할당이 되어있지 않은 상태이다.

        ```swift
        // Any
        var someAny: Any = 100
        someAny = "어떤 타입도 수용 가능함"
        someAny = 123.12

        let someDouble: Double = someAny // 오류 발생 (실수값이지만 Double에 할당할 수 없다. type이 다르므로)
        // cannot convert value of type 'Any' to specified type 'Double'

        // AnyObject
        class SomeClass {}
        var someAnyObject: AnyObject = SomeClass()  // class SomeClass type의 인스턴스를 변수 someAnyObject에 할당/initialize 했음

        // nil
        someAny = nil  // 오류 발생 (어떤 데이터 타입도 할당할수 있지만 nil은 할당 불가함)
        someAnyObject = nil  // 마찬가지로 오류 발생

        var possibleNil: Int? = 1
        possibleNil = nil  // You set an optional variable to a valueless state by assigning it <nil>.
        ```

- Never
    - 비반환 함수 (Nonreturning function)의 반환 type으로 사용한다. (Function 항목 참고)
- Tuple Type (L/R)
    - 지정 데이터의 묶음, Type 이름이 따로 없음

        ```swift
        // String, Int, Double - Type의 Tuple을 선언
        var person: (String, Int, Double) = ("yagom", 100, 182.5)

        // index를 통해 값을 추출/할당 가능
        print("이름: \(person.0), 나이: \(person.1), 키: \(person.2)")
        person.1 = 90
        ```

        ```swift
        let http404Error = (404, "Not Found")  // http404Error is a tuple of type (Int, String)
        let (statusCode, statusMessage) = http404Error
        // You can decompose a tuple’s contents into separate constants or variables, which you then access as usual.

        print("The status code is \(statusCode)")  // Prints "The status code is 404"

        -
        let http200Status = (statusCode: 200, description: "OK")  // tuple의 각 elements에 name을 부여할 수 있음

        print(http200Status.0)  // prints 200 (index 0번의 값을 불러옴)
        print(http200Status.description)  // prints OK (element name으로 값을 불러옴
        ```

    - Tuple의 element 별 이름을 지정 가능함

        ```swift
        var person: (name: String, age: Int, height: Double) = ("yagom", 100, 182.5)

        // element 별 이름을 통해 추출/할당 가능
        print("이름: \(person.name), 나이: \(person.age), 키: \(person.height)")
        person.age = 90
        person.1 = 80  // index 도 사용 가능
        ```

    - Tuple의 Type Alias가 가능함 (반복 사용할 때)

        ```swift
        typealias PersonTuple = (name: String, age: Int, height: Double)

        var yagom: PersonTuple = ("yagom", 100, 182.5)
        var eric: PersonTuple = ("eric", 150, 190.5)
        ```

### Floating Point (부동소수점)

- 근사값 - 컴퓨터가 부동소수점을 처리하는 방법

    소수점수는 소수점을 가진 수이다.
    10진법에서는 0.1을 10번 더해서 1.0을 만든다. 2진법에서는 0.1을 2번 더해서 1.0을 만든다. `0.1(2) + 0.1(2) = 1.0(2)`

    - 10진수

        10진소수점수 (10진법의 소수점을 가진 수) 중에서 유한소수점수는 10진유한소수점수 (Finite Decimals), 무한소수점수인 10진무한소수점수 (Infinite Decimals)이다.

        10진유한소수점수에 속하는 314.159는 소수점 기준 왼쪽은 *10, 오른쪽은 /10을 한 것이다.
        10진유한소수점수 0.2는 2/10이다. 0.45는 45/100이다. 0.159는 159/1000이다. 이처럼 10진유한소수점수는 10으로 나누다보면 어느 순간 정확히 나누어 떨어진다.

        반면, 10진무한소수점수는 10의 거듭제곱으로 나누어 떨어지지 않는다.
        10진무한소수점수 1/3이 그렇다. 0.33333...

    - 2진수

        2진법에서는 0.1을 2번 더해서 1.0을 만든다고 했으므로 2진소수점수 0.1은 1.0을 2개로 나눈 것 중 하나이다. 
        이 0.1을 10진분수로 표현하면 5/10(=1/2), 10진수로 표현하면 0.5이다. 즉, 2진소수점수 0.1은 10진소수점수 0.5와 동일하다.

        2진소수점수도 10진소수점수와 마찬가지로 2진유한소수점수와 2진무한소수점수가 있다.

        2진유한소수점수는 2로 나누다보면 어느 순간 정확히 나누어 떨어진다.
        10진유한소수 0.5(=1/2)는 2진법으로 표현하면 0.1이고, 10진유한소수 0.625(=1/8)는 2진법으로 표현하면 0.101이다.

        2의 거듭제곱으로 나누어 떨어지지 않는 2진무한소수점수의 예는 10진수 0.1을 2진수로 표현한 것이다. 0.00011001100... → 이것을 컴퓨터에 저장하면?

        ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%201.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%201.png)

    - 컴퓨터의 한계

        컴퓨터는 유한한 저장공간을 가지고 있으므로 2진무한소수점수를 저장할 수 없다! 
        따라서 일반적으로 32비트의 저장공간에 이 무한소수점수를 우겨넣고, 나머지 뒷자리는 생략한다.

        ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%202.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%202.png)

        ```c
        int main() {
        		float number = 0.1f;
        		printf("%f", number); // 기본 출력 - 0.100000 (우겨서 저장한 값을 반올림하고, 사용자가 읽을 수 있는 십진수로 변환하여 출력한다.)
        		printf("%20f", number); // 20자리로 출력 - 0.10000000149011611938 (float type에 저장 가능한 범위를 초과하므로 잘못된 값이 출력된다.)
        		return 0;
        }
        ```

        - Float & Double type

            32비트 소수점수는 Float type에 담는다.     *Float : single precision floating point number 
            64비트 소수점수는 Double type에 담는다. *Double : double precision floating point number

            Float은 32비트 중에서 23비트를 소수점수를 표현하는 데에 사용한다. 소수점 아래 (소수점 기준 오른쪽)부터 기입하되 첫번째로 나오는 1은 생략 가능하므로 생략한다. (아래에서 설명)
            나머지 비트는 부호 (Sign)와 지수부 (Exponent)에 사용한다.

            ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%203.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%203.png)

        따라서 10진수 0.1이 2진수로 변환된 23비트 짜리 이진소수점수 0.00011001100...를 다시 10진수로 변환하면 0.10000000149011611938이 된다. 
        즉, 컴퓨터는 2진무한소수점수인 "10진수 0.1"을 정확히 표현할 수 없다. 그러므로 근사값을 사용한다.

    - 근사치 연산의 오류

        10진수 0.1의 근사치를 제곱한 값, 0.10000000149011611938 * 0.10000000149011611938은 
        또 다른 2진무한소수점수인 0.01의 근사치 (0.01에 가장 근접한 23비트의 이진소수점수)와 일치하지 않는다. 이것을 '근사치 연산의 오류'라고 한다.

        따라서 컴퓨터는 `0.1*0.1 == 0.01`의 결과를 false로 판단한다. (단, 컴파일러나 시스템에 따라 결과가 다를 수 있다.)

        ```c
        int main() {
        		float number = 0.1f;
        		if(num*num = 0.01f) {
        				printf("참");
        		} else {
        				printf("거짓");
        		}
        		return 0;
        } // 출력 - 거짓
        ```

        컴퓨터는 연산을 할 때, CPU의 ALU (Arithmetic Logic Unit)에서 값을 처리하는데, 소수점수 연산을 할 때는 ALU 내부의 FPU (Floating point Unit)에서 값을 처리한다.
        따라서 간단한 정수 연산은 FPU 없이 소프트웨어적으로 처리가 가능하다. 반면 소수점수 연산을 하면 근사치 연산을 하므로 항상 정밀도 문제가 발생한다.
        계산기 앱이나 은행 앱에서는 근사치 연산이 아니라 소프트웨어적인 10진 연산을 수행하기 때문에 문제가 없다.

    출처: [https://www.youtube.com/watch?v=vOO-oLS0H68](https://www.youtube.com/watch?v=vOO-oLS0H68)

- 컴퓨터에서 사용하는 부동소수점 숫자는 기본적으로 근사값이다. 컴퓨터는 내부적으로 모든 데이터를 이진수로 표현한다. 
정수나 자연수에 있어서 진법은 특정한 값을 표시하기 위해 필요한 숫자의 개수만 달라지지만, 소수점 이하의 값에 대해서는 그 사정이 다르다.

    대략 소수점 11자리까지의 정밀도로 표현하면 십진수 0.1은 이진수로 0.0001100110011(2) 쯤 되고, 이 값을 다시 십진수로 변환하면 0.0999755859375 정도가 된다. 즉 엄밀하게 0.1이 아닌 것이다. 2진법으로 표현했을 때 딱 떨어지는 값이 아닌 이상, 많은 실수값들이 컴퓨터에서는 실질적으로 근사값을 사용하고 있는 셈이다.

    수학에서 근사값을 표현하는 방식으로 가수부와 지수부를 나눠서 표시하는 방식이 있다. 예를 들어 12500 이라는 값이 있을 때, 이를 1.25 * 10^4 으로 표현하는 것이다. 이 때 1.25를 가수부라 하고 10^4을 지수부라 한다. 이 표현을 사용하면 12500은 1.25 * 10^4 으로 나타낼 수도 있고, 1.250 * 10^4 으로 나타낼 수도 있다. 둘 다 같은 값 처럼 보이지만, 후자의 표현은 10의 자리 숫자 0까지 신뢰할 수 있는 유효숫자라는 것을 알려주는 표기방법이다.

    많은 프로그래밍 언어에서 실수를 표현하는 타입들은 이 표현을 데이터를 저장하는 구조에 그대로 반영한다. 즉 부호, 가수부, 실수부로 값이 이루어진다고 보고, 그것들을 메모리에 잘 정돈해 넣어두는 것이다. 

    [https://soooprmx.com/swift-float-타입-사용법/](https://soooprmx.com/swift-float-%ED%83%80%EC%9E%85-%EC%82%AC%EC%9A%A9%EB%B2%95/)

- IEEE754 표준에 따르면 소수를 표현하는 과정은 아래와 같다. (Institute of Electrical and Electronics Engineers, 전기전자기술자협회)
    1. 소수를 2진수로 변환한다.
    2. 제일 앞에 있는 1을 기준으로 뒤로 소수점을 옮기고, 옮긴 만큼의 수를 지수비트에 기록한다.
    3. 이동된 소수점 뒤의 모든 수를 가수 비트에 기록한다

    예를 들어 소수 263.3을 1) 2진수로 변환하면, 100000111.010011001100110...이 된다. 2) 그리고 맨 앞의 1 뒤로 소수점을 옮기면, 1.00000111010011001100110 * 2^8이 된다. 
    2^8을 한 번 더 처리하여 (8+127=135) 2진수로 변환한 10000111을 지수부 비트에 기록한다.
    3) 맨 앞의 1을 제외한 소수점 아래의 수인 00000111010011001100110을 가수 비트에 기록한다.
    결과적으로 아래와 같이 표현된다.
    - 부호 비트(1bit) : 0(양수)
    - 지수 비트(8 bit) : 10000111 (127 + 8 = 135)
    - 가수 비트(23 bit) : 00000111010011001100110

- 과학적 표기법 (Scientific Notations)

    어떤 정수 하나가 있다고 했을 때, 그 정수는 `m *10^n` 형식 안에서 여러 가지 형태로 표시할 수 있다. 과학적 표기법의 장점은 유효숫자를 명시 가능하다는 점이다.

    `(표현방식) 123.45 = 12345E-2 = 12345 * 10^(-2) = 1.2345E2 = 1.2345 * 10^2`
    - 소수점의 위치가 가변적이다. 즉, 부동소수점이다. 

    `1.2345 * 10^2`
    - 유효숫자가 5개 (1.2345)이다. *Significant (유효숫자)

    `1.23450 * 10^2`
    - 유효숫자가 6개 (1.23450)이다.

    `m *10^n`
    - m : Significand (정수부???)
    - n : Exponent
    - 1.0 ≤ m ≤ 10 일 때, Modified Normalized Form 이라고 한다. 0.1 ≤ m ≤ 1.0 일 때, True Normalized Form 이라고 한다. (값을 쉽게 비교하기 위해 만들었다.)
    - `0.xxx` 형태를 Normed Significand 라고 한다.
    - `1.xxx *2^n` 형태를 Normalized Significand 라고 한다. 
       컴퓨터에서 어떤 값을 부동소수점 data type으로 저장할 때, 이러한 Normalized Significand 형태로 변환하여 처리한다!
       *Float은 32비트 중에서 23비트를 소수점수를 표현하는 데에 사용한다. 소수점 아래 (소수점 기준 오른쪽)부터 기입하되 첫번째로 나오는 1은 생략 가능하므로 생략한다. ← 이 부분

    - 32비트 부동소수점수 (Float type)이 메모리에 저장되는 과정

        ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%204.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%204.png)

        ⛳️⛳️⛳️ 즉, Normalized Significand 인 `1.xxx *2^n` 형태를 보면, 여기서 1.xxx이 fraction이고, 2^n이 exponent이다.

        - sign : 0이면 양수, 1이면 음수이다. 
        위 그림에서 0이므로 양수를 나타낸다.
        - exponent (2^n) : 8비트를 사용하므로 2^8 (256)개 숫자인 0~255롤 2의 지수에 사용 가능하다. 이때 값을 한번 더 처리 (-127) 하므로 -127~128 (0-127에서 255-127) 범위의 수를 표현 가능하다. (단, -127, 128은 다른 용도로 사용함) 
        위 그림에서 01111100(2)은 10진법으로 124이다. 이것을 처리한 -3 (124-127=-3)을 2의 지수에 사용한다.
        - fraction (1.xxx) : 첫번째로 나오는 1은 항상 1이므로 생략한다. 그리고 소수점 이하의 수를 2진법으로 저장한다. 
        위 그림에서 0100000...0(2)은 10진법으로 2^(-2)이다. 따라서 fraction의 값은 1 + 2^(-2) 이다.
        - 결론적으로, 3개 요소를 가져와서 곱하면, (+1) * 2^-3 * ( 1 +  2^(-2) ) = 0.15625 이다.

        출처: [https://www.youtube.com/watch?v=a62KzRlbmLU](https://www.youtube.com/watch?v=a62KzRlbmLU), [https://tonks.tistory.com/221](https://tonks.tistory.com/221)

- ↔ 고정소수점

    '정수'를 표현하는 비트와 '소수'를 표현하는 비트의 비트 수를 사전에 미리 정해두고, 해당 비트만큼 의미를 부여하여 소수를 표현하는 방식이다. 예를 들어 실수 표현에 4byte(32bit)를 사용하고, 부호 표현에 1bit, 정수 표현에 16bit, 소수 표현에 15bit를 사용하기로 약속한 '소수 표현방식'이 있다고 가정한다. 해당 시스템에서 263.3을 소수로 표현하게 되면 (0)0000000100000111.010011001100110 와 같은 수로 표현이 된다.

    고정 소수점 방식을 사용하게 되면 사용하지 않는 비트를 낭비해야 한다는 단점이 있다. 정수를 표현하는 bit 수를 늘리면 더 큰 수를 표현할 수 있겠지만, 소수를 표현하는 bit가 상대적으로 작아지기 때문에 정밀한 수를 표현하기에는 무리가 있다. 반면 소수를 표현하는 bit를 늘리게 될 경우 큰 수를 표현할 수 없게 된다.

    이러한 문제점을 해결하기 위해 정수와 소수를 나타내는 비트를 사전에 정해 놓지 않고, 상황에 따라 변경되는 부동소수점(floating point) 방식을 사용한다. floating point란 소수점이 둥둥 떠다닌다는 의미로 쉽게 말해 소수점의 위치가 변한다는 뜻이다. (부동소수점 표기법은 숫자를 나타내는 데 필요한 자릿수를 줄임으로써 아주 크거나 아주 작은 숫자들을 다룰 수 있게 해준다.)

    IEEE754 방식은 '정수'와 '소수' 부분으로 나눴던 고정소수점 방식과는 다르게, '지수' 비트와 '가수' 비트로 나뉜다. 지수 비트에는 소수점의 위치를 기록하고 가수 비트에는 수 전체의 형태를 기록한다.

    출처: [https://ooeunz.tistory.com/98](https://ooeunz.tistory.com/98)

- Float 및 Double의 차이

    ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%205.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%205.png)

## 3-2. Operator

- 연산자 종류
    - 기능
        - 할당 =
        - 산술 +-*/ %(나머지)
        - 비교 ==
        - 삼항 조건 A ? B : C
        - 범위 ...<
        - 부울 (Bool) && ||

            ```swift
            print(true || false) // true
            print(true && false) // false
            ```

        - 비트 논리 연산

            ```swift
            // 2진법 등 0&1의 비트 단위로 연산할 때 사용한다.
            true & true  // 이 연산은 불가하다. Swift에서 true == 1이 아니므로 Bool type은 2진법으로 나타낼 수 없기 때문이다.
            true | true
            ```

        - 복합할당 A += B (A와 B의 합을 A에 할당 A = A + B)
        - 오버플로 &+ &- &*(UInt 등 연산 시 오버플로를 자동처리 해줌)
        - 기타 (?? nil 병합 연산자, A? 옵셔널 연산자, A! 옵셔널 강제추출 연산자, -A 부호변경 연산자 등)
    - 분류
        - 전위 prefix  !A
        - 중위 infix     A + B
        - 후위 postfix A!

- 우선순위 (precedence) : 연산자 종류에 따라 우선순위가 높은 순으로 실행함 (precedencegroup 확인)
- 결합방향 (associativity) : 어느 방향부터 그룹지을 것인지 
ex. (1+2+3+4) 연산은 우선순위가 같으므로 (((1+2)+3)+4) 순으로 왼쪽부터 그룹으로 묶어 연산하므로 결합방향이 "왼쪽"임
- 사용자 정의 연산자
    - 복잡한 연산을 하나의 특수문자로 구현하는 등 강력한 무기로 활용 가능하다.
    - 전위/중위/후위연산자 모두 가능하며, 중위연산자는 우선순위 그룹을 명시 가능하다.

    ```swift
    prefix operator ** // 전위연산자 정의

    prefix func ** (value: Int) -> Int { // 전위연산자 함수 정의 - 어떤 data type에 연산자가 동작할지 구현한다.
        return value * value
    }
    //

    let five: Int = 5
    let sqrFive: Int = **five
    print(sqrFive) // 25

    prefix func ** (value: String) -> String { // 전위연산자 함수 중복 정의 - 기능을 추가 가능하다.
        return value + " " + value
    }
    //

    let str: String = **"yagom"
    print(str) // yagom yagom

    let ten: Int = 10 // 기존 기능이 제거되지 않는다.
    print(**ten) // 100
    ```

# 4. Collection Type

- Array : 멤버의 순서 (index)가 있는 리스트 컬렉션
- Dictionary : 순서가 없고, [key: value]의 쌍으로 구성된 컬렉션 (key는 중복 불가)
- Set : 순서가 없고, 중복되는 멤버가 없는 컬렉션 (집합)

    ```swift
    // Mutability of Collections
    var varArray: [Int] = [1, 2, 3] 
    varArray[0] = 100 // [100, 2, 3, 4] - mutable

    let letArray: [Int] = [1, 2, 3] 
    //letArray.append(4) // 컴파일 에러 발생  - immutable 

    var someVariable = letArray // 변수에 let 선언 collection을 할당하면
    someVariable.append(4) // [1, 2, 3, 4] - mutable
    ```

- 설명
    - Array 설명 [Array]
        - Array Handling

            ```swift
            var arrayControl: [Int] = [1,2,3,4,5]
            arrayControl.isEmpty // false

            arrayControl[1] // 2 (index 1의 위치의 값)

            arrayControl[2] = 300
            arrayControl

            arrayControl.append(6) // 맨 뒤에 추가
            arrayControl // [1,2,3,4,5,6]

            arrayControl.append(7)
            arrayControl.append(contentsOf: [8,9,10]) // 복수라서 [] 써준다

            arrayControl.insert(400, at: 3) // 특정 위치에 추가
            arrayControl.insert(contentsOf: [500,600], at: 4) // 복수라서 [] 써준다

            arrayControl.first // 1
            arrayControl.last  // 10
            arrayControl[4]

            arrayControl.firstIndex(of: 500) // element 500의 빠른 순서 index (동일한 값 500이 중복 존재할 수 있으므로 순서가 빠른 index를 반환함)
            // arrayControl.index(of: 500) // element 500의 index (.index는 deprecated)

            arrayControl.remove(at: 4) // 특정 위치 삭제
            arrayControl

            arrayControl.removeFirst()
            arrayControl.removeLast()
            arrayControl

            var firstItem = arrayControl.removeFirst() // element를 삭제 후 return 함
            var lastItem = arrayControl.removeLast()
            arrayControl

            print(firstItem, lastItem)
            print(firstItem, lastItem, separator: "", terminator: "")

            arrayControl[1...3] // 부분 출력
            //arrayControl[0...arrayControl.count]

            arrayControl
            var number: Int = arrayControl.count
            arrayControl[0...number-1]  

            arrayControl[3...5] = [400,500,600] // 여러 개 element를 수정 가능
            // arrayControl[3,4,5] = [4,5,6] // 오류 발생
            ```

        ```swift
        // **빈** Int Array 생성
        var integers: Array<Int> = [Int]() // 축약 리터럴 등 리터럴 설명은 아래 참고 🎃
        integers.append(1)  // -> [1] *append method : 요소를 맨 뒤에 추가한다.
        integers.append(100)  // -> [1, 100]
        // integers.append(100.1)  // 오류 발생

        integers.contains(100)  // -> true *contains method : 해당 요소를 포함하고 있는가?
        integers.contains(99)  // -> false

        integers[0] = 99  // index 0의 멤버 교체

        integers.remove(at: 0)  // -> [99] 제거 *remove method : (0번 index의) 요소를 제거한다.
        integers.removeLast()  // -> [100] 제거 *맨 뒤의 요소 삭제
        integers.removeAll()  // -> [] *모든 요소 삭제

        integers.count  // -> 다 제거해서 0 *요소의 개수를 확인한다.

        // integers[0]  // 오류 발생. 다 제거해서 요소가 없으므로

        // let으로 Array를 선언하면 불변 Array
        let immutableArray = [1,2,3]
        // immutableArray.append(4)  // 오류 발생
        ```

        ```swift
        var integers: [Int] = [1, 50, 100]

        integers[0] = 20 // 기존의 element 변경
        print(integers)  // [20, 50, 100]

        integers[3] = 300 // 런타임 에러 발생 (컴파일 에러는 아님) - Fatal error: Index out of range
        //print(integers)
        ```

        Dictionary와 달리 Array는 invalid index를 넣는 경우, 새로운 item이 추가되지 않는다.
        `shoppingList[100]= "new item"` // 런타임 에러 발생

        - [x]  컴파일 에러
            - Compilation error 
            프로그램의 실행을 막는 오류이다. 컴파일러가 이해하지 못하는 코드가 발견되면 컴파일 오류가 발생한다. 보통 문법적 오류에 기인한다.
        - [x]  런타임 에러
            - Run-time error
            프로그램 실행 중에 발생하는 오류이다. 프로그램이 수행 불가한 작업을 시도하면 발생한다. 설계 미숙에 기인한다. 
            ex. 무한루프, 0으로 나누기, 생성되지 않은 객체를 참조 (존재하지 않는 메모리 위치에 접근) 등
    - Dictionary 설명 [key: value]
        - Dictionary Control
            - [ ]  print(dicCtl["key2"]) // Optional(200) 
            print(dicCtl["key2", default: 0]) // 200 → nil이면 default로 설정한 값 0이 출력되므로 nil 발생 가능성이 없어서?

            ```swift
            var dicCtl: [String: Int] = [:]
            dicCtl.isEmpty // true

            dicCtl["key1"] = 100 // key:value 추가 (순서 X)
            dicCtl["key2"] = 200
            dicCtl["key3"] = 300
            // dicCtl["key3"] = 300, 310, 320 // 오류 발생
            print(dicCtl) // ["key1": 100, "key2": 200, "key3": 300]

            dicCtl.count // 3

            dicCtl["key1"] // 100
            dicCtl["key4"] // nil (Array와 달리 에러 발생 안하지만, nil return)

            dicCtl.removeValue(forKey: "key1") // 특정 key:value pair 삭제 
            // dicCtl.removeValue(forKey: "key2","key3") // 오류 발생
            dicCtl["key3"] = nil // 특정 key:value pair 삭제 

            print(dicCtl["key2"]) // Optional(200) -> Dictionary key 중에서 "key2"에 해당하는 것이 없으면 nil 발생 가능성이 있으므로 Optional 이다.
            print(dicCtl["key2", default: 0]) // 200 -> ? 왜 이건 Optional이 아니지? nil이면 default로 설정한 값 0이 출력되므로 nil 발생 가능성이 없어서?

            dicCtl.removeValue(forKey: "key2")
            print(dicCtl["key2", default: 0]) // 0 (nil이면 default로 정해둔 값이 출력됨)

            print(dicCtl) // 모두 삭제하면 [:] empty Dictionary가 출력된다. (nil이 아님)
            ```

            - Dictionary의 `updateValue(_:forKey:)` method를 통해 특정 key에 대한 value를 설정(set)하거나 업데이트(update)한다. 해당 key가 기존 key에 없으면 value를 설정하고, 있으면 value를 업데이트한다.
            단, updateValue 메서드는 업데이트 이후 old value (기존의 값)을 반환한다.

                ```swift
                var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]

                if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
                    print("The old value for DUB was \(oldValue).")
                } // Prints "The old value for DUB was Dublin." - 기존 값이 반환된다.

                print(airports) // ["LHR": "London Heathrow", "DUB": "Dublin Airport", "YYZ": "Toronto Pearson"] // key DUB에 대한 value는 업데이트 되었다.
                ```

            ```swift
            var anyDic: [String: Int] = [:]

            anyDic["1"] = 1
            anyDic.removeAll()

            print(anyDic) // [:] 출력 - nil이 아님

            anyDic["1"] = 1
            anyDic["2"] = 2
            anyDic["1"] = nil
            anyDic["2"] = nil

            print(anyDic) // [:] 출력 - nil이 아님 ["1": nil, "2": nil] X
            ```

            - [ ]  왜 nil이 아니라 empty Dictionary가 출력되지? print(anyDic) 결과로 [:]이 출력되는데 ["1": nil, "2": nil] 이 아닌 이유는?
                - Dictionary의 type이 Optional이 아니라서???!!!!

                ```swift
                var d = ["foo": nil] as [String: Any?]

                d["foo"] = nil // d is now [:]
                print(d)

                let y: String? = nil
                d["foo"] = y // d is now [:]
                print(d)

                let x: Any? = nil
                d["foo"] = x // d is ["foo": nil]
                print(d)

                d.removeValue(forKey: "foo")
                print(d) // [:]
                ```

                [https://stackoverflow.com/questions/39630322/swift-setting-dictionary-value-to-nil-confusion](https://stackoverflow.com/questions/39630322/swift-setting-dictionary-value-to-nil-confusion)

                - A-1 : If you assign 'nil' as the value for the given key, the dictionary removes that key and its associated value.

        ```swift
        // key가 String 타입이고, value가 Any 타입인 빈 Dictionary 생성
        // *마찬가지로 축약 리터럴로 Dictionary<String,Any>와 [String: Any]는 동일한 표현이다. 

        var anyDictionary: Dictionary<String,Any> = [String: Any]()
        anyDictionary["someKey"] = "value" // "someKey"라는 key에 할당한 값이 "value"이다.
        anyDictionary["anotherKey"] = 100

        anyDictionary // ->["someKey": "value", "anotherKey": 100]

        anyDictionary.removeValue(forKey: "anotherKey") // *특정 key:value pair를 제거한다.
        anyDictionary["someKey"] = nil // 특정 key에 nil을 할당할 수 있다.
        anyDictionary // -> [:]

        // 선언할 때 값을 할당해줌
        let initializedDictionary: [String: String] = ["name": "yagom", "gender": "male"]

        let someValue: String = initializedDictionary["name"] // 오류 발생
        // 원래는 key name에 해당하는 값인 yagom -> someValue 값으로 할당
        // 사람 입장에서는 이 과정이 가능할 것으로 보이지만, yagom이 없을 수도 있다는 불확실성 때문에 안된다.
        => optional 개념 배운 이후
        let someValue: String? = initializedDictionary["name"]  // 이렇게 하면 가능함
        ```

    - 🎃 참고 - 리터럴

        ```swift
        동일한 표현

        var integers: Array<Int> = Array<Int>()

        // var integers: Array<Int> = Array<Int>()  // 타입 & 생성
        // var integers: Array<Int> = [Int]()
        // var integers: Array<Int> = []  // type을 명시했다면 []으로 빈 배열 생성 가능함
        // var integers: [Int] = Array<Int>()
        // var integers: [Int] = [Int]()
        // var integers: [Int] = []
        // var integers = Array<Int>()
        // var integers = [Int]()
        // var integers = [1,2,3]   
        // var integers = []  // 불가 (Empty collection literal requires an explicit type)
        // var integers = [1.1, 2.2, 3.3]  // 불가 (Cannot convert value of type 'Double' to expected element type 'Array<Int>.ArrayLiteralElement' (aka 'Int'))

        // *Array Init
        var integers = [Int](1...3)  // [1,2,3] 생성
        var integers = Array(1...3)  // [1,2,3] 생성
        var integers = Array(repeating: 1, count: 5)  // [1,1,1,1,1] 생성  

        var anyDictionary: Dictionary<String, Any> = Dictionary<String, Any>() 

        // var anyDictionary: Dictionary <String, Any> = Dictionary<String, Any>() 
        // var anyDictionary: Dictionary <String, Any> = [String: Any]()
        // var anyDictionary: Dictionary <String, Any> = [:] 
        // var anyDictionary: [String: Any] = Dictionary<String, Any>() 
        // var anyDictionary: [String: Any] = [String: Any]() 
        // var anyDictionary: [String: Any] = [:] 
        // var anyDictionary = Dictionary<String, Any>() 
        // var anyDictionary = [String: Any]()
        // var anyDictionary = ["HJ": 20, "SB": 23]
        // var anyDictionary = [:]  // 불가 (Empty collection literal requires an explicit type)
        ```

    - Set 설명 {Set}

        ```swift
        // 빈 Int Set 생성
        // *축약 리터럴 없음, 순서 (index) 없음
        var integerSet: Set<Int> = Set<Int>()
        integerSet.insert(1)  // *요소 추가 method 
        integerSet.insert(100)
        integerSet.insert(99)
        integerSet.insert(99)
        integerSet.insert(99) // set는 중복된 데이터는 추가 저장하지 않는다.

        integerSet // -> {100,99,1}
        integerSet.contains(1) // -> true
        integerSet.contains(2) // -> false

        integerSet.remove(100)   // 100 삭제
        integerSet.removeFirst() // 1 or 99 삭제 (순서가 없으므로 랜덤으로 삭제된다.)

        integerSet.count

        // 집합 개념으로 접근하기
        let setA: Set<Int> = [1,2,3,4,5]  // {3,4,2,1,5} - [Array element]와 {Set elements}를 구분하기 위해 {}로 쓰는듯
        let setB: Set<Int> = [3,4,5,6,7]  // {6,3,4,5,7}
        // let setC: Set<Int> = {1,2,3,4,5}  // 오류 발생 
        print(setA)  // [4,1,3,2,5] (순서 랜덤)

        // 합집합
        let union: Set<Int> = setA.union(setB) // {4,1,3,2,6,7,5}
        print(union) // [4, 1, 3, 2, 6, 7, 5] 출력 (순서 랜덤)

        // 합집합 오름차순 정렬 - 동일한 타입의 Array로 변환하는 method
        // Array 뿐만 아니라 Set도 sorted 메서드가 있다.
        let sortedUnion: [Int] = union.sorted() // [1,2,3,4,5,6,7] - 오름차순(<, ascending order)으로 정렬된다. descending order로 정렬하려면 sorted(by: >) 메서드를 사용한다.
        // let sortedUnion: Array<Int> = union.sorted()
        print(sortedUnion) // [1, 2, 3, 4, 5, 6, 7] 출력 - 순서 고정!

        // 교집합
        let intersection: Set<Int> = setA.intersection(setB) // {5,3,4}
        print(intersection) // [5, 4, 3] 출력 (순서 랜덤)

        // 차집합
        let subtracting: Set<Int> = setA.subtracting(setB) // {2,1}
        print(subtracting) // [2, 1] 출력 (순서 랜덤)
        ```

        ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%206.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%206.png)

        - [ ]  Declaration에 보면 sorted 메서드의 return type이 [Int]인데, Discussion의 상수 sortedStudent를 보면 [String] type으로 return 됨 ???
- 자주 쓰는 메서드

    ```swift
    collection1.sort() // collection에 저장된 element의 순서를 정렬한다. (default: 오름차순/A-Z)
    var var1 = collection1.sorted() // collection에 저장된 element에는 영향이 없다. 변수에 할당해야 사용 가능하다. (Set도 사용 가능-Array를 반환)

    collection2.shuffle() // collection에 저장된 element의 순서가 바뀐다. (Set는 사용 불가)
    var var2 = collection2.shuffled() // collection에 저장된 element에는 영향이 없다. 변수에 할당 가능하다.

    var array1 = [1,2,3,4,5]
    var array2 = array1[0...2] // index range에 해당하는 array의 일부분을 꺼낼 수 있다.
    print(array2) // [1,2,3]
    ```

# 5. Function

함수 호출 시, argument 앞에 있는 것이 무조건 argument label이다!
함수 선언 시, argument label을 따로 정의하지 않으면, default로 parameter name을 argument label로 사용한다!

- 함수 선언 및 호출

    ```swift
    // 함수 선언 syntax
    func 함수이름(매개변수1이름: 매개변수1타입, 매개변수2이름: 매개변수2타입) -> 반환타입 {
    		 함수 구현부
    	   return 반환값
    }

    func sum(a: Int, b: Int) -> Int {
         return a + b
    }
    sum(a: 3, b: 5)  // 8
    // * JS에서는 함수호출 때 매개변수를 따로 안써줬지만 여기는 쓰는구나.
    // (JS에서는 sum (3,5)로 호출 가능) -> swift에서 이렇게 하려면 _를 추가하면 된다. (전달인자 레이블에 와일드카드 식별자를 사용) func sum(_ a: Int, _ b: Int) -> Int {}

    // 반환값이 없는 함수 선언 syntax1
    func 함수이름(매개변수1이름: 매개변수1타입, 매개변수2이름: 매개변수2타입) -> Void {   // *Void : 없다라는 뜻의 타입 별칭
    		 함수 구현부
    	   ~~return~~
    }  

    func printMyName(name: String) -> Void {
    	   print(name)
    }
    printMyName(name: "yagom")  // yagom

    // 반환값이 없는 함수 선언 syntax2
    func 함수이름(매개변수1이름: 매개변수1타입, 매개변수2이름: 매개변수2타입) {   // Void 자체를 생략할 수 있다.
    		 함수 구현부
    	   ~~return~~
    }  

    // 매개변수가 없는 함수
    func 함수이름() -> 반환타입 {
    	   함수구현부
         return 반환값
    }

    func hello() -> Void {
    	   print("hello")
    }
    ```

- 매개변수 기본값, 전달인자 레이블, 가변 매개변수

    ```swift
    // 1. 매개변수 기본값
    func 함수이름(매개변수1이름: 타입, 매개변수2이름: 타입 = 기본값) -> 반환타입 {
    	   함수구현부
    		 return 반환값
    }

    func greeting(friend: String, me: String = "yagom") {
    		 print("Hello \(friend)! I'm \(me)")
    }

    // 매개변수 기본값을 가지는 매개변수는 호출 시 생략 가능함
    greeting(friend: "hana") // Hello hana! I'm yagom
    greeting(friend: "john", me: "eric") // Hello john! I'm eric

    // 2. 전달인자 레이블 (Argument Label)
    // 함수를 호출할 때 함수 사용자의 입장에서 매개변수의 역할을 명확하게 표현할 때 사용함
    // 전달인자 레이블을 지정하면, 함수명을 변경하는 효과를 가지므로 중복정의 (overload) 역할을 수행함 *overload : 함수이름은 동일하나 매개변수 (전달인자 레이블만 다르더라도 가능), 반환타입 등을 다르게 하여 함수를 중복 선언 가능함 (함수, 서브스크립트, 생성자)

    func greeting(to friend: String, from me: String) {
    		 print("Hello \(friend)! I'm \(me)")  // *함수 내부에서는 매개변수이름을 사용
    }

    // 함수를 호출할 때에는 전달인자 레이블을 사용해야 함
    greeting(to: "hana", from: "yagom") // Hello hana! I'm yagom *함수 외부에서는 전달인자 레이블을 사용
    // greeting(friend: "Why", me: "not") // 오류 발생

    // overload test
    func greeting(to friend: String, from me: String = "kevin") {
             print("Hello \(friend)! I'm \(me)")  
    }
    greeting(to: "hana") // Hello hana! I'm kevin - overload 했으므로 I'm yagom이 아님!

    // 전달인자 레이블을 사용하고 싶지 않은 경우 와일드카드 식별자 사용 가능
    // 와일드카드 식별자를 사용하는 것도 런달인자 레이블을 설정하는 것에 속하므로 overload 이다.
    func greeting(_ friend: String, _ me: String) {
    		 print("Hello \(friend)! I'm \(me)")  
    }
    greeting("hana", "yagom") // Hello hana! I'm yagom *함수 호출 시 parameter를 안써도 됨

    // 3. 가변 매개변수...
    // 전달 받을 값의 개수를 알기 어려울 때 사용함
    // 함수는 여러 개의 가변 매개변수를 가질 수 있음 (Swift 버전 5.4 이후부터)

    func 함수이름(매개변수1이름: 타입, 매개변수2이름: 타입...) -> 반환타입 {
    	   함수구현부
    		 return 반환값
    }

    func sayHelloToFriends(me: String, friends: String...) -> String {
        return "Hello \(friends)! I'm \(me)!"
    }

    // 매개변수2에 여러 멤버가 들어가는 경우
    print(sayHelloToFriends(me: "yagom", friends: "hana", "eric", "wing"))
    // Hello ["hana", "eric", "wing"]! I'm yagom! // 가변 매개변수로 들어온 arguments 값은 Array 처럼 사용됨

    // 가변매개변수를 생략하고 함수를 호출할 수 있음 (friends: nil은 불가함)
    print(sayHelloToFriends(me: "yagom"))
    // Hello []! I'm yagom! // empty array 형태로 출력됨

    // 4. in-out 매개변수
    // inout 키워드를 통해 입출력 매개변수 사용 가능 (매개변수 기본값 지정 불가, 가변 매개변수로 사용 불가)
    var numbers: [Int] = [1,2,3]

    func nonReferenceParameter(_ arr: [Int]) { // 함수 내부/외부 단절 (내부 이벤트가 함수 외부에 영향을 주지 않음)
    		var copiedArr: [Int] = arr // 함수에 arguments를 전달할 때, 복사된 값이 전달됨 (CS50-메모리 교환 참고)
    		copiedArr[1] = 1 // 결과적으로 index 1의 값이 변경되지 않음
    }

    func referenceParameter(_ arr: inout [Int]) { // in-out parameters - 함수 내부에서 수정된 매개변수를 함수 외부에서 사용 가능
    		arr[1] = 1 // 값이 아닌 주소가 전달됨, 결과적으로 index 1의 값이 변경됨
    }

    nonReferenceParameter(numbers)
    print(numbers[1]) // 2 

    referenceParameter(&numbers) // & : 주소 추출 연산자 
    print(numbers[1]) // 1
    ```

    ```swift
    func arrayParameter(a numbers: [Double]) -> String {
        return "\(numbers)"
    }

    func variadicParameter(b numbers: Double...) -> String {
        return "\(numbers)"
    }

    print(arrayParameter(a: [1, 2, 3, 100])) // [1.0, 2.0, 3.0, 100.0]
    print(variadicParameter(b: 3.5, 4.5, 4.5)) // [3.5, 4.5, 4.5]

    print(arrayParameter(a: [])) // []
    print(variadicParameter(b: )) // (Function)

    //print(arrayParameter()) // 컴파일 에러 발생 - Missing argument for parameter #1 in call  <- array는 parameter 생략이 불가하다.
    print(variadicParameter()) // []  <- 가변 매개변수는 parameter 생략이 가능하다.
    ```

    - [x]  가변 매개변수는 함수당 하나만 가질 수 있음? 여러 개 되는데?
        - *Swift 버전 5.4 이후부터 여러 개의 가변 매개변수를 가질 수 있도록 변경되었다.

- data type으로서의 함수 (변수에 함수 할당, 전달인자/반환 값으로 전달)
    - 스위프트는 함수형 프로그래밍 패러다임을 포함하는 다중 패러다임 언어이므로 스위프트의 함수는 일급객체이다. 
    그래서 1) 함수를 변수에 할당하고, 2) 매개변수의 data type으로 할당하고, 3) 전달인자로 전달하고, 4) 반환 값으로 반환하는 것이 가능하다.

    ```swift
    // 함수의 타입이란?
    func sayHello(name: String, times: Int) -> String {}
    // 여기서 함수의 타입은 (String, Int) -> String 이다.
    // 참고 - 전달인자 레이블은 함수 타입의 구성요소가 아님

    // 변수의 타입을 함수로 선언했다. 즉, 변수를 "함수 타입"으로 할당하겠다는 의미이다.
    var someFunction: (String, String) -> Void = greeting(to:from:)
    **someFunction("eric", "yagom")
    ```

    ```swift
    // 함수의 타입 표현
    // 변수에 함수 할당 시, 반환타입 생략불가
    var 변수이름: (매개변수1타입, 매개변수2타입) -> 반환타입 = 함수이름(매개변수1:매개변수2:)

    -
    func greeting(to friend: String, from me: String) {
        print("Hello \(friend)! I'm \(me)")
    }

    // 1) 함수 자체를 변수에 할당 가능하다.
    var someFunction: (String, String) -> Void = greeting(to:from:) // 함수의 축약표현
    **// 매개변수 String 타입이고 반환값이 없는 "함수"를 할당할거야! 변수 someFunction에다가 - 라는 의미임

    someFunction("eric", "yagom") // Hello eric! I'm yagom (변수 자체가 함수이기 때문에 바로 실행시킬 수 있음) *즉, someFunction(매개변수1:arg1, 매개변수2:arg2) 안써도 됨!

    someFunction = greeting2(friend:me:) // 매개변수 type이 동일한 (String) 다른 함수도 할당 가능함
    someFunction("eric", "yagom") // Hello eric! I'm yagom

    // *주의 - friends가 가변매개변수라서 type이 다르다고 판단함
    // someFunction = sayHelloToFriends(me: friends:) // 오류 발생. 타입이 다른 함수는 할당할 수 없음

    // 2) 함수 자체를 매개변수의 data type으로 할당 가능하다.
    // 매개변수 myParameters의 type은 함수이다. (1) 함수의 매개변수는 String이고, 2) 반환값이 없고, 3) 그 함수의 매개변수의 값(argument)은 각각 "jenny", "mike"이다. 라는 의미임
    func runAnother(myParameters: (String, String) -> Void) {
        myParameters("jenny", "mike")  // 그래서 매개변수를 바로 실행시킬 수 있음. 함수 type 이니까!
    }
    // 원래 형태는 func runAnother(parameter1: String, parameter2: String){어쩌구}

    runAnother(myParameters: greeting(friend:me:)) // 매개변수 myParameters를 다른 함수에 직접 넘겨줄 수 있다. *greeting 함수에 매개변수 "jenny", "mike"를 넣어주는 효과 (???)
    // Hello jenny! I'm mike 

    runAnother(myParameters: someFunction) // 또한 함수가 할당되어있는 변수에도 넘겨줄 수 있다.
    // Hello jenny! I'm mike 

    // 3) 함수 자체를 argument(전달인자)로 전달 가능하다.
    typealias CalculateTwoInts = (Int, Int) -> Int // 함수 type

    func addTwoInts(_ a: Int, _ b: Int) -> Int {
        return a + b
    }
    func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
        return a * b
    }

    var mathFunc: CalculateTwoInts = addTwoInts // 변수 type 생략 가능 (전달인자 레이블 유무와 관계없이 가능)
    print(mathFunc(1,2))

    func printMathResult(_ hereMathFunc: CalculateTwoInts, _ a: Int, _ b: Int) { 
        print("Result: \(hereMathFunc(a, b))")  // \()으로 이런 것도 가능하다.
    }
    printMathResult(addTwoInts, 3, 4) // Result: 7 출력 - 함수 자체를 argument로 전달함

    // 4) 함수 자체를 반환값으로 반환 가능하다.
    func chooseMathFunc(_ toAdd: Bool) -> CalculateTwoInts {
        return toAdd ? addTwoInts : multiplyTwoInts // 조건에 따라 특정 함수를 반환함
    }
    printMathResult(chooseMathFunc(true), 4, 5) // Result: 9 출력 - chooseMathFunc(true) 결과로 addTwoInts 함수가 반환됨
    ```

    - [x]  다시 이해하기
    runAnother(myParameters: greeting(friend:me:)) // 매개변수 myParameters를 다른 함수에 직접 넘겨줄 수 있다. *greeting 함수에 매개변수 "jenny", "mike"를 넣어주는 효과 (???)
    // Hello jenny! I'm mike

        ```swift
        func sumA(a: Int, b: Int) { // 함수의 type : (Int, Int) -> Void
            print(a + b)
        }
        sumA(a: 1,b: 2)  // 3 출력

        func multA(a: Int, b: Int) { // 함수의 type : (Int, Int) -> Void
        		print(a * b)
        }
        multA(a: 3, b: 4)  // 12 출력

        func parametersDataTypeFunc(testParameters: (Int, Int) -> Void) { // testParameters이라는 parameter의 type은 함수인데, 이 함수는 parameter가 Int이고 반환값이 없는 type이다.
            testParameters(10,20)  // 함수명() 형태로 함수 호출처럼 실행해준다. "매개변수로 이 값을 할당해주는 함수" 타입의 (매개)변수
        }

        parametersDataTypeFunc(testParameters: sumA(a:b:))  // 30 출력
        parametersDataTypeFunc(testParameters: multA(a:b:)) // 200 출력

        // sumA(testParameters)  // 이건 불가
        // sumA(parametersDataTypeFunc(testParameters: <#T##(Int, Int) -> Void#>))  // 이것도 불가 sumA의 매개변수 type은 Int 여야 하므로

        // 설명
        parametersDataTypeFunc 함수의 testParameters라는 parameter의 Data Type이 함수이다. 
        그래서 매개변수 자체가 함수처럼 "함수명()"을 통해 실행된다.

        원래는 매개변수를 각각 sumA(a:10, b:20) 이렇게 써줘야 하는데,
        testParameters(10,20)을 통해 그 과정을 대신한다.
        즉, 특정 매개변수의 세트를 여러 개의 함수에 테스트할 때 활용할 수 있다.

        매개변수에 (a,b)를 넣어주는 함수! 라고 볼 수 있다.
        ```

        ```swift
        // 다른 예시

        typealias calculateTwoInts = (Int, Int) -> Int

        func addTwoInts(_ a: Int, _ b: Int) -> Int {
            return a + b
        }

        var mathFunc: calculateTwoInts = addTwoInts // 함수명만 써도 할당 가능 (매개변수 기본값 유뮤와 상관없이)
        print(mathFunc(1,2))

        //
        func addTwoInts2(left a: Int, right b: Int) -> Int {
            return a + b
        }

        var mathFunc2 = addTwoInts2 // 변수 선언 시 type 생략 가능 (: calculateTwoInts), 할당할 함수의 전달인자 레이블을 생략 가능
        print(mathFunc2(2,3))
        ```

- 중첩 함수
    - 일반적으로 함수는 전역함수이지만, 함수 내부의 함수로 구현된 중첩함수는 상위 함수의 블럭 내에서만 사용 가능하다.
    - 기존

        ```swift
        typealias MoveFunc = (Int) -> Int

        func goLeft(_ currentPosition: Int) -> Int {
            return currentPosition - 1
        }

        func goRight(_ currentPosition: Int) -> Int {
            return currentPosition + 1
        }

        func funcForMove(_ shouldGoLeft: Bool) -> MoveFunc {
            return shouldGoLeft ? goLeft : goRight
        }

        var position: Int = 3

        let moveToZero: MoveFunc = funcForMove(position > 0) // 0보다 큰 위치에 있으므로 true가 argument로 전달됨 -> funcForMove(true) 실행 -> goLeft 함수가 반환되어 상수에 할당됨

        while position != 0 {
            print("\(position)...")
            position = moveToZero(position) // 즉, 할당된 goLeft(position)을 호출 -> 왼쪽으로 이동
        } // 3... 2... 1... 호출
        ```

    - 중첩 함수 사용
        - 목적 - 굳이 전역함수로 사용할 필요가 없으므로 사용 범위를 한정하고 싶은 경우에 사용한다. 즉, 상위 함수인 funcForMove 함수블럭 내에서만 사용 가능하다.

        ```swift
        typealias MoveFunc = (Int) -> Int

        func funcForMove(_ shouldGoLeft: Bool) -> MoveFunc {   
        		func goLeft(_ currentPosition: Int) -> Int {
        		    return currentPosition - 1
        		}

        		func goRight(_ currentPosition: Int) -> Int {
        		    return currentPosition + 1
        		}
        		
        		return shouldGoLeft ? goLeft : goRight
        }

        var position: Int = 3

        let moveToZero: MoveFunc = funcForMove(position > 0) // 0보다 큰 위치에 있으므로 true가 argument로 전달됨 -> funcForMove(true) 실행 -> goLeft 함수가 반환되어 상수에 할당됨

        while position != 0 {
            print("\(position)...")
            position = moveToZero(position) // 즉, 할당된 중첩함수 goLeft(position)을 호출 -> 왼쪽으로 이동
        } // 3... 2... 1... 호출
        ```

- 비반환 함수 (Nonreturning function)
    - 정상적으로 종료되지 않는 함수를 의미한다. 프로세스가 종료되며 비반환 함수 내에서 오류를 던지거나, 중대한 시스템 오류를 보고하는 등의 목적으로 사용한다.
    - 어디서든 호출 가능하다. (guard 구문의 else 블록에서도 호출 가능하다.)
    - 반환 타입을 Never로 명시한다.

        ```swift
        func crashAndBurn() -> Never {
            fatalError("Something bad happened.")
        }

        crashAndBurn() // console 에러메시지 - Fatal error: Something bad happened.
        ```

- 반환값을 무시 가능한 함수
    - 함수 선언 시 반환값이 있다고 선언했지만, 반환값이 필요 없는 특정 상황에 사용한다.
    - @discardableResult 선언 속성을 사용한다.

        ```swift
        func say(_ sth: String) -> String {
            print(sth)
            return sth
        }

        @discardableResult func discardableResultSay(_ sth: String) -> String {
            print(sth)
            return sth
        }

        say("Hello") // 반환값을 사용하지 않았으므로 컴파일러 경고가 나타날 수 있다.

        discardableResultSay("Hello") // 반환값을 사용하지 않아도 컴파일러 경고가 나타나지 않는다.
        ```

        - [ ]  속성이란?
            - 1) 선언 또는 2) type 또는 3) 스위치 케이스에 대한 부가 정보를 나타낸다.
            - @available, @discardableResult, @objc 등이 있다.

                ```swift
                @속성이름
                @속성이름(매개변수)
                ```

- 자주 혼동하는 내용

    ```swift
    func test() -> Int {
        print(1)
        print(2)
        print(3)
        return -4
    }

    test() // 1 2 3
    print(test()) // 1 2 3 -4 <- print 뿐만 아니라 반환값도 출력된다. 왜지???
    var var1 = test() // 1 2 3 <- 함수의 반환값이 할당됨과 동시에 함수가 실행된다. (함수 내부의 print 함수가 실행됨)
    ```

    '함수의 반환 값'을 상수에 할당하면, 그 과정에서 함수가 호출된다!

## 5-2. Method

- 기본적으로 함수와 동일하지만, Type이나 인스턴스와 연관된 함수를 Method라고 한다.
- 다른 언어와 달리 구조체와 열거형도 메서드를 가질 수 있다.
- 특정 메서드가 자신 Type의 인스턴스 내부의 프로퍼티 값을 변경할 때, Class는 상관 없지만 Struct 등 값 타입은 `mutating` 키워드가 필요하다.

    ```swift
    struct LevelStruct {
        var level: Int = 0 {
            didSet {
                print("Now Level is \(level)")
            }
        }
        
        mutating func levelUp() { // mutating 키워드가 없으면 에러 발생 - Left side of mutating operator isn't mutable: 'self' is immutable.
            print("Level UP!")
            level += 1
        }
    } 

    var levelInstance: LevelStruct = LevelStruct()
    levelInstance.levelUp() // Level UP!, Now Level is 1
    levelInstance.levelUp() // Level UP!, Now Level is 2
    ```

- 인스턴스를 함수처럼 호출하는 메서드 (callAsFunction)
    - 사용자 정의 명목 타입의 호출 가능한 값 (Callable values of user-defined normal type)을 구현하기 위해 인스턴스를 함수처럼 호출 가능하게 하는 메서드 (Call-as-function method)가 있다.
    - callAsFunction 메서드를 구현하여 사용한다. parameter type, return type만 다르면 동일한 메서드를 생성 가능하다. (mutating, throws/rethrows 사용 가능)

        ```swift
        struct Puppy {
            var name: String = "멍멍이"
            
            func callAsFunction() { // 인스턴스를 함수처럼 호출 가능하게 한다.
                print("멍멍")
            }

            func callAsFunction(destination: String) { // parameter type, return type만 다르면 동일한 메서드를 생성 가능
                print("\(destination)으로 달려갑니다.")
            }
            
            func callAsFunction(color: String) -> String {
                return "\(color) 응가"
            }
            
            mutating func callAsFunction(name: String) {
                self.name = name
            }
        }

        var dogInstance: Puppy = Puppy()

        dogInstance.callAsFunction() // 멍멍
        dogInstance() // 멍멍 *위 코드와 동일한 표현이다. (인스턴스를 함수처럼 사용!)

        dogInstance.callAsFunction(destination: "집") // 집으로 달려갑니다.
        dogInstance(destination: "공원") // 공원으로 달려갑니다.

        print(dogInstance(color: "무지개색")) // 무지개색 응가

        dogInstance(name: "Changed - 댕댕이")
        print(dogInstance.name) // Changed - 댕댕이

        // let allocate: (String) -> Void = dogInstance(destination:) // 메서드 호출 외에 일반적인 함수 표현으로는 사용 불가
        let allocate2: (String) -> Void = dogInstance.callAsFunction(destination:) // 이렇게는 가능
        ```

# 6. 조건문 (condition)

- 참고 - [https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html)
- if
    - In its simplest form, the if statement has a single if condition. It executes a set of statements only if that condition is true.
    if it is true, a message is printed. Otherwise, no message is printed, and code execution continues after the if statement’s closing brace. (if 문 {} 이 닫힌 후 코드가 계속 실행됩니다.)
    The if statement can provide an alternative set of statements, known as an `else clause`, for situations when the if condition is false. These statements are indicated by the `else keyword.` One of these two branches is always executed. If it is false, `else branch` is triggered instead. 
    You can chain multiple if statements together to consider additional clauses. The final else clause is optional, however, and can be excluded if the set of conditions does not need to be complete.

    ```swift
    if condition {          // (condition) 소괄호 생략가능
         statements
    } else if condition {
         statements
    } else {
         statements
    }

    let someInteger = 100

    if someInteger < 100 {
        print("100 미만")
    } else if someInteger > 100 {
        print("100 초과")
    } else {
        print("100")
    } // 100
    ```

- switch
    - A switch statement considers a value and compares it against several possible matching patterns. It then executes an appropriate `block of code`, based on the first pattern that matches successfully. A switch statement provides an alternative to the if statement for responding to multiple potential states. 
    Each case is a separate branch of code execution. The switch statement determines which branch should be selected. This procedure is known as switching on the value that is being considered. Every switch statement must be exhaustive. (That is, every possible value of the type being considered must be matched by one of the switch cases.)
    You can define a default case to cover any values that are not addressed explicitly. This default case is indicated by the `default` keyword, and must always appear last.
    - No Implicit `Fallthrough` 
    In contrast with switch statements in C and Objective-C, switch statements in Swift do not fall through the bottom of each case and into the next one by default. Instead, the entire switch statement finishes its execution as soon as the first matching switch case is completed, without requiring an explicit `break` statement. This makes the switch statement safer and easier to use than the one in C and avoids executing more than one switch case by mistake.
    - To make a switch with a single case that matches both "yagom" and "minho", combine the two values into a `compound case`, separating the values with commas.

        ```swift
        switch 비교값 {
        case 패턴:
            statements
        default:
            statements
        }

        // 범위 연산자를 활용하면 쉽고 유용함
        let someInteger = 100

        switch someInteger {
        case 0:
            print("zero")
        case 1..<100:         // 1 이상 100 미만
            print("1~99")
        case 100:
            print("100")
        case 101...Int.max:   // 101 이상 Int.max 이하
            print("over 100")
        default:
            print("unknown")  // someInteger = -100 입력하면 default에 해당됨
        } // 100 출력

        // 대부분의 기본 타입을 사용할 수 있음
        switch "yagom" {
        case "jake":
            print("jake")
        case "mina":
            print("mina")
        case "yagom", "minho":  // switch "minho"를 입력하는 경우에도 "yagom!!'이 출력됨
            print("yagom!!")
        default:
            print("unknown")
        } // yagom!! 출력

        // break는 사용하지 않음
        // 대신 fallthrough를 활용하면 다음 case의 statements도 실행됨

        switch "jake" {
        case "jake":
            print("jake")
            fallthrough
        case "mina":       // case 조건을 미충족하지만 실행됨
            print("mina")  // jake mina 출력  
        case "yagom", "minho":
            print("yagom!!")
        default:
            print("unknown")
        } 

        // switch문의 비교값으로 Tuple도 가능
        typealias NameAge = (name: String, age: Int)
        let tupleValue: NameAge = ("kevin", 30)
        var 개체: String = "인간"

        switch tupleValue {
        case ("yagom", 40) where 개체 == "인간": // where문을 통해 조건 추가 가능
            print("정확히 맞췄습니다.")
        case ("yagom", _): // *와일드카드 식별자(_) 사용 가능
            print("이름만 맞았습니다. 나이는 \(tupleValue.age)입니다.") // 값에 접근 방법-1 (tuple의 element name으로 접근)
        case (let name2, 30): // 값에 접근 방법-2 (let으로 binding 가능)
            print("나이만 맞았습니다. 이름은 \(name2)입니다.")
        case _: // "case _"는 default와 동일
            print("누굴 찾고 있나요?")
        } // 나이만 맞았습니다. 이름은 kevin입니다. 출력

        // enum에 case 추가 가능성이 있으면, @unknown 사용
        enum Menu {
            case chicken
            case pizza
            case rice
        }
        var lunchMenu: Menu = .chicken
        lunchMenu = .rice

        switch lunchMenu {
        case .chicken:
            print("반반 무 많이")
        case .pizza:
            print("핫소스 많이")
        @unknown case _:  // 스위치 케이스 속성) 향후 enum에 case를 추가할 경우, switch문을 수정하라는 컴파일러 경고가 뜸 - Switch must be exhaustive (case _ 또는 default 에만 사용가능) 
            print("오늘 메뉴가 뭐죠?")
        } // 오늘 메뉴가 뭐죠?
        ```

# 7. 반복문 (iteration)

- for-in
    - You use the `for-in loop` to iterate over a sequence, such as items in an array, ranges of numbers, or characters in a string.
    - You can also iterate over a dictionary to access its key-value pairs. Each item in the dictionary is returned as a (key, value) tuple when the dictionary is iterated, and you can decompose the (key, value) tuple’s members as explicitly named constants for use within the body of the for-in loop. In the code example below, the dictionary’s keys are decomposed into a constant called 'name', and the dictionary’s values are decomposed into a constant called 'age'.
    - 'name/'age/'index을 따로 상수 선언하지 않는 이유
    'name/'age/'index is a constant whose value is automatically set at the start of each iteration of the loop. As such, index does not have to be declared before it is used. It is implicitly declared simply by its inclusion in the loop declaration, without the need for a let declaration keyword.
    (인덱스는 루프의 각 반복 시작 시 값이 자동으로 설정되는 상수이다. 따라서 인덱스를 사용하기 전에 먼저 선언할 필요가 없다. 
    let 선언 키워드가 필요 없이 루프 선언에 포함됨으로써 암시적으로 선언된다.)

        ```swift
        for i in range {  // in Collection Type
            statements
        }

        -
        var integers = [1, 2, 3]  // Int type의 Array 생성
        let people = ["yagom": 10, "eric": 15, "mike": 12]  // String, Int type의 Dictionary 생성

        // 반복문은 Collection과 함께 활용됨
        for integer in integers {   // integers의 Array 값들을 item integer로 받고, 반복하여 출력한다. (integer를 따로 선언할 필요 없음)
            print(integer)
        }  // 123 출력

        // Dictionary의 item은 key와 value로 구성된 *튜플 tuple 타입임
        for (name, age) in people {   // people의 Dictionary key/value값들을 item (name, age)로 받고, 반복하여 출력한다.
            print("\(name): \(age)")
        } // eric: 15 yagom: 10 mike: 12 출력

        // numeric ranges에 사용 가능하다. 
        for i in 1...3 {
            print("\(i) times 5 is \(i * 5)")
        }
        // 1 times 5 is 5
        // 2 times 5 is 10
        // 3 times 5 is 15

        // 기본 data type을 item으로 사용 가능하다.
        let helloSwift: String = "Hello Swift!"
        for char in helloSwift {
            print(char)
        } // 한 줄씩 띄워서(\n) Hello Swift! 출력

        // sequence에 해당하는 값 (item)이 필요 없으면, 와일드카드 식별자(_) 사용 가능
        for _ in 1...3 {
        		result *= 10
        } // 1000 출력
        ```

- while
    - A `while loop` performs a set of statements until a condition becomes false. These kinds of loops are best used when the number of iterations is not known before the first iteration begins. Swift provides two kinds of while loops:
        - `while` evaluates its condition at the start of each pass through the loop.
        - `repeat``while` evaluates its condition at the end of each pass through the loop.

    ```swift
    while condition {   // (condition) 소괄호 생략 가능
        statements
    }
    //
    var integers: [Int] = [1,2,3] 
    print(integers)  // [1,2,3]

    staySmallLoop: while integers.count > 1 {   // 구문 이름표 지정 가능
        integers.removeLast()  // 조건이 참이면 (integers 멤버의 수가 1 초과이면) 반복해서 removeLast 해라. 
    }
    print(integers)  // [1] 출력

    while integers.count > 10 {
        integers.removeLast()
    } 
    print(integers)  // [1,2,3] 출력

    // continue 키워드로 다음 sequence로 이동 가능
    for i in 0...5 {
        if i.isMultiple(of: 2) {
            print(i)
            continue  // 여기서 종료하고 다음 sequence (item i)로 넘어감
        }
        
        print("\(i) == 홀수")
    }
    //0
    //1 == 홀수
    //2
    //3 == 홀수
    //4
    //5 == 홀수
    ```

- repeat-while
    - The `repeat-while loop` performs a single pass through the loop block first, (일단 statements를 한 번 먼저 실행하고) before considering the loop’s condition. 
    It then continues to repeat the loop until the condition is false. (조건문이 거짓이 되기 전까지 반복해서 실행한다)
    - NOTE 
    The repeat-while loop in Swift is analogous to a `do-while loop` in other languages. (다른 언어의 do-while문과 비슷하다)

    ```swift
    repeat {
        statements
    } while conditon

    -
    print(integers)  // [1,2,3] 출력

    Q1)
    repeat {
        integers.removeLast()
    } while integers.count > 1   // 일단 실행하고, 조건이 참이면 (integers 멤버의 수가 1 초과하면) 반복해서 removeLast 해라. 
    print(integers)  // [1] 출력

    Q2)
    repeat {
        integers.removeLast()
    } while integers.count > 10  // 일단 실행하고, 
    print(integers)  // [1,2] 출력
    ```

# 8. Optional

- Syntax
    - Optional : 값이 있을수도, 없을수도 있다는 의미이다. 즉, nil 의 가능성을 명시적으로 표현해준다. (nil은 옵셔널 type에만 할당 가능하다.)

        You use optionals in situations where a value may be absent. 
        An optional represents two possibilities: Either there is a value, and you can unwrap the optional to access that value, or there isn’t a value at all.

    - 사용 목적 (변수에 nil이 있음을 가정하는 이유)
    - 함수에 전달되는 argument의 값이 잘못된 값일 경우 nil을 반환한다.
    - argument를 굳이 넘기지 않아도 되는 상황 (required parameter가 아닌 상황)에서 parameter type을 optional로 정의한다.
    - 장점 
    - 문서화/주석 작성 시간 절약
    - 전달받은 값이 옵셔널이 아니라면 nil 체크를 하지 않더라도 안심하고 사용 가능하다. 따라서 예외 상황을 최소화하는 안전한 코딩을 할 수 있다.

    - 옵셔널 정의

        옵셔널은 generic이 적용된 enum (열거형)으로 구현된다. 따라서 switch문을 통해 값이 nil인지 여부를 확인 가능하다.

        ```swift
        enum Optional<Wrapped>: ExpressibleByNiliteral {   // 옵셔널 기본형은 enum (열거형)
                 case none           // 옵셔널 값이 없다 (nil이다)
                 case some(Wrapped)  // 옵셔널 값이 있다 (값이 옵셔널에 Wrapped 되었다) -> 둘 중 하나
        }

        // 동일한 표현임
        let optionalValue: Optional<Int> = nil
        let optionalValue: Int? = nil
        ```

        ```swift
        // Int
        let someConstant: Int = nil  // 오류 발생 (nil 할당 불가)

        // 옵셔널 Int
        let optionalConstant: Int? = nil  // 옵셔널이므로 nil 할당 가능 (연산 불가)

        // 참고 - 암시적 추출 옵셔널 (비추)
        var myName: String! = "kevin"	// nil 할당이 가능한 옵셔널이 아닌 변수/상수 (연산 가능)
        															// 어쨌든 옵셔널이므로 옵셔널 바인딩 및 강제추출이 가능하다. (단, nil일 때 값에 접근하면 런타임 오류 발생)
          														// 옵셔널 바인딩으로 매번 값을 하기 귀찮거나 로직상 nil 발생 가능성이 없다고 확신할 때 사용한다.
        print(myName) // Optional("kevin")
        ```

        ```swift
        // The example below uses the initializer to try to convert a String into an Int:

        let possibleNumber = "123"  // Q. 이건 상수라서 값의 변경이 불가한데 왜 nil 가능성이 있다고 보는거지? -> A. 컴파일러 기준에서 nil 가능성이 있다고 판단해서
        let convertedNumber = Int(possibleNumber)  // convertedNumber is inferred to be of type "Int?"

        // Because the initializer might fail, it returns an optional Int (= Int?), rather than an Int. 
        // The question mark indicates that the value it contains is optional, meaning that it might contain some Int value, or it might contain no value at all.
        ```

        - [x]  let possibleNumber = "123"  // -> 이 상수는 초기값 변경 불가한데 왜 nil 가능성이 있다고 보는거지?
        let convertedNumber = Int(possibleNumber)
            - 우리가 봤을 땐 "123"이라는 상수가 전혀 문제 없는데, 실제로 이 코드를 동작시키기 전까지는 컴퓨터는 모릅니다. 그래서 최대한 방어적 가정을 해야합니다. 
            단순히 컴퓨터는 Int()라는 곳 안에 "문자열"이 전달되었다고 생각하지 "123"이라는 문자열 값이 전달되었다고 생각하지 않는것입니다. 
            그래서 컴퓨터에게는 "123"을 전달할 때나 "일이삼"을 전달할 때나 똑같이 문자열이라는 타입의 값을 전달했다는 사실만 있는거죠. - yagom

        ```swift
        // 참고 - DoitSwift

        var flightCode = [ // Dictionary
        		"oz" : "아시아나",
        		"ke" : "대한항공",
        		"ze" : "이스타항공",
        		"lj" : "진에어",
        ]

        var flightNumber = "aa"

        //
        if flightCode[flightNumber] != nil {
        		print ("항공사 코드 \(flightNumber)는 \(flightCode[flightNumber]!) 입니다.")
        } else {
        		print("없는 항공사 입니다.") // "aa" key가 없으므로 nil => "없는 항공사 입니다." 출력
        }
        		
        		// or Optional binding
        		if let flightCodeA = flightCode[flightNumber] {
        				print("항공사 코드 \(flightNumber)는 \(flightCodeA) 입니다.")
        		}
        		
        //
        flightNumber = "oz" // 이때 flightNumber을 수정한다면

        // var flightCompany: String = flightCode[flightNumber] // 에러 발생 - nil 가능성 ("aa" 처럼) 때문에 String? type (옵셔널)으로 수정해야 함
        var flightCompany: String? = flightCode[flightNumber]	// 옵셔널 변수에 값이 할당되면 "값이 옵셔널에 wrapped 되었다"고 표현함			 																             		

        // 옵셔널 변수 사용 - 1) Forced unwrapping
        print(flightCompany) // 불가 - 컴파일 에러 발생 (옵셔널이 아닌 parameter에 옵셔널 argument를 전달 불가하다!)
        print(flightCompany!) // Forced unwrapping 해야 값에 접근 가능함 - 아시아나 출력

            // or 옵셔널 변수 사용 - 2) Implicitly unwrapping
            var flightCompany2: String! = flightCode[flightNumber]	// String? 대신 String!으로 선언
            print(flightCompany2)  // 가능. 단, Optional("아시아나") 출력 - coercion of implicitly unwrapped value of type String? to Any does not unwrap optional.		
        		print(flightCompany2!) // 이것도 가능 - 아시아나 출력

        		// *참고
        		func printName(name: String) {
        		    print(name)
        		}
        		printName(name: flightCompany2)  // 아시아나 출력 -> print(flightCompany2)와 다른 이유인 듯. 왜 다르지?
        		printName(name: flightCompany2!) // 아시아나 출력 
        ```

        - [x]  1), 2), 모두 출력결과가 동일함 - Optional("아시아나"), 아시아나 왜지?
            - 코드가 길어져서 오류 발생한 듯. 다시 돌리면 아님
        - [ ]  print(flightCompany2)  // 가능. 단, Optional("아시아나") 출력 
        printName(name: flightCompany2)  // 아시아나 출력 -> print(flightCompany2)와 다른 이유인 듯. 왜 다르지?

- 암시적 추출 옵셔널 (Implicitly unwrapped optional) Int!
    - nil 할당이 가능하지만, optional이 아닌 변수가 필요할 때 사용한다. 
    (nil을 할당할 수 있지만 optional binding으로 값을 추출하기 귀찮거나, 로직상 nil로 인한 런타임 오류 발생 가능성이 없을 때 사용한다. ← 비추)
    - 기존 변수처럼 연산이 가능하다. (단, 현재 nil이 할당되지 않은 경우만)
    - You can think of an implicitly unwrapped optional as giving permission for the optional to be force-unwrapped if needed.

        ```swift
        var implicitlyUnwrappedOptionalValue: Int! = 100   // 암시적 추출 옵셔널인 Int type의 변수를 선언하고

        switch implicitlyUnwrappedOptionalValue {   // Optional의 값이 nil 인지 (첫번째 .none case), 값이 있는지 (두번째 .some case) 확인하는 switch문
        case .none:
            print("This Optional variable is nil")
        case .some(let value):
            print("Value is \(value)")
        }

        // 1. 기존 변수처럼 사용 가능  
        implicitlyUnwrappedOptionalValue = implicitlyUnwrappedOptionalValue + 1

        // 2. nil 할당 가능
        implicitlyUnwrappedOptionalValue = nil
        //implicitlyUnwrappedOptionalValue = implicitlyUnwrappedOptionalValue + 1  // 참고 - 잘못된 접근으로 인한 런타임 오류 발생 (nil에는 연산을 할 수 없다)

        ```

        ```swift
        // 일반적인 옵셔널과 비교
        var optionalValue: Int? = 100

        switch optionalValue {   // 위의 암시적 추출 옵셔널과 동일함
        case .none:
            print("This Optional variable is nil")
        case .some(let value):
            print("Value is \(value)")
        }

        // 1. 기존 변수처럼 사용 불가 - 옵셔널과 일반 값 (Int type 등)은 다른 타입이므로 연산불가함
        //optionalValue = optionalValue + 1

        // 2. nil 할당 가능
        optionalValue = nil
        ```

- Optional Unwrapping (옵셔널 값 추출)
    - 옵셔널 값을 옵셔널이 아닌 변수/parameter 등에 할당할 때 Unwrapping이 필요하다.

        1) Optional binding : nil 체크 후에 값이 있으면 안전하게 추출

        2) Force unwarpping (강제 추출) : nil 값이 없으면 오류 발생하므로 비추

    - 설명
        - Optional binding 설명 (if-let을 통한 unwrapping)

            ```swift
            func printName(name: String) {  
                print(name)
            }

            var myName: String? = nil
            myName = "kevin"
            // printName(name: myName) // 오류 발생 - Optional String은 String이 아니므로 (다른 type으로 인식) 다른 함수로 전달 불가
                                       // => Value of optional type 'String?' must be unwrapped to a value of type 'String'

            * 참고
            var myName2: String! = nil // 암시적 추출 옵셔널
            myname2 = "kevin"
            printName(name: myName2)   // kevin 출력
            // ! (Implicitly unwarpped optional) type의 변수는 출력 가능 (이유는 아래에서 설명 - 강제 추출을 위한 변수!가 자동으로 입력되는 효과)
            ```

            ```swift
            // if-let 또는 if-var 구문을 활용 (상수/변수는 if문 내부에서만 사용 가능하다. else문에서도 사용 불가)

            func printName(name: String) {
                print(name)
            }
            var myName: String? = nil 

            if let name: String = myName {   // Optional String (myName)을 일반적인 String (name)으로 unwrapping 해주는 과정
                printName(name: name)
            } else {
                print("myName == nil")  
            }  // myName == nil - 출력

            // printName(name: name)  // 컴파일 오류 발생- name 상수는 if-let 구문 내에서만 사용 가능

            // ,를 사용해 한 번에 여러 옵셔널을 바인딩 할 수 있음. 단, 하나라도 nil이면 실행되지 않음
            myName = "yagom"
            var yourName: String! = nil 

            if let name = myName, let friend = yourName { // if let name :String = myName, let friend :String = yourName { *type 생략 가능
                print("\(name) and \(friend)")
            } // yourName이 nil이기 때문에 실행되지 않음 (컴파일 에러는 아님)

            yourName = "hana"

            if let name = myName, let friend = yourName {
                print("\(name) and \(friend)")
            } // yagom and hana 출력
            ```

            - [x]  여러 개
                - 위 아래 동일함

                    ```swift
                    if let firstNumber = Int("4"), let secondNumber = Int("42"), firstNumber < secondNumber && secondNumber < 100 {
                        print("\(firstNumber) < \(secondNumber) < 100")
                    }  // Prints "4 < 42 < 100"

                    if let firstNumber = Int("4") {
                        if let secondNumber = Int("42") {
                            if firstNumber < secondNumber && secondNumber < 100 {
                                print("\(firstNumber) < \(secondNumber) < 100")
                            }
                        }
                    }  // Prints "4 < 42 < 100"
                    ```

        - Force unwarpping 설명 (print(!))
            - Sometimes it’s clear from a program’s structure that an optional will always have a value, after that value is first set. 
            In these cases, it’s useful to remove the need to check and unwrap the optional’s value every time.
            - You can think of an implicitly unwrapped optional as giving permission for the optional to be force-unwrapped if needed.

            ```swift
            func printName(name: String) {
                print(name)
            }

             // 일반 옵셔널
            var myName: String? = "yagom"
            //printName(name: myName)  // 런타임 에러 발생 - must be unwrapped
            printName(name: myName!) // yagom 출력. Optional type이라 원래는 바로 못꺼내지만 !를 써서 강제 추출함 (일반적인 String type으로 넘겨주는 효과)

            myName = nil  
            // printName(name: myName!) // 단, 강제 추출 시 값이 없으면 런타임 오류 발생 -> 사용 비추

            // 암시적 추출 옵셔널
            var yourName: String! = "kevin" 
            printName(name: yourName)  // kevin 출력 - !을 안 붙여도 되는 이유는 String! 즉, 선언할 때부터 이미 type 자체에 !가 붙어있는 Implicitly unwrapped optional이기 때문이다.
            printName(name: yourName!) // kevin 출력 (이것도 가능)

            yourName = nil
            //printName(yourName)  // nil 값이 전달되기 때문에 런타임 오류 발생 -> 사용 비추
            ```

# 9. Structure (구조체)

- 특징
    - Swift 대부분 타입은 구조체로 이루어져 있다.
    - 구조체는 값 (value) 타입이다.
- 구조체 정의

    ```swift
    struct Name {   // 타입명은 Upper camel case를 사용하여 정의한다. (타입이므로 대문자)
    	/* statements */
    }

    // property : Structure에서 정의한 변수
    // method : Structure에서 정의한 함수

    -
    struct Sample {  // 타입명은 Sample 이다. (구조체의 타입?)
        var mutableProperty: Int = 100    // 가변 인스턴스 프로퍼티 (var - 값 변경 가능)    
        let immutableProperty: Int = 100  // 불변 인스턴스 프로퍼티 (let - 값 변경 불가능)
        
        static var typeProperty: Int = 100  // 타입 프로퍼티 (static : 타입 자체가 사용하는 프로퍼티) 
        
        func instanceMethod() {  // 인스턴스 메서드 
            print("instance method")
        }
       
        static func typeMethod() {  // 타입 메서드 (static : 타입 자체가 사용하는 메서드)
            print("type method")
        }
    }
    ```

- 구조체 사용
    - If you create an instance of a structure and assign that instance to a constant, you cannot modify the instance’s properties, even if they were declared as variable properties:
    This behavior is due to structures being *value types*. When an instance of a value type is marked as a constant, so are all of its properties.
        - The same is not true for classes, which are *reference types*. If you assign an instance of a reference type to a constant, you can still change that instance’s variable properties.

    ```swift
    // mutable이라는 가변 인스턴스 생성
    var mutable: Sample = Sample()  // Sample 타입의 가변 인스턴스 생성 (var)

    mutable.mutableProperty = 200  // 가변 인스턴스 프로퍼티이므로 수정 가능
    //mutable.immutableProperty = 200  // 불변 인스턴스 프로퍼티는 수정 불가하므로 컴파일 오류 발생

    -
    // immutable이라는 불변 인스턴스 생성
    let immutable: Sample = Sample() // Sample 타입의 불변 인스턴스 생성 (let) 

    //immutable.mutableProperty = 200  // 불변 인스턴스는 아무리 가변 프로퍼티라도 수정 불가하므로 컴파일 오류 발생
    //immutable.immutableProperty = 200

    -
    // 타입 프로퍼티 및 메서드
    Sample.typeProperty = 300  // Sample 자체의 type property
    Sample.typeMethod()        // Sample 자체의 type method - type method 출력

    //mutable.typeProperty = 400  // 인스턴스에서는 타입 프로퍼티나 타입 메서드를 사용할 수 없으므로 컴파일 오류 발생
    //mutable.typeMethod()
    ```

- 구조체 활용

    ```swift
    struct Student {
        var name: String = "unknown"
        var `class`: String = "Swift"  // class 자체가 키워드이기 때문에 ``으로 묶어서 변수명(프로퍼티명)임을 표시한다.
        
        static func selfIntroduce(){  // 타입 메서드
            print("Student 타입입니다")  // print("\(name)")는 왜 안되지? - instance member 'name' cannot be used on type 'Student'
        }
        
        func selfIntroduce(){  // 인스턴스 메서드
            print("저는 \(`class`)반의 \(name) 입니다.")  // *self는 인스턴스 자신을 지칭하며, 몇몇 경우를 제외하고 사용은 선택사항입니다
        }                                             // print("저는 \(self.class)반 \(name)입니다") 로 대체 가능
    }

    Student.selfIntroduce()  // Student 타입입니다 - 출력 (Student 자체의 type method)

    // 가변 인스턴스 생성
    var yagom: Student = Student()  
    yagom.name = "yagom"
    yagom.class = "스위프트"
    yagom.selfIntroduce()   // 저는 스위프트반 yagom입니다 - 출력 (타입 메서드가 아니라 인스턴스 메서드로 실행됨)

    // 불변 인스턴스 생성
    let jina: Student = Student()
    // jina.name = "jina" // 불변 인스턴스이므로 프로퍼티 값 변경 불가하므로 컴파일 오류 발생 -> 그래도 인스턴스 메서드는 호출해줄 수 있다.
    jina.selfIntroduce()  // 저는 Swift반 unknown입니다 - 출력 
    ```

    - [ ]  static func selfIntroduce(){  // 타입 메서드
            print("Student 타입입니다")  // print("\(name)")는 왜 안되지? - instance member 'name' cannot be used on type 'Student'

- [x]  Instance (인스턴스) @Java (생활코딩)
    - 인스턴스 : 구체적인 객체 (객체를 인스턴스화 한뒤에 변수에 담았다)

        *객체 : 변수와 메서드로 이루어진 기능 덩어리

    - [https://www.youtube.com/watch?v=Y370ydbIb7Y](https://www.youtube.com/watch?v=Y370ydbIb7Y)

        Class가 설계도라면, Instance는 이것을 구현한 제품 (각각의 변수를 가진)이다. 
        또는 Class가 오리지널 냉장고라면, Instance는 여러 사람이 이 유용한 냉장고를 사용할 수 있도록 냉장고를 복제한 것이다.

    - [https://www.youtube.com/watch?v=TEyLPQeo6pc&feature=youtu.be](https://www.youtube.com/watch?v=TEyLPQeo6pc&feature=youtu.be)

        객체 (클래스, 인스턴스)의 필요성

# 10. Class (클래스) - let 선언 인스턴스 (불변 인스턴스X)

- L/G
    - syntax

        ```swift
        struct Resolution {
            var width = 0  // 프로퍼티
            var height = 0
        }
        class VideoMode {
            var resolution = Resolution() // 프로퍼티 <- 에다가 struct Resolution Type의 인스턴스를 할당했음
            var interlaced = false
            var frameRate = 0.0
            var name: String?
        }
        // The first property, resolution, is initialized with a new Resolution structure instance, which infers a property type of Resolution.

        let someResolution = Resolution()  // 인스턴스 - struct Resolution Type의
        let someVideoMode = VideoMode() // 인스턴스 - class VideoMode Type의

        print("The width of someResolution is \(someResolution.width)")
        // Prints "The width of someResolution is 0"
        print("The width of someVideoMode is \(someVideoMode.resolution.width)")
        // Prints "The width of someVideoMode is 0"
        // You can drill down into subproperties, such as the width property in the resolution property of a VideoMode.
        ```

        - [ ]  맞나? // struct Resolution Type의 인스턴스를 -> 프로퍼티 resolution에 할당했음
- 특징
    - 클래스는 참조 (reference) 타입이다.
    - Swift는 다중 상속이 불가하다.
- 클래스 정의

    ```swift
    class Name {
    	/* statements */
    }
    ```

- 클래스 사용

    ```swift
    class Sample { // 클래스 정의
        var mutableProperty: Int = 100  // 가변 인스턴스 프로퍼티
        let immutableProperty: Int = 100 // 불변 인스턴스 프로퍼티
        
        static var typeProperty: Int = 100 // 타입 프로퍼티
        
        func instanceMethod() {  // 인스턴스 메서드
            print("instance method")
        }
        
        // 아래 모두 타입 메서드
        static func typeMethod() {     // 상속 시 재정의 (override) 불가 타입 메서드 - static
            print("type method - static")
        }
        
        class func classMethod() {    // 상속 시 재정의 (override) 가능 타입 메서드 - class
            print("type method - class")
        }
    }

    // 인스턴스 생성 - 참조정보 수정 가능 (가변 인스턴스 X -> var선언 인스턴스)
    var mutableReference: Sample = Sample()

    mutableReference.mutableProperty = 200
    //mutableReference.immutableProperty = 200  // 불변 프로퍼티는 수정 불가하므로 컴파일 오류 발생 (Structure와 동일)

    -

    // 인스턴스 생성 - 참조정보 수정 불가 (불변 인스턴스 X -> let선언 인스턴스)
    let immutableReference: Sample = Sample()

    immutableReference.mutableProperty = 200  // *클래스의 인스턴스는 참조 타입이므로 <let선언 인스턴스>라도 <가변 프로퍼티>의 값 변경이 가능하다! (Structure와 다름)
    //immutableReference.immutableProperty = 200  // 참조 타입이라도 불변 프로퍼티는 수정 불가하므로 컴파일 오류 발생

    //immutableReference = mutableReference  // 단, 참조정보를 변경할 수는 없으므로 컴파일 오류 발생

    -

    Sample.typeProperty = 300  // 타입 프로퍼티 및 메서드
    Sample.typeMethod()  // type method-static 출력
    Sample.classMethod() // type method-class 출력

    //mutableReference.typeProperty = 400  // 인스턴스에서는 타입 프로퍼티나 타입 메서드를 사용 불가하므로 컴파일 오류 발생
    //mutableReference.typeMethod()
    ```

- 클래스 활용

    ```swift
    class Student {
        var name: String = "unknown"  // 가변 프로퍼티
        var `class`: String = "Swift"
       
        class func selfIntroduce() {  // 타입 메서드
            print("Student 타입의 class 입니다")
        }
        
        func selfIntroduce() {  // 인스턴스 메서드
            print("저는 \(self.class)반 \(name)입니다")
        }
    }

    // 타입 메서드 사용
    Student.selfIntroduce() // Student 타입의 class 입니다 - 출력

    // var선언 인스턴스 생성
    var yagom: Student = Student()
    yagom.name = "yagom"
    yagom.class = "스위프트"
    yagom.selfIntroduce()   // 저는 스위프트반 yagom입니다 - 출력

    // let선언 인스턴스 생성
    let jina: Student = Student()
    jina.name = "jina"    // let선언 인스턴스이지만 가변 프로퍼티는 수정가능하다! (Structure와 다름)
    jina.selfIntroduce()  // 저는 Swift반 jina입니다 - 출력 (Structure에서는 unknown)
    ```

- 응용 (Do it Swift - 알람 만들기)
    - var alarmTime : class 내부에서 선언했기 때문에 로컬변수가 아니라 프로퍼티이다. 
    (function 내에서 선언했으면 로컬변수가 맞음. 즉, formatter 변수처럼 다른 function에서 독립적으로 기능함)
    따라서 해당 class 내부의 method 끼리는 그 프로퍼티를 사용할 수 있다.

        ```swift
        class ViewController {
        		var alarmTime: String = ""

        		func changeDatePicker(){
        				alarmTime = "AAA"
        		}

        		func updateTime(){
        				currentTime = "BBB"
        				if (alarmTime == currentTime) {
        						view.backgroundColor = UIColor.red
        				} else {
        						view.backgroundColor = UIColor.white
        				}
        		}
        }
        ```

# 11. Enum (열거형)

- L/G
    - An enumeration defines a common type for a group of related values. 
    Enumerations in Swift are `first-class types` in their own right. They adopt many features traditionally supported only by classes, such as computed properties to provide additional information about the enumeration’s current value, and instance methods to provide functionality related to the values the enumeration represents. Enumerations can also define initializers to provide an initial case value; can be extended to expand their functionality beyond their original implementation; and can conform to protocols to provide standard functionality.
- 특징
    - Swift 열거형은 다른 언어의 열거형과 많이 다르며, 강력한 기능을 지니고 있다.
    - 유사한 종류의 여러 값을 한 곳에 모아서 정의한 것입니다. 예) 요일, 월, 계절 등
    - 각 case는 그 자체가 고유의 값입니다. (각 case에 자동으로 정수값이 할당되지 않음)

- 열거형 정의 & 사용
    - (참고 1) The type of day is inferred when it’s initialized with one of the possible values of Weekday. Once day is declared as a Weekday, you can set it to a different Weekday value using a `shorter dot syntax`.

    ```swift
    enum Name { // 대문자 카멜케이스를 사용하여 enum명을 정의합니다. (enum 자체가 하나의 Data Type이므로) 각 case는 소문자 카멜케이스로 정의합니다.
    	case name1  // 각 case를 한 줄에 개별로 정의할 수 있다.
    	case name2
    	case name3, name4, name5  // 한 줄에 여러 개도 정의할 수 있다.
    	// ...
    }

    enum Weekday {
        case mon  // String 데이터가 아닌 case 자체의 이름이므로 case "mon" 형태로 쓰지 않음
        case tue
        case wed
        case thu, fri, sat, sun  
    }

    // 열거형 타입과 케이스를 모두 사용하여도 됩니다 ??
    var day: Weekday = Weekday.mon  // 변수 day의 타입은 Weekday라는 enum이다. *열거형의 case를 나타내는 syntax : 열거형명.case명 

    day = .tue  // 위와 같이 day의 타입을 명시한 이후로 .case명 처럼 축약하여도 무방하다. (참고 1)

    print(day) // tue 출력

    // You can match individual enumeration values with a switch statement:
    switch day {   // switch의 비교값에 열거형 타입이 위치할 때, 모든 열거형 case를 포함한다면 default를 작성할 필요가 없다. (단, case 중 하나라도 빠지면 경고 뜸)
    case .mon, .tue, .wed, .thu:
        print("평일입니다")
    case Weekday.fri:
        print("불금 파티!!")
    case .sat, .sun:
        print("신나는 주말!!")
    }
    ```

    - [x]  mon → String 이니까 "mon"으로 써야하는거 아닌가?
        - 문자열이 아닌 case 자체의 이름입니다. 변수이름도 문자열인데 큰따옴표를 쓰지 않는 것처럼, `값`으로 취급하는 문자열에만 큰따옴표를 사용합니다.

- Iterating over Enum Cases
    - For some enumerations, it’s useful to have a collection of all of that enumeration’s cases. You enable this by writing `: CaseIterable` after the enumeration’s name. 
    Swift exposes a collection of all the cases as an `allCases property` of the enumeration type:

        ```swift
        enum Beverage: CaseIterable {  // CaseIterable 프로토콜을 Adapt해야 함 (rawValue가 있는 Enum이면, enum Beverage: String, CaseIterable 형태로 정의 가능함)
            case coffee, tea, juice
        }
        let numberOfChoices = Beverage.allCases.count    // 프로토콜 내부에 allCases 라는 타입 프로퍼티가 정의되어 있음
        print("\(numberOfChoices) beverages available")  // Prints "3 beverages available"

        -

        for beverage in Beverage.allCases {
            print(beverage)
        } // coffee, tea, juice - 출력
        ```

- raw value (원시값) - 모두 동일한 type
    - 원시값 지정은 Enum 선언 시에만 가능하다. (아마도????)
    - 연관값 대신에 enumeration cases can come prepopulated with default values (called raw values), which are all of the same type.
    If a raw value is provided for each enumeration case, the value can be a string, a character, or a value of any integer or floating-point type.
    즉, 함수 type은 원시값이 될 수 없다. (단, 연관값은 될 수 있다.)
    - you don’t have to explicitly assign a raw value for each case. When you don’t, Swift automatically assigns the values for you. 
    For example, when integers are used for raw values, the implicit value for each case is one more than the previous case. If the first case doesn’t have a value set, its value is 0.
    - When strings are used for raw values, the implicit value for each case is the text of that case’s name.
    Swift enumeration cases don’t have an integer value set by default, unlike languages like C and Objective-C. In the CompassPoint example, north, south, east and west don’t implicitly equal 0, 1, 2 and 3. Instead, the different enumeration cases are values in their own right (north, south, east and west).
    - rawValue는 case별로 각각 다른 값을 가져야한다.
    - case 마다 자동으로 1이 증가된 값이 할당된다. when integers are used for raw values, the implicit value for each case is one more than the previous case.
    - rawValue를 반드시 지닐 필요가 없다면 굳이 만들지 않아도 된다.
    - rawValue를 지정한 경우 nil 가능성이 있으므로 Enum의 인스턴스는 항상 optional type이다. (실패가능한 이니셜라이저 참고)

    ```swift
    // *Int type 원시값
    enum Fruit: Int {  // rawValue를 할당할 때는 타입을 지정해준다.
        case apple = 0
        case grape = 1  // 1을 지워도 자동으로 1이 할당된다.
        case peach
     // case mango = 0  // mango와 apple의 원시값이 같으므로 mango 케이스의 원시값을 0으로 정의할 수 없다.
    }

    // raw value 값을 꺼낼 때는 enum명.case명.rawValue를 쓴다.
    print(Fruit.peach.rawValue)  // 2 출력

    // 참고 - rawValue를 지정한 경우 nil 가능성이 있으므로 Enum의 인스턴스는 항상 optional type이다. (실패가능한 이니셜라이저 참고)
    var fruitInstance: Fruit? = Fruit(rawValue: 0)
    print(fruitInstance) // optional(apple)
    var fruitInstanceImpossible: Fruit? = Fruit(rawValue: 10)
    print(fruitInstanceImpossible) // nil

    // *Int type 뿐만 아니라, Hashable 프로토콜을 따르는 모든 type을 원시값의 type으로 지정 가능하다.
    enum School: String {  // rawValue로 String 타입도 가능하다.
        case elementary = "초등"
        case middle = "중등"
        case high = "고등"
        case university
    }

    print(School.middle.rawValue)  // 중등 - 출력

    print(School.university.rawValue) // university - 출력
    // 열거형의 원시값 타입이 String일 때, 원시값이 지정되지 않았다면 "case명"을 원시값으로 사용함
    ```

- Associated Values (연관값) - 다른 type도 가능
    - it’s sometimes useful to be able to store values of other types alongside these case values. This additional information is called an `associated value`, and it varies each time you use that case as a value in your code.

        ```swift
        enum Barcode {
            case upc(Int, Int, Int, Int)  // case upc(numberSystem: Int, manufacturer: Int, product: Int, check: Int) 형태로도 연관값을 표현 가능함
            case qrCode(String)
        }
        // This can be read as: 
        //“Define an enumeration type called Barcode, which can take either a value of upc with an associated value of type (Int, Int, Int, Int), 
        //or a value of qrCode with an associated value of type String.”
        //This definition doesn’t provide any actual Int or String values — it just defines the type of associated values 
        //that Barcode constants and variables can store when they are equal to Barcode.upc or Barcode.qrCode

        var productBarcode = Barcode.upc(8, 85909, 51226, 3)
        productBarcode = .qrCode("ABCDEFGHIJKLMNOP")  // 변수의 값 변경

        // You extract 1) each associated value as a constant (with the let prefix) or a variable (with the var prefix) for use within the switch case’s body
        switch productBarcode {
        case .upc(let numberSystem, let manufacturer, let product, let check):
            print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
        case .qrCode(let productCode):
            print("QR code: \(productCode).")
        } // Prints "QR code: ABCDEFGHIJKLMNOP."

        // or If 2) all of the associated values for an enumeration case are extracted, you can place a single var or let annotation before the case name
        switch productBarcode {
        case let .upc(numberSystem, manufacturer, product, check):
            print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
        case let .qrCode(productCode):
            print("QR code: \(productCode).")
        } // Prints "QR code: ABCDEFGHIJKLMNOP."
        ```

    - rawValue와 달리 Associate Value에는 함수 type 할당이 가능하다.

        ```swift
        enum SomeEnum {
            case closureOne (String, Double -> Double)
            case closureTwo (String, (Double, Double) -> Double)
        }
        ```

        - [ ]  이걸 어떻게 활용?
- Initializing from a Raw Value / 원시값을 통한 초기화
    - If you define an enumeration with a raw-value type, the enumeration automatically receives an initializer that takes a value of the raw value’s type (as a parameter called rawValue) and returns either an enumeration case or nil. You can use this initializer to try to create a new instance of the enumeration.
    - however, *Not all possible Int values will find a matching planet. Because of this, the raw value initializer always returns an optional enumeration case. (The raw value initializer is a `failable initializer.`)

        ```swift
        enum Planet: Int {
            case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
        } // This example identifies Uranus from its raw value of 7:

        let possiblePlanet = Planet(rawValue: 7)
        // possiblePlanet is of type Planet? (optional Planet) and equals Planet.uranus

        //If you try to find a planet with a position of 11, the optional Planet value returned by the raw value initializer will be nil
        let positionToFind = 11
        if let somePlanet = Planet(rawValue: positionToFind) {  // This statement creates an optional Planet, and sets somePlanet to the value. (여기서는 nil)
            switch somePlanet {
            case .earth:
                print("Mostly harmless")
            default:
                print("Not a safe place for humans")
            }
        } else {
            print("There isn't a planet at position \(positionToFind)")
        } // Prints "There isn't a planet at position 11"

        // This example uses optional binding to try to access a planet with a raw value of 11. 
        // In this case, it isn’t possible to retrieve a planet with a position of 11 (=> nil), and so the else branch is executed instead.
        ```

    - *rawValue가 case에 해당하지 않을 수 있으므로, rawValue를 통해 초기화 한 인스턴스는 항상 옵셔널 타입이다. (nil의 가능성 때문)

        ```swift
        enum Fruit: Int {  
            case apple = 0
            case grape = 1 
            case peach

        // rawValue를 통해 초기화 한 열거형 값은 옵셔널 타입이다.
        // let apple: Fruit = Fruit(rawValue: 0)
        let apple: Fruit? = Fruit(rawValue: 0)  // 즉, apple은 optional Fruit 타입 (type Fruit?)이다. (rawValue에 대한 해당 case가 없거나, type이 달라서 nil 가능성이 있으므로???)

        // 옵셔널 타입이므로 if let 구문을 사용하면 rawValue에 해당하는 케이스를 곧바로 사용 가능하다.
        if let orange: Fruit = Fruit(rawValue: 5) {   // optional enum type인 Fruit을 일반적인 enum type Fruit 으로 바꿔주는/unwrapped 해주는 과정
            print("rawValue 5에 해당하는 케이스는 \(orange)입니다")
        } else {
            print("rawValue 5에 해당하는 케이스가 없습니다")
        } // rawValue 5에 해당하는 케이스가 없습니다 - 출력 (case 원시값은 0~2 밖에 없으므로)

        *참고 (Optional)
        if let name: String = myName {   // Optional String (myName)을 일반적인 String type (name)으로 바꿔주는/unwrapped 해주는 과정
        ```

- Recursive Enum (순환 열거형)
    - 열거형 case의 연관값이 열거형 자신이고자 할 때 사용한다. 이진 탐색 트리 등 순환 알고리즘 구현에 사용 가능하다.
    - A recursive enumeration is an enumeration that has another instance of the enumeration as the associated value for one or more of the enumeration cases. 
    You indicate that an enumeration case is recursive by writing `indirect` before it, which tells the compiler to insert the necessary layer of indirection. (indirect 키워드를 enum명 앞에 붙이거나, 특정 case에만 한정할 때는 case 앞에 붙임) 
    *Arithmetic Expression: 산술 연산

        ```swift
        indirect enum ArithmeticExpression {
            case number(Int) // number case의 연관값의 type이 Int이다.
            case addition(ArithmeticExpression, ArithmeticExpression) // addition case의 연관값 type이 enum ArithmeticExpression 이다.
            case multiplication(ArithmeticExpression, ArithmeticExpression)
        }

        let five = ArithmeticExpression.number(5) // number case의 연관값으로 5를 할당
        let four = ArithmeticExpression.number(4)
        let sum = ArithmeticExpression.addition(five, four) // 연관값 5를 addition case의 연관값 중 하나로 할당 (상수 five가 enum type이므로 가능)
        let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))

        func evaluate(_ expression: ArithmeticExpression) -> Int {  // evaluate : Recursive Function(순환 함수)
            switch expression {
            case let .number(value):
                return value
            case let .addition(left, right):
                return evaluate(left) + evaluate(right)
            case let .multiplication(left, right):
                return evaluate(left) * evaluate(right)
            }
        }

        print(evaluate(product))  // Prints "18"
        ```

- Comparable Enum (비교 가능한 열거형)
    - Comparable 프로토콜을 adapt하면, case를 비교 가능하다. (단, 프로토콜을 conform 하는 연관값만 갖거나, 연관 값이 없는 열거형만 비교 가능하다.)

        ```swift
        enum Condition: Comparable { 
        		case terrible
        		case bad   // 비교하면 앞에 위치한 case가 더 작은 값이 되므로, bad < great
        		case good
        		case great
        } 

        let myCondition: Condition = .great
        let yourCondition: Condition = .bad

        if myCondition >= yourCondition {
        		print("제 상태가 더 낫네요.")
        } else {
        		print("당신 상태가 더 낫네요")  // 당신 상태가 더 낫네요 - 출력
        }
        ```

- 열거형에 메서드 추가 가능

    ```swift
    enum Month {
        case dec, jan, feb
        case mar, apr, may
        case jun, jul, aug
        case sep, oct, nov
        
        func printMessage() {
            switch self {
            case .mar, .apr, .may:
                print("따스한 봄~")
            case .jun, .jul, .aug:
                print("여름 더워요~")
            case .sep, .oct, .nov:
                print("가을은 독서의 계절!")
            case .dec, .jan, .feb:
                print("추운 겨울입니다")
            }
        }
    }

    Month.mar.printMessage()  // 따스한 봄~ - 출력
    ```

    ```swift
    func funcA(numbers: [Double]) -> Double {
        var sum: Double = 0
        for number in numbers {
            sum += number
        }
        return sum
    }

    func funcB(numbers: [Double]) -> Double {
        let sum = funcA(numbers: numbers)
        return sum / Double(numbers.count)
    }

    enum Choose {
        case doA
        case doB

        func calculateOperation(numbers: [Double]) -> Double {
            switch self {
                case Choose.doA:
                    return funcA(numbers: numbers)
                case Choose.doB:
                    return funcB(numbers: numbers)
            }
        }
    }

    func perform(numbers: [Double], as method: Choose = Choose.doA) -> Double {
        return method.calculateOperation(numbers: numbers)
    }

    perform(numbers: [1, 2, 3, 4, 5], as: .doB) // 함수 B 실행
    perform(numbers: [1, 2, 3, 4, 5], as: .doA) // 함수 A 실행
    perform(numbers: [1, 2, 3, 4, 5])  // 함수 A 실행
    ```

## 11-2. Class & Stucture & Enum 의 차이

- 값 & 참조 차이
    - 데이터를 전달할 때, 값 (value) 타입은 데이터를 복사하여 전달하고, 참조 (reference) 타입은 데이터의 참조, 즉 메모리 위치 (C의 포인터처럼)를 전달한다.
    * struct 또는 class를 전달하는 경우 : 1) 변수에 할당, 2) 함수의 argument로 전달
        - [ ]  info.age = 1 // 에러 발생 - Cannot assign to property: 'info' is a 'let' constant ??? 왜 불가? (parameter인데 let constant라고 취급하나?)
            - Swift의 argument는 모두 상수로 취급되어 전달된다. → 어디서 확인?

        ```java
        // 데이터 전달 - 1) 변수에 할당
        struct BasicInfo {
            let name: String
            var age: Int
        }

        var yagomInfo: BasicInfo = BasicInfo(name: "yagom", age: 20)

        var friendInfo: BasicInfo = yagomInfo // *변수에 할당하여 데이터 전달 (값 전달) - friendInfo 인스턴스에 yagomInfo 인스턴스의 값을 복사하여 할당한다.
        friendInfo.age = 100 // yagomdInfo.age에 영향 없음

        print(yagomInfo.age) // 20
        print(friendInfo.age) // 100

        class Person {
            var height: Int = 0
            var weight: Int = 0
        }
        var yagom: Person = Person()

        var friend: Person = yagom // *변수에 할당하여 데이터 전달 (참조 전달) - friendInfo 인스턴스에 yagomInfo 인스턴스의 참조 (메모리 주소)를 할당한다.
        friend.height = 180 // yagom.height에 영향 있음

        print(yagom.height) // 180 (friend 인스턴스가 yagom 인스턴스를 참조하므로 값이 변경됨)
        print(friend.height) // 180

        // 데이터 전달 - 2) 함수의 argument로 전달 (parameter가 struct & class type인 경우)
        func changeBasicInfo (_ info: BasicInfo) { // parameter인데 let constant라고 취급하나 ???
        //    info.age = 1 // 에러 발생 - Cannot assign to property: 'info' is a 'let' constant ???
            var copiedInfo: BasicInfo = info
            copiedInfo.age = 1 // parameter가 값 타입이므로 yagomInfo에 영향 없음 (argument의 값을 복사하여 전달됨)
        }

        func changePersonInfo(_ info: Person) {
            info.height = 150 // parameter가 참조 타입이므로 yagomInfo에 영향 있음 (argument인 yagom Class의 참조가 전달되어 값이 변경됨)
        }

        changeBasicInfo(yagomInfo)
        print(yagomInfo.age) // 20 (함수의 1이 할당되지 않음)

        changePersonInfo(yagom)
        print(yagom.height) // 150 (함수의 150이 할당됨)

        // 참고 - 인스턴스 참조가 동일한지 확인하려면 식별 연산자 ===를 사용한다.
        print(yagom === friend) // true

        var anotherFriend: Person = Person()
        print(yagom === anotherFriend) // false
        ```

- Quiz - 조금 어려움
    - 문제

        ```swift
        struct someStruct {
            var someProperty: String = "Property"
        }

        var someStructInstance: someStruct = someStruct()

        func someFunction(structInstance: someStruct){
            var localVar: someStruct = structInstance
            localVar.someProperty = "ABC"
        }

        someFunction(structInstance: someStructInstance)
        print(someStructInstance.someProperty)  // 1. 이것의 답은?

        -

        class aClass {
            var aProperty: String = "Property"
        }

        var aInstance2 = aClass()

        func aFunction2(classInstance: aClass){
            var localVar: aClass = classInstance
            localVar.aProperty = "ABC"
        }

        aFunction2(classInstance: aInstance2)
        print(aInstance2.aProperty)  // 2. 이것의 답은?
        ```

    - 답 1

        = Property 출력

        ```swift
        struct aStruct {
            var aProperty: String = "Property"
        }

        var aInstance = aStruct()  // 구조체 인스턴스 생성

        func aFunction(structInstance: aStruct){   // 이 함수의 parameter는 구조체 aStruct 타입임
            var localVar: aStruct = structInstance // 인스턴스를 변수로 가져와서 *구조체 데이터가 전달될 때 값이 복사됨 (따라서 새로운 하나의 구조체가 들어온 것이나 마찬가지, 따로따로)
            localVar.aProperty = "ABC"             // 그 인스턴스의 프로퍼티를 수정해줌 (값이 복사된 변수의 프로퍼티가 수정된 것)
            print(aInstance.aProperty)             // (확인용) Property 출력 - 그래봤자 aInstance의 프로퍼티는 수정되지 않음
        }

        aFunction(structInstance: aInstance)
        print(aInstance.aProperty)  // Property 출력
        ```

    - 답 2

        = ABC 출력

        ```swift
        class aClass {
            var aProperty: String = "Property"
        }

        var aInstance2 = aClass()  // 클래스 인스턴스 생성

        func aFunction2(classInstance: aClass){    // 이 함수의 parameter는 클래스 aClass 타입임
            var localVar: aClass = classInstance   // 이 인스턴스를 변수로 가져와서 (참조정보를 할당함) *클래스 데이터가 전달될 때 참조정보가 전달됨 (따라서 전체가 동일한 메모리를 공유함)
            localVar.aProperty = "ABC"             // 그 인스턴스의 프로퍼티를 수정해줌 (해당 인스턴스의 참조정보가 수정된 것)
            print(aInstance2.aProperty)            // (확인용) ABC 출력 - 그래서 인스턴스 참조정보 자체가 수정됨
        }

        aFunction2(classInstance: aInstance2)
        print(aInstance2.aProperty)  // ABC 출력
        ```

- Class & Stucture & Enum 차이
    - Class
        - Apple Framework 대부분 큰 뼈대를 클래스로 구성함
        - 단일 상속 (Subclassing), 참조 타입
        - 인스턴스 : type casting 가능, deinit (디이니셜라이저) 사용 가능, reference counting (참조 횟수 계산) 가능
    - Structure
        - Swift의 대부분 큰 뼈대는 구조체로 구성함 (스위프트의 기본 데이터 타입은 모두 구조체로 구현됨)
        - 상속 불가, 값 타입
        - 다른 객체나 함수로 전달될 때 참조가 아닌 복사를 원할 때, 상속을 주고받을 필요가 없을 때 사용
    - Enum
        - 상속 불가, 값 타입, 열거형 자체가 하나의 데이터 타입 (동시에 case 하나하나를 유의미한 값으로 취급)
- 상속 (Inheritance) @생활코딩
    - 상황 : 기존에 작성된 클래스 코드의 기능을 추가하고 싶은데, 
    1) 해당 클래스를 남이 작성했거나 
    2) 이미 많은 사람들이 사용 중이라 수정하기 어려우면
    - 효과 : 그 클래스를 <상속>해준 다른 클래스를 생성하여 
    A) 내가 원하는 기능을 추가할 수 있다. - ex. sum + print
    B) 부모 클래스가 가진 기능을 수정할 수 있다. (재정의/덮어쓰기, Overriding) - ex. minus
    - 장점 : 가독성, 유지보수, 코드 재사용, 중복 최소화

        ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%207.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%207.png)

    - 참고 - overriding vs overloading
        - overloading : 기존의 함수 (sum)과 동일한 함수명을 사용하면서 형태를 변형한 경우 - ex. parameter 1개를 추가

            ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%208.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%208.png)

    - this vs super
        - this : 자기 자신
        - super : 부모 클래스

            ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%209.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%209.png)

            ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2010.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2010.png)

    - 생성자 (constructor) : 부모 클래스에서 정의한 생성자들을 자식 클래스에서 그대로 사용할 수 있는 기능
    - Polymorphism (다형성)

# 12. Closure

- 특징
    - 클로저는 실행가능한 코드 블럭이다.
    - 함수는 클로저의 일종으로, 이름이 있는 클로저이다.
    - 함수와 다르게 이름정의는 필요하지는 않지만, 매개변수 전달과 반환 값이 존재할 수 있다는 점이 동일하다.
    - 일급객체/일급시민 (first-citizen)으로 변수에 저장하거나 함수의 전달인자로 전달이 가능하다.
        - [x]  일급객체 ?
            - [https://ko.wikipedia.org/wiki/일급_객체](https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B8%89_%EA%B0%9D%EC%B2%B4)
            - 다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체를 가리킨다. 보통 함수에 매개변수로 넘기기, 수정하기, 변수에 대입하기와 같은 연산을 지원할 때 일급 객체라고 한다.
    - 함수 및 클로저는 참조 type이다.

- Syntax
    - 클로저는 중괄호 { }로 감싸져 있고, 괄호를 이용해 (parameter)를 정의한다.
    - -> 을 이용해 반환 타입을 명시한다. in 키워드로 실행 코드와 분리한다.
    - 클로저는 주로 함수의 전달인자로 많이 사용된다. 함수 내부에서 원하는 코드블럭을 실행 가능하다.

    ```swift
    { (parameter 목록) -> 반환타입 in  
        실행 코드
    }

    // 1. 함수를 변수에 할당
    func sumFunction (a: Int, b: Int) -> Int {
        return a+b
    }

    var resultSum: Int = sumFunction(a: 1, b: 2)
    print(resultSum) // 3

    // 2. closure을 변수에 다시 할당
    var sumClosure: (Int, Int) -> Int = { (a: Int, b: Int) -> Int in   // 변수 sum에 closure/함수를 할당할거야. 매개변수가 Int type이고, 반환값이 Int type인! 
        return a+b
    }

    resultSum = sumClosure(3,4)  // 변수 자체가 closure/함수이기 때문에 a:b: 없이 바로 실행시킬 수 있음
    print(resultSum) // 7
    ```

    ```swift
    let add: (Int, Int) -> Int          // 1. add라는 상수의 data type을 closure/함수로 선언한다.
    add = { (a: Int, b: Int) -> Int in  // 2. 상수 add의 초기값으로 closure/함수를 할당한다.
        return a + b
    }

    func calculate(a: Int, b: Int, method: (Int, Int) -> Int) -> Int {   // 3. method라는 parameter의 타입을 closure/함수로 선언하고, 
        return method(a, b)   // 4. 함수 내부에서 method를 호출한다. (즉, parametor로서 전달받은 closure/함수를 호출해준다.)
    }

    var calculatedA: Int
    calculatedA = calculate(a: 50, b: 10, method: add)  // 5. parametor method에 add라는 closure를 호출한다.
    print(calculatedA) // 60 

    print(calculate(a: 50, b: 10, method: add)  // 60
    print(calculate(a: 50, b: 10, method: sumFunction)  // 60 - method parameter에 closure 외에 동일한 data type을 가진 함수도 넣을 수 있다.

    // 따로 클로저를 변수에 넣어 전달하지 않고, 
    // 함수를 호출할 때 클로저 전체를 작성하여 전달할 수도 있다.

    calculatedA = calculate(a: 50, b: 10, method: { (left: Int, right: Int) -> Int in
        return left + right
    })
    print(calculatedA) // 60
    ```

    - 참고 - data type으로서의 함수 (변수에 함수를 할당)

        ```swift
        var 변수이름: (매개변수1타입, 매개변수2타입) -> 반환타입 = 함수이름(매개변수1:매개변수2:)

        func greeting(to friend: String, from me: String) {
            print("Hello \(friend)! I'm \(me)")
        }

        var someFunction: (String, String) -> Void = greeting(to:from:)
        **// 반환값이 없고, 매개변수 String 타입인 "함수"를 할당할거야! 변수 someFunction에다가 - 라는 의미임
        someFunction("eric", "yagom") // Hello eric! I'm yagom (변수 자체가 함수이기 때문에 바로 실행시킬 수 있음) *즉, someFunction(매개변수1:어쩌구, 매개변수2:저쩌구) 안써도 됨!
        ```

- 클로저 표현 간소화
    - 클로저 표현방법
        1. 후행 클로저 : 함수의 마지막 매개변수에 전달되는 클로저는 `후행클로저(trailing closure)`로 함수 외부에 구현 가능하다.
        2. 반환타입 생략 : 컴파일러가 클로저의 타입을 유추할 수 있는 경우 매개변수, 반환 타입을 생략 가능하다.
        3. 단축 인자이름 : 전달인자의 이름이 굳이 필요없고, 컴파일러가 타입을 유추할 수 있는 경우 축약된 전달인자 이름(`$0`, `$1`, `$2`...)을 사용 가능하다. 이때 in 키워드 생략 가능하다.
        4. 암시적 반환 표현 : 반환 값이 있는 경우, 암시적으로 클로저의 맨 마지막 줄은 return 키워드를 생략해도 반환 값으로 취급한다.

        ```swift
        *축약 전
        result = calculate(a: 10, b: 10, method: { (left: Int, right: Int) -> Int in
            return left + right
        })

        -

        *축약 후
        result = calculate(a: 10, b: 10, method: { $0 + $1 })
        or
        result = calculate(a: 10, b: 10) { $0 + $1 }
        ```

        - 설명
            - 기본 클로저 표현
                - [ ]  // or 따로 클로저를 변수 (여기서는 method)에 넣어 전달하지 않고, 함수를 호출할 때 클로저 전체를 작성하여 전달할 수도 있습니다. ?? method: 넣으라고 나오는데?

                ```swift
                let add: (Int, Int) -> Int  // closure/함수 type을 변수에 할당 (매개변수 type이 Int, Int 이고, return값이 Int인)        
                add = { (a: Int, b: Int) -> Int in  
                    return a + b
                }

                // 클로저를 매개변수로 갖는 함수 calculate(a:b:method:)와 결과값을 저장할 변수 result 선언
                func calculate(a: Int, b: Int, method: (Int, Int) -> Int) -> Int {
                    return method(a, b)
                }

                var result: Int
                result = calculate(a: 1, b: 2, method: add)
                print(result)  // 3

                // or 따로 클로저를 변수 (여기서는 method)에 넣어 전달하지 않고, 함수를 호출할 때 클로저 전체를 작성하여 전달할 수도 있습니다. ?? method: 넣으라고 나오는데?

                var result: Int = calculate(a: 1, b: 2, { (a: Int, b: Int) -> Int in  
                    return a + b
                })
                print(result)  // 3
                ```

            - 1. 후행 클로저
                - 클로저가 함수의 마지막 전달인자일 때, 마지막 매개변수 이름을 생략한 후 함수 소괄호 () 외부에 클로저 {}를 구현할 수 있다.
                - 또한 해당 함수의 parameter가 클로저 1개 뿐인 경우, 소괄호를 생략 가능하다. (추가 예시 참고)

                ```swift
                result = calculate(a: 10, b: 10) { (left: Int, right: Int) -> Int in
                    return left + right
                }
                print(result) // 20
                ```

            - 2. 반환타입 생략
                - 함수를 정의할 때, calculate(a: Int, b: Int, method: (Int, Int) -> Int)
                method 매개변수는 Int 타입을 반환할 것이라는 사실을 컴파일러가 이미 알기 때문에 굳이 클로저에서 반환타입을 명시해 주지 않아도 된다. 
                (단, in 키워드는 생략 불가하다. 단축 인자이름을 사용하는 경우에만 in을 생략한다.)
                - 1) return 값의 type 생략, 2) 매개변수의 type 생략, 3) 둘 다 생략 - 모두 가능하다.

                ```swift
                result = calculate(a: 10, b: 10, method: { (left: Int, right: Int) -> Int in  // 1) -> Int 생략 (return값 type)
                    return left + right
                })

                result = calculate(a: 10, b: 10, method: { (left: Int, right: Int) -> Int in  // 2) :Int, :Int 생략 (매개변수 type) // 3) 매개변수, return값 type 모두 생략 가능
                    return left + right
                })

                -

                result = calculate(a: 10, b: 10, method: { (left: Int, right: Int) in
                    return left + right
                })

                // 당연히 후행클로저와 함께 사용 가능하다.
                result = calculate(a: 10, b: 10) { (left: Int, right: Int) in
                    return left + right
                }
                ```

            - 3. 단축 인자이름
                - 마찬가지로 컴파일러가 이미 method의 매개변수 (전달인자?)의 타입을 알고 있고, 굳이 매개변수 이름을 설정할 필요가 없기 때문에 
                method의 매개변수는 생략 가능하고, 단축 인자이름을 활용할 수 있다. 
                (단, 단축 인자이름을 사용하는 경우 in 키워드는 항상 생략한다.)
                - 단축 인자이름은 클로저의 매개변수의 순서대로 `$0, $1, $2`... 형태로 표현한다.

                ```swift
                result = calculate(a: 10, b: 10, method: { (left, right) in   // 단축 인자이름을 사용하면, 항상 (left, right) in 을 생략한다.
                    return left + right
                })

                -

                result = calculate(a: 10, b: 10, method: {
                    return $0 + $1    // $0 는 첫 번째 매개변수, $1 은 두 번째 매개변수를 의미함
                })

                // 당연히 후행 클로저와 함께 사용할 수 있다.
                result = calculate(a: 10, b: 10) {
                    return $0 + $1
                }
                ```

            - 4. 암시적 반환 표현
                - 클로저가 반환하는 값이 있다면 클로저의 마지막 줄의 결과값은 암시적으로 반환값으로 취급한다. 따라서 return 키워드는 생략 가능하다.

                ```swift
                result = calculate(a: 10, b: 10) {
                    $0 + $1   // return 키워드 생략
                }

                -

                result = calculate(a: 10, b: 10, method: { (left, right) in   
                    left + right   // return 키워드 생략 (이렇게도 가능)
                }
                })
                ```

        - Quiz
            - [ ]  plus는 매개변수가 있어야하는데 어떻게 가능한거지?

            ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2011.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2011.png)

    - 추가 예시 (교재)

        ```swift
        // 구현 - 알파벳 큰 순서로 정렬하는 Array

        func backwards(first: String, second: String) -> Bool {
            return first > second // z가 a보다 크다 = z가 a보다 뒷쪽에 있다
        }
        print(backwards(first: "a", second: "z")) // false

        var names: [String] = ["a", "c", "z"]

        // 방법-1 sorted 메서드의 argument로 함수를 전달
        var reversed: [String] = names.sorted(by: backwards) // sorted 메서드에 의해 값이 정렬되고 Array type으로 return 된다.
        print(reversed) // ["z", "c", "a"]

        // 방법-2 sorted 메서드의 argument로 Closure를 전달
        reversed = names.sorted(by: {(first: String, second: String) -> Bool in
            return first > second
        })
        print(reversed) // ["z", "c", "a"]
        ```

        - 후행 클로저

            ```swift
            // 2-2. 후행 클로저 표현 (Trailing Closure)
            reversed = names.sorted() {(first: String, second: String) -> Bool in // 마지막 argument로 클로저가 전달되는 경우, () 밖에 {클로저} 구현 가능
                return first > second
            }
            print(reversed) // ["z", "c", "a"]

            reversed = names.sorted(by:) {(first: String, second: String) -> Bool in // *sorted(by:) 메서드의 argument label 생략 가능
                return first > second
            }
            print(reversed) // ["z", "c", "a"]

            reversed = names.sorted {(first: String, second: String) -> Bool in // *sorted(by:) 메서드의 소괄호도 생략 가능
                return first > second
            }
            print(reversed) // ["z", "c", "a"]
            ```

            ```swift
            // 다중 후행 클로저

            func doSth(paraA: (String) -> Void,  // 함수의 parameter에 클로저가 여러 개 있는 경우
                       paraB: (String) -> Void,
                       paraC: (String) -> Void) {
                // statements
            }

            doSth { (first: String) in  // *첫번째 클로저의 argument label은 생략해야 한다. paraA: 넣으면 오류 발생
                // A Closure
            } paraB: { (first: String) in
                // B Closure
            } paraC: { (first: String) in
                // C Closure
            }
            ```

        - 클로저 표현 간소화
            - 타입 유추
            - 단축 인자 이름
            - 암시적 반환
            - 연산자 함수

            ```swift
            // 3-1. 타입 유추
            reversed = names.sorted {(first, second) in // parameter type, 개수, return type이 일치하는 클로저만 변수에 할당 가능하다. 즉, 컴파일러는 이미 타입을 유추 가능하다. 따라서 생략 가능하다.
                return first > second
            }
            print(reversed) // ["z", "c", "a"]

            // 3-2. 단축 인자 이름
            reversed = names.sorted { // 클로저의 parameter는 단축 인자이름으로 대체 가능하다. 첫번째 parameter부터 차례로 $0, $1, $2... 순서로 표현하며, 이때 in 키워드는 항상 생략한다.
                return $0 > $1
            }
            print(reversed) // ["z", "c", "a"]

            // 3-3. 암시적 반환 표현
            reversed = names.sorted { $0 > $1 } // 클로저가 반환값을 가지며, 실행문이 1줄이라면 return을 생략 가능하다.
            print(reversed) // ["z", "c", "a"]

            // 3-4. 연산자 함수
            reversed = names.sorted(by: >) // 연산자 함수 '>'를 클로저의 역할로 사용했다.
            print(reversed) // ["z", "c", "a"]

            // 비교연산자는 2개의 피연산자를 통해 Bool type 반환을 한다. (reversed 변수에 전달한 클로저와 동일한 조건이다.)
            // 클로저는 parameter type과 return type이 연산자를 구현한 함수의 형태와 동일하면, 연산자만 표기 가능하다. (연산자는 함수의 일종이다.)
            ```

    - 참고 - DoItSwift Alert

        ```swift
        // Closure 기본 형태 
        { (parameter) -> 반환타입 in 
           return ... }

        // 예시 - 함수 및 Closure 비교
        		func completeWork (a: Int) -> void {
        				print("completed : \(a)")
        		} 
        		
        		// 위 함수를 Closure 형태로 바꾸면
        		{		(a: Int) -> void in
        				print("completed : \(a)")
        		}
        		
        		// 컴파일러가 반환 타입, 매개변수 타입을 미리 알고 있어서 생략 가능하면
        		{		(a) in 
        				print("completed : \(a)")
        		}

        		{		a in  // (a) -> a로 가능 (매개변수 타입 생략 시 () 소괄호 생략 가능)
        				print("completed : \(a)")
        		}

        // UIAlertAction의 handler: { in } 이 Closure 임
        let offAction = UIAlertAction(title: "네", style: .default, handler: {
        									                                            ACTION in self.lampImg.image = self.imgOff
        									                                            self.isLampOn = false
        })
        ```

- 값 획득 (Capture)
    - [ ]  ?
    - 클로저는 자신이 정의된 위치의 주변 문맥을 통해 변수를 획득 (Capture) 가능하다.
    값 획득을 통해 클로저는 주변에 정의한 변수가 더 이상 존재하지 않더라도 (메모리에서 free 되었더라도???) 해당 변수 값을 자신 내부에서 참조 또는 수정 가능하다. (클로저 및 함수는 참조 type이므로 ???)
        - 클로저는 비동기 작업에 많이 사용된다. 비동기 Call-back을 작성하는 경우, 현재 상태를 미리 획득해두지 않으면 실제로 클로저 기능을 실행하는 순간에는 주변 변수가 이미 메모리에 존재하지 않는 경우가 발생한다.
        - 이를 대비하기 위해 클로저의 일종인 중첩함수는 주변의 변수를 미리 획득해 놓을 수 있다. 즉, 자신을 포함하는 지역변수를 획득해 놓을 수 있다.

        ```swift
        func makeIncrementer(amount: Int) -> (() -> Int) {  // return type : Closure - 함수객체를 반환한다는 의미 ???
        		var total = 0

        		func incrementer() -> Int { // 중첩함수 incrementer는 자신 주변의 total 및 amount 변수 값을 획득한다. (직접 incrementer 함수에서 선언하지 않은 변수이지만, 획득하여 변수 값을 참조해둔다.)
        				total += amount
        				return total
        		} // incrementer 함수가 호출될 때마다 parameter amount 값만큼 total 값이 증가한다.

        		return incrementer  // makeIncrementer 함수는 클로저 incrementer를 return 한다. - 왜 클로저??? 함수 타입이라서???
        }

        // incrementer 함수는 자신 주변의 total 및 amount 변수의 참조를 획득한다. (값 획득??? 참조 획득???) 
        // 참조를 획득하면 total 및 amount는 makeIncrementer 함수의 실행이 끝나도 사라지지 않고, incrementer가 호출될 때마다 계속 사용 가능하다.
        ```

        ```swift
        let incrementByTwo: (() -> Int) = makeIncrementer(amount: 2)  // 상수에 함수를 할당한다. (=함수의 값이 아니라 참조를 할당했다. =상수에 함수의 참조를 설정했다.)

        let first: Int = incrementByTwo()   // 2 - incrementer 함수가 호출될 때마다 2 만큼 total 변수 값이 증가하고 return 된다.
        let second: Int = incrementByTwo()  // 4
        let third: Int = incrementByTwo()   // 6

        let incrementByTwo2: (() -> Int) = makeIncrementer(amount: 2) // 상수 incrementByTwo2는 상수 incrementByTwo와 별개의 참조를 갖는 total 변수 값을 획득했다.
        let incrementByTen: (() -> Int) = makeIncrementer(amount: 10)

        let first2: Int = incrementByTwo2()   // 2 
        let second2: Int = incrementByTwo2()  // 4
        let third2: Int = incrementByTwo2()   // 6

        let ten: Int = incrementByTen()      // 10 
        let twenty: Int = incrementByTen()   // 20 
        let thirty: Int = incrementByTen()   // 30 

        let sameWithIncrementByTwo: (() -> Int) = incrementByTwo // 참조변수 지정 - 상수에 함수의 참조를 할당했으므로 두 상수가 동일한 클로저를 가르킨다.
        let continuedFourth: Int = sameWithIncrementByTwo()  // 8
        let continuedFifth: Int = incrementByTwo()  // 10
        ```

- 탈출 (Escape)
    - 함수의 argument로 전달한 클로저가 함수 종료 이후에 호출될 때, "클로저가 함수를 탈출한다"고 표현한다.
    parameter로 클로저를 갖는 함수를 선언할 때, parameter type 부분에 `: @escaping` 키워드를 사용하여 클로저 탈출을 허용한다고 명시한다. (탈출 조건인데 명시하지 않으면 컴파일 오류 발생)
    - 비동기 작업을 실행하는 함수는 Completion handler 전달인자로 클로저를 받아온다. 비동기 작업으로 함수가 종료된 이후 (함수 return 이후) 호출할 필요가 있는 클로저를 사용해야 할 때 탈출 클로저가 필요하다.
    클로저가 함수를 탈출한 상태여야 호출될 수 있다.
        - 함수 외부에 정의된 변수에 저정된 함수가 종료된 이후에 사용하는 경우???
        - [ ]  returnedClosure()  // Closure A 출력 - 상수인데 실행 가능해서 ()를 쓰나?

        ```swift
        var handlers: [() -> Void] = [] // Closure type Array

        func aFuncWithEscapingClosure(completionHandler: @escaping () -> Void) {  // parameter type : Closure (escape 허용)
            handlers.append(completionHandler)
        }
        ```

        ```swift
        typealias VoidVoidClosure = () -> Void

        let firstClosure: VoidVoidClosure = {
            print("Closure A")
        }
        let secondClosure: VoidVoidClosure = {
            print("Closure B")
        }

        func returnOneClosure(first: @escaping VoidVoidClosure, second: @escaping VoidVoidClosure, shouldReturnClosure: Bool) -> VoidVoidClosure { 
            return shouldReturnClosure ? first : second // *argument로 받은 클로저를 함수 외부로 return 하므로 함수를 탈출하는 클로저이다.
        }

        let returnedClosure: VoidVoidClosure = returnOneClosure(first: firstClosure, second: secondClosure, shouldReturnClosure: true)
        // return한 클로저 (firstClosure)를 함수 외부의 상수에 저장한다.
        returnedClosure()  // Closure A 출력 - 상수인데 실행 가능해서 ()를 쓰나?

        var closures: [VoidVoidClosure] = [] // Closure type Array

        func appendClosure(parameterClosure: @escaping VoidVoidClosure) {
            closures.append(parameterClosure) // *argument로 전달받은 클로저가 함수 외부의 변수에 저장되므로 함수를 탈출하는 클로저이다.
        }
        ```

    - 타입 내부 메서드의 parameter로 탈출 클로저를 명시한 경우, 클로저 내부에서 해당 타입의 프로퍼티/메서드/서브스크립트 등에 접근하려면 `self` 키워드를 명시해야 한다. (비탈출 클로저는 명시 필요 없음)

        ```swift
        typealias VoidVoidClosure = () -> Void

        func funcWithNoEscapeClosure(parameterClosure: VoidVoidClosure) {
            parameterClosure()  // Closure 실행
        }

        func funcWithEscapingClosure(completionHandler: @escaping VoidVoidClosure) -> VoidVoidClosure {
            return completionHandler  // 함수 외부로 탈출 클로저를 return
        }

        class SomeClass {
            var x = 10
            
            func runNoEscapeClosure() {
                funcWithNoEscapeClosure { x = 200 }  // 비탈출 클로저에서 self 사용은 선택 사항이다. *아래 참고-원래 형태는 funcWithNoEscapeClosure(parameterClosure: { x = 200 })
            }
            
            func runEscapingClosure() -> VoidVoidClosure {
                return funcWithEscapingClosure { self.x = 100 }  // 탈출 클로저에서 명시적으로 self를 사용해야 한다.
            }
        }

        let aInstance: SomeClass = SomeClass()

        aInstance.runNoEscapeClosure() // funcWithNoEscapeClosure 함수는 자체 실행 기능이 있다. 그래서 바로 출력 가능하다.
        print(aInstance.x) // 200 출력

        let returnClosure: VoidVoidClosure = aInstance.runEscapingClosure() // funcWithEscapingClosure 함수는 자체 실행 기능이 없고, return만 하므로 선언 이후 별도로 실행해줘야 한다.
        print(aInstance.x) // 200 출력
        returnClosure() // 실행해야 프로퍼티 x 값이 변경된다.
        print(aInstance.x) // 100 출력
        ```

        - [x]  funcWithNoEscapeClosure { a = 200 } ?
            - 후행 클로져 기능

                ```swift
                funcWithNoEscapeClosure(parameterClosure: VoidVoidClosure) { // 함수 정의
                		// ...
                }

                funcWithNoEscapeClosure(parameterClosure: {someClosure}) // 함수 실행 (argument로 {...} 형태의 someClosure가 전달됨) -> 이때 후행 클로저 기능을 사용 가능하다.

                // 후행 클로저 기능 - 1) 함수의 마지막 argument로 클로저가 전달되는 경우, 2) argument가 클로저 1개 뿐인 경우에 해당하므로 () 밖에 {클로저} 구현 가능하며, ()를 생략 가능하다.
                funcWithNoEscapeClosure {someClosure}

                // 즉, 아래 두 표현은 동일하다.
                funcWithNoEscapeClosure(parameterClosure: { x = 200 }) 
                funcWithNoEscapeClosure { x = 200 } 
                ```

    - withoutActuallyEscaping 함수 - 비탈출 클로저로 전달한 클로저가 탈출 클로저인 척 해야하는 경우가 있다. 실제로는 탈출하지 않는데, 다른 함수에서 탈출 클로저를 요구하는 상황에 해당한다. 
    이때, withoutActuallyEscaping 함수를 통해 해당 비탈출 클로저를 탈출 클로저인 것처럼 사용 가능하다.
        - [ ]  lazy 컬렉션은 비동기 작업을 할 때 사용하므로 filter 메서드는 탈출 클로저를 요구한다.
        - [ ]  lazy 컬렉션
            - 엄밀하게, 필요한 경우에만 해당 요소에 메소드 오퍼레이션을 적용하므로, 무한하거나 매우 큰 컬렉션을 다루는 데 효율적이다.
        - [ ]  do 전달인자는 비탈출 클로저 escapablePredicate를 전달받아 실제로 작업을 실행할 탈출 클로저를 전달한다.

        ```swift
        func hasElements(in array: [Int], match predicate: (Int) -> Bool) -> Bool { // 비탈출 클로저 predicate를 parameter로 전달
            return (array.lazy.filter { predicate($0) }.isEmpty == false) // 그런데 lazy 컬렉션은 비동기 작업을 할 때 사용하므로 filter 메서드는 탈출 클로저를 요구한다.
        } // 컴파일 에러 발생 - Escaping closure captures non-escaping parameter 'predicate'
        ```

        ```swift
        let numbers: [Int] = [2,4,6,8]

        let evenNumberPredicate = { (number: Int) -> Bool in // Closure 할당
            return number % 2 == 0
        }

        let oddNumberPredicate = { $0 % 2 == 1 } // Closure 할당 - 가능

        // withoutActuallyEscaping 함수를 통해 비탈출 클로저를 탈출 클로저처럼 사용 가능하다.
        func hasElements(in array: [Int], match predicate: (Int) -> Bool) -> Bool { // withoutActuallyEscaping 함수의 첫번째 argument로 비탈출 클로저 predicate를 전달 
            return withoutActuallyEscaping(predicate, do: { escapablePredicate in // withoutActuallyEscaping 함수의 두번째 argument로 비탈출 클로저 escapablePredicate를 전달 (lazy 때문에 탈출 클로저인 척 해야한다.)
                return (array.lazy.filter { escapablePredicate($0) }.isEmpty == false) // do 전달인자는 비탈출 클로저 escapablePredicate를 전달받아 실제로 작업을 실행할 탈출 클로저를 전달한다. ???
            })
        }

        let hasEvenNumber = hasElements(in: numbers, match: evenNumberPredicate)
        let hasOddNumber = hasElements(in: numbers, match: oddNumberPredicate)

        print(hasEvenNumber) // true 출력
        print(hasOddNumber) // false 출력
        ```

- 자동 클로저
    - L/R

        `@autoclosure` : Apply this attribute to delay the evaluation of an expression (argument로 전달되는 구문을 closure로 전달하지 않고, 예를 들어 실행결과인 String이 전달되고???) by automatically wrapping that expression in a closure with no arguments. (해당 String을 ()→String type의 클로저로 자동변환시켜준다.???) 
        You apply it to a parameter’s type in a function declaration, for a parameter whose type is a function type that takes no arguments and that returns a value of the type of the expression. (함수 선언 시, 함수의 parameter type이 함수 type (()→String type)인 경우 사용한다.)

    - 자동클로저 (Auto Closure) : 함수의 argument로 전달하는 표현을 자동으로 변환해주는 클로저이다.
    - `@autoclosure`로 명시한다. (`@noescape` 속성이 암시적으로 부여된다. 따라서 자동클로저를 탈출 클로저로 사용한다면, `@autoclosure @noescape` 으로 명시한다.)
    - 자동클로저는 argument를 갖지 않는다. 자동클로저는 호출되었을 때 자신이 감싸고 있는 코드의 결과값을 반환한다.
    - 함수로 전달하는 클로저인 경우, 어려운 문법을 사용하지 않도록 문법적 편의를 제공한다. (원래 () {}로 복잡하게 써야함)
    - Swift 표준 라이브러리의 assert (condition:message:file:line) 함수는 condition 및 message parameter가 자동클로저이다. (condition parameter는 디버그용 빌드에서만 실행되고, message parameter는 condition이 false일 때만 실행된다.)
    - 자동클로저는 클로저가 호출되기 전까지 클로저 내부의 코드가 동작하지 않는다. 따라서 연산 지연이 가능하다. (연산에 자원 소모가 크거나, 부작용이 우려될 때 유용하게 사용한다.)

        ```swift
        // 상수에 클로저를 할당하고, 상수를 실행
        var customersInLine: [String] = ["kevin","mike","john"] // 대기 중인 손님들
        print(customersInLine.count) // 3

        let customerProvider: () -> String = { 
            return customersInLine.removeFirst() // 첫번째 element를 제거하고, 해당 값을 return한다.
        }
        print(customersInLine.count) // 3 - 클로저 선언만 한 상태에는 클로저 내부의 코드가 실행되지 않는다. (연산 지연)

        print("Now serving \(customerProvider())!!!") // 클로저 실행 - Now serving kevin!!!
        print(customersInLine.count) // 2

        print("Now serving \(customerProvider())!!!") // Now serving mike!!!
        print(customersInLine.count) // 1

        print("Now serving \(customerProvider())!!!") // Now serving john!!!
        print(customersInLine.count) // 0
        ```

        ```swift
        // 함수의 parameter로 클로저를 전달
        var customersInLine: [String] = ["kevin","mike","john"]
        print(customersInLine.count) // 3

        func serveCustomer(_ customerProvider: () -> String) { 
            print("Now serving \(customerProvider())!!!")
        }

        serveCustomer( {customersInLine.removeFirst()} ) // Now serving kevin!!! - // 위와 동일한 클로저를 함수의 argument로 전달하는 경우
        print(customersInLine.count) // 2

        serveCustomer( {customersInLine.removeFirst()} ) // Now serving mike!!!
        print(customersInLine.count) // 1

        serveCustomer( {customersInLine.removeFirst()} ) // Now serving john!!!
        print(customersInLine.count) // 0
        ```

        ```swift
        // 자동클로저 사용
        var customersInLine: [String] = ["kevin","mike","john"]
        print(customersInLine.count) // 3

        func serveCustomer(_ customerProvider: @autoclosure () -> String) { // 자동클로저 명시. 2. String값(kevin)을 매개변수가 없는 String 값을 반환하는 클로저로 변환한다. 
            print("Now serving \(customerProvider())!!!")
        }

        serveCustomer(customersInLine.removeFirst()) // Now serving kevin!!! - 자동클로저 호출, {} 필요 없음. 1. 실행결과인 String(kevin)을 argument로 전달한다.
        print(customersInLine.count) // 2

        serveCustomer(customersInLine.removeFirst()) // Now serving mike!!!
        print(customersInLine.count) // 1

        serveCustomer(customersInLine.removeFirst()) // Now serving john!!!
        print(customersInLine.count) // 0

        // 1. 자동클로저 parameter customerProvider는 
        // 클로저 {}를 argument로 넘겨주는 것이 아니라 
        // customersInLine.removeFirst()의 실행 결과 (String type)를 argument로 넘겨준다. 🐠🐠🐠

        // 2. String값(kevin)을 매개변수가 없는 String 값을 반환하는 클로저로 변환한다. 
        // 자동클로저는 argument를 갖지 않기 때문에 반환 타입의 값(String)이 자동클로저의 parameter로 전달되면, 이것을 클로저로 바뀌주는 것이다. (자동으로 클로저로 바뀌주기 때문에 이름이 자동클로저)
        // 자동클로저를 사용하면 기존처럼 클로저를 argument로 전달 불가하다.
        ```

        - 자동클로저를 탈출 클로저로 사용

            ```swift
            var customersInLine: [String] = ["kevin","mike","john"]
            print(customersInLine.count) // 3

            func serveCustomer(_ customerProvider: @autoclosure @escaping () -> String) -> (() -> String) { // return type도 클로저
                return customerProvider // 함수 반환값으로 return 되므로 클로저가 함수를 escape 한다.
            }

            let customerProvider: () -> String = serveCustomer(customersInLine.removeFirst()) // String값이 자동클로저의 argument로 전달되고, 클로저로 변환된다.
            print("Now serving \(customerProvider())!!!") // Now serving kevin!!!
            ```

# 13. Property

- 종류 (저장/연산/타입)
    - 인스턴스/타입 **저장** 프로퍼티
    - 인스턴스/타입 **연산** 프로퍼티
    - 지연 저장 프로퍼티
- Language Guide - properties
    - [x]  provided? : 사용 가능함, 선언 가능함
    - Stored properties (저장 프로퍼티) : a constant/variable that is stored as part of an instance of a particular class or structure. 
    Stored properties are provided only by classes and structures. (class, structure 에서만 사용 가능하다. enumerations 불가)
        - Lazy Stored Properties (지연 저장 프로퍼티) : 호출될 때까지 값의 할당을 지연한다. a property whose initial value is not calculated until the first time it is used. 
        You must always declare a lazy property as a variable (with the var keyword), because its initial value might not be retrieved until after instance initialization completes.
            - Lazy properties are useful when the initial value for a property is dependent on outside factors whose values are not known until after an instance’s initialization is complete. Lazy properties are also useful when the initial value for a property requires complex or computationally expensive setup that should not be performed unless or until it is needed.
            지연 프로퍼티는 프로퍼티가 특정 요소에 의존적이어서 그 요소가 끝나기 전에 적절한 값을 알지 못하는 경우에 유용합니다. 또 복잡한 계산이나 부하가 많이 걸리는 작업을 지연 프로퍼티로 선언해 사용하면 실제 사용되기 전에는 실행되지 않아서 인스턴스의 초기화 시점에 복잡한 계산을 피할 수 있습니다.
            (class 인스턴스의 저장 프로퍼티로 다른 class 인스턴트 또는 struct 인스턴스를 할당할 때, 인스턴스를 초기화하면서 저장 프로퍼티로 사용되는 인스턴스들이 한 번에 생성되어야 하는데, 이때 굳이 모든 저장 프로퍼티를 사용할 필요가 없는 경우)

                ```java
                struct CoordinatePoint {
                    var x: Int = 0
                    var y: Int = 0
                }

                class Position {
                    lazy var point: CoordinatePoint = CoordinatePoint() // 저장 프로퍼티에 struct 인스턴스를 할당하는 경우. 의도 - point 변수에 CoordinatePoint가 바로 할당되지 않음
                    let name: String
                    
                    init(name: String) {
                        self.name = name
                    }
                }

                let yagomPosition: Position = Position(name: "yagom")
                print(yagomPosition.point) // CoordinatePoint(x: 0, y: 0) - point 프로퍼티에 처음 접근할 때 Coordinate가 초기화되어 할당됨
                ```

    - Computed properties (연산 프로퍼티) : calculate (rather than store) a value. provide a `getter` and an `optional setter` to retrieve and set other properties and values indirectly.
    메서드보다 간편하고 직관적이다.
    getter (접근자)는 값을 연산하여 반환하고, optional한 setter (설정자)는 값을 변경한다.
        - ex. 사각형의 Point, Size, Rect 구조체 👍
            - The Rect structure also provides a computed property called center. The current center position of a Rect can always be determined from its origin and size, and so you don’t need to store the center point as an explicit Point value. Instead, Rect defines a custom getter and setter for a computed variable called center, to enable you to work with the rectangle’s center as if it were a real stored property.
            - The square variable (a new Rect variable) is initialized with an origin point of (0, 0), and a width and height of 10. This square is represented by the blue square in the diagram below.
                - The square variable’s center property is then accessed through dot syntax (square.center), which causes the getter for center to be called, to retrieve the current property value. Rather than returning an existing value, the getter actually calculates and returns a new Point to represent the center of the square. As can be seen above, the getter correctly returns a center point of (5, 5).
                - The center property is then *set to a new value of (15, 15), which moves the square up and to the right, to the new position shown by the orange square in the diagram below. Setting the center property calls the setter for center, which modifies the x and y values of the stored origin property, and moves the square to its new position.

            ```swift
            struct Point {
                var x = 0.0, y = 0.0  // 저장 var 프로퍼티
            }

            struct Size {
                var width = 0.0, height = 0.0  // 저장 var 프로퍼티
            }

            struct Rect {
                var origin = Point()
                var size = Size()
                var center: Point {  // *연산 var 프로퍼티
                    get {
                        let centerX = origin.x + (size.width / 2)
                        let centerY = origin.y + (size.height / 2)
                        return Point(x: centerX, y: centerY)  // *다른 프로퍼티들의 값을 활용하여 연산한 결과값을 return 해준다. (Point(x,y) 값을 center에 할당해줌)
                    }
                 // get {  // *Shorthand - 구조체 Point의 x,y 프로퍼티 값을 single expression으로 할당하면, 해당 expression을 암시적으로 return 해준다. 이때 return 생략 가능하다. (함수 return 생략가능 rule과 동일)
                 //     Point(x: origin.x + (size.width / 2),
                 //           y: origin.y + (size.height / 2))
                 // }

                    set (newCenter) {
                        origin.x = newCenter.x - (size.width / 2)  // *새로운 Point 값 (newCenter)이 입력되면 연산한 결과값을 다른 프로퍼티 (origin)에 할당해준다.
                        origin.y = newCenter.y - (size.height / 2)
                    }
                 // set {  // *Shorthand - setter의 parameter (ex. set (newCenter))를 별도 지정하지 않을 경우 <newValue>를 암시적 parameter로 사용한다.(위와 동일한 결과)
                 //     origin.x = newValue.x - (size.width / 2)
                 //     origin.y = newValue.y - (size.height / 2)
                 // }
                }
            }

            var square = Rect()  // Rect에 대한 인스턴트 생성
            square.origin = Point(x: 0.0, y: 0.0)  // origin 프로퍼티 할당 (즉, 구조체 Point의 저장 프로퍼티 값을 할당)
            square.size = Size(width: 10.0, height: 10.0)  // size 프로퍼티 할당 (즉, 구조체 Size의 저장 프로퍼티 값을 할당)

            print(square.center)  // **Point(x: 5.0, y: 5.0) - 출력 (0 + 10/2 = 5) 

            //var square = Rect(origin: Point(x: 0.0, y: 0.0),   // 한꺼번에 이렇게도 가능 (위와 동일한 결과)
            //                  size: Size(width: 10.0, height: 10.0))

            let initialSquareCenter = square.center // **새로운 Point 값 (newCenter)이 입력됨
            square.center = Point(x: 15.0, y: 15.0)  // 구조체 Rect의 center 프로퍼티 - *set(newCenter) 부분에 들어가는 새로운 Point 값!! (15-5=10, 이로 인해 origin 값이 변경됨)

            print("square.origin is now at (\(square.origin.x), \(square.origin.y))") // square.origin is now at (10.0, 10.0) 출력
            ```

            - [x]  var center의 type이 왜 Point? 단지 좌표 형태라서?
              print(square.center)  // Point(x: 5.0, y: 5.0) - 출력

                ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2012.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2012.png)

        - Read-only computed properties (읽기전용 연산 프로퍼티) : with a getter but no setter. 
        A read-only computed property always returns a value, and can be accessed through dot syntax, but cannot be set to a different value.
            - get keyword and its braces {} 생략 가능

                ```swift
                struct Cuboid {
                    var width = 0.0, height = 0.0, depth = 0.0
                    var volume: Double {  // computed property는 항상 type을 명시해줘야 함
                        ~~get {~~
                            return width * height * depth
                        ~~}~~
                    }
                }

                let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
                print("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
                ```

    - Type properties (연산 프로퍼티) : 인스턴스 소속이 아니라 타입 소속의 프로퍼티이다.
    인스턴스 생성 여부와 상관 없이 타입 프로퍼티의 값은 하나이며, 모든 인스턴스에 공통적으로 적용할 값을 정의할 때 사용한다. (C의 static과 유사함)

- 특징
    - 프로퍼티는 구조체, 클래스, 열거형 내부에 구현할 수 있다. (함수, 메서드, 클로저, 타입 등의 외부에 위치한 지역/전역 변수에도 사용 가능)
    - 다만 열거형 내부에는 연산 프로퍼티만 구현할 수 있다. (저장 프로퍼티 불가)

    - 저장 프로퍼티 : 값을 저장한다.
    - 연산 프로퍼티 { get{} set{} } : 값을 저장하는 것이 아니라 특정 연산의 결과값을 의미한다. *get을 통해 값을 연산하여 자신의 값을 return 하거나, set을 통해 연산하여 남의 값을 할당한다.
        - `var`로만 선언할 수 있다. (값이 가변적이므로)
        - 읽기전용으로는 구현할 수 있지만, 쓰기전용으로는 구현할 수 없다. (setter is optional)
        - 읽기전용으로 구현하려면 `get` 블럭만 작성한다. 읽기전용은 `get{}`을 생략 가능하다.
        - `set` 블럭에서 암시적 매개변수 `newValue`를 사용 가능하다. (즉, set(parameter)에서 parameter를 따로 지정하지 않은 경우)

            ```swift
            struct Student {
            		var koreanAge: Int = 0
            		    
            		var westernAge: Int {  
            				// read
            		    get { // 읽기전용은 get{}을 생략 가능하다.
            		        return koreanAge - 1  // koreanAge 값이 할당되는 즉시, 연산 결과값을 westernAge에 return 한다.
            		    }
            		    
            				// write
            		    set(newValue) {  // westernAge 새로운 값이 할당되는 즉시, 연산 결과값을 koreanAge에 할당한다.
            		        koreanAge = newValue + 1
            		    }
            		}
            }

            var yagom: Student = Student();
            yagom.koreanAge = 10 // koreanAge의 값이 할당되는 즉시, westernAge의 값을 지정한다. westernAge 입장에서 본인의 값을 get/read한다. (getter로 10-1 연산을 하고, 결과값이 westernAge 본인 값에 할당된다.)
            print(yagom.koreanAge) // 10
            print(yagom.westernAge) // 9 (getter 결과)

            yagom.westernAge = 30 // westernAge의 값이 할당되는 즉시, koreanAge의 값을 지정한다. westernAge 입장에서 남의 값을 set/write한다. (setter로 westernAge 본인 값을 newValue를 넣어서, 30+1 연산을 하고, 결과값이 koreanAge에 할당된다.)
            print(yagom.koreanAge) // 31 (setter 결과)
            print(yagom.westernAge) // 30
            ```

            ```swift
            // 읽기전용 - setter의 역할 참고
            struct Student {
            		var koreanAge: Int = 0
            		    
            		var westernAge: Int {  
            		    get { 
            		        return koreanAge - 1 
            		    }
            		}
            }

            var yagom: Student = Student();
            yagom.koreanAge = 10 
            print(yagom.koreanAge) // 10
            print(yagom.westernAge) // 9 (getter)

            yagom.westernAge = 30 // setter가 없으므로 할당 불가하여 컴파일 에러 발생!!! - Cannot assign to property: 'westernAge' is a get-only property

            ```

    - 연산 프로퍼티 및 프로퍼티 감시자는 전역변수, 지역변수 모두에 사용 가능하다. (단, 전역변수는 지연 저장 프로퍼티처럼 처음 접근할 때 최초 연산이 실행되므로 lazy 키워드가 필요 없다.)

    ```swift
    struct Student {
        var name: String = "unkown"   // 인스턴스 저장 프로퍼티 
        var `class`: String = "Swift"
        var koreanAge: Int = 0
        
        var westernAge: Int {   // 인스턴스 연산 프로퍼티 
            get {  // *koreanAge에서 -1 연산한 값을 westernAge에 return 해준다.
                return koreanAge - 1 // getter의 코드가 한 줄이고, return type이 프로퍼티의 type과 동일하면 return 키워드 생략 가능
            }
            
            set(inputValue) {  // *westernAge의 새로운 값인 inputValue를 저장하는 것이 아니라, 해당 값을 +1 연산해서 koreanAge에 할당해준다.
                koreanAge = inputValue + 1
            }
        }
        
        static var typeDescription: String = "학생"   // 타입 저장 프로퍼티   
    -
        // func selfIntroduce() {    // 인스턴스 메서드 - parameter와 반환값이 없는
        //    print("저는 \(self.class)반 \(name)입니다")
        // }
        
        // 위의 selfIntroduce() 메서드를 대체할 수 있습니다. *get만 있고 set 없음: 읽기전용
        var selfIntroduction: String {    // 읽기전용 인스턴스 연산 프로퍼티 - 사칙연산은 아니지만 다른 프로퍼티 (self.class 및 name)의 값을 활용한 결과값을 return 해주므로
            get {
                return "저는 \(self.class)반 \(name)입니다"
            }
        }
    -        
        // static func selfIntroduce() {     // 타입 메서드
        // print("학생타입입니다")
        // }
        
        // 읽기전용에서는 get을 생략할 수 있다.
        static var selfIntroduction: String {     // 읽기전용 타입 연산 프로퍼티
            ~~get {~~ return "학생타입입니다" ~~}~~
        }
    }

    print(Student.selfIntroduction)  // 타입 연산 프로퍼티 사용
    // 학생타입입니다 - 출력

    var yagom: Student = Student()  // 인스턴스 생성
    yagom.koreanAge = 10
    yagom.name = "yagom"  // 인스턴스 저장 프로퍼티 사용
    print(yagom.name) // yagom - 출력

    print(yagom.selfIntroduction)  // 인스턴스 연산 프로퍼티 사용
    // 저는 Swift반 yagom입니다 - 출력

    print("제 한국나이는 \(yagom.koreanAge)살이고, 미쿡나이는 \(yagom.westernAge)살입니다.")
    // 제 한국나이는 10살이고, 미쿡나이는 9살입니다.
    ```

    - [x]  var selfIntroduction : 읽기전용 인스턴스 연산 프로퍼티 -> 사칙연산 안하는데 왜 연산 프로퍼티? 저장이 아니라서?
        - 사칙연산은 아니지만 다른 프로퍼티 (self.class 및 name)의 값을 활용한 결과값을 return 해주므로 연산 프로퍼티이다. (저장 프로퍼티처럼 값을 저장하지 않는다.)

        ```swift
        var selfIntroduction: String {   // 읽기전용 인스턴스 연산 프로퍼티 
                get {
                    return "저는 \(self.class)반 \(name)입니다"
                }
            }
        ```

    - 연산 프로퍼티 활용 예시

        ```swift
        class ViewController: UIViewController {
        var numImg: Int = 1
            
        // var imgName: String = String(numImg) + ".png"
            // 컴파일 에러 발생 - 함수 내부에 각각 선언해준다
            // *에러 - Cannot use instance member 'numImg' within property initializer; property initializers run before 'self' is available
            // you cannot use any instance methods nor any instance properties in an initial value of other (usual) instance properties.
            var imgName: String {
                return String(numImg) + ".png"
            }
        // => solution : You can use read-only computed property. (get keyworkd 생략 가능)
        }
        ```

- 응용 (환전)

    ```swift
    struct Money {
        var currencyRate: Double = 1100
        var dollar: Double = 0
        var won: Double {
            get {
                return dollar * currencyRate
            }
            set {
                dollar = newValue / currencyRate
            }
        }
    }

    var moneyInMyPocket = Money()

    moneyInMyPocket.won = 11000  // => setter 작동함 (newValue에 할당하여 dollar에 할당됨)
    print(moneyInMyPocket.won) // 11000 

    moneyInMyPocket.dollar = 10  // => getter 작동함 (새로 연산된 값을 return 하여 won에 할당함)
    print(moneyInMyPocket.won) // 11000
    ```

## 13-2. Property Observers (프로퍼티 감시자)

- 특징
    - property observers : you can define property observers to monitor changes in a property’s value, which you can respond to with custom actions. 
    Property observers can be added to stored properties you define yourself, and also to properties that a subclass inherits from its superclass.
        - property wrapper : You can also use a property wrapper to reuse code in the getter and setter of multiple properties.
    - 프로퍼티의 값이 변경될 때 원하는 동작을 수행한다. 값이 변경되기 직전에 `willSet`블럭이, 변경된 직후에 `didSet`블럭이 호출된다. (둘 중 하나만 구현해도 무관)
    - 변경되려는 값이 기존 값과 동일하더라도 프로퍼티 감시자는 항상 동작한다.
    - "저장된 값"이 변경될 때 호출되므로 저장 프로퍼티에만 사용된다. (연산 프로퍼티에 사용 불가. 연산 프로퍼티는 setter에서 값의 변화를 감지 할 수 있으므로 옵저버가 필요 없다.)
    프로퍼티를 재정의하여 상속받은 저장/연산 프로퍼티에도 사용 가능하다.
    - 프로퍼티 감시자는 함수, 메서드, 클로저, 타입 등의 지역/전역 변수에 모두 사용 가능하다.
- Syntax
    - [ ]  myAccount.dollarValue = 2 // 잔액을 2.0달러로 변경 중입니다. *감시자가 있는 프로퍼티가 함수의 argument로 전달되면, 감시자를 호출한다. (이 설명이 이 경우에 맞나???)

    ```swift
    // 1) 저장 프로퍼티에 적용
    struct Money {
        var currencyRate: Double = 1100 {   // 프로퍼티 감시자 사용
            willSet(newRate) {   // willSet : 변경 직전에 호출됨
                print("환율이 \(currencyRate)에서 \(newRate)으로 변경될 예정입니다")
            }
            didSet(oldRate) {   // didSet : 변경 직후에 호출됨
                print("환율이 \(oldRate)에서 \(currencyRate)으로 변경되었습니다")
            }
        }

        var dollar: Double = 0 {   // 프로퍼티 감시자 사용
            willSet {   // willSet의 암시적 매개변수 이름 newValue (willSet(parameter)에서 parameter 지정하지 않은 경우)
                print("\(dollar)달러에서 \(newValue)달러로 변경될 예정입니다")
            }       
            didSet {    // didSet의 암시적 매개변수 이름 oldValue
                print("\(oldValue)달러에서 \(dollar)달러로 변경되었습니다")
            }
        }

        var won: Double {   // 연산 프로퍼티
            get {
                return dollar * currencyRate
            }
            set {
                dollar = newValue / currencyRate
            }         
         // willSet {}   // 프로퍼티 감시자는 저장 프로퍼티에만 사용 가능 (연산 프로퍼티에는 불가) 
         // didSet {}
        }    
    }

    var moneyInMyPocket = Money()

    // 직전) 환율이 1100.0에서 1150.0으로 변경될 예정입니다 <- 동일한 currencyRate 이지만 다른 값이 나옴 (willSet은 변경 직전 값)
    moneyInMyPocket.currencyRate = 1150
    // 직후) 환율이 1100.0에서 1150.0으로 변경되었습니다   <- 동일한 currencyRate 이지만 다른 값이 나옴  (didSet은 변경 직후 값)

    // 직전) 0.0달러에서 10.0달러로 변경될 예정입니다
    moneyInMyPocket.dollar = 10
    // 직후) 0.0달러에서 10.0달러로 변경되었습니다

    print(moneyInMyPocket.won)  // 11500.0
    ```

    ```swift
    // 2) 상속받은 연산 프로퍼티에 적용

    class Account {
        var credit: Int = 0 {
        
            willSet {
                print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정입니다.")
            }
            didSet {
                print("잔액이 \(oldValue)원에서 \(credit)원으로 변경되었습니다.")
            }
        }
        
        var dollarValue: Double {
            get {
                return Double(credit) / 1000.0
            }
            set {
                credit = Int(newValue * 1000)
                print("잔액을 \(newValue)달러로 변경 중입니다.")
            }
        }
    }

    class ForeignAccount: Account {
        override var dollarValue: Double { // override를 통해 감시자를 추가함
            
            willSet {
                print("잔액이 \(dollarValue)달러에서 \(newValue)달러로 변경될 예정입니다.")
            }
            didSet {
                print("잔액이 \(oldValue)달러에서 \(dollarValue)달러로 변경되었습니다.")
            }
        }
    }

    let myAccount: ForeignAccount = ForeignAccount()

    // 직전) 잔액이 0원에서 1000원으로 변경될 예정입니다.
    myAccount.credit = 1000
    // 직후) 잔액이 0원에서 1000원으로 변경되었습니다.

    // 잔액이 1.0달러에서 2.0달러로 변경될 예정입니다.
    // 잔액이 1000원에서 2000원으로 변경될 예정입니다.
    // 잔액이 1000원에서 2000원으로 변경되었습니다.
    myAccount.dollarValue = 2 // 잔액을 2.0달러로 변경 중입니다. *감시자가 있는 프로퍼티가 함수의 argument로 전달되면, 감시자를 호출한다. (이 설명이 이 경우에 맞나???)
    // 잔액이 1.0달러에서 2.0달러로 변경되었습니다.
    ```

- Example - Language Guide

    ```swift
    class StepCounter {
        var totalSteps: Int = 0 {
            willSet(newTotalSteps) {
                print("About to set totalSteps to \(newTotalSteps)")
            }
            didSet {
                if totalSteps > oldValue  {
                    print("Added \(totalSteps - oldValue) steps")  
                }
            }
        }
    }

    let stepCounter = StepCounter()
    stepCounter.totalSteps = 200
    // About to set totalSteps to 200
    // Added 200 steps
    stepCounter.totalSteps = 360
    // About to set totalSteps to 360
    // Added 160 steps
    ```

## 13-3. Key Path

- 프로퍼티의 값을 바로 가져오는 게 아니라 프로퍼티의 위치만 참조하는 것이 가능하다.
- Key Path type은 AnyKeyPath 클래스로부터 파생된다. 가장 확장된 ??? key path type은 WritableKeyPath<Root, Value> (값 타입에 대해), ReferenceWritableKeyPath<Root, Value> (참조 타입, 즉 Class 타입에 대해) 타입 (제네릭 type)이다.
- 장점 : Type 간 의존성을 낮출 수 있다.
- Syntax

    ```swift
    \TypeName.path.path.path // path : property name

    // Key Path의 type 확인
    class Person {
        var name: String
        
        init(name: String) {
            self.name = name
        }
    }

    struct Stuff {
        var name: String
        var owner: Person
    }

    print(type(of: \Person.name)) // ReferenceWritableKeyPath<Person, String> 출력
    print(type(of: \Stuff.name)) // WritableKeyPath<Stuff, String> 출력

    // Key Path type의 경로 연결
    let keyPath = \Stuff.owner // Key Path를 상수에 할당한다.
    let nameKeyPath = keyPath.appending(path: \.name) // appending 메서드로 하위 경로를 덧붙였다. Stuff.Person.name

    // KeyPath 서브스크립트와 Key Path 활용
    let yagom = Person(name: "Yagom kim") // let 선언 인스턴스
    let hana = Person(name: "Hana Son")

    let macbook = Stuff(name: "MacBook Pro", owner: yagom)
    var iMac = Stuff(name: "iMac", owner: yagom) // var 선언 인스턴스
    let iPhone = Stuff(name: "iPhone", owner: hana)

    let stuffNameKeyPath = \Stuff.name // Key Path 할당
    let ownerKeyPath = \Stuff.owner
    let ownerNameKeyPath = ownerKeyPath.appending(path: \.name) // Stuff.owner 보다 구체적인 이름을 지정

    print(macbook[keyPath: stuffNameKeyPath]) // MacBook Pro
    print(iMac[keyPath: stuffNameKeyPath]) // iMac
    print(iPhone[keyPath: stuffNameKeyPath]) // iPhone

    print(macbook[keyPath: ownerNameKeyPath]) // Yagom kim
    print(iMac[keyPath: ownerNameKeyPath]) // Yagom kim
    print(iPhone[keyPath: ownerNameKeyPath]) // Hana Son

    iMac[keyPath: stuffNameKeyPath] = "Changed - iMac Pro" // Key Path를 통해 프로퍼티 값 변경이 가능하다. - Struct의 var 선언 인스턴스
    iMac[keyPath: ownerKeyPath] = hana

    print(iMac[keyPath: stuffNameKeyPath]) // Changed - iMac Pro
    print(iMac[keyPath: ownerNameKeyPath]) // Hana Son

    // macbook[keyPath: stuffNameKeyPath] = "Change - macBook M1" // Struct의 let 선언 인스턴스, 불변 프로퍼티는 프로퍼티 값 변경이 불가하다.
    // yagom[keyPath: \Person.name] = "bear"
    ```

## 13-4. *self 프로퍼티

- 모든 인스턴스는 암시적으로 생성된 self 프로퍼티를 가진다.
- 인스턴스 자기 자신을 가르킨다.
- 단, 타입 메서드의 self는 타입 자기 자신을 가르킨다. (인스턴스가 아니라 타입)
- 목적
1) 인스턴스를 더 명확히 지칭하고 싶을 때, 2) 값 타입 인스턴스 자체의 값을 치환할 때 사용한다. (Class 인스턴스는 참조 타칩이므로 self 프로퍼티에 다른 참조를 할당 불가함)

    ```swift
    // 1) 인스턴스를 더 명확히 지칭하고 싶을 때
    class LevelClass {
        var level: Int = 0
        
        func jumpLevel(to level: Int) {
            print("Jump to Level \(level)")
            self.level = level // self.level : Class 인스턴스의 프로퍼티, level : argument
        }
    }

    // 2) 값 타입 인스턴스 자체의 값을 치환할 때 
    // struct
    struct LevelStruct {
        var level: Int = 0
        
        mutating func levelUp() {
            print("Level Up!")
            level += 1
        }
        
        mutating func reset() {
            print("Reset!")
            self = LevelStruct() // *level = 0으로 프로퍼티 값을 초기화한다. (self : Stract 인스턴스)
        }
    }

    var levelInstance: LevelStruct = LevelStruct()
    levelInstance.levelUp() // Level Up!
    levelInstance.levelUp() // Level Up!
    print(levelInstance.level) // 2

    levelInstance.reset() // Reset!
    print(levelInstance.level) // 0

    // enum
    enum OnOffSwitch {
        case on, off
        mutating func changeState() {
            self = self == .on ? .off : .on // *인스턴트 자신의 현재 값을 판단하여 반전시킨다. (self : enum 인스턴스)
        }
    }

    var toggle: OnOffSwitch = OnOffSwitch.off
    toggle.changeState()
    print(toggle) // on

    // *타입 메서드의 self는 예외 - 인스턴스가 아니라 타입을 가르킴
    struct SystemVolume {
        static var volume: Int = 5 // 인스턴스 프로퍼티이면, 아래 메소드 정의에서 오류 발생 - instance member 'volume' cannot be used on type 'SystemVolume'
        
        static func mute() {
            self.volume = 0 // SystemVolume.volume = 0 과 동일한 표현 (self : Struct SystemVolume 타입)
        }
    }
    ```

# 14. 상속 (Inheritance) / 재정의 (Override)

- Language Guide (*용어 익숙)
    - Super/Sub Class
        - When one class inherits from another, the inheriting class is known as a subclass, and the class it inherits from is known as its superclass.
        The subclass inherits characteristics from the existing class, which you can then refine.
        - Subclasses can themselves be subclassed.
        - you access the superclass version of a method, property, or subscript by using the super prefix.
    - Property Overriding
        - You can use property overriding to provide your own custom getter and setter for that property, or to add property observers to an inherited property.
            - This enables you to be notified when the value of an inherited property changes, regardless of how that property was originally implemented. Property observers can be added to any property, regardless of whether it was originally defined as a stored or computed property.
            - If you provide a setter as part of a property override, you must also provide a getter for that override.
            - 단, 같은 프로퍼티에 옵저버를 추가하고 setter를 추가해 둘을 동시에 사용할 수 없다. 이미 setter를 설정했다면 옵저버를 붙인 것과 동일한 동작을 하기 때문이다.
            - example - getter override

                ```swift
                class Vehicle {  // superclass 생성
                    var currentSpeed = 0.0
                    var description: String {
                        return "traveling at \(currentSpeed) miles per hour"
                    }
                    func makeNoise() {
                        // do nothing - an arbitrary vehicle doesn't necessarily make a noise
                    }
                }
                let someVehicle = Vehicle() // Vehicle class의 인스턴스 생성
                print("Vehicle: \(someVehicle.description)") // Vehicle: traveling at 0.0 miles per hour

                class Bicycle: Vehicle {  // subclass 생성
                    var hasBasket = false  // superclass에 없는 새로운 프로퍼티 추가
                		var gear = 1
                    override var description: String {  
                        return super.description + " in gear \(gear)"
                }
                // The new Bicycle class automatically gains all of the characteristics of Vehicle, 
                // such as its currentSpeed and description properties and its makeNoise() method.

                let bicycle = Bicycle() // Bicycle class의 인스턴스 생성
                bicycle.hasBasket = true // 프로퍼티의 default 값 수정
                bicycle.currentSpeed = 15.0  // subclass Bicycle에서 따로 선언해주지 않아도, superclass Vehicle에서 정의한 프로퍼티를 사용 가능하다.
                print("Bicycle: \(bicycle.description)") // Bicycle: traveling at 15.0 miles per hour

                class Tandem: Bicycle {  // subclass의 subclass 생성 (tandem = Bicycle for a two-seater bicycle)
                    var currentNumberOfPassengers = 0
                }
                // Tandem inherits all of the properties and methods from Bicycle, 
                // which in turn inherits all of the properties and methods from Vehicle.

                let tandem = Tandem()
                tandem.hasBasket = true
                tandem.currentNumberOfPassengers = 2
                tandem.currentSpeed = 22.0
                print("Tandem: \(tandem.description)") // Tandem: traveling at 22.0 miles per hour
                ```

            - test - property observer 상속
                - [ ]  // *superclass의 인스턴스에 영향을 받지 않고, superclass의 default 값을 oldValue로 받아옴 (왜지?)
                - [ ]  // *인스턴스의 프로퍼티 값이 변경되어 프로퍼티 감시자가 작동할 때, superclass 및 subclass에 속한 감시자가 둘다 호출됨 (순서는 뭐지? override 했는데 superclass의 프로퍼티 감시자는 왜 작동하지???)

                ```swift
                // inheritance - property observer test

                class Money {
                    var currencyRate: Double = 1100 {   // 프로퍼티 감시자 사용
                        willSet(newRate) {   // willSet : 변경 직전에 호출됨
                            print("환율이 \(currencyRate)에서 \(newRate)으로 변경될 예정입니다")
                        }
                        didSet(oldRate) {   // didSet : 변경 직후에 호출됨
                            print("환율이 \(oldRate)에서 \(currencyRate)으로 변경되었습니다")
                        }
                    }

                    var dollar: Double = 0 {   // 프로퍼티 감시자 사용
                        willSet {   // willSet의 암시적 매개변수 이름 newValue (willSet(parameter)에서 parameter 지정하지 않은 경우)
                            print("\(dollar)달러에서 \(newValue)달러로 변경될 예정입니다")
                        }
                        didSet {    // didSet의 암시적 매개변수 이름 oldValue
                            print("\(oldValue)달러에서 \(dollar)달러로 변경되었습니다")
                        }
                    }

                    var won: Double {   // 연산 프로퍼티
                        get {
                            return dollar * currencyRate
                        }
                        set {
                            dollar = newValue / currencyRate
                        }
                     // willSet {}   // 프로퍼티 감시자는 저장 프로퍼티에만 사용 가능 (연산 프로퍼티에는 불가)
                     // didSet {}
                    }
                }

                class subClassMoney: Money {
                    override var currencyRate: Double {   // 프로퍼티 감시자 사용
                        willSet(newRate) {   // willSet : 변경 직전에 호출됨
                            print("override - 환율이 \(currencyRate)에서 \(newRate)으로 변경될 예정입니다")
                        }
                        didSet(oldRate) {   // didSet : 변경 직후에 호출됨
                            print("override - 환율이 \(oldRate)에서 \(currencyRate)으로 변경되었습니다")
                        }
                    }
                }

                var moneyInMyPocket = Money()

                moneyInMyPocket.currencyRate = 1150
                // 환율이 1100.0에서 1150.0으로 변경될 예정입니다 <- 동일한 currencyRate 이지만 다른 값이 나옴 (willSet은 변경 직전 값)
                // 환율이 1100.0에서 1150.0으로 변경되었습니다   <- 동일한 currencyRate 이지만 다른 값이 나옴  (didSet은 변경 직후 값)

                moneyInMyPocket.dollar = 10
                // 0.0달러에서 10.0달러로 변경될 예정입니다
                // 0.0달러에서 10.0달러로 변경되었습니다

                print(moneyInMyPocket.won)  // 11500.0

                var moneyInSubClass = subClassMoney()

                moneyInSubClass.currencyRate = 3000
                // 출력값 -
                //override 환율이 1100.0에서 3000.0으로 변경될 예정입니다
                //환율이 1100.0에서 3000.0으로 변경될 예정입니다
                //환율이 1100.0에서 3000.0으로 변경되었습니다
                //override 환율이 1100.0에서 3000.0으로 변경되었습니다
                // *superclass의 인스턴스에 영향을 받지 않고, superclass의 default 값을 oldValue로 받아옴 (왜지?)
                // *인스턴스의 프로퍼티 값이 변경되어 프로퍼티 감시자가 작동할 때, superclass 및 subclass에 속한 감시자가 둘다 호출됨 (순서는 뭐지? override 했는데 superclass의 프로퍼티 감시자는 왜 작동하지???)

                moneyInSubClass.dollar = 20
                // 출력값 -
                //0.0달러에서 20.0달러로 변경될 예정입니다
                //0.0달러에서 20.0달러로 변경되었습니다
                // *superclass의 인스턴스에 영향을 받지 않고, superclass의 default 값을 oldValue로 받아옴 (왜지?)

                ```

        - You can prevent a method, property, or subscript from being overridden by marking it as final. (You can mark an entire class as final.)

- 특징
    - 상속은 클래스, 프로토콜 등에서 가능하다. (열거형, 구조체는 불가)
    - 스위프트의 클래스는 단일 상속으로, 다중 상속을 지원하지 않는다. (부모클래스는 1개만 가질 수 있다.)
    - 상속받은 메서드, 프로퍼티, 서브스크립트 등의 특성을 사용 가능하다.
    1) 그대로 사용 - 부모와 동일
    2) override를 통해 기능을 변경 
    3) 자식만의 새로운 기능을 추가 - 새로운 프로퍼티/메서드 생성
- Syntax
    - 정의
    - Superclass (부모 클래스) & Subclass (자식 클래스)
    - 조상 클래스 : 부모 클래스를 포함한 상위 부모클래스
    - Base class (기반 클래스) : 다른 클래스를 상속받지 않는 클래스 (조상 클래스가 없는 클래스)

        ```swift
        class 이름: 부모클래스 이름 {
            /* 구현부 */
        }
        ```

## Override (재정의, 덮어쓰기)

- `override` 키워드를 사용하면, 컴파일러가 조상클래스에 해당 특성(=메서드/프로퍼티/서브스크립트)이 있는지 확인하고 재정의한다. (조상에 없으면 컴파일 오류 발생)
- 자식클래스에서 특성을 override 했지만 부모클래스의 특성을 활용하고 싶을 경우, `super`를 사용한다. (`super.methodA` `super.propertyA` `super[index]`으로 부모버전을 호출한다.)
- 메서드 재정의
    - 메서드 이름 및 return type이 일치해야 재정의 가능하다. (return type이 다르면 다른 메서드로 취급한다. - 중복정의 (overload))
    - 메서드 재정의는 인스턴스 메서드, class 타입 메서드만 가능하다. (static 타입 메서드 X, final이 붙은 메서드 X)
    - 타입 메서드
    - static 키워드를 사용해 타입 메서드를 만들면 override 불가하다.
    - class 키워드를 사용해 타입 메서드를 만들면 override 가능하다.
    - class 앞에 final을 붙이면 (final class func) static 키워드를 사용한것과 동일하게 동작한다.
        - 활용1

            ```swift
            // 클래스 정의
            class Person {   // 부모 클래스 (기반 클래스) Person
                var name: String = "unknown"   // 인스턴스 프로퍼티
                
                func selfIntroduce() {   // 인스턴스 메서드   **override 가능!!!
                    print("저는 \(name)입니다")
                }
                
                final func sayHello() {    // final 키워드를 사용하여 override를 방지 (final 인스턴스 메서드)
                    print("hello")
                }
                
                static func typeMethod() {    // override 불가 타입 메서드 - static
                    print("type method - static")
                }
                
                class func classMethod() {    // override 가능 타입 메서드 - class   **override 가능!!!
                    print("type method - class")
                }
                
                final class func finalCalssMethod() {    // 메서드 앞의 `static`과 `final class`는 동일한 역할 (override 불가)
                    print("type method - final class")
                }
            }

            class Student: Person {   // 부모 클래스 Person을 상속받는 자식 클래스 Student
            //  var name: String = "unknown"   // 오류 발생 - 저장 프로퍼티는 override 불가함 (부모 클래스에서 이미 정의했으므로)
                var major: String = ""
                
                override func selfIntroduce() {    // 기존의 인스턴스 메서드를 override 했다.
                    print("저는 \(name)이고, 전공은 \(major)입니다")
                }

            //  super.selfIntroduce()   // *super.메서드명 - 부모 클래스의 메서드를 호출한다.
                
                override class func classMethod() {   // class 타입 메서드를 override 했다.
                    print("overriden type method - class")
                }
                
            //  override static func typeMethod() {}    // static을 사용한 타입 메서드는 override 불가
                
            //  override func sayHello() {}    // final 키워드를 사용한 메서드, 프로퍼티는 override 불가
            //  override class func finalClassMethod() {}
            }

            // 클래스 사용
            let yagom: Person = Person()   // 부모 클래스 Person의 인스턴스 생성
            let hana: Student = Student()  // 자식 클래스 Student의 인스턴스 생성

            yagom.name = "yagom"
            hana.name = "hana"
            hana.major = "Swift"

            yagom.selfIntroduce() // 저는 yagom입니다
            hana.selfIntroduce() // 저는 hana이고, 전공은 Swift입니다 - override 했으므로

            Person.classMethod() // type method - class
            Person.typeMethod() // type method - static
            Person.finalCalssMethod() // type method - final class

            Student.classMethod() // overriden type method - class
            Student.typeMethod() // type method - static
            Student.finalCalssMethod() // type method - final class
            ```

        - 활용2

            ```swift
            class Student {
                var name: String = "unknown"  // 인스턴트 저장 프로퍼티
                
                static var storedProperty: Int = 10  // 타입 저장 프로퍼티
                
                func selfIntroduce() {  // 인스턴스 메서드
                    print("저는 \(name)입니다")
                }

                final func finalMethod() {  // 인스턴스 메서드
                    print("finalMethod 입니다")
                }

                static func typeMethodStatic() {  // 타입 메서드 - static
                    print("static type method")
                }
               
                class func typeMethodClass() {  // 타입 메서드 - class (override 가능)
                    print("class type method")
                }
                
                final class func finalTypeMethodClass() {  // final 타입 메서드 - class (override 불가)
                    print("final - class type method")
                }
            }

            class University: Student {
                var major: String = "major0"
                
                override func selfIntroduce() {
                    print("저는 \(name) 이고, 전공은 \(major) 입니다.")
                }
                
                override class func typeMethodClass() {
                    print("override class type method")
                }
            }

            // subclass
            University.typeMethodClass()  // child class의 타입 메서드 (부모 class의 타입 메서드를 override 했던)
            // override class type method - 출력

            var kevin: University = University()
            kevin.name = "kevin"
            kevin.major = "computer science"
            kevin.selfIntroduce()  // 저는 kevin 이고, 전공은 computer science 입니다.
            kevin.finalMethod()  // 부모 class의 인스턴스 메서드 호출해보기 - finalMethod 입니다 출력

            // superclass
            // 타입 프로퍼티 확인
            print(Student.storedProperty) // 10

            // 타입 메서드 사용
            Student.typeMethodStatic() // static type method - 출력
            Student.typeMethodClass() // class type method - 출력
            Student.finalTypeMethodClass() // final - class type method

            // var선언 인스턴스 생성
            var yagom: Student = Student()
            yagom.name = "yagom"
            yagom.selfIntroduce() // 저는 yagom입니다
            yagom.finalMethod() // finalMethod 입니다

            // let선언 인스턴스 생성
            let jina: Student = Student()
            jina.name = "jina"    // let선언 인스턴스이지만 가변 프로퍼티는 수정가능하다! - Class는 값이 아니라 참조이므로 (Structure와 다름)
            jina.selfIntroduce()  // 저는 jina입니다 - 출력 (Structure에서는 unknown)

            yagom.selfIntroduce()  // 저는 yagom입니다
            ```

- 프로퍼티 재정의
    - 저장 프로퍼티는 재정의 불가하다. (인스턴스 및 타입 모두 불가)
    - 프로퍼티 이름 및 type이 일치해야 재정의 가능하다.
    - 프로퍼티 재정의는 프로퍼티 자체가 아니라 <프로퍼티 getter&setter, 프로퍼티 감시자>를 재정의하는 것을 의미한다.
        - 조상클래스의 저장 프로퍼티 및 연산 프로퍼티의 getter&setter 재정의가 가능하다. (원래는 연산 프로퍼티만 getter&setter 사용이 가능하다.)
        - 자식클래스 입장에서 조상클래스의 프로퍼티 종류(저장/연산)는 알 수 없고, 이름 및 type만 알기 때문이다.
            - 읽고-쓰기 프로퍼티를 재정의할 때, getter 또는 setter 중 하나를 그대로 쓰더라도 모두 재정의해야 한다. (그대로 쓰면 `super.sameProperty`로 부모 값을 받아서 반환한다.)
            - 조상클래스에서 읽기전용 프로퍼티였더라도 자식클래스에서 읽고-쓰기 프로퍼티로 재정의 가능하다. 
            - 단, 읽고-쓰기 프로퍼티를 읽기전용으로 재정의는 불가하다.

        ```swift
        class Person {
            var name: String = ""
            var westernAge: Int = 0
            
            var koreanAge: Int { // 부모-읽기전용
                return self.westernAge + 1
            }
            
            var introduction: String {
                return "이름 : \(name), 나이 : \(westernAge)"
            }
        }

        class Student: Person {
            var grade: String = "A"
            
            override var introduction: String {
                return super.introduction + ", 학점 : \(self.grade)" // 부모클래스의 String 뒤에 추가 String을 붙여서 반환한다.
            }
            
            override var koreanAge: Int {
                get {
                    return super.koreanAge // getter는 그대로 - 부모클래스 접근자 super를 사용하여 부모 값을 받아서 반환한다.
                }
                set { // 자식-쓰기 기능 추가
                    self.westernAge = newValue - 1
                }
            }
        }

        let yagom: Person = Person()
        yagom.name = "yagom"
        yagom.westernAge = 55
        //yagom.koreanAge = 56 // *setter가 없으므로 컴파일 오류 발생 - 연산 프로퍼티 참고
        print(yagom.introduction) // 이름 : yagom, 나이 : 55
        print(yagom.koreanAge) // 56 - getter 결과로 자동 할당된 상태이다.

        let sam: Student = Student()
        sam.name = "sam"
        sam.westernAge = 15
        sam.koreanAge = 30 // *setter가 있으므로 할당 가능
        print(sam.introduction) // 이름 : sam, 나이 : 29, 학점 : A
        print(sam.koreanAge) // 30
        print(sam.westernAge) // 29 - setter 결과
        ```

- 프로퍼티 감시자 재정의
    - 상속받은 연산 프로퍼티 및 저장 프로퍼티의 프로퍼티 감시자를 구현 가능하다.
    - 원래는 연산 프로퍼티를 정의한 부모클래스에서 연산 프로퍼티에 감시자 구현이 불가하다.
    - 상수 저장 프로퍼티, 읽기전용 연산 프로퍼티는 감시자 재정의가 불가하다. (선언 이후 값 설정(set)이 불가해서, 감시자의 사용이 원천적으로 불가하므로)
    - getter 및 감시자는 '동시에 재정의' 불가하다. ??? (동시 작동을 원한다면, 재정의하는 getter에 프로퍼티 감시자 역할을 구현해야 한다.)
    - 동일한 프로퍼티에 setter 및 감시자를 '동시에 정의' 불가하다. (이미 setter를 설정했다면 감시자를 붙인 것과 동일하게 동작하므로) - 재정의와 상관 없이???
    - 자식클래스가 감시자를 재정의하더라도 조상클래스에 정의한 감시자도 동작한다.
        - [ ]  왜 이렇게 만들었지???

        ```swift
        class Person {
            var name: String = ""
            var westernAge: Int = 0 {
                didSet {  
                    print("PO didSet - Person age : \(self.westernAge)")
                }
            }
            var koreanAge: Int {
                return self.westernAge + 1
            }
            var fullName: String { // 연산 프로퍼티
                get {
                    return self.name
                }
                set {
                    self.name = newValue
                }
            }
        }

        class Student: Person {
            var grade: String = "A"
            
            override var westernAge: Int {
                didSet {
                    print("PO didSet - Student age : \(self.westernAge)")  // 감시자 내용 변경
                }
            }
            
            override var koreanAge: Int {
                get {
                    return super.koreanAge
                }
                set {
                    self.westernAge = newValue - 1  // setter 추가
                }
        //      didSet {} // 컴파일 오류 발생 - 'didSet' cannot be provided together with a getter (+ setter 및 감시자 동시 정의 또한 불가하다)
            }
            
            override var fullName: String {
                didSet { // 부모클래스에서 연산 프로퍼티로 정의되었으나, 자식클래스는 감시자 구현이 가능하다.
                    print("PO didSet - Full Name : \(self.fullName)")  // 감시자 추가
                }
            }
        }

        let yagom: Person = Person()
        yagom.name = "yagom"
        yagom.westernAge = 55
        // PO didSet - Person age : 55
        print(yagom.koreanAge) // 56

        yagom.fullName = "Jo yagom"

        let san: Student = Student()
        san.name = "san"
        san.westernAge = 15
        // PO didSet - Person age : 15 -> ***재정의했지만 조상클래스의 감시자도 동작한다.
        // PO didSet - Student age : 15

        san.koreanAge = 30
        // PO didSet - Person age : 29 -> ***재정의했지만 조상클래스의 감시자도 동작한다.
        // PO didSet - Student age : 29

        print(san.koreanAge) // 30
        print(san.westernAge) // 29

        san.fullName = "An san"
        // PO didSet - Full Name : An san
        ```

- 서브스크립트 재정의
    - 서브스크립트 이름, parameter/return type이 일치해야 재정의 가능하다.
    - 메서드 재정의와 방법이 동일하다.

        ```swift
        struct Student {
            var grade: String = "A"
        }

        class School {
            var students: [Student] = [Student]()
            
            subscript(number: Int) -> Student {
                print("School Subscript")
                return students[number]
            }
        }

        class MiddleSchool: School {
            var middleStudents: [Student] = [Student]()
            
        		override subscript(index: Int) -> Student {
                print("Middle School Subscript")
                return middleStudents[index]
            }
        }

        let university: School = School()
        university.students.append(Student())
        university[0] // School Subscript

        let middle: MiddleSchool = MiddleSchool()
        middle.middleStudents.append(Student())
        middle[0] // (서브스크립트 재정의를 한 경우) Middle School Subscript

        middle[0] // 참고 - (안한 경우) School Subscript, 에러 발생 - Swift/ContiguousArrayBuffer.swift:580: Fatal error: Index out of range
        ```

- 재정의 방지 (final)
    - `final` 키워드로 특성의 재정의를 방지한다. `final var` `final func` `final class func` `final subscript` (final-을 재정의 시 컴파일 오류 발생)
    - static은 원래 재정의 불가하다. (static 타입 메서드 또는 static 타입 프로퍼티)
    - Class 자체를 상속하지 못하게 하려면 `final class`로 명시한다.

## Class 이니셜라이저

- 값 타입과 달리, Class 이니셜라이저는 이니셜라이저 위임을 위해 '지정 이니셜라이저(Designated Initializer)' 및 '편의 이니셜라이저(Convenience Initializer)'로 역할을 구분한다.
    - 지정init / 편의init
        - 지정 이니셜라이저 (main 역할)
        - 해당 Class의 모든 프로퍼티를 초기화해야 한다. (`var name: String = ""`와 같이 Class를 정의할 때 모든 프로퍼티의 기본값을 지정하면 이니셜라이저가 필요 없다.)
        - 부모클래스의 이니셜라이저를 호출 가능하다. 
        - 모든 Class는 1개 이상의 지정 이니셜라이저를 갖는다. (단, 상속받은 조상클래스의 지정 이니셜라이저가 자손클래스에서 충분히 역할 수행이 가능하다면, 자손클래스는 지정 이니셜라이저를 갖지 않을 수 있다.) - 상속받아서 암시적으로 갖고 있는 거 아닌가??
        - 편의 이니셜라이저 (Optional)
        - 편의 이니셜라이저의 내부에서 지정 이니셜라이저를 호출한다. 
        - Class 설계자의 의도를 반영하여 인스턴스를 초기화하는 등 특수 목적에 따라 선택적으로 사용한다.
        - 🎃🎃🎃 인스턴스 생성 후 프로퍼티 값을 수정하기 어려운 경우에는 이니셜라이저 init을 통해 인스턴스가 가져야 할 초기값을 전달 가능하다.

            ```swift
            class PersonB {
                var name: String
                var age: Int
                var nickName: String
                
                init(nameKeyIn: String, ageKeyIn: Int, nickNameKeyIn: String) {  // 이니셜라이저
                    self.name = nameKeyIn  // 인스턴스 초기화 단계에서 argument (오른쪽)가 각 프로퍼티 (왼쪽)로 들어간다.
                    self.age = ageKeyIn
                    self.nickName = nickNameKeyIn
                }
            }

            let hana: PersonB = PersonB(nameKeyIn: "hana", ageKeyIn: 20, nickNameKeyIn: "하나")  // 인스턴스 생성 시 init의 parameter에 따라 초기값을 지정할 수 있다.
            // let hana: PersonB = PersonB() 로 입력하면 parameter 없다고 오류 발생

            print(hana.name) // hana
            print(hana.age) // 20
            print(hana.nickName) // 하나
            ```

        - 일부 프로퍼티가 필수 항목이 아닐 때는 옵셔널을 사용하고, 이니셜라이저 init을 2개 생성할 수 있다.
        → 초기화 하는 방법이 2가지인 것임

            ```swift
            // nickname이 선택 사항인 경우

            class PersonC {
                var name: String
                var age: Int
                var nickName: String?
                
                init(nameKeyIn: String, ageKeyIn: Int) {
                    self.name = nameKeyIn
                    self.age = ageKeyIn
                }
                
                init(nameKeyIn: String, ageKeyIn: Int, nickNameKeyIn: String) { // init parameter는 프로퍼티명과 꼭 동일할 필요 없음
                    self.name = nameKeyIn
                    self.age = ageKeyIn
                    self.nickName = nickNameKeyIn
                }
            }

            let kevin: PersonC = PersonC(nameKeyIn: "kevin", ageKeyIn: 10) // 초기화 방법이 2가지
            let mike: PersonC = PersonC(nameKeyIn: "mike", ageKeyIn: 15, nickNameKeyIn: "m")

            print(kevin.nickname)  // nil 출력
            ```

            ```swift
            // convenience init을 통해 중복 최소화 

            class PersonC {
                var name: String
                var age: Int
                var nickName: String?
                
                **init**(nameKeyIn: String, ageKeyIn: Int) {
                    self.name = nameKeyIn
                    self.age = ageKeyIn
                }
                
            /*     ~~init(nameKeyIn: String, ageKeyIn: Int, nickNameKeyIn: String) {
                    self.name = nameKeyIn
                    self.age = ageKeyIn
                    self.nickName = nickNameKeyIn
                } */~~

            		convenience init(nameKeyIn: String, ageKeyIn: Int, nickNameKeyIn: String) {  // 위와 동일한 기능
                   self**.init**(nameKeyIn: nameKeyIn, ageKeyIn: ageKeyIn)  // type이 아니라 다른 init의 parameter로 전달될 argument가 들어감
                   self.nickName = nickNameKeyIn
              }
            }

            let kevin: PersonC = PersonC(nameKeyIn: "kevin", ageKeyIn: 10)
            let mike: PersonC = PersonC(nameKeyIn: "mike", ageKeyIn: 15, nickNameKeyIn: "m")
            ```

            - [x]  self.init(nameKeyIn: nameKeyIn, ageKeyIn: ageKeyIn)  // 왜 type을 명시하지 않지?
                - type이 아니라 다른 init의 parameter로 전달될 argument가 들어감

            - 암시적 추출 옵셔널! 은 인스턴스 사용에 꼭 필요하지만 초기값을 할당하지 않고자 할 때 사용

                ```swift
                // 구현 - 강아지는 주인없이 산책하면 안돼요!
                class Puppy {
                    var name: String
                    var owner: PersonC!  // String! 이 아니라 프로퍼티 owner의 data type이 PersonC Class라는 뜻
                    
                    init(name: String) {
                        self.name = name
                    }
                    
                    func goOut() {
                        print("\(name)가 주인 \(owner.name)와 산책을 합니다")  // owner가 nil인 경우 오류 발생함 (암시적 추출 옵셔널!을 사용했기 때문)
                    }
                }

                let happy: Puppy = Puppy(name: "happy")
                //happy.goOut()   // 주인이 없는 상태라서 오류 발생 
                happy.owner = kevin   // "kevin"이 아님 (PersonC Class type의 인스턴스 kevin 이므로)
                happy.goOut()  // happy가 주인 kevin와 산책을 합니다 - 출력
                ```

    - Class 초기화
        - 초기화 위임 규칙

            ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2013.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2013.png)

            1. 자식의 모든 지정init은 부모의 지정init을 반드시 호출해야 한다.
            2. 편의init은 자신이 속한 Class의 다른 init을 반드시 호출해야 한다. (부모의 init 호출 불가)
            3. 편의init은 궁극적으로 지정init을 반드시 호출해야 한다.

            즉, *누군가=다른 지정init 또는 다른 편의init
            - 누군가는 지정init에게 초기화를 반드시 위임한다.
            - 편의init은 초기화를 반드시 누군가에 위임한다. (다른 지정init 또는 다른 편의init)

            다른 버전으로 보면,
            - 지정init은 반드시 위임을 superclass로 해야하고
            - 편리init은 반드시 위임을 같은 레벨에서 해야한다.

        - 초기화 과정 (2단계)
            - Swift 컴파일러는 초기화를 에러 없이 완료하기 위해 4단계 안전확인(safety-check)을 한다.
            - 안전확인 이후 초기화 1단계/2단계를 진행한다. 맞나?????
            - 초기화 안전확인 (safty-checks)
                1. 자식의 지정init은 부모의 init을 호출하기 이전에 "자신의 프로퍼티"를 모두 초기화했는지 확인한다. 
                (*따라서 자식의 지정init이 부모의 지정init을 호출하기 이전에 자신의 모든 (기본값이 없는) 프로퍼티에 값을 할당해야 한다.) → 충돌할 수도 있어서????????
                = 자식의 지정init은 "자신의 프로퍼티"를 모두 초기화한 이후에 → 부모의 init을 호출한다.
                2. 자식의 지정init은 "상속받은 프로퍼티"에 값을 할당하기 이전에 부모의 지정init를 호출해야 한다.
                = 자식의 지정 init은 부모의 지정init을 호출한 이후에 → 자식이 "상속받은 프로퍼티"에 값을 할당한다.
                3. 편의init은 (자신의 Class에 정의한 프로퍼티를 포함하여) 어떤 프로퍼티라도 값을 할당하기 이전에 다른 init을 호출해야 한다. ????????
                = 편의init은 우선 다른 init을 호출한 이후에 → 프로퍼티의 값을 할당한다.
                4. 초기화 1단계 완료 이전에 모든 init은 인스턴스 메서드 호출, 인스턴스 프로퍼티의 값 읽기, self 활용이 불가하다.
                = init이 인스턴스 메서드 호출, 인스턴스 프로퍼티의 값 읽기, self 활용을 하려면 초기화 1단계가 완료되어야 한다.

            - 초기화 1단계 : init을 통해 저장 프로퍼티에 초기값을 할당한다.
                1. Class가 지정init 또는 편의init을 호출한다.
                2. Class 인스턴스를 위해 메모리가 할당된다. 메모리는 아직 초기화되지 않은 상태이다.
                3. 지정init은 해당 Class의 모든 프로퍼티 (자식 본인의 프로퍼티)에 값이 있는지 확인한다. 이제 Class 부분까지의 저장 프로퍼티를 위한 메모리가 초기화되었다.
                4. 지정init은 부모init이 동일하게 동작하도록 초기화를 양도한다. ???? 동작주체가 넘어간다는 뜻?
                5. 부모클래스는 상속 체인을 따라 최상위 클래스에 도달할 때까지 해당 작업을 반복한다. 
                (최상위 클래스까지의 모든 저장 프로퍼티에 값이 있음을 확인하면, 해당 인스턴스의 메모리는 모두 초기화된 것이다.)
            - 초기화 2단계 : 저장 프로퍼티를 사용자 정의한다. ?????
            - 1단계 완료 이전에 프로퍼티 값에 접근하는 것을 방지하여 안전성을 확보하는 목적이다. (다른 init이 실수로 프로퍼티 값을 변경하는 것을 방지함)
                1. 최상위 클래스부터 최하위 클래스까지 상속 체인을 따라내려오면서 지정init들이 인스턴스를 각각 사용자 정의한다. (여기서 self를 통해 프로퍼티 값을 수정하거나, 인스턴스 메서드의 호출 등 가능)
                2. 편의init을 통해 self를 통한 사용자 정의를 진행한다.

            ```swift
            class Person {
                var name: String
                var age: Int
                
                init(nameInput: String, ageInput: Int) {
                    self.name = nameInput
                    self.age = ageInput
                }
            }

            class Student: Person {
                var major: String
                
                init(nameInput2: String, ageInput2: Int, majorInput: String) {
                    self.major = "Swift" // 구현 - 프로퍼티 major는 항상 동일한 값으로 초기화
                    super.init(nameInput: nameInput2, ageInput: ageInput2) // 없으면 컴파일 오류 발생 - 'super.init' isn't called on all paths before returning from initializer.
                }
            		// 1. 자식의 지정init은 부모의 지정init을 호출하기 이전에 자신의 self로 major 프로퍼티의 값을 할당한다. - 안전확인 조건-1 만족
            		// 2. super.init으로 부모의 init을 호출하고, 상속받은 프로퍼티 (name, age)에 값을 할당한다. 그 후 값을 할당할 다른 상속받은 프로퍼티가 없다. - 안전확인 조건-2 만족
                
                convenience init(nameInput3: String) {
                    self.init(nameInput2: nameInput3, ageInput2: 7, majorInput: "iOS") // 구현 - 프로퍼티 age, major는 항상 동일한 값으로 초기화
                }
            		// 3. 편의init은 차후에 값을 할당할 프로퍼티가 없고, 다른 init을 호출했다. - 안전확인 조건-3 만족
            		// 4. init 어디에서도 인스턴스 메서드를 호출하거나, 인스턴스 프로퍼티의 값을 읽어오지 않았다. - 안전확인 조건-4 만족
            }

            var st: Student = Student(nameInput2: "지정init", ageInput2: 100, majorInput: "뭘넣어도결과는Swift") // 지정init으로 초기화
            print("\(st.name), \(st.age), \(st.major)") // 지정init, 100, Swift 출력 - 항상 major는 Swift

            st = Student(nameInput3: "7살의iOSmater신동") // 편의init으로 초기화
            print("\(st.name), \(st.age), \(st.major)") // 7살의iOSmater신동, 7, Swift 출력 - 항상 age는 7, major는 iOS
            ```

## Class 이니셜라이저 상속/재정의

- Class는 이니셜라이저 상속이 가능하므로 재정의에 주의해야 한다.
- init 재정의
    - 기본적으로 init은 부모클래스의 init을 상속받지 않는다. 
    (부모init은 자식클래스에 최적화되어 있지 않으므로 자식클래스의 인스턴스가 부정확하게 초기화되는 것을 방지하는 목적이다. 단, 안전하다고 판단되면 부모의 init이 상속받기도 한다. - init 자동상속 참고)
    - 자식이 부모와 동일한 init을 사용하고 싶을 경우, 보통 부모의 init과 동일하게 자식의 init을 구현한다. (이게 기본init???)
        - 부모의 지정init과 *동일하게 자식의 지정init이 구현하려면 재정의를 한다. *동일한 = parameter가 동일한???
            - 부모의 지정init을 자식의 편의init이 재정의 가능하다.
            - 반면, 부모의 편의init을 자식의 편의init에 구현하려면 재정의하지 않는다. (자식은 부모의 편의init을 절대 호출 불가하다. 따라서 부모의 편의init은 재정의 불가) 부모의 편의init을 복붙하여 자식의 편의init으로 쓴다.

            ```swift
            class Person {
                var name: String
                var age: Int
                
                init(nameInput: String, ageInput: Int) {
                    self.name = nameInput
                    self.age = ageInput
                }

                convenience init(nameInputA: String) {
                    self.init(nameInput: nameInputA, ageInput: 0)
                }
            }

            class Student: Person {
                var major: String

            		override init(nameInput nameInput2: String, ageInput ageInput2: Int) { // 주의 - override한 부모의 init과 argument label이 일치해야 한다. (label을 따로 지정하지 않으면, parameter이름이 label이 된다)
            //  override init(nameInput2: String, ageInput2: Int) {
                // 오류 발생 - Argument labels for initializer 'init(nameInput2:ageInput2:)' do not match those of overridden initializer 'init(nameInput:ageInput:)'
                
                    self.major = "Swift"
                    super.init(nameInput: nameInput2, ageInput: ageInput2)
                }	 

            		convenience init(nameInput3: String) { // 부모의 편의init과 자식의 편의init이 동일하지만, 재정의 불가하다.
            //      self.init(nameInput2: nameInput3, ageInput2: 7, majorInput: "iOS")
                    // 오류 발생1 - Extra argument 'ageInput2' in call
                    // 오류 발생2 - incorrect argument label in call (have 'nameInput2:ageInput:', expected 'nameInput:ageInput:')
                    
                    self.init(nameInput: nameInput3, ageInput: 100) // 주의 - self.init의 argument label (parameter명이 아니라?!)을 기준으로 일치해야 한다.
                }

            		init(nameInput2: String, ageInput2: Int, majorInput: String) { // 확인용 - extra init도 사용 가능하다.
                    self.major = "extra init-Swift"
                    super.init(nameInput: nameInput2, ageInput: ageInput2)
                }
            }

            var st = Student(nameInput: "재정의_지정init", ageInput: 999)
            print("\(st.name), \(st.age), \(st.major)") // 재정의_지정init, 999, Swift

            st = Student(nameInput3: "7살의iOSmater신동")
            print("\(st.name), \(st.age), \(st.major)") // 7살의iOSmater신동, 100, Swift - 편의init이므로 100 출력

            st = Student(nameInput2: "확인용", ageInput2: 1, majorInput: "뭐든")
            print("\(st.name), \(st.age), \(st.major)") // 확인용, 1, extra init-Swift 
            ```

            ```swift
            // 자동상속

            class Person {
                var name: String
                var age: Int
                
                init(nameInput: String, ageInput: Int) {
                    self.name = nameInput
                    self.age = ageInput
                }
            }

            class Test: Person { // init을 따로 구현하지 않았고, 부모의 init을 상속받는다.
                func gogo() {
                    print("\(self.name), \(self.age)")
                }
            }
            var test: Test = Test(nameInput: "auto", ageInput: 0)
            test.gogo() // auto, 0
            ```

- 실패가능한 이니셜라이저 init? 재정의
    - 부모클래스의 실패가능한 이니셜라이저(init?)를 자식클래스에서 재정의하는 경우,
    실패가능한 이니셜라이저(init?), 실패하지 않는 이니셜라이저(init) 모두 사용 가능하다.
        - [ ]  super.init(name: name, age: age) // 부모의 init?을 재정의했고, 부모의 init?을 호출하므로 nil 가능성이 있어서 -> init?으로 사용한다. 
        // 근데 왜 super.init?(name: name, age: age) -> 가 아니지?

        ```swift
        class Person {
            var name: String
            var age: Int
            
            init() {
                self.name = "Unknown"
                self.age = 0
            }
            
            init?(name: String, age: Int) {
                if name.isEmpty { // 참고) "".isEmpty = true
                    return nil
                }
                self.name = name
                self.age = age
            }
            
            init?(age: Int) {
                if age < 0 {
                    return nil
                }
                self.name = "Unknown"
                self.age = age
            }
        }

        class Student: Person {
            var major: String
            
            override init?(name: String, age: Int) { // 부모의 2번째 init (init?)을 *init?으로 override
                self.major = "누구나 Swift"
                super.init(name: name, age: age) // 부모의 init?을 재정의했고, 부모의 init?을 호출하므로 nil 가능성이 있어서 -> init?으로 사용한다. 
        				// 근데 왜 super.init?(name: name, age: age) -> 가 아니지?
            }
            
            override init(age: Int) { // 부모의 3번째 init (init?)을 override *init으로 override
                self.major = "누구나 Swift"
                super.init() // ***부모의 init?을 재정의했지만, (상속받은 부모 init?이 아니라) 부모의 init을 호출하므로 nil 가능성이 제거되었으므로 -> init으로 사용한다.
            }
        }

        var st: Student = Student(name: "kevin", age: -1)!
        print("\(st.name), \(st.age), \(st.major)") // kevin, -1, 누구나 Swift <- age=-1이 할당된다. 부모의 두번째 init?이 호출되기 때문 (이름만 ""가 아니면 age는 그대로 출력)

        st = Student(name: "", age: -1)! // nil이 return되어 컴파일 에러 발생 - Unexpectedly found nil while unwrapping an Optional value
            
        st = Student(age: -1) // nil 가능성이 없으므로 unwrapping도 필요 없음
        print("\(st.name), \(st.age), \(st.major)") // Unknown, 0, 누구나 Swift <- age=-1이 할당되지 않는다. 부모의 첫번째 init이 호출되기 때문
        ```

- init 자동상속
    - 대부분의 경우 자식클래스에서 init 재정의를 할 필요가 없다...
    - 자동상속 2가지 규칙

        - 전제 : 자식클래스에서 프로퍼티 기본값을 모두 제공한다. (상속받지 않은 자식 본인의 프로퍼티???????) 또는 프로퍼티 기본값을 제공하지 않으나 init을 통해 초기화된다.

        1. 자식클래스에서 별도의 지정init을 구현하지 않는 경우, 부모의 모든 init (부모의 지정init 및 부모의 편의init)이 자동상속된다.
        2. 규칙-1에 따라 자식클래스에서 부모의 지정init을 자동상속 받은 경우,
        또는 자식클래스에서 부모의 지정init을 모두 **재정의하여 사용하는 경우, 부모의 편의init이 자동상속된다.

               - 자식클래스에 지정init 및 편의init을 추가해도 유효하다.
               - **자식클래스에서 부모의 지정init을 (자식의 지정init으로 재정의하지 않고) 자식의 편의init으로 구현하더라도 (이것도 재정의이다.) 규칙-2를 만족한다.

    - init 자동상속

        ```swift
        class Person {
            var name: String
            
            init(name: String) {
                self.name = name
            }

            convenience init() {
                self.init(name: "Unknown")
            }
        }

        class Student: Person {
            var major: String = "Swift" // 프로퍼티 기본값을 제공하고, 별도 지정init을 구현하지 않았다. *자동상속의 전제, 규칙1 및 규칙2를 만족
        } 

        // 부모의 지정init 자동상속
        let yagom: Person = Person(name: "yagom")
        let hana: Student = Student(name: "hana")
        print(yagom.name) // yagom
        print(hana.name) // hana

        // 부모의 편의init 자동상속
        let kevin: Person = Person()
        let sam: Student = Student()
        print(kevin.name) // Unknown
        print(sam.name) // Unknown
        ```

    - 편의init 자동 상속-1

        ```swift
        class Person {
            var name: String
            
            init(name: String) {
                self.name = name
            }

            convenience init() {
                self.init(name: "Unknown-name")
            }
        }

        class Student: Person {
            var major: String // 프로퍼티 기본값이 없다. 그러나 init을 통해 초기화한다. - *자동상속 전제 만족

            override init(name: String) { // 부모의 지정init를 모두 재정의하여 사용한다. - *자동상속 규칙2 만족
                self.major = "Unknown-major"
                super.init(name: name)
            }
            
            init(name: String, major: String) { // init 추가
                self.major = major
                super.init(name: name)
            }
        }

        // 부모의 편의init 자동상속
        let kevin: Person = Person()
        let sam: Student = Student()
        print(kevin.name) // Unknown-name
        print(sam.name) // Unknown-name
        ```

    - 편의init 자동 상속-2

        ```swift
        class Person {
            var name: String
            
            init(name: String) {
                self.name = name
            }

            convenience init() {
                self.init(name: "Unknown-name")
            }
        }

        class Student: Person {
            var major: String // 프로퍼티 기본값이 없다. 그러나 init을 통해 초기화한다. - *자동상속 전제 만족
            
            override convenience init(name: String) { // ** <부모의 지정init를 자식의 편의init으로 재정의하여 사용한다.> - 자동상속 규칙2 만족
                self.init(name: name, major: "Unknown-major")
            }
            
            init(name: String, major: String) { // init 추가
                self.major = major
                super.init(name: name)
            }
        }

        let kevin: Person = Person()
        let sam: Student = Student(name: "sam") // 부모의 지정init을 재정의한 자식의 convenience init로 초기화
        print(kevin.name) // Unknown-name
        print(sam.name) // sam
        print(sam.major) // Unknown-major

        // 부모의 편의init 자동상속
        let james: Student = Student()
        print(james.name) // Unknown-name
        print(james.major) // Unknown-major
        ```

    - 편의init 자동 상속-3
        - '부모클래스의 init을 모두 상속받는다'는 뜻은 조상클래스의 init을 사용 가능하다는 의미이다. (부모가 상속받은 조상의 init)

            ```swift
            class Person { // 조상
                var name: String
                
                init(name: String) {
                    self.name = name
                }

                convenience init() {
                    self.init(name: "Unknown-name")
                }
            }

            class Student: Person { // 부모
                var major: String // 프로퍼티 기본값이 없다. 그러나 init을 통해 초기화한다. - *자동상속 전제 만족
                
                override convenience init(name: String) { // <부모의 지정init를 자식의 편의init으로 재정의하여 사용한다.> - *자동상속 규칙2 만족
                    self.init(name: name, major: "Unknown-major")
                }
                
                init(name: String, major: String) { 
                    self.major = major
                    super.init(name: name)
                }
            }

            class UniversityStudent: Student { // 자식
                var grade: String = "A"
                var description: String {
                    return "\(self.name), \(self.major), \(self.grade)"
                }
                
                convenience init(name: String, major: String, grade: String) {
                    self.init(name: name, major: major) // *self.init = 상속받은 부모의 지정init
            				// 자식클래스에 써있지 않지만, 자동상속 전체 및 규칙1을 만족 (프로퍼티 기본값이 있고, 별도의 지정init을 구현하지 않음)하므로 -> 부모의 모든 init이 자동상속된 상태이다. (부모의 지정init 및 편의init를 모두 사용 가능하다.)
                    self.grade = grade
                }
            }

            let nova: UniversityStudent = UniversityStudent() // *조상의 편의init을 사용 (부모가 상속받은 편의init이므로 가능)
            print(nova.description) // Unknown-name, Unknown-major, A

            let raon: UniversityStudent = UniversityStudent(name: "raon") // 부모의 편의init을 사용
            print(raon.description) // raon, Unknown-major, A

            let joker: UniversityStudent = UniversityStudent(name: "joker", major: "CS") // 부모의 지정init을 사용
            print(joker.description) // joker, CS, A

            var chope: UniversityStudent = UniversityStudent(name: "chope", major: "PC", grade: "C") // 자식 본인의 편의init을 사용
            print(chope.description) // chope, PC, C

            chope = UniversityStudent() // 다른 init으로 재할당 가능하다.
            print(chope.description) // Unknown-name, Unknown-major, A
            ```

- 요구init
    - 자식클래스에서 반드시 구현해야 하는 이니셜라이저는 `required` 키워드를 사용한다. (L/G - 필수 이니셜라이저 (Required Initializers))
    이때, 재정의하더라도 override를 쓰지 않고 `required` 만 사용한다. (이유는 아래 참고)

        ```swift
        class Person {
            var name: String
            
            required init() {  // 요구이니셜라이저 정의
                self.name = "Unkown-name"
            }
        }

        class Student: Person {
            var major: String = "Unknown-major"
        }
        // 자식클래스에 상속받은 요구 이니셜라이저를 구현하지 않아도 에러가 발생하지 않았다.
        // 자동상속 가정 및 규칙1을 만족하므로 부모의 이니셜라이저가 자동상속 됐기 떄문이다.

        let kevin: Student = Student()
        print("\(kevin.name), \(kevin.major)") // Unkown-name, Unknown-major
        ```

        ```swift
        class Person {
            var name: String
            
            required init() { // 요구이니셜라이저 정의
                self.name = "Unkown-name"
            }
        }

        class Student: Person {
            var major: String = "Unknown-major1"
            
            init(major: String) { // 자식클래스 자신의 지정init을 구현 (=> 자동상속 조건 불만족)
                self.major = major
                super.init()
            }
            
            required init() { // 상속받은 부모의 요구 이니셜라이저를 구현한다. 이 경우에는 없으면 오류 발생 - 'required' initializer 'init()' must be provided by subclass of 'Student'
        //      self.major = "Unknown-major2" // *기능 추가 -> 안해도 가능. major 프로퍼티를 정의할 때 기본값을 지정했으므로. (그럼 재정의가 아니라 단순 구현인가????)
                super.init()
            }

        class University: Student {
            var grade: String
            
            init(grade: String) { // 자식클래스 자신의 지정init을 구현 (=> 자동상속 조건 불만족)
                self.grade = grade
                super.init()
            }
            
            required init() { // 이 경우에는 없으면 오류 발생
                self.grade = "A" // 생략 불가. grade 프로퍼티를 초기화하지 않으면 오류 발생 - Property 'self.grade' not initialized at super.init call
                super.init() 
            }
        }

        var kevin: Student = Student()
        print("\(kevin.name), \(kevin.major)") // Unkown-name, Unknown-major1 (*required init에 major 할당을 추가하면 -> Unknown-major2)

        kevin = Student(major: "Swift")
        print("\(kevin.name), \(kevin.major)") // Unkown-name, Swift

        var uni: University = University()
        print("\(uni.name), \(uni.major), \(uni.grade)") // 부모의 init 사용 - Unkown-name, Unknown-major1, A

        uni = University(grade: "A++")
        print("\(uni.name), \(uni.major), \(uni.grade)") // 자식의 init 사용 - Unkown-name, Unknown-major1, A++
        ```

    - 부모의 일반init (required가 아닌)을 자식클래스부터 요구init으로 변경할 경우, `required override`로 명시한다. (재정의함과 동시에 요구init가 될 것임을 표현)
    이때, 부모의 편의init도 요구init으로 변경 가능하며, `required convenience`로 명시한다.

        자식클래스에서 편의init을 요구init으로 정의할 때에도 `required convenience`로 명시한다.

        ```swift
        class Person {
            var name: String
            
            init() {
                self.name = "Unkown-name"
            }
        }

        class Student: Person {
            var major: String = "Unknown-major1"
            
            init(major: String) { // 자식클래스 자신의 지정init을 구현 (=> 자동상속 조건 불만족)
                self.major = major
                super.init()
            }
            
            required override init() { // 부모의 일반 init을 요구init으로 변경함과 동시에 재정의 (!= 재정의만 할때는 required)
                self.major = "Unknown-major2"
                super.init()
            }
            
            required convenience init(name: String) { // 편의init을 요구init으로 정의 (상속과 상관 없음)
                self.init()
                self.name = name
            }
        }

        class University: Student {
            var grade: String
            
            init(grade: String) { // 자식클래스 자신의 지정init을 구현 (=> 자동상속 조건 불만족)
                self.grade = grade
                super.init()
            }
            
            required init() { // Student Class에서 요구했으므로 구현해야 한다.
                self.grade = "A" // 내용을 추가하여 재정의했지만 required override를 쓰지 않는다.
                super.init()
            }
            
            required convenience init(name: String) { // Student Class에서 요구했으므로 구현해야 한다.
                self.init()
                self.name = name // 부모의 init과 동일한 내용
            }
        }

        var uni: University = University()
        print("\(uni.name), \(uni.major), \(uni.grade)") // Unkown-name, Unknown-major1, A

        uni = University(grade: "A++")
        print("\(uni.name), \(uni.major), \(uni.grade)") // Unkown-name, Unknown-major2, A++

        uni = University(name: "uniuni")
        print("\(uni.name), \(uni.major), \(uni.grade)") // uniuni, Unknown-major2, A
        ```

# 15. Initializer / de-Initializer (인스턴스의 생성과 소멸)

- L/G - initializer
    - Initializers 상속 2가지 방법
        - Designated initializers (지정 이니셜라이저) : the primary initializers for a class. A designated initializer fully initializes all properties introduced by that class and calls an appropriate superclass initializer to continue the initialization process up the superclass chain.
        Every class must have at least one designated initializer. (모든 프로퍼티에 대해 기본값을 지정하지 않은 경우)
        - Convenience Initializers (편리 이니셜라이저) : convenience init. You can define a convenience initializer to call a designated initializer from the same class as the convenience initializer with some of the designated initializer’s parameters set to default values.
    - superclass에서 subclass로 이니셜라이저를 default로 상속하지 않습니다. 이유는 superclass의 이니셜라이저가 무분별하게 상속되면 복잡해져서 subclass에서 이것들이 잘못 초기화 되는 것을 막기 위함입니다. (override는 가능함)
    하지만 특정 상황에서 '자동상속'을 받습니다. 사실 많은 상황에서 직접 초기자를 오버라이드 할 필요가 없습니다. subclass에서 새로 추가한 모든 프로퍼티에 기본 값을 제공하면 다음 두가지 규칙이 적용됩니다. 
    - 규칙1. subclass가 지정초기자를 정의하지 않으면 자동으로 수퍼클래스의 모든 지정초기자를 상속합니다. 
    - 규칙2. subclass가 superclass의 지정초기자를 모두 구현한 경우 (규칙 1의 경우 혹은 모든 지정초기자를 구현한 경우) 자동으로 superclass의 편리한 초기자를 추가합니다.
        - [ ]  // var jam = Hoverboard()  // 오류 발생 왜지? superclass init 형태 암시적으로 상속해서 가능한거 아니었나?

        ```swift
        class Vehicle {
            var numberOfWheels = 0   // Every class must have at least one designated initializer. ? 여기는 init이 없는데?? (numberOfWheels = 0을 하나의 designated initializer으로 보는건가??)
            var description: String {
                return "\(numberOfWheels) wheel(s)"
            }
        }

        let vehicle = Vehicle()
        print("Vehicle: \(vehicle.description)") // Vehicle: 0 wheel(s)

        class Bicycle: Vehicle {  // subclass에서 superclass의 initializer 전체에 대해 override 하므로 override 키워드를 사용함 (superclass에서 init을 정의하지 않았더라도)
            override init() {
                super.init()  // subclass의 전체 프로퍼티에 대한 초기값
                numberOfWheels = 2
            }
        }

        let bicycle = Bicycle()  // override init의 parameter가 없으므로 가능
        print("Bicycle: \(bicycle.description)") // Bicycle: 2 wheel(s)

        -
        class Hoverboard: Vehicle {
            var color: String
            init(color: String) {
                self.color = color  // super.init() implicitly called here
            }
            override var description: String {
                return "\(super.description) in a beautiful \(color)"
            }
        }

        // var jam = Hoverboard()  // 오류 발생 ???
        var jamjam = Hoverboard(color: "red")
        ```

        - [x]  super.init()  // subclass의 전체 프로퍼티에 대한 초기값을 말하는 건가?
            - The Bicycle subclass defines a custom designated initializer, init(). This designated initializer matches (일치함???) a designated initializer from the superclass of Bicycle, and so the Bicycle version of this initializer is marked with the override modifier.
            - The init() initializer for Bicycle starts by calling super.init(), which calls the default initializer for superclass, Vehicle. This ensures that the numberOfWheels inherited property is initialized by Vehicle before Bicycle has the opportunity to modify the property. 
            After calling super.init(), the original value of numberOfWheels is replaced with a new value of 2.
    - example - init 상속
        - [x]  init(name: String, quantity: Int) {  // init 정의 (override init이 아니라?)
            - superclass init의 parameter (name)과 subclass init의 parameter (name, quantity)가 달라서 가능. 즉, 덮어쓰기가 아님
        - [ ]  let oneMysteryItem = RecipeIngredient()  // 왜지? subclass에는 init 모두 parameter가 있는데...? superclass의 convenience init이 자동으로 상속된 건가?-override 했는데?
        - [ ]  convenience init() {   // 왜 convenience init(name: String) 이 아니지?
                self.init(name: "[Unnamed]") // 보통 self.init(name: name)
            }

        ```swift
        class Food {
            var name: String
            init(name: String) {
                self.name = name
            }
            convenience init() {   // 왜 convenience init(name: String) 이 아니지?
                self.init(name: "[Unnamed]") // 보통 self.init(name: name)
            }
        }
        let namedMeat = Food(name: "Bacon")  // namedMeat's name is "Bacon"

        let mysteryMeat = Food()  // mysteryMeat's name is "[Unnamed]"  // 가능 (convenience init의 parameter가 없으므로)

        class RecipeIngredient: Food {
            var quantity: Int
            init(name: String, quantity: Int) {  // init 정의 (override init이 아닌 이유 - parameter가 달라서 다른 초기화 방식임)
                self.quantity = quantity
                super.init(name: name)
            }
            override convenience init(name: String) {  // superclass의 convenience init을 override 했음
                self.init(name: name, quantity: 1)
            }
        }

        -
        // RecipeIngredient 클래스는 다음 3가지 형태의 initializer를 이용해 인스턴스를 생성할 수 있습니다.
        let oneMysteryItem = RecipeIngredient()  // 왜지? subclass에는 init 모두 parameter가 있는데...? superclass의 convenience init이 자동으로 상속된 건가?-override 했는데?
        let oneBacon = RecipeIngredient(name: "Bacon")
        let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)
        ```

        - 수퍼클래스의 init(name: name) initializer를 상속받아 지정초기자를 생성하고 그 지정초기자를 convenience init(name: String)에서 오버라이딩해 사용합니다. 
        RecipeIngredient에서 initializer가 사용되는 구조를 표현하면 다음 그림과 같습니다.

            ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2014.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2014.png)

- 기본 이니셜라이저 (Default Initializers)
    - 모든 인스턴스는 초기화와 동시에 모든 저장 프로퍼티에 유효한 값이 할당되어 있어야 한다. (Class는 Class 선언 시 기본값이나 초기값을 할당하지 않으면 오류 발생)
    방법-1 저장 프로퍼티 선언 시, 프로퍼티 기본값을 할당하거나
    방법-2 이니셜라이저 (init)에서 초기값을 할당해야 한다.
        - 저장 프로퍼티의 기본값이 모두 지정되어 있고, 사용자 정의 이니셜라이저가 없으면 자동으로 기본 이니셜라이저를 사용한다.
        이때, Struct는 멤버와이즈 이니셜라이저를 제공하고, Class는 제공하지 않는다. (멤버와이즈 이니셜라이저 : 인스턴스 생성 시, () 내부에다가 저장 프로퍼티명을 parameter로 자동 지정한다.)
    - 프로퍼티에 미리 기본값을 할당해두면 (ex. 클래스를 선언할 때) 인스턴스가 생성됨과 동시에 초기값을 갖게 된다.
    - 이니셜라이저 메서드도 다양한 매개변수를 가질 수 있다. (argument label, 와일드카드 식별자 등)

        ```swift
        class Person {   // 클래스 선언 시 모든 저장 프로퍼티에 기본값 (Default) 할당 
            var name: String = "unknown" // 방법-1 저장 프로퍼티 선언 시, 프로퍼티 기본값을 할당하거나
            var age: Int = 0
            var nickName: String = "nick"
        }

        let jason: Person = Person()  // 인스턴스 생성 - 초기값이 할당된 상태임 (초기화 단계)
        jason.name = "jason"  // 초기화 이후 프로퍼티 값을 수정
        jason.age = 30
        jason.nickName = "j"

        class Apartment {
            var buildingNumber: String // 원래 변수 선언 시 기본값을 할당 (var buildingNumber: String = dong2) 해야하지만, 대신 init을 통해 초기값을 할당 가능하다.
            var roomNumber: String
            var `guard`: Person?
            var owner: Person?
            
            init(dong2 : String, ho2 : String) {  // *dong, ho : argument label임 // 방법-2 이니셜라이저 (init)에서 초기값을 할당해야 한다.
                buildingNumber = dong2  // 인스턴스 초기화 단계에서의 할당값 (dong, ho의 argument)이 buildingNumber 프로퍼티로 들어간다!!!
                roomNumber = ho2  // self.roomNumber도 가능 (더 명확하게 표현하기 위해)
            }
        }

        let apart: Apartment? = Apartment(dong2: "101", ho2: "202")
        ```

        - [ ]  이렇게 입력하면 아래의 print(mike.name)에서 random이 출력됨... 왜 수정 안되지?

            ```swift
            class PersonC {
                var name: String
                
                init(name: String){
                    self.name = "random" // 이렇게 입력하면 아래의 print(mike.name)에서 random이 출력됨... 왜 수정 안되지?
                }
            }

            let mike = PersonC(name: "mike")
            print(mike.name)  // random 출력
            mike.name = "mike"
            print(mike.name)  // mike 출력
            ```

- 초기화 위임 (Initialization Delegation)
    - 값 type인 Struct, Enum은 코드 중복을 줄이기 위해 초기화 위임을 구현 가능하다. (*Class는 상속이 있으므로 위임 불가)
    - 최소 2개 이상의 '사용자 정의 이니셜라이저'가 있을 때, 이니셜라이저가 `self.init` 키워드로 다른 이니셜라이저에게 일부 초기화 내용을 위임하는 것이다.
    - 단, 사용자 정의 이니셜라이저가 있는 동시에 기본 이니셜라이저 또는 멤버와이즈 이니셜라이즈를 사용하고 싶다면, Extension을 사용하여 사용자 정의 이니셜라이저를 구현하면 된다.

        ```swift
        enum Student {
            case elementary, middle, high
            case none
            
            init(koreanAge: Int) { // 사용자 정의 이니셜라이저-1
                switch koreanAge {
                case 8...13:
                    self = .elementary // self : enum 인스턴스 자기 자신
                case 14...16:
                    self = .middle
                case 17...19:
                    self = .high
                default:
                    self = .none
                }
            }
            
            init(bornAt: Int, currentYear: Int) { // 사용자 정의 이니셜라이저-2
                self.init(koreanAge: currentYear - bornAt + 1) // self.init : 사용자 정의 이니셜라이저-1 을 가르킴
            }
            
            init() { // 사용자 정의 이니셜라이저가 있는 경우, init() 메서드를 구현해야 기본 이니셜라이저를 사용 가능하다.
                self = .none 
            }
        }

        var kevin: Student = Student(koreanAge: 16)
        print(kevin) // middle

        kevin = Student(bornAt: 2011, currentYear: 2021)
        print(kevin) // elementary

        var yagom: Student = Student() // 기본 이니셜라이저
        print(yagom) // none
        ```

cf. Class의 이니셜라이저는 <Notion 14. 상속> 파트 참고

- 실패가능한 이니셜라이저 (Failable Initializers) init?
    - 실패가능한 이니셜라이저의 return type은 옵셔널 타입이다.
    - 이니셜라이저 매개변수로 전달되는 초기값이 잘못된 경우, 인스턴스 생성에 실패할 수 있다. 인스턴스 생성에 실패하면 nil을 반환한다.

        ```swift
        class PersonD {
            var name: String
            var age: Int
            var nickName: String?
            
            init?(name: String, age: Int) {   // 인스턴트 초기화 단계에서 전달이 실패 가능하므로 init?을 사용함
                if (0...120).contains(age) == false {   // age가 0 이상 120 이하가 아니라면 return nil 해라
                    return nil
                }
                
                self.name = name
                self.age = age
            }
        }

        //let john: PersonD = PersonD(name: "john", age: 23)  // 오류 발생 - Value of optional type 'PersonD?' must be unwrapped to a value of type 'PersonD'

        let john: PersonD? = PersonD(name: "john", age: 23)   // 인스턴스 초기화 단계 - 반환이 실패 가능하므로 옵셔널 타입 personD?를 사용함
        let joker: PersonD? = PersonD(name: "joker", age: 123)

        print(john)  // Optional(__lldb_expr_52.PersonD) 
        print(john?.name) // Optional("john")
        print(john?.age)  // Optional(23)

        print(jocker)  // nil 출력
        print(joker?.name) // nil
        print(joker?.age)  // nil
        ```

- Enum에서 사용하는 실패가능한 이니셜라이저 (Failable Initializers for Enumerations)
    - 1) 특정 case에 해당하지 않는 값이 전달되거나, 2) rawValue로 초기화할 때 잘못된 rawValue 값이 전달된 경우, 이니셜라이저 생성에 실패할 수 있다.
    - rawValue를 통한 이니셜라이저는 기본적으로 실패가능한 이니셜라이저로 제공된다.

        ```swift
        enum Student: String {
            case elementary = "초등학생", middle = "중학생", high = "고등학생" // raw Value 지정 (지정 삭제하더라도 switch문 return nil 있으므로 init? 필수, 인스턴스는 optional type이어야 함)
            case none
            
            init?(koreanAge: Int) { 
                switch koreanAge {
                case 8...13:
                    self = .elementary
                case 14...16:
                    self = .middle
                case 17...19:
                    self = .high
                default:
                    return nil // cf. 앞의 예제에서는 self = .none 이었음 (nil 가능성 없으므로 init)
                }
            }
            
            init?(bornAt: Int, currentYear: Int) { 
                self.init(koreanAge: currentYear - bornAt + 1)
            }
        }

        var younger: Student? = Student(koreanAge: 20) // (switch문 return nil 대신 .none이면, init 가능하지만) rawValue를 지정했으므로 인스턴스는 optional type이어야 함
        print(younger) // nil

        younger = Student(bornAt: 2020, currentYear: 2016)
        print(younger) // nil

        younger = Student(rawValue: "만학도")
        print(younger) // nil

        younger = Student(rawValue: "고등학생")
        print(younger) // Optional(__lldb_expr_63.Student.high)
        ```

        ```swift
        enum TemperatureUnit {
            case kelvin, celsius, fahrenheit

            init?(symbol: Character) { 
                switch symbol {
                case "K":
                    self = .kelvin   
                case "C":
                    self = .celsius
                case "F":
                    self = .fahrenheit
                default:
                    return nil
                }
            }
        }

        -
        let fahrenheitUnit = TemperatureUnit(symbol: "F")
        if fahrenheitUnit != nil {
            print("This is a defined temperature unit, so initialization succeeded.")
        }
        // Prints "This is a defined temperature unit, so initialization succeeded."

        let unknownUnit = TemperatureUnit(symbol: "X")
        if unknownUnit == nil {
            print("This is not a defined temperature unit, so initialization failed.")
        }
        // Prints "This is not a defined temperature unit, so initialization failed."
        ```

    - Raw Values를 활용하면 더 간단히 나타낼 수 있다. (init 없이 가능)

        ```swift
        enum TemperatureUnit: Character {
            case kelvin = "K", celsius = "C", fahrenheit = "F"
        }

        let fahrenheitUnit = TemperatureUnit(rawValue: "F")
        if fahrenheitUnit != nil {
            print("This is a defined temperature unit, so initialization succeeded.")
        }
        // Prints "This is a defined temperature unit, so initialization succeeded."

        let unknownUnit = TemperatureUnit(rawValue: "X")
        if unknownUnit == nil {
            print("This is not a defined temperature unit, so initialization failed.")
        }
        // Prints "This is not a defined temperature unit, so initialization failed."
        ```

- 클로저/함수를 사용한 프로퍼티 기본값 설정
    - 사용자 정의 연산을 통해 저장 프로퍼티의 기본값을 설정할 경우, 클로저나 함수를 사용 가능하다.
    - 인스턴스를 생성하여 초기화할 때, 클로저/함수가 호출되면서 연산 결과값을 프로퍼티 기본값으로 제공한다. (따라서 클로저/함수의 return type은 프로퍼티 type과 동일해야 한다.)
    - 단, 클로저 내부에서 인스턴스의 다른 프로퍼티를 사용하여 연산하는 것은 불가하다. (클로저 실행시점은 초기화 단계에서 다른 프로퍼티 값이 설정되기 이전이다. 다른 프로퍼티에 기본값이 있어도 불가하다.)
    또한 클로저 내부에서 self 프로퍼티 사용, 인스턴스 메서드 호출은 불가하다.

        ```swift
        // Syntax
        class SomeClass {

        		var someProperty: someType = { // 기본값을 설정할 프로퍼티 
        				// 사용자 정의 연산을 거치고, 결과값 someValue를 프로퍼티에 return 함
        				return someValue 
        		}() // 클로저 실행
        }
        ```

        ```swift
        class GreyClass {

            var christinaInterns: [Int] = {  // 기본값을 설정할 프로퍼티 

                var internsNameArray: [Int] = []
                
                for number in 1...5 {
                    internsNameArray.append(number)
                }

        				print(internsNameArray[0]) // 확인용 - 1 출력
                return internsNameArray // return type은 프로퍼티 type과 동일
            }() // 클로저 실행
        }

        var interns2021: GreyClass = GreyClass() // Class의 인스턴스가 초기화할 때, 클로저가 호출되고 프로퍼티 기본값이 제공된다.
        print(interns2021.christinaInterns) // [1, 2, 3, 4, 5]
        print(interns2021.christinaInterns.count) // 5
        ```

        ```swift
        struct StudentInfo {
            var name: String?
            var number: Int?
        }

        class SchoolClass {
            
            var students: [StudentInfo] = { // 기본값을 설정할 프로퍼티 - Struct StudentInfo의 인스턴스를 요소로 갖는 Array type

                var arr: [StudentInfo] = [StudentInfo]()
                
                for num in 1...15 {
                    var person: StudentInfo = StudentInfo(name: nil, number: num)
                    arr.append(person)
                }
                
        //      arr 프로퍼티 관련 확인용
        //   // print(arr.[0]) // Array index0의 요소 - StudentInfo(name: nil, number: Optional(1))
        //      arr.insert(StudentInfo(name: "insert", number: 100), at: 0)
        //      print(arr) // 이런 식으로 들어있다. [__lldb_expr_90.StudentInfo(name: Optional("insert"), number: Optional(100)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(1)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(2)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(3)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(4)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(5)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(6)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(7)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(8)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(9)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(10)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(11)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(12)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(13)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(14)), __lldb_expr_90.StudentInfo(name: nil, number: Optional(15))]
                
                return arr // return type은 students 프로퍼티와 동일한 [StudentInfo] Array type
            }() 
        }

        let myClass: SchoolClass = SchoolClass() // Class의 인스턴스가 초기화할 때, 클로저가 호출되고 프로퍼티 기본값이 제공된다.
        print(myClass.students.count) // 15 출력 
        ```

- 디이니셜라이저 deinit
    - 인스턴스가 해제되는 시점에 실행할 부가작업을 구현한다. (deinit은 Class당 1개만 구현 가능하고, 인스턴스의 모든 프로퍼티에 접근 가능하다.)
    - 부가작업이 필요한 경우 : 인스턴스 내부에서 파일을 open/modify하는 등 외부 자원을 사용한 경우 (파일을 wrtie/close 하는 등 파일을 닫아야 메모리에서 해당 파일이 해제되므로) 
       또는 인스턴스에 저장된 데이터를 디스크에 파일로 저장하는 경우
    - 클래스에만 구현 가능하다. (클래스 인스턴트는 참조 타입이므로 더 이상 참조가 필요 없을 때 메모리에서 해제된다. - 인스턴스의 소멸)
    - 인스턴스의 소멸 직전에 호출된다. (자동으로 호출되므로 직접 호출할 수 없다.)
    - 인스턴스가 메모리에서 해제되는 시점은 ARC(Automatic Reference Counting) 의 규칙에 따라 결정된다.

        ```swift
        class someClass {
        		deinit {
        				print("Instance will be deallocated immediately!")
        		}
        }

        var someInstance: someClass? = someClass() // nil 할당을 위해 optional type으로 선언함
        someInstance = nil // 메모리 해제 - 인스턴스 소멸 직전에 Instance will be deallocated immediately! 출력
        ```

        ```swift
        class PersonE {
            var name: String
            var pet: Puppy?
            var child: PersonC
            
            init(name: String, child: PersonC) {
                self.name = name
                self.child = child
            }
            
            deinit {   // 인스턴스가 메모리에서 해제되는 시점에 자동 호출 (매개변수 없음)  // optional을 unwrapping 해주는 Optional binding (if let 구문)
                if let petName = pet?.name { // 만약 pet이 있다면 실행. *pet? : value of optional type 'Puppy?' must be unwrapped to refer to member 'name' of wrapped base type 'Puppy'
                    print("\(name)가 \(child.name)에게 \(petName)를 인도합니다")
                    self.pet?.owner = child
                }
            }
        }

        var donald: PersonE? = PersonE(name: "donald", child: kevin)
        donald?.pet = happy  // *donald? : value of optional type 'PersonE?' must be unwrapped to refer to member 'pet' of wrapped base type 'PersonE'

        donald = nil   // 1. [사용자 입력] donald 인스턴스가 더 이상 필요없으므로 메모리에서 해제시킴 -> 2. deinit 자동 호출
        donald가 kevin에게 happy를 인도합니다 // 3. 출력됨
        ```

# 16. 옵셔널 체이닝과 nil 병합

- 옵셔널 체이닝
    - 옵셔널의 내부의 내부의 내부로 옵셔널이 연결되어 있을 때 유용하게 활용할 수 있습니다. 
    (struct나 class 등 덩어리 안에 다른 덩어리의 인스턴스를 생성하는 경우가 반복될 때, 그 data type이 optional 이라서 nil check를 매번 해주기 번거로울 때 활용한다.)
    - 매번 nil 확인을 하지 않고 최종적으로 원하는 값이 있는지 없는지 확인할 수 있습니다.

        ```swift
        class Person {
            var name: String
            var job: String?  // optional type의 default값은 nil 이다.
            var home: Apartment?  // optional type의 default값은 nil 이다.
            
            init(name: String) {
                self.name = name
            }
        }

        class Apartment {
            var buildingNumber: String
            var roomNumber: String
            var `guard`: Person?
            var owner: Person?
            
            init(dong: String, ho: String) {  // dong, ho : *argument label (변수 선언 X)
                buildingNumber = dong  // 인스턴스 초기화 단계에서의 할당값 (dong, ho의 입력값)이 변수 buildingNumber로 들어간다
                roomNumber = ho
            }
        }
        // *함수 선언시 사용하는 argument label처럼 init(dong buildingNumber: String, ho roomNumber: String) 형태로 하면 오류 발생 -> 왜지?

        // 옵셔널 체이닝 사용
        let yagom: Person? = Person(name: "yagom")
        let apart: Apartment? = Apartment(dong: "101", ho: "202")
        let superman: Person? = Person(name: "superman")
        // (의도) 옵셔널 체이닝이 실행 후 결과값이 nil일 수 있으므로 결과 타입도 옵셔널입니다

        // Quiz - 만약 우리집의 경비원의 직업이 궁금하다면?
        // 방법-1) 옵셔널 체이닝을 사용하지 않는 경우
        func guardJob(owner: Person?) { // 여러 번 optional binding으로 unwrapping 해야 해서 번거로움
            if let owner = owner {  // owner라는 프로퍼티 (optional type)를 unwrapping 해서 값을 다시 owner에 할당하겠다? (nil이면 할당 안하고)
                if let home = owner.home {
                    if let `guard` = home.guard {
                        if let guardJob = `guard`.job {
                            print("우리집 경비원의 직업은 \(guardJob)입니다")
                        } else {
                            print("우리집 경비원은 직업이 없어요")
                        }
                    }
                    else { print("owner, home, guard, job 중에 nil이 있어요") } // nil 일 때 statements도 따로 지정해야 해서 번거로움
                }
                else { print("owner, home, guard, job 중에 nil이 있어요") }
            }
            else { print("owner, home, guard, job 중에 nil이 있어요") }
        }
        guardJob(owner: yagom) // owner, home, guard, job 중에 nil이 있어요 - 출력

        // 방법-2) 옵셔널 체이닝을 사용하는 경우
        func guardJobWithOptionalChaining(owner: Person?) {
            if let guardJob = owner?.home?.guard?.job { // owner가 있나? -> home이 있나? -> guard가 있나? -> (if let) job이 있나? 차례로 nil check
                print("우리집 경비원의 직업은 \(guardJob)입니다")
            } else {
                print("우리집 경비원은 직업이 없어요") // 하나만 nil 이어도 else block이 출력됨
            }
        }
        guardJob(owner: yagom) // 출력 안됨
        guardJobWithOptionalChaining(owner: yagom) // 우리집 경비원은 직업이 없어요 - 출력

        if yagom?.home == nil {
            print("yagom은 home이 없다")  // (확인용) yagom은 home이 없다 - 출력
        } 

        yagom?.home?.guard?.job // nil (yagom은 home이 없는 상태이므로)
        yagom?.home = apart  // *apart 인스턴스를 할당해줌
        yagom?.home // Optional(Apartment)

        yagom?.home?.guard // nil (guard가 없는 상태이므로)
        yagom?.home?.guard = superman // *superman 인스턴스를 할당해줌
        yagom?.home?.guard // Optional(Person)
        yagom?.home?.guard?.name // superman (superman 인스턴스의 초기값이 이미 들어가있음)
        yagom?.home?.guard?.job // nil
        yagom?.home?.guard?.job = "경비원2" // superman 직업을 할당해줌

        guardJob(owner: yagom) // 우리집 경비원의 직업은 경비원2입니다 - 출력
        guardJobWithOptionalChaining(owner: yagom) // 우리집 경비원의 직업은 경비원2입니다 - 출력
        ```

- 옵셔널 체이닝을 통한 메서드 호출

    ```swift
    class Room {
        var number: Int
        
        init(number: Int) {
            self.number = number
        }
    }

    class Building {
        var name: String
        var room: Room?
        
        init(name: String) {
            self.name = name
        }
    }

    struct Address {
        var province: String
        var city: String
        var street: String
        
        var building: Building?
        var detailAddress: String?
    } // 이 상태로 struct 인스턴스를 생성하면, optional type을 포함하여 모든 값을 입력하도록 자동 init이 된다. 이 중 optional은 입력하지 않아도 에러가 발생하지 않는다.

    // 옵셔널 체이닝을 통한 메서드 호출
    struct Address2 {
        var province: String
        var city: String
        var street: String
        
        var building: Building?
        var detailAddress: String?
        
        init(province: String, city: String, street: String) {
            self.province = province
            self.city = city
            self.street = street
        }
        
        func fullAddress() -> String? {
            var restAddress: String? = nil
            
            if let buildingInfo: Building = self.building { // self.building가 nil이 아니면, 상수 buildingInfo에 담는다.
                restAddress = buildingInfo.name // buildingInfo의 name 프로퍼티 값을 변수 restAddress에 할당한다. (근데 메소드 내부 로컬변수라서 fullAddress 함수가 return하면 없어질텐데? -> 아래 코드 확인. fullAddress 내부에 담겨서 return 되어 밖으로 나간다!)
            } else if let detail: String = self.detailAddress { // self.building가 nil이고, self.detailAddress가 nil이 아니면~
                restAddress = detail
            }
            
            if let rest: String = restAddress { // restAddress가 nil이 아니면~
                var fullAddress: String = self.province
                
                fullAddress += " " + self.city
                fullAddress += " " + self.street
                fullAddress += " " + rest
                
    //          print(fullAddress) // 참고 - printAddress() 함수를 대체 가능하다!
                return fullAddress
            } else {
                return nil
            }
    		}

    		func printAddress() {
            if let address: String = self.fullAddress() {
                print(address)
            }
        }
    }

    class Person {
        var name: String
        var address: Address2?
        
        init(name: String) { // class는 name 선언 시 기본값을 주거나, init으로 초기화가 필요하다. (struct은 프로퍼티로 자동 init이 됨)
            self.name = name
        }
    }

    let yagom: Person = Person(name: "yagom")

    if let roomNumber: Int = yagom.address?.building?.room?.number { // 옵셔널 체이닝
        print(roomNumber)
    } else {
        print("Cannot find room number")
    } // Cannot find room number 출력
    yagom.address?.fullAddress() // nil
    yagom.address?.fullAddress()?.isEmpty // nil

    yagom.address = Address2(province: "전북", city: "군산시", street: "수송로")
    yagom.address?.building = Building(name: "스타팰리스")
    yagom.address?.building?.room = Room(number: 505)
    yagom.address?.building?.room?.number = 1306 // 옵셔널 체이닝을 통해 할당 가능

    // yagom.address?.fullAddress() // 전북 군산시 수송로 스타팰리스 - 출력 (대체 가능!)
    yagom.address?.fullAddress()?.isEmpty // 전북 군산시 수송로 스타팰리스 - 출력, false 반환

    yagom.address?.printAddress() // 전북 군산시 수송로 스타팰리스, 전북 군산시 수송로 스타팰리스 - 왜 두번 출력? -> print(address)가 없어도 1번 출력된다.
    ```

    ```swift
    // guard 구문 활용
    func fullAddress() -> String? {
            var restAddress: String? = nil
            
            if let buildingInfo: Building = self.building { 
                restAddress = buildingInfo.name 
            } else if let detail: String = self.detailAddress { 
                restAddress = detail
            }
            
    //        if let rest: String = restAddress {
    //            var fullAddress: String = self.province
    //
    //            fullAddress += " " + self.city
    //            fullAddress += " " + self.street
    //            fullAddress += " " + rest
    //
    //            return fullAddress
    //        } else {
    //            return nil
    //        }
            
            guard let rest: String = restAddress else {  // 위 코드를 대체 가능하다.
                return nil  // guard-else 예외처리
            }
            var fullAddress: String = self.province

            fullAddress += " " + self.city
            fullAddress += " " + self.street
            fullAddress += " " + rest

            return fullAddress
        }
    }
    ```

- 옵셔널 체이닝을 통한 서브스크립트 호출

    ```swift
    let optionalArray: [Int]? = [1,2,3]
    optionalArray?[1] // 2

    var optionalDictionary: [String:[Int]]? = [String:[Int]]()
    optionalDictionary?["numberArray"] = optionalArray
    optionalDictionary?["numberArray"]?[2] // 3
    ```

- nil 병합 연산자
    - 중위 연산자입니다. **`??` (**Optional **??** Value)
    - 옵셔널 값이 nil 인 경우, 우측의 값을 반환합니다.

        ```swift
        var guardJob: String
            
        guardJob = yagom?.home?.guard?.job ?? "슈퍼맨" // "yagom?.home?.guard?.job" 이 중에서 nil이 하나라도 있으면 변수 guardJob에 "슈퍼맨"을 할당해라.
        print(guardJob) // 경비원2 (nil이 아니므로 본래 값 출력)

        yagom?.home?.guard?.job = nil // job에 nil을 할당
        guardJob = yagom?.home?.guard?.job ?? "슈퍼맨"
        print(guardJob) // 슈퍼맨 (nil이므로 ??가 작동함)
        ```

# 17. 타입 캐스팅 (타입 확인-is, 업캐스팅-as, 다운캐스팅-as?, as!)

- L/G
    - Type casting is a way to check the type of an instance, or to treat that instance as a different superclass or subclass from somewhere else in its own class hierarchy.
    - simple way to check the type of a value or cast a value to a different type. (지정해준 다른 타입으로 취급)
    to cast that instance to another class within the same hierarchy.
    - 1) Defining a Class Hierarchy for Type Casting
        - library가 갖고 있는 Movie,Song 인스턴스의 공통 부모는 MediaItem이기 때문에 library는 타입 추론에 의해 [MediaItem] 배열의 형을 갖게 됩니다. 
        library를 순회(iterate)하면 배열의 아이템은 Movie, Song 타입이 아니라 MediaItem 타입이라는 것을 확인할 수 있습니다.
        - 타입 지정을 위해서는 downcasting을 이용해야 합니다.
            - [x]  개개별의 type (Movie or Song)이 확인이 되는데 왜 downcasting이 필요하지 ???

            🎃🎃🎃 The items stored in library are still Movie and Song instances behind the scenes. However, if you iterate over the contents of this array, the items you receive back are typed as MediaItem, and not as Movie or Song. In order to work with them as their native type, you need to check their type, or downcast them to a different type.

            ```swift
            class MediaItem {
                var name: String
                init(name: String) {
                    self.name = name
                }
            }
            class Movie: MediaItem {
                var director: String
                init(name: String, director: String) {
                    self.director = director
                    super.init(name: name)
                }
            }

            class Song: MediaItem {
                var artist: String
                init(name: String, artist: String) {
                    self.artist = artist
                    super.init(name: name)
                }
            }

            let library = [
                Movie(name: "Casablanca", director: "Michael Curtiz"),
                Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
                Movie(name: "Citizen Kane", director: "Orson Welles"),
                Song(name: "The One And Only", artist: "Chesney Hawkes"),
                Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
            ] // the type of "library" is inferred to be [MediaItem]
            ```

    - 2) Checking Type
        - is 연산자를 이용해 특정 인스턴스의 타입을 확인할 수 있습니다.
        - 아래 코드는 library 배열을 iterate하고 아이템이 특정 타입 (Movie or Song) 일 때마다 그 숫자를 증가하는 예제 코드입니다.

            This example iterates through all items in the library array. On each pass, the for-in loop sets the item constant to the next MediaItem in the array.

            ```swift
            var movieCount = 0
            var songCount = 0

            for item in library {
                if item is Movie {
                    movieCount += 1
                } else if item is Song {
                    songCount += 1
                }
            }
            print("Media library contains \(movieCount) movies and \(songCount) songs")
            // "Media library contains 2 movies and 3 songs" 출력
            ```

    - 3) Downcasting
        - 특정 클래스 타입의 상수나 변수는 특정 서브클래스의 인스턴스를 참조하고 있을 수 있습니다. 
        A constant or variable of a certain class type may actually refer to an instance of a subclass behind the scenes.
        - 어떤 타입의 인스턴스인지 확인할 수 있습니다.
            - as?는 특정 타입이 맞는지 확신할 수 없을때 사용하고, as!는 특정 타입이라는 것이 확실한 경우에 사용합니다.
            - 단 as!으로 다운캐스팅을 했는데 지정한 타입이 아니라면 런타임 에러가 발생합니다.
        - 캐스팅은 실제 인스턴스나 값을 바꾸는 것이 아니라 지정한 타입으로 취급하는 것 뿐입니다.
        - 다음 예제는 library 배열의 mediaItem이 Movie 인스턴스 일수도 있고 Song 인스턴스일 수도 있기 때문에 as?연산자를 사용했습니다.
        Because item is a MediaItem instance, it’s possible that it might be a Movie; equally, it’s also possible that it might be a Song, or even just a base MediaItem. 
        Because of this uncertainty, the as? form of the type cast operator returns an optional value.
            - if let movie = item as? Movie → can be read as:
            1. Try to access item as a Movie. (Movie type 맞는지 확인) 
            2. If this is successful, set a new temporary constant called movie to the value stored in the returned optional Movie.

            ```swift
            for item in library { 
                if let movie = item as? Movie {  // downcasting and unwrapping  // The result of <item as? Movie> is of type Movie? (optional Movie)
                    print("Movie: \(movie.name), dir. \(movie.director)")
                } else if let song = item as? Song {
                    print("Song: \(song.name), by \(song.artist)")
                }
            }
            // Movie: Casablanca, dir. Michael Curtiz
            // Song: Blue Suede Shoes, by Elvis Presley
            // Movie: Citizen Kane, dir. Orson Welles
            // Song: The One And Only, by Chesney Hawkes
            // Song: Never Gonna Give You Up, by Rick Astley
            ```

    - Type Casting for Any and AnyObject
        - Use Any and AnyObject only when you explicitly need the behavior and capabilities they provide.
        - Any : 함수를 포함한 모든 타입의 인스턴스
        - AnyObject : 클래스 타입의 모든 인스턴스
            - example of using Any to work with a mix of different types.

            ```swift
            var things = [Any]()  // Any 타입 배열을 선언해 여러 타입의 값을 저장합니다. 여기에는 Int, String, 함수, 클로저까지 포함됩니다.

            things.append(0)
            things.append(0.0)
            things.append(42)
            things.append(3.14159)
            things.append("hello")
            things.append((3.0, 5.0))
            things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))  // function
            things.append({ (name: String) -> String in "Hello, \(name)" })  // closure
            ```

            - things를 iterate 하며 타입캐스팅이 되는지 switch case문에 as 연산자로 확인해 타입캐스팅 되는 배열의 원소의 값을 적절히 출력합니다.
            - [ ]  any 타입 등 여러 type이 섞여있는 변수/인스턴스가 있을 때, 각 요소들의 type 별로 다른 실행을 연결시키기 위해 down casting을 하는 것이 맞나?
            as?! 안하고 그냥 for...in if 구문으로 해결하면 되지 않나?

            ```swift
            for thing in things {
                switch thing {
                case 0 as Int:
                    print("zero as an Int")
                case 0 as Double:
                    print("zero as a Double")
                case let someInt as Int:
                    print("an integer value of \(someInt)")
                case let someDouble as Double where someDouble > 0:
                    print("a positive double value of \(someDouble)")
                case is Double:
                    print("some other double value that I don't want to print")
                case let someString as String:
                    print("a string value of \"\(someString)\"")  // a string value of "hello" 출력
                case let (x, y) as (Double, Double):
                    print("an (x, y) point at \(x), \(y)") // an (x, y) point at 3.0, 5.0
                case let movie as Movie:
                    print("a movie called \(movie.name), dir. \(movie.director)") // a movie called Ghostbusters, dir. Ivan Reitman
                case let stringConverter as (String) -> String:
                    print(stringConverter("Michael")) // closure - Hello, Michael
                default:
                    print("something else")
                }
            }
            // Int, Double뿐만 아니라 튜플, 함수도 Any 타입에 포함될 수 있다는 것을 확인할 수 있습니다.
            ```

- 특징
    - 인스턴스의 타입을 확인 하는 용도
    - 클래스의 인스턴스를 superclass/subclass의 타입으로 사용할 수 있는지 확인 하는 용도

        ```swift
        var result: Bool  // true or false로 결과값을 보여주는 용도

        result = yagom is Person // true  // 인스턴스 yagom이 Person type인가? 맞으면 true
        result = yagom is Student // false
        result = yagom is UniversityStudent // false
        ```

    - cf) type conversion (형 변환)은 타입 캐스팅이 아니라 새로운 인스턴스를 생성하는 것이다.
        - [ ]  왜 인스턴스라고 하지???????? 🤯🤯🤯

        ```swift
        let someInt: Int = 2
        let someDouble: Double = Double(someInt)  // Double type의 인스턴스를 생성하는 것. someInt의 값만 활용한다. (타입 캐스팅이 아님)
        ```

- 예제 - Type 확인 (is)

    ```swift
    class Person {
        var name: String = ""
        func breath() {
            print("숨을 쉽니다")
        }
    }
    class Student: Person {  // subclass
        var school: String = ""
        func goToSchool() {
            print("등교를 합니다")
        }
    }
    class UniversityStudent: Student {  // subclass의 subclass
        var major: String = ""
        func goToMT() {
            print("멤버쉽 트레이닝을 갑니다 신남!")
        }
    }
    var yagom: Person = Person()  
    var hana: Student = Student()
    var jason: UniversityStudent = UniversityStudent()
    ```

    ```swift
    var result: Bool  // true or false로 결과값을 보여주는 용도

    result = yagom is Person // true  // 타입 확인 - 인스턴스 yagom이 Person type인가? 맞으면 true
    result = yagom is Student // false
    result = yagom is UniversityStudent // false

    result = hana is Person // true  // Student class는 Person class를 상속받아서 Person의 특성을 모두 가지고 있으므로 true
    result = hana is Student // true
    result = hana is UniversityStudent // false

    result = jason is Person // true
    result = jason is Student // true
    result = jason is UniversityStudent // true

    if yagom is UniversityStudent {
        print("yagom은 대학생입니다")
    } else if yagom is Student {
        print("yagom은 학생입니다")
    } else if yagom is Person {
        print("yagom은 사람입니다")
    } // yagom은 사람입니다

    switch yagom {
    case is UniversityStudent:
        print("yagom is UniversityStudent")
    case is Student:
        print("yagom is Student")
    case is PersonZ:  // 이 case에 해당되므로 print 실행
        print("yagom is Person")  
    default:
        print("error")
    } // yagom is Person - 출력

    switch jason {
    case is UniversityStudent:
        print("jason은 대학생입니다")
    case is Student:
        print("jason은 학생입니다")
    case is Person:
        print("jason은 사람입니다")
    default:
        print("jason은 사람도, 학생도, 대학생도 아닙니다")
    } // jason은 대학생입니다 - 출력
    ```

- 업 캐스팅(Up Casting) - 활용 빈도 낮음
    - as를 사용하여 부모클래스의 인스턴스로 행세 (사용)할 수 있도록 컴파일러에게 타입정보를 전환해줍니다.
    - Any 혹은 AnyObject로 타입정보를 변환할 수 있습니다.
    - 암시적으로 처리되므로 꼭 필요한 경우가 아니라면 생략해도 무방합니다.

        ```swift
        var mike: Person = UniversityStudent() as Person  // UniversityStudent 인스턴스를 생성하여 superclass Person 행세를 할 수 있도록 업 캐스팅

        //var mike2: UniversityStudent = Person() as UniversityStudent // 컴파일 오류

        var jina: Any = Person() as Any // as Any 생략가능  // Any type에 Person class가 들어갈 수 있다.
        ```

- 다운 캐스팅(Down Casting) - 활용 빈도 높음 as?/as!
    - 자식 클래스의 인스턴스로 사용할 수 있도록 컴파일러에게 인스턴스의 타입정보를 전환해줍니다.
        - 1) 조건부 다운 캐스팅 (as?)
            - Casting에 실패하면, 즉 Casting하려는 타입에 부합하지 않는 인스턴스라면 nil을 반환하기 때문에 결과의 타입은 옵셔널 타입입니다.
                - [ ]  optionalCasted = mike as? UniversityStudent  // 2. mike는 실질적으로 UniversityStudent의 인스턴스이므로 subclass UniversityStudent type으로 Casting 성공 ???

                ```swift
                var mike: Person = UniversityStudent() as Person // 1. mike는 Person type이지만, 실질적으로 UniversityStudent의 인스턴스이다.
                var jenny: Student = Student()

                -
                var optionalCasted: Student?

                optionalCasted = mike as? UniversityStudent  // 2. mike는 실질적으로 UniversityStudent의 인스턴스이므로 subclass UniversityStudent type으로 Casting 성공 ???

                optionalCasted = jenny as? UniversityStudent // nil
                optionalCasted = jenny as? Student // always suceeds

                optionalCasted = jina as? UniversityStudent // nil  // jina는 Person Type 이므로 Student 또는 UniversityStudent로 Casting 실패
                optionalCasted = jina as? Student // nil
                ```

        - 2) 강제 다운 캐스팅 (as!)
            - 캐스팅에 실패하면, 즉 캐스팅하려는 타입에 부합하지 않는 인스턴스라면 런타임 오류가 발생합니다.
            - 캐스팅에 성공하면 옵셔널이 아닌 일반 타입을 반환합니다.

                ```swift
                var forcedCasted: Student

                forcedCasted = mike as! UniversityStudent
                //forcedCasted = jenny as! UniversityStudent // 런타임 오류 // 강제로 UniversityStudent type이 되라고 명령했지만, 불가능해서 오류 발생
                //forcedCasted = jina as! UniversityStudent // 런타임 오류
                //forcedCasted = jina as! Student // 런타임 오류
                ```

- Type Casting 활용

    ```swift
    func doSomethingWithSwitch(someone: Person) {
        switch someone {  // someone으로 가져온 것이 어떤 type인지 확인하기 위한 switch case-is 구문
        case is UniversityStudent:
            (someone as! UniversityStudent).goToMT()  // 확인 결과 해당 값을 가져오려면 다시 down casting 해줘야 한다. (즉, mike는 Person type 이므로 Uni type으로 Casting 해줌)
        case is Student:
            (someone as! Student).goToSchool()
        case is Person:
            (someone as! Person).breath()
        }
    }
    doSomethingWithSwitch(someone: mike as Person) // 멤버쉽 트레이닝을 갑니다 신남!
    doSomethingWithSwitch(someone: mike) // 멤버쉽 트레이닝을 갑니다 신남!
    doSomethingWithSwitch(someone: jenny) // 등교를 합니다
    doSomethingWithSwitch(someone: yagom) // 숨을 쉽니다

    -
    func doSomething(someone: Person) {
        if let universityStudent = someone as? UniversityStudent {  // switch 대신 if-let을 쓰면 as?를 통해 casting 함과 동시에 unwrapping (옵셔널 추출)해서 값을 가져올 수 있다.
            universityStudent.goToMT()
        } else if let student = someone as? Student {
            student.goToSchool()
        } else if let person = someone as? Person {
            person.breath()
        }
    }
    doSomething(someone: mike as Person) // 멤버쉽 트레이닝을 갑니다 신남!
    doSomething(someone: mike) // 멤버쉽 트레이닝을 갑니다 신남!
    doSomething(someone: jenny) // 등교를 합니다
    doSomething(someone: yagom) // 숨을 쉽니다
    ```

- Yagom Textbook
    - Type Casting (타입 캐스팅)
        - 1) 인스턴스의 타입을 확인하거나, 2) 자신을 다른 타입의 인스턴스인 양 행세할 수 있는 방법으로 사용한다.
        - 데이터 타입을 확인하는 방법
            1. is 연산자를 통해 인스턴스가 어떤 클래스의 인스턴스 인지 (또는 어떤 클래스의 자식클래스의 인스턴스인지) 타입을 확인한다. (is 연산자는 클래스 인스턴스 뿐만 아니라 모든 데이터 타입에 사용 가능하다.)
            2. 메타 타입 (Meta Type) 타입을 이용한다.
                - 메타 타입 설명

                    ```swift
                    // 타입이름.Type -> 메타 타입
                    // 타입이름.self -> 타입을 값으로 표현한 값을 반환함

                    let intType: Int.Type = Int.self
                    print(intType)  // Int
                    print(Int.self) // Int
                    //print(Int.Type) // 컴파일 에러 - Expected member name or constructor call after type name
                    print(Int.Type.self) // Int.Type

                    let intType2 = Int.self // 참고
                    print(intType2) // Int
                    print(type(of: intType2)) // Int.Type

                    class SomeClass {}
                    let classType: SomeClass.Type = SomeClass.self

                    print(classType) // SomeClass
                    print(SomeClass.self) // SomeClass
                    //print(SomeClass.Type) // 컴파일 에러
                    print(SomeClass.Type.self) // SomeClass.Type

                    var someType: Any.Type
                    //print(someType) // 초기화 되지 않은 상태이므로 사용 불가
                    someType = intType
                    print(someType) // Int

                    someType = classType
                    print(someType) // SomeClass
                    ```

        - 타입 캐스팅은 참조 타입에서 주로 사용한다.
            - 예시 - Coffee 클래스를 상속받는 Latte 클래스 및 Americano 클래스가 있을 때, Coffee는 Latte나 Americano인 척할 수 없지만, Latte나 Americano는 Coffee인 척할 수 없다.

                ```swift
                // Coffee 클래스를 상속받는 Latte 클래스 및 Americano 클래스
                class Coffee {
                    let name: String
                    let shot: Int
                    
                    var description: String {
                        return "\(shot) shot(s) \(name)"
                    }
                    
                    init(shot: Int) {
                        self.shot = shot
                        self.name = "coffee"
                    }
                }
                class Latte: Coffee {
                    var flavor: String
                    
                    override var description: String {
                        return "\(shot) shot(s) \(flavor) latte"
                    }
                    
                    init(flavor: String, shot: Int) {
                        self.flavor = flavor
                        super.init(shot: shot)
                    }
                }
                class Americano: Coffee {
                    let iced: Bool
                    
                    override var description: String {
                        return "\(shot) shot(s) \(iced ? "iced" : "hot") americano"
                    }
                    
                    init(shot: Int, iced: Bool) {
                        self.iced = iced
                        super.init(shot: shot)
                    }
                }

                let myCoffee: Coffee = Coffee(shot: 1)
                print(myCoffee.description) // 1 shot(s) coffee

                let myAmericano: Americano = Americano(shot: 2, iced: false)
                print(myAmericano.description) // 2 shot(s) hot americano

                let myLatte: Latte = Latte(flavor: "green tea", shot: 3)
                print(myLatte.description) // 3 shot(s) green tea latte

                // is 연산자를 통해 타입 확인
                print(myCoffee is Coffee)         // true
                print(myCoffee is Americano)      // false
                print(myCoffee is Latte)          // false

                print(myAmericano is Coffee)    // true - Americano 클래스는 Coffee 클래스의 자식클래스이므로 true
                print(myAmericano is Americano) // true
                print(myAmericano is Latte)     // false

                print(myLatte is Coffee)        // true - Latte 클래스는 Coffee 클래스의 자식클래스이므로 true
                print(myLatte is Americano)     // false
                print(myLatte is Latte)         // true

                // 메타 타입 타입을 통해 타입 확인
                print(type(of: myCoffee) == Coffee.self)         // true
                print(type(of: myCoffee) == Americano.self)      // false
                print(type(of: myCoffee) == Latte.self)          // false

                print(type(of: myAmericano) == Coffee.self)     // false - is 연산자와 달리 자식클래스이지만 false
                print(type(of: myAmericano) == Americano.self)  // true
                print(type(of: myAmericano) == Latte.self)      // false

                print(type(of: myLatte) == Coffee.self)         // false - is 연산자와 달리 자식클래스이지만 false
                print(type(of: myLatte) == Americano.self)      // false
                print(type(of: myLatte) == Latte.self)          // true
                ```

    - 다운캐스팅 (Down Casting)
        - 어떤 클래스 타입의 상수/변수가 있을 때, 실제로는 해당 클래스 인스턴스를 참조하지 않을 수 있다. 
        예를 들어 Latte 클래스의 인스턴스가 Coffee 클래스의 인스턴스인양 Coffee 행세를 할 수도 있다.
            - [ ]  print(type(of: actingConstant) === actingConstant.self) // false - 왜지???????
            - [ ]  Coffee 인스턴스를 참조하도록 선언했지만, 실제로는 Coffee 타입인 척하는 Lattee 타입의 인스턴스를 참조하고 있다 - 왜 이게 가능하도록 만들었지????

            ```swift
            let actingConstant: Coffee = Latte(flavor: "vanilla", shot: 2) // Coffee 인스턴스를 참조하도록 선언했지만, 실제로는 Coffee 타입인 척하는 Lattee 타입의 인스턴스를 참조하고 있다
            print(actingConstant.description) // 2 shot(s) vanilla latte

            print(actingConstant is Coffee) // true
            print(actingConstant is Latte)  // true

            print(type(of: actingConstant)) // Latte
            print(actingConstant.self)      // test.Latte (이때 test는 실행중인 command 프로젝트 이름)

            //print(type(of: actingConstant) == actingConstant.self) // 컴파일 에러 - Binary operator '==' cannot be applied to operands of type 'Coffee.Type' and 'Coffee'
            print(type(of: actingConstant) === actingConstant.self) // false - 왜지???????
            //print(actingConstant.Type) // 컴파일 에러 - Value of type 'Coffee' has no member 'Type'
            ```

        - Coffee 인스턴스를 참조하도록 선언했지만, 실제로는 Coffee 타입인 척하는 Lattee 타입의 인스턴스를 참조하고 있다.
        이러한 상황에서 actingConstant가 참조하는 인스턴스를 진짜 타입인 Latte 타입으로 사용해야 할 때가 있다.
        예를 들면 Latte 타입에 정의된 메서드/프로퍼티를 사용해야 할 때이다. 이때는 Latte 타입으로 변수의 타입을 변환해줘야 한다. 이것을 다운캐스팅 (Down Casting) 이라고 한다.
        클래스 상속 모식도에서 부모클래스의 타입 (위)을 자식클래스의 타입 (아래)으로 캐스팅한다고 해서 다운캐스팅이라고 부른다.
        단, 클래스 인스턴스 뿐만 아니라 Any 타입 등에서도 다운캐스팅을 사용한다.
        - 타입캐스트 연산자는 `as?` 그리고 `as!` 가 있다. 
        다운캐스팅은 실패의 여지가 있으므로 ? / ! 두 종류가 있는 것이다.

            as? : 다운캐스팅 실패 시 nil을 반환한다. 성공 시 옵셔널 타입으로 인스턴스를 반환한다. (반환 타입이 옵셔널임)
            as! : 다운캐스팅을 강제하므로 다운캐스팅 실패 시 런타임 오류가 발생한다. 성공 시 옵셔널이 아닌 인스턴스를 반환한다. (반환 타입이 옵셔널이 아님)

        - 타입캐스팅은 실제로 인스턴스를 수정하는 작업이 아니다. 인스턴스는 메모리에 똑같이 남아있다. 단, 인스턴스 사용 시 어떤 타입으로 다루고/접근할지 판단하도록 컴파일러에게 힌트를 주는 것이다.
            - [x]  동일한 타입으로 캐스팅하는 것도 다운캐스팅에 속하는건가?
                - 속한다. 항상 성공하는 다운캐스팅이다. 컴파일러도 알고 있다. (경고 메시지 - Conditional cast from 'Coffee' to 'Coffee' always succeeds)

            ```swift
            // as?
            let actingConstant2: Coffee = Latte(flavor: "vanilla", shot: 2) // Coffee 인스턴스를 참조하도록 선언했지만, 실제로는 Coffee 타입인 척하는 Lattee 타입의 인스턴스를 참조하고 있다

            if let actingOne2: Latte = actingConstant2 as? Latte {
                print("\(actingConstant2) - This looks like Coffee, but actually this is Latte") // Coffee (부모)에서 Latte (자식)으로 캐스팅한 다운캐스팅 성공!
            } else {
                print(actingConstant2.description)
            } // test.Latte - This looks like Coffee, but actually this is Latte

            // 이것들도 다운캐스팅인가?????
            if let actingOne: Coffee = myCoffee as? Coffee { // myCoffee가 실제 참조하는 게 Coffee 타입의 인스턴스라면 상수 actingOne에 할당한다.
                print("This is just Coffee") // 항상 성공하는 다운캐스팅이다. (동일한 타입으로 캐스팅하는 것도 다운캐스팅에 속하긴 한다.) - Conditional cast from 'Coffee' to 'Coffee' always succeeds
            } else {
                print(myCoffee.description)
            } // This is just Coffee

            if let actingOne: Latte = myCoffee as? Latte { // myCoffee가 실제 참조하는 게 Latte 타입의 인스턴스라면 상수 actingOne에 할당한다.
                print("This is Latte")
            } else {
                print(myCoffee.description) // *다운캐스팅 실패 - Latte는 Coffee인 척 할수있지만, 그 반대는 불가하므로 else문이 실행된다. 
            } // 1 shot(s) coffee

            if let actingOne: Latte = myLatte as? Latte { // myLatte가 실제 참조하는 게 Latte 타입의 인스턴스라면 상수 actingOne에 할당한다.
                print("This is Latte") // 다운캐스팅 성공
            } else {
                print(myLatte.description)
            } // This is Latte

            if let actingOne: Coffee = myLatte as? Coffee { // myLatte가 실제 참조하는 게 Coffee 타입의 인스턴스라면 상수 actingOne에 할당한다.
                print("This is just Coffee") // 다운캐스팅 성공 - Latte는 Coffee인 척 할 수 있으므로
            } else {
                print(myLatte.description)
            }

            // as!
            let castedCoffee: Coffee = myLatte as! Coffee // 강제 다운캐스팅 성공
            //let castedLatte: Latte = myCoffee as! Latte // 강제 다운캐스팅 실패로 런타임 오류 발생 - Could not cast value of type 'test.Coffee' (0x10000c4c0) to 'test.Latte'
            ```

    - Any / AnyObject의 타입캐스팅
        - Swift 표준 라이브러리에는 거의 없지만, 기업이 만들어 제공하는 프레임워크의 API에는 Any / AnyObject가 사용된 것을 종종 볼 수 있다.
        어떤 타입의 데이터라도 전달 가능하다라는 의미로 해석할 수 있다.
        문제는 반환 타입이 Any / AnyObject라면 전달받은 데이터가 어떤 타입인지 확인하고 사용해야 한다는 것이다. (Swift는 암시적 타입 변환을 허용하지 않기 때문)
        - AnyObject의 타입 확인 및 타입캐스팅

            AnyObject는 클래스 인스턴스를 수용할 수 있다.

            ```swift
            func checkType(of item: AnyObject) { // AnyObject의 타입 확인
                if item is Latte {
                    print("item is Latte")
                } else if item is Americano {
                    print("item is Americano")
                } else if item is Coffee {
                    print("item is Coffee")
                } else {
                    print("Unknwon Type")
                }
            }
            checkType(of: myCoffee) // item is Coffee
            checkType(of: myLatte)  // item is Latte
            checkType(of: actingConstant) // item is Latte - if문에서 "is Coffee"를 먼저 만났다면, "item is Coffee"를 출력

            func castTypeToAppropriate(item: AnyObject) { // AnyObject의 타입캐스팅
                if let castedItem: Latte = item as? Latte {
                    print(castedItem.description)
                } else if let castedItem: Americano = item as? Americano {
                    print(castedItem.description)
                } else if let castedItem: Coffee = item as? Coffee {
                    print(castedItem.description)
                } else {
                    print("Unknwon Type")
                }
            }
            castTypeToAppropriate(item: myCoffee) // 1 shot(s) coffee
            castTypeToAppropriate(item: myLatte)  // 3 shot(s) green tea latte
            castTypeToAppropriate(item: myAmericano) // 2 shot(s) hot americano
            castTypeToAppropriate(item: actingConstant) // 2 shot(s) vanilla latte
            ```

        - Any의 타입캐스팅

            Any는 모든 타입의 인스턴스를 수용할 수 있다. (함수/구조체/클래스/열거형 등 모든 타입의 인스턴스를 의미할 수 있다.)

            - [ ]  함수/클로저가 왜 인스턴스가 있지????????????????????

            ```swift
            func checkAnyType(of item: Any) {
                switch item {
                case 0 as Int:
                    print("Zero as an Int")
                case 0 as Double:
                    print("Zero as an Double")
                case let someInt as Int:
                    print("an Int value of \(someInt)")
                case let someDouble as Double where someDouble > 0:
                    print("a positive Double value of \(someDouble)")
                case is Double:  // 바로 윗 case에 해당하지 않는 Double이 들어온다.
                    print("Some other double value that I don't want to print")
                case let someString as String:
                    print("a String value of \"\(someString)\"")
                case let (x, y) as (Double, Double):
                    print("an (x, y) point at \(x), \(y)")
                case let someLatte as Latte:
                    print(someLatte.description)
                case let stringConverter as (String) -> String:  // (String) -> String는 일급객체인 함수 타입이다. 클로저가 전달될 수 있다.
                    print(stringConverter("yagom"))
                default:
                    print("something else: \(type(of: item))")
                }
            }
            checkAnyType(of: 0)           // Zero as an Int
            checkAnyType(of: 0.0)         // Zero as an Double
            checkAnyType(of: 42)          // an Int value of 42
            checkAnyType(of: 3.14)        // a positive Double value of 3.14
            checkAnyType(of: -0.5)        // Some other double value that I don't want to print
            checkAnyType(of: "Hello")     // a String value of "Hello"
            checkAnyType(of: (3.0, 5.0))  // an (x, y) point at 3.0, 5.0
            checkAnyType(of: myLatte)     // 3 shot(s) green tea latte
            checkAnyType(of: myCoffee)    // something else: Coffee
            checkAnyType(of: { (name: String) -> String in
                return "Hello, \(name)!!!"
            })                            // Hello, yagom!!!
            // 어차피 switch문에서 타입을 명시했으므로 생략 가능...하지 않다. 컴파일 에러 - Unable to infer type of a closure parameter 'name' in the current context
            //checkAnyType(of: { (name) in
            //    return "Hello, \(name)!!!"
            //})
            ```

    - 옵셔널 및 Any

        Any 타입은 옵셔널 타입을 포함하여 ??????? 모든 값 타입을 표현한다. 단, Any 타입의 값이 들어올 자리에 옵셔널 값이 들어오면, 컴파일러는 경고를 한다.
        이때 as 연산자로 명시적 타입캐스팅을 하면, 경고 메세지가 사라진다.

        - [ ]  Any 타입 상수에 nil 할당 가능이 불가능 <=> 타입은 옵셔널 타입을 포함하여 모든 값 타입을 표현한다. - 다른 얘긴가 왜지???????

        ```swift
        let optionalValue: Int? = 100
        print(optionalValue) // Optional(100). 경고 메세지 - Expression implicitly coerced from 'Int?' to 'Any' - 기본값을 지정하거나 강제 추출하라고 권함
        print(optionalValue as Any) // Optional(100). 경고 메세지 없음 (as 연산자로 명시적 타입캐스팅을 했음)
        //print(optionalValue as Int) // 컴파일 에러 발생 - Value of optional type 'Int?' must be unwrapped to a value of type 'Int' - 병합연산자 ??를 사용하거나, 강제 추출하라고 권함

        //let someAny: Any = nil // 컴파일 에러 - nil은 옵셔널에만 할당 가능하다.
        // Any 타입 상수에 nil 할당 가능이 불가능 <=> 타입은 옵셔널 타입을 포함하여 모든 값 타입을 표현한다. - 다른 얘긴가 왜지???????
        ```

# 18. assert / guard

- 애플리케이션이 동작 도중에 생성하는 다양한 연산 결과값을 동적으로 확인하고 안전하게 처리한다.
- 참고 - [http://seorenn.blogspot.com/2016/05/swift-assertion.html](http://seorenn.blogspot.com/2016/05/swift-assertion.html)
- [ ]  찾아보기
    - `assertionFailure() / preconditionFailure()` : assert() 나 precondition() 의 조건이 실패한 것 처럼 동작시키는 코드 ???
    - `fatalError()` : 에러를 발생시키고 앱을 죽여야 할 상황에서 사용하며, 활용 빈도 낮다.
- Assertion

    You use assertion and precondition to make sure an essential condition is satisfied before executing any further code. If the Boolean condition evaluates to true, code execution continues as usual. If the condition evaluates to false, the current state of the program is invalid; code execution ends, and your app is terminated.

    It causes your app to terminate more predictably if an invalid state occurs, and helps make the problem easier to debug. Stopping execution as soon as an invalid state is detected also helps limit the damage caused by that invalid state.

    - `assert(_:_:file:line:)` 함수를 사용합니다.
    - 예상했던 조건의 검증을 위하여 사용합니다.
    - assert 함수는 디버깅 모드에서만 동작합니다.
    - 배포 환경 (production builds)의 애플리케이션에서 사용하지 않는다. 주로 디버깅 (debug builds) 중 어떤 조건을 검증하기 위해서 사용한다. 
    (만약 디버깅 모드에서 assert() 등을 통해 앱이 죽으면 죽은 위치가 확실하게 잡힌다. 이 점을 이용하면 버그를 미연에 방지하거나 디버그에 유용한 정보로 쓸 수 있다.)
        - [x]  precondition?
        - assert(*:*:file:line:)와 같은 역할을 하지만 실제 배포 환경 (production builds) 에서도 동작하는 precondition(*:*:file:line:) 함수도 있습니다.
            - Assertions help you find mistakes and incorrect assumptions during development, and preconditions help you detect issues in production.
            - The difference between assertions and preconditions is in when they’re checked: Assertions are checked only in debug builds, but preconditions are checked in both debug and production builds.
            - Use a precondition whenever a condition has the potential to be false, but must definitely be true for your code to continue execution.

    ```swift
    var someInt: Int = 0

    // 검증 조건과 실패시 나타날 문구를 작성해 줍니다
    assert(someInt == 0, "someInt는 0이 아니다")  // someInt == 0 이라는 조건이 true이면 pass하고, false이면 "someInt는 0이 아니다" 문구를 작성한다.

    someInt = 1

    // assert(someInt == 0) // 동작 중지, 검증 실패 (assertion fails, terminating the application)
    // assert(someInt == 0, "someInt != 0") // 동작 중지, 검증 실패
    // => assertion failed: someInt는 0이 아니다: file guard_assert.swift, line 26

    -

    func functionWithAssert(age: Int?) {  // parameter를 통해 입력된 argument를 검증할 때도 활용한다. // nil이 할당될 수 있으므로 optional type
        
        assert(age != nil, "age == nil")  // 1. age가 nil이 아니라는 조건이 true이면 pass한다. false이면 "" 문구를 작성한다.
        assert((age! >= 0) && (age! <= 130), "나이값 입력이 잘못되었습니다") // 2. age가 0 이상 and 130 이하라는 조건이 true이면 pass한다. *age! : forced unwrapping

        print("당신의 나이는 \(age)세 입니다") // 3. 두 가지 assert를 모두 pass 해야 print 가능하다.
        print("당신의 나이는 \(age!)세 입니다") *age! : forced unwrapping
    }

    functionWithAssert(age: 50)  // 당신의 나이는 Optional(30)세 입니다 / 당신의 나이는 30세 입니다

    // functionWithAssert(age: nil) // 동작 중지, 검증 실패 (중지되어 아래의 function도 실행되지 않음) // Assertion failed: age가 nil 입니다: file __lldb_expr_193/yagom_basic9 assert.guard.playground, line 14
    // functionWithAssert(age: 999) // 동작 중지, 검증 실패 // Assertion failed: age가 잘못 입력되었습니다: file __lldb_expr_191/yagom_basic9 assert.guard.playground, line 15
    ```

    - [x]  age! 느낌표 왜 찍지?

        assert((age! >= 0) && (age! <= 130), "나이값 입력이 잘못되었습니다") ← *forced unwrapping

- guard (빠른 종료- Early Exit)

    You use a guard statement to require that a condition must be true in order for the code after the guard statement to be executed.

    Using a guard statement for requirements improves the readability of your code, compared to doing the same check with an if statement.

    - `guard`를 사용하여 잘못된 값의 전달 시 특정 실행구문을 빠르게 종료합니다.
    - 디버깅 모드 뿐만 아니라 어떤 조건에서도 동작합니다.
    - `else` 블럭 내부에는 특정 코드블럭을 종료하는 지시어 (제어문 전환 명령어, control transfer statement) (`return`, `break`, `continue`, `throw` 등)가 꼭 있어**야 합니다.**
    - 타입 캐스팅, 옵셔널과 자주 사용됩니다.
    - 그 외에도 단순조건 판단 후 빠르게 종료할 때도 용이합니다.
        - if-let / guard-let optional binding 비교

            ```swift
            // 1. if / guard 차이

            var age: Int

            if age < 100 { ...
            } else { ...
              return
            }

            guard age < 100 else { ...
              return
            }

            // 즉, guard 문은 if 문에서 true인 경우에 실행할 statements가 생략된 형태이다. (true인 경우 pass 시키는 것이 목적이므로, 예외상황만을 처리하고 싶을 때 사용한다.)
            ```

            ```swift
            // 2. if-let / if-guard 차이

            if let unwrapped: Int = someValue { 
               // statements
               unwrapped = 3
            } 
            // unwrapped = 5  // 오류 발생 - if 구문 외부에서는 unwrapped 사용이 불가능 합니다. (if-let 구문에서 암시적으로 선언되었고, if-let 구문 안에서만 사용된다는 뜻)

            -
            guard let unwrapped: Int = someValue else { // someValue가 nil이 아니면 상수 unwrapped에 값을 할당한다.
                     return 
            }
            unwrapped = 3  // gaurd 구문 이후에도 unwrapped 사용 가능합니다. ('로컬상수' 선언으로 취급한다는 뜻. 즉, 일반적인 let선언 + guard-필수조건)
            print("Possible to use \(unwrapped)outside the guard statement")
            // Any variables/constants that were assigned values using an optional binding as part of the condition 
            // are available for the rest of the code block that the guard statement appears in.
            ```

        ```swift
        guard Bool type 조건 else {
        		// 예외상황 실행문
            // 제어문 전환 명령어 - 자신보다 상위의 코드블록을 종료하는 코드
        }
        // ...
        ```

        ```swift
        func functionWithGuard(age: Int?) {
            
            guard let unwrappedAge = age,  // 1. age unwrapping - age가 nil 이라면, 아래 statements가 실행되지 않고 바로 return (함수를 종료) 된다.
                unwrappedAge <= 130,
                unwrappedAge >= 0 else {   // 2. 만약 unwrappedAge가 130 이하이고 0 이상인 조건이 false 라면, else block이 실행되고 return (함수를 종료) 된다.
                print("나이값 입력이 잘못되었습니다")
                return
            }
            print("당신의 나이는 \(unwrappedAge)세 입니다")  // 3. 모든 조건이 true 라면 pass 한다. // 4. guard-let의 상수 (unwrappedAge)는 guard-let 구문 밖에서도 사용 가능하다.
        }

        funcWithGuard(age: 50)  // 당신의 나이는 50세 입니다 - 출력
        funcWithGuard(age: 500)  // 나이값 입력이 잘못되었습니다 - 출력 (else에서 return되어 함수가 종료되므로 당신의 나이는~ 이 출력되지 않음)

        -
        var count = 1

        while true {  // while 문에도 guard를 활용할 수 있다.
            guard count < 3 else {  // count < 3 이라는 조건이 true 이면 pass하고, false 이면 else block이 실행되고 break (while문을 종료) 한다.
                break
            }
            print(count)
            count += 1
        }
        // 1
        // 2

        -
        func someFunction(info: [String: Any]) {  // 1. parameter info의 type을 Dictionary로 지정했다. (value는 Any type이므로 실질적으로 사용하기 위해 casting이 필요함)
            guard let name = info["name"] as? String else { // 2. name key의 value (nil 가능해서 optional type임)에 대해 -> <as? String> 즉, String type으로 casting 시도
                return  // 3. 그래서 optional binding이 성공하면 name 상수에 value가 할당 되고, 실패하면 else block이 실행되면서 return (함수가 종료) 된다.
            }
            
            guard let age = info["age"] as? Int, age >= 0 else { // 4. age key의 value가 optional binding 성공하고, age>=0 조건이 true 이면 pass, false이면 return 된다.
                return
            }
            
            print("\(name): \(age)")  // 5. 위의 두 가지 guard를 모두 pass 해야 print 실행이 가능하다.
        }

        someFunction(info: ["name": "jenny", "age": "10"]) // age type이 String 이므로 함수 종료
        someFunction(info: ["name": "mike"]) // age가 nil 이므로 함수 종료
        someFunction(info: ["name": "yagom", "age": 10]) // yagom: 10 - 출력

        *참고 - Dictionary [key : value]
        var anyDictionary: Dictionary<String,Any> = [String: Any]()
        anyDictionary["someKey"] = "value"   // "someKey"라는 key에 할당한 값이 "value"이다.
        ```

# 19. Protocol

- L/G
    - 프로토콜도 하나의 타입으로 사용됩니다. 그렇기 때문에 다음과 같이 타입 사용이 허용되는 모든 곳에 프로토콜을 사용할 수 있습니다.
        - 함수, 메소드, 이니셜라이저의 파라미터 타입 혹은 리턴 타입
        - 상수, 변수, 프로퍼티의 타입
        - 컨테이너인 배열, 사전 등의 아이템 타입

- 특징
    - *프로토콜(Protocol)* 은 특정 기능을 수행하기 위한 메서드, 프로퍼티, 이니셜라이저 등의 요구 사항을 정의합니다. (즉, 특정 기능에 필요한 요구사항에 따르라고 강요하는 것이다.)
    - 구조체, 클래스, 열거형은 프로토콜을 채택(Adopted) 해서 프로토콜의 요구사항을 실제로 구현할 수 있습니다.
    - 어떤 프로토콜의 요구사항을 모두 따르는 타입은 그 프로토콜을 준수한다(Conform) 고 표현합니다.
    - 프로토콜은 기능을 정의하고 제시 할 뿐이지 스스로 기능을 구현하지는 않습니다.
- 프로토콜 구현
    - 프로퍼티 - 저장된 프로퍼티인지 연산 프로퍼티인지 명시하지 않는다. 하지만 프로퍼티의 이름과 타입 그리고 gettable, settable 한지는 명시합니다.
        - 프로퍼티 요구는 항상 var 키워드를 사용합니다.
        - get은 '읽기가 가능해야 한다'는 뜻이며 (읽기 전용 또는 읽기쓰기 모두 가능), get과 set을 모두 명시하면 '읽기쓰기 모두 가능'한 프로퍼티여야 합니다.
        - 타입 프로퍼티는 static 키워드로 선언합니다.
    - 메서드 - 필수 메소드 지정시 함수명과 반환값을 지정할 수 있다. parameter는 명시할 수 없다.
        - mutating 키워드를 사용해 인스턴스 메서드에서 "자신의 내부 값을 변경 가능하다"ㅇ          는 것을 표시할 수 있습니다. 
        (mutating 키워드는 값 타입 (구조체, 열거형)에만 사용한다. 클래스는 필요 없음)

            ```swift
            protocol Togglable {
                mutating func toggle() // method is allowed to modify the instance it belongs to
            }

            enum OnOffSwitch: Togglable {
                case off, on
                mutating func toggle() {
                    switch self {
                    case .off:
                        self = .on
                    case .on:
                        self = .off
                    }
                }
            }
            var lightSwitch = OnOffSwitch.off  // enum OnOffSwitch의 case인 off 를 할당함
            lightSwitch.toggle()  // lightSwitch is now equal to .on (off -> on 으로 변경됨)
            ```

    - 이니셜라이저
        - required 키워드
            - You can implement a protocol `initializer requirement` on a conforming class as either a designated initializer or a convenience initializer.  ???
            In both cases, you must mark the initializer implementation with the required modifier: ???
            - ensures that you provide an explicit implementation of the initializer requirement on all subclasses.

            ```swift
            protocol SomeProtocol {
                init(someParameter: Int)
            }

            class SomeClass: SomeProtocol {
                required init(someParameter: Int) {
                    // initializer implementation goes here
                }
            }
            ```

            - required, override 키워드를 모두 사용하는 경우
                - superclass의 이니셜라이저를 서브클래싱하여 override 하고, 프로토콜의 필수 이니셜라이저를 구현하는 경우
                (If a subclass overrides a designated initializer from a superclass, and also implements a matching initializer requirement from a protocol,)

                    ```swift
                    protocol SomeProtocol {
                        init()
                    }

                    class SomeSuperClass {
                        init() {
                            // initializer implementation goes here
                        }
                    }

                    class SomeSubClass: SomeSuperClass, SomeProtocol {
                        // "required" from SomeProtocol conformance; "override" from SomeSuperClass
                        required override init() {
                            // initializer implementation goes here
                        }
                    }
                    ```

    ```swift
    protocol 프로토콜 이름 {
        /* statements */
    }

    protocol Talkable {  // talk 기능을 구현하기 위한 프로토콜을 생성하겠다. 이를 위해 프로퍼티, 메서드, 이니셜라이즈를 요구 사항으로 정의하겠다.
        
        // 프로퍼티 요구 // 프로퍼티가 읽기전용 인지, 읽기쓰기 모두 가능한지 명시해준다.
        var topic: String { get set } // 읽기쓰기 모두 가능해야 한다.
        var language: String { get }  // 읽기가 가능해야 한다.  // { get set }으로 수정하면 => 오류 문구 - candidate is not settable, but protocol requires it.
        
        // 메서드 요구  // 프로토콜에서는 메서드를 제시하는 것이지 구현하지는 않는다. (구현하려면 func talk() {} 형태)
        func talk()
        
        // 이니셜라이저 요구
        init(topic: String, language: String) 
    }
    ```

- 프로토콜 채택 및 준수
    - [ ]  프로토콜 필수 이니셜라이저이므로 require init을 써야하는거 아닌가?

    ```swift
    struct Person: Talkable {  // Person 구조체는 Talkable 프로토콜을 Adapt 했다.
        // 프로퍼티 요구 준수
        var topic: String    // 1. topic 프로퍼티를 읽기쓰기 모두가능으로 정의했으므로 var만 사용 가능하다. (let 사용불가) 
        let language: String // 2. language 프로퍼티를 읽기전용으로 정의했으므로 1) let을 사용 가능하며, 2) var을 통해 읽기쓰기 모두가능으로 변경도 가능하다. (정의 조건이 '읽기가 가능' 이므로)
        
        // 위의 저장 프로퍼티를 연산 프로퍼티로 대체 가능하다.
    //    var language: String { ~~get {~~ return "한국어" ~~}~~ }   // 읽기전용 연산 프로퍼티의 get은 생략 가능
    //    var language: String { return "한국어" }  
        
    //    var subject: String = ""
    //    var topic: String {
    //        get {
    //            return self.subject
    //        }
    //        set {
    //            self.subject = newValue
    //        }
    //    }
        
        // 메서드 요구 준수    
        func talk() {
            print("\(topic)에 대해 \(language)로 말합니다")  // 구현
        }

        // 이니셜라이저 요구 준수    
        init(topic: String, language: String) {  // 구현
            self.topic = topic
            self.language = language
        }
    }
    ```

- 프로토콜 상속
    - 프로토콜은 하나 이상의 프로토콜을 상속받아 기존 프로토콜보다 더 많은 요구사항을 추가 가능하다.
    - 프로토콜 상속 문법은 클래스의 상속 문법과 유사하지만, 프로토콜은 클래스와 달리 다중상속이 가능합니다.

    ```swift
    protocol 프로토콜 이름: 부모 프로토콜 이름 목록 {
    	  /* statements */
    }

    -
    protocol Readable {  // super1
        func read()
    }
    protocol Writeable {  // super2
        func write()
    }

    protocol ReadSpeakable: Readable {  // sub1 - 프로토콜 단일상속
        func speak()  // func read()를 따로 명시하지 않아도 요구하게 됨 (요구사항을 상속 받았으므로)
     // func read(a: Int) // 이렇게 parameter가 추가된 함수 read(a:)를 명시할 경우, 해당 프로토콜을 adapt한 클래스는 read(), read(a:)를 모두 구현해줘야 함
    }
    protocol ReadWriteSpeakable: Readable, Writeable {  // sub2 - 프로토콜 다중상속
        func speak()  // func read(), write()를 요구하게 됨 (요구사항을 상속 받았으므로)
    }

    struct SomeType: ReadWriteSpeakable {  // sub2 ReadWriteSpeakable 프로토콜을 conform 하는 구조체 SomeType을 생성했다.
        func read() {
            print("Read")
        }
        func write() {
            print("Write")
        }
        func speak() {   // 요구사항 (함수 read, write, speak) 중 하나라도 빠지면 오류 발생
            print("Speak")
        }
    }
    ```

    - 클래스에서 1) 클래스 상속과 2) 프로토콜 Adapt를 동시에 하려면, 상속받으려는 superclass를 먼저 명시하고 그 뒤에 Adapt할 프로토콜 목록을 작성합니다.
        - Protocol Composition (결합) : 여러 프로토콜을 동시에 adapt/conform 하는 경우

    ```swift
    class SuperClass: Readable {
        func read() { print("read 기능 실행") }
    }

    class SubClass: SuperClass, Writeable, ReadSpeakable { // 앞) Superclass를 상속하며, 뒤) 프로토콜을 Adapt하는 subClass를 정의했다. // *Protocol Composition
    //  func read() {}  // override 하거나 반드시 생략해야 한다. (상속 받았으므로)
    R
        func write() { print("write 기능 실행") }
        func speak() { print("speak 기능 실행") }
    }
    ```

- 프로토콜 준수 확인
    - is, as 연산자를 사용해서 인스턴스가 특정 프로토콜을 conform 하는지 확인 가능하다.

    ```swift
    let sup: SuperClass = SuperClass()
    let sub: SubClass = SubClass()

    var someAny: Any = sup  // Any type의 변수 someAny에 SuperClass 클래스를 할당했다.
    someAny is Readable // true 
    someAny is ReadSpeakable // false - ReadSpeakable 프로토콜을 conform 하는가? 를 확인 가능하다.

    someAny = sub
    someAny is Readable // true
    someAny is ReadSpeakable // true

    -
    someAny = sup

    if let check = someAny as? Readable {  // Any type 이거나 Dictionary로 쓰다가 다운캐스팅이 필요할 때 이렇게 사용 가능하다.
        check.read()
    } // read 기능 실행

    if let check = someAny as? ReadSpeakable {
        check.speak()
    } // 동작하지 않음 (SuperClass는 ReadSpeakable 프로토콜을 conform 하지 않으므로 다운캐스팅 실패해서)

    someAny = sub

    if let check: Readable = someAny as? Readable {
        check.read()
    } // read 기능 실행 

    if let check = someAny as? ReadSpeakable {
        check.speak()
    } // speak 기능 실행 (SubClass는 ReadSpeakable 프로토콜을 conform 하므로)
    ```

- 야곰 textbook
    - 메서드 요구
        - "프로토콜 타입의 인스턴스"는 해당 프로토콜을 준수하는 타입의 인스턴스라는 의미이다.

        ```swift
        protocol Receivable { // 무언가를 수신받을 수 있는 기능
            func received(data: Any, from: Sendable)
        }

        protocol Sendable { // 무언가를 발신할 수 있는 기능
            var from: Sendable { get }   // *Sendable 프로토콜을 준수하는 타입의 인스턴스여야 한다.
            var to: Receivable? { get }  // *Receivable 프로토콜을 준수하는 타입의 인스턴스여야 한다.
            
            func send(data: Any)
            
            static func isSendableInstance(_ instance: Any) -> Bool
        }

        // 수신/발신이 가능한 클래스
        class Message: Sendable, Receivable { // 프로토콜은 다중상속 가능 (클래스는 단일상속)
            var from: Sendable { // 발신자는 발신 가능해야 한다.
                return self
            }
            
            var to: Receivable? // 수신자는 수신 가능해야 한다.
            
            func send(data: Any) { // 메시지를 발신한다. - 왜 to를 parameter로 지정을 안하고????
                guard let receiver: Receivable = self.to else { // 수신자가 있으면 receiver 상수에 담는다. - 프로퍼티 to를 초기화하지 않으면 guard문이 실패함
                    print("Message has no receiver")
                    return
                }
                receiver.received(data: data, from: self.from) // 상수 receiver의 received 메서드를 호출한다. (수신자가 있으면 메시지 받았다고 출력)
            }
            
            func received(data: Any, from: Sendable) { // 메시지를 수신한다.
                print("Message received \(data) from \(from)")
            }

            class func isSendableInstance(_ instance: Any) -> Bool {
                if let sendableInstance: Sendable = instance as? Sendable { // 1) 전달인자가 Any 이므로 as? 를 통해 Sendable를 준수하는지 확인
                    return sendableInstance.to != nil  // return true가 아님 - 2) 프로퍼티 to가 지정되었는지 확인
                }
                return false
            }
        }

        // 수신/발신이 가능한 클래스
        class Mail: Sendable, Receivable {
            var from: Sendable {
                return self
            }
            
            var to: Receivable?
            
            func send(data: Any) {
                guard let receiver: Receivable = self.to else {
                    print("Mail has no receiver")
                    return
                }
                receiver.received(data: data, from: self.from)
            }
            
            func received(data: Any, from: Sendable) {
                print("Mail received \(data) from \(from)")
            }
            
            static func isSendableInstance(_ instance: Any) -> Bool {
                if let sendableInstance: Sendable = instance as? Sendable {
                    return sendableInstance.to != nil
                }
                return false
            }
        }

        let myPhoneMessage: Message = Message()
        let yourPhoneMessage: Message = Message()

        // from/to 프로퍼티가 초기화되지 않았으므로 아무것도 안들어있음
        print(myPhoneMessage.from) // test.Message (test: 프로젝트 이름)
        print(myPhoneMessage.to)   // nil

        // 아직 수신받을 인스턴스가 없음
        myPhoneMessage.send(data: "Hello") // Message has no receiver - 수신자가 없음

        myPhoneMessage.to = yourPhoneMessage // 수신자 지정
        myPhoneMessage.send(data: "Hello") // Message received Hello from test.Message (test: 프로젝트 이름) - 수신자가 있으므로 메시지 받았다고 출력

        let myMail: Mail = Mail()
        let yourMail: Mail = Mail()

        myMail.send(data: "Hi") // Mail has no receiver

        myMail.to = yourMail
        myMail.send(data: "Hi") // Mail received Hi from test.Mail

        myMail.to = myPhoneMessage
        myMail.send(data: "ByeToPhone") // Message received ByeToPhone from test.Mail

        print(Message.isSendableInstance("Hello")) // false - String은 Sendable 프로토콜을 준수하지 않기 떄문
        print(Message.isSendableInstance(myPhoneMessage)) // true - myPhoneMessage는 Message 클래스 인스턴스이며, 1) Sendable 프로토콜을 준수하며, 2) 프로퍼티 to가 nil이 아니기 때문

        print(Message.isSendableInstance(yourPhoneMessage)) // false - 1) Sendable 프로토콜은 준수하지만, 2) 프로퍼티 to가 초기화되기 전이므로
        print(Mail.isSendableInstance(myPhoneMessage)) // true
        print(Mail.isSendableInstance(myMail)) // true
        ```

    (다른 내용 생략)

    - 위임 (Delegation)을 위한 프로토콜
        - 위임

            클래스, 구조체가 자신의 책무를 다른 타입의 인스턴스에게 위임하는 디자인 패턴이다.

            책무를 위임하기 위해 프로토콜을 정의할 수 있다. 해당 프로토콜을 준수하는 타입은 자신에게 위임될 일정 책무를 수행할 수 있음을 보장한다. 따라서 다른 인스턴스에게 자신이 해야할 일을 믿고 맡길 수 있다.

            위임은 사용자의 특정 행동에 반응하기 위해 사용하기도 하고, 비동기 처리에도 사용한다.

        - 위임 패턴

            애플의 프레임워크에서 사용하는 주요 패턴 중 하나이다. (언어 자체의 기능이 아니라 디자인 패턴임)

            "0000Delegate"라는 이름의 프로토콜이 위임 패턴을 사용한다.

            ex) UITableView 타입의 인스턴스가 해야할 일을 위임받아 처리하는 인스턴스는 UITableViewDelegate 프로토콜을 준수하면 된다.
            = 위임받은 인스턴스 (즉, UITableViewDelegate 프로토콜을 준수하는 인스턴스)는 UITableView 타입의 인스턴스가 해야할 일을 대신 처리해준다.

# 20. Extension

- 특징
    - 스위프트의 강력한 기능 중 하나입니다.
    - 구조체, 클래스, 열거형, 프로토콜, 제네릭 타입에 새로운 기능을 추가 ****할 수 있는 기능입니다.
    - 기능을 추가하려는 타입의 구현된 소스 코드를 알지 못하거나 볼 수 없다 해도, 타입만 알고 있다면 그 타입의 기능을 확장할 수 있습니다.

    - 익스텐션이 타입에 추가할 수 있는 기능
        - 연산 타입/인스턴스 프로퍼티
        - 타입/인스턴스 메서드
        - 이니셜라이저
        - 서브스크립트
        - 중첩 타입
        - 특정 프로토콜을 준수하도록 기능 추가

    - 클래스의 상속과 익스텐션 비교
        - 클래스의 상속은 클래스 타입에서만 가능하지만 익스텐션은 그 외 타입에도 적용이 가능합니다.
        - 클래스의 상속은 특정 타입을 물려받아 하나의 새로운 타입을 정의하고 추가 기능을 구현하는 수직 확장이지만, 익스텐션은 기존의 타입에 기능을 추가하는 수평 확장입니다.
        - 클래스 상속을 받으면 기존 기능을 override 가능하지만, 익스텐션은 override 불가합니다.

    - 익스텐션 활용
        - 익스텐션을 사용하는 대신 원래 타입을 정의한 소스에 기능을 추가하는 방법도 있겠지만, 외부 라이브러리나 프레임워크를 가져다 썼다면 원본 소스를 수정하지 못합니다. 
        이처럼 외부에서 가져온 타입에 내가 원하는 기능을 추가하고자 할 때 익스텐션을 사용합니다. 따로 상속을 받지 않아도 되며, 구조체와 열거형에도 기능을 추가할 수 있으므로 편리한 기능입니다.
        - 익스텐션은 모든 타입에 적용할 수 있습니다. 모든 타입이라 함은 구조체, 열거형, 클래스, 프로토콜, 제네릭 타입 등을 뜻합니다. 즉, 익스텐션을 통해 모든 타입에 연산 프로퍼티, 메서드, 이니셜라이저, 서브스크립트, 중첩 데이터 타입 등을 추가할 수 있습니다. 
        더불어 프로토콜과 함께 사용하면 강력한 기능을 선사합니다. 이 부분과 관련해 프로토콜 중심 프로그래밍(Protocol Oriented Programming)에 대해 더 알아보는 것을 추천합니다.
            - [ ]  프로토콜 중심 프로그래밍(Protocol Oriented Programming) 검색

- 정의

    ```c
    extension 확장할 타입 이름 {
        /* 타입에 추가될 새로운 기능 구현 */
    }
    ```

    - 활용 예시

        ```c
        // data type인 String 이나 Int에 특정 기능을 추가할 수 있다.
        extension String {
        		// statements
        }

        // 추가적으로 다른 프로토콜을 채택하도록 확장 가능하다. 이 경우 클래스나 구조체에서 사용하던 방법과 동일하게 프로토콜 이름을 나열한다.
        extension 확장할 타입 이름: 프로토콜1, 프로토콜2, 프로토콜3... {
            /* 프로토콜 요구사항 구현 */
        }
        ```

    - 스위프트 라이브러리를 살펴보면 실제로 익스텐션이 굉장히 많이 사용되고 있습니다. data type인 Double에는 수많은 프로퍼티와 메서드, 이니셜라이저가 정의되어 있으며 수많은 프로토콜을 채택하고 있을 것이라고 예상되지만, 실제로 Double 타입의 정의를 살펴보면 그 모든것이 다 정의되어 있지는 않습니다. 
    그러면 Double 타입이 채택하고 준수해야 하는 수많은 프로토콜은 어디로 갔을까요? 어디에서 채택하고 어디에서 준수하도록 정의되어 있을까요? 당연히 답은 익스텐션입니다. 이처럼 스위프트 표준 라이브러리 타입의 기능은 대부분 익스텐션으로 구현되어 있습니다.
        - [ ]  라이브러리 타입 정의/익스텐션 활용 예 검색

- 구현
    - 연산 프로퍼티 추가
        - data type인 Int에 두 개의 연산 프로퍼티를 추가했습니다.
        Int 타입의 인스턴스가 짝수 (even)인지 홀수 (odd)인지 판별하여 Bool 타입으로 알려주는 (Bool 값을 return 해주는) 연산 프로퍼티입니다. 
        익스텐션으로 Int 타입에 추가해준 연산 프로퍼티는 Int 타입의 어떤 인스턴스에도 사용이 가능합니다. 
        인스턴스 연산 프로퍼티를 추가 가능하며, static 키워드를 사용하여 타입 연산 프로퍼티도 추가 가능합니다. 
        저장 프로퍼티 및 프로퍼티 옵저버는 추가 불가합니다.
            - [x]  소스 코드가 이해가 안감
                - 아하! 연산 프로퍼티의 syntax구나!

                    ```c
                    extension Int {
                        var isEven: Bool {
                            return self % 2 == 0
                        }
                        var isOdd: Bool {
                            return self % 2 == 1
                        }
                    }

                    // 아래와 동일하다

                    extension Int {
                        var isEven: Bool {
                          get{
                    				  return self % 2 == 0  // return 값 (true or false)을 isEven에 할당해줌 (읽기전용 기능)
                    			}
                        }

                        var isOdd: Bool {
                          get{
                    	        return self % 2 == 1
                    			}
                        }
                    }
                    ```

            - [x]  // 1: Int type의 정수를 나타내는 리터럴 문법이다 → 리터럴 문법 무슨 뜻?
                - 상수/변수에 저장되어 있는 값을 사용하지 않고 (간접적인 사용), 값을 직접 코드에 나타낸 것이 리터럴이다. (직접적인 사용)

                    ex) 숫자 1 등을 코드에 직접 나타내면, 정수 리터럴이라고 한다. 
                    ex) "이건 문자열 리터럴" 등 "" 내부에 텍스트를 적은 형태의 문자열을 코드에 직접 나타낸 것을 문자열 리터럴이라고 한다.
                    ex) [1, 2, 3] 등 [] 내부에 데이터를 적은 형태의 Array를 코드에 직접 나타낸 것을 Array 리터럴이라고 한다.

            ```c
            extension Int {
                var isEven: Bool {
                    return self % 2 == 0  // self는 Int type의 인스턴스 값 -> 이 값을 2로 나눠서 나머지가 0 이 맞으면 true, 0이 아니면 false를 return값으로 isEven에 할당
                }
                var isOdd: Bool {
                    return self % 2 == 1
                }
            }

            var number: Int = 3  // Int type의 인스턴스를 생성하고, 초기값으로 3 을 할당
            print(number.isEven) // number(3)를 2로 나누면 나머지가 1이므로 (!== 0) -> false 출력
            print(number.isOdd) // true 출력

            number = 2
            print(number.isEven) // true 출력
            print(number.isOdd) // false 출력

            print(1.isEven) // false 출력 // 1도 Int type이기 때문에 인스턴스처럼 넣어줄 수 있다. // *1: Int type의 정수를 나타내는 리터럴 문법이다. (따라서 1이라는 인스턴스로 취급된다.) ??
            print(2.isEven) // true 출력
            print(1.isOdd)  // true 출력
            print(2.isOdd)  // false 출력
            ```

    - 메서드 추가
        - data type인 Int에 인스턴스 메서드 multiply(by:) 를 추가했습니다. 
        여러 기능을 여러 익스텐션 블록으로 나눠서 구현해도 전혀 문제가 없습니다.

        ```c
        extension Int {
            func multiply(by n: Int) -> Int {
                return self * n
            }
        }

        var number: Int = 3
        print(number.multiply(by: 2))   // 3 * 2 = 6 이므로 return 값인 6 출력
        print(number.multiply(by: 3))   // 3 * 3 = 9

        print(3.multiply(by: 2))  // 3 * 2 = 6
        print(4.multiply(by: 5))  // 20
        ```

    - 이니셜라이저 추가
        - 인스턴스를 초기화(이니셜라이즈)할 때 인스턴스 초기화에 필요한 다양한 데이터를 전달받을 수 있도록 여러 종류의 이니셜라이저를 만들 수 있습니다. 타입의 정의부에 이니셜라이저를 추가하지 않더라도 익스텐션을 통해 이니셜라이저를 추가할 수 있습니다.
        - 익스텐션으로 클래스 타입에 편의 이니셜라이저는 추가 가능하지만, 지정 이니셜라이저 (designated initializer) 및 디이니셜라이저는 추가 불가합니다.

            ```swift
            extension String { // Swift의 기본 데이터 타입인 String은 struct로 구현되어 있다
                init(intTypeNumber: Int) {
                    self = "\(intTypeNumber)"   // Int type의 입력값 (intTypeNumber)을 "String 타입"으로 변경해주는 기능
                }
                
                init(doubleTypeNumber: Double) {
                    self = "\(doubleTypeNumber)"
                }
            }

            let stringFromInt: String = String(intTypeNumber: 100)  // let선언 인스턴스를 생성하고, 이니셜라이저 intTypeNumber의 초기값으로 100을 할당한다.
            print(stringFromInt) // "100" 출력

            let stringFromDouble: String = String(doubleTypeNumber: 100.0)    
            print(stringFromDouble) // "100.0" 출력
            ```

    - [ ]  퀴즈2 앞부분 틀린것들 다시 풀기

# 21. Error Handling

- L/G
    - Swift는 에러 처리와 관련하여 에러 발생(throwing), 감지(catching), 전파?(propagating), 조작(manipulating)을 지원하는 일급 클래스 (first-class)를 제공함
    - 에러를 반환하는 throw 구문은 일반적인 반환 구문인 return과 performance characteristic 측면에서 비슷함
    - 4가지 error hadling 방법
    1) propagate the error from a function to the code that calls that function (에러가 발생한 함수에서 리턴값으로 에러를 반환하여 해당 함수를 호출한 코드에서 에러를 처리하도록 함)
    - `throwing function` : 에러 발생의 여지가 있는 함수 (메서드, initializer 또한 가능)를 indicate 하기 위해 throw 키워드를 함수 선언부의 parameter 및 return type 사이에 명시함
    - A throwing function propagates errors that are thrown inside of it to the scope from which it’s called. 
      throwing function은 함수 내부에서 에러를 만들어 (throw) 함수가 호출된 곳에 전달함
    2) use a do-catch statement
    3) return an optional value (try? / try!)
    4) assert that the error will not occur
    - Do-Catch statement
        - If an error is thrown by the code in the do clause, it’s matched against the catch clauses to determine which one of them can handle the error.
        - general form

            ```swift
            do {
                try expression
                statements
            } catch pattern 1 {
                statements
            } catch pattern 2 where condition {
                statements
            } catch pattern 3, pattern 4 where condition {
                statements
            }
            ```

    - ex) VendingMachine (어려움)
        - [ ]  var inventory = [ // 이 프로퍼티의 타입이 dictionary 인가? - key가 "Candy Bar", value가 Item strtuct로 구성된 형태 ?? 
                "Candy Bar": Item(price: 12, count: 7),
                "Chips": Item(price: 10, count: 4),
                "Pretzels": Item(price: 7, count: 11)
            ]
        - [ ]  guard let item = inventory[name] else {  // inventory[name] - 프로퍼티 inventory + 메서드 vend의 parameter ?? 이게 name이 <snack의 name>인지 어떻게 알지? 정의 안해줬는데
        - String type 이라서 자체 추론?

        ```swift
        enum VendingMachineError: Error {
            case invalidSelection
            case insufficientFunds(coinsNeeded: Int)
            case outOfStock
        }

        struct Item { // 자판기에 들어갈 item의 구조체 정의
            var price: Int
            var count: Int
        }

        class VendingMachine { 
            var inventory = [ // 이 프로퍼티의 타입이 dictionary - key가 "Candy Bar", value가 Item strtuct로 구성된 형태 맞음
                "Candy Bar": Item(price: 12, count: 7),
                "Chips": Item(price: 10, count: 4),
                "Pretzels": Item(price: 7, count: 11)
            ]
            var coinsDeposited = 0
            
            func vend(itemNamed name: String) throws {  // 메서드 정의
                guard let item = inventory[name] else { // inventory[key]
                    throw VendingMachineError.invalidSelection
                }
                
                guard item.count > 0 else { // *복습) if-let 구문과 달리 guard-let의 item 변수는 구문 밖에서도 활용 가능함
                    throw VendingMachineError.outOfStock
                }
                
                guard item.price <= coinsDeposited else {
                    throw VendingMachineError.insufficientFunds(coinsNeeded: item.price - coinsDeposited)
                }
                
                coinsDeposited -= item.price
                
                var newItem = item
                newItem.count -= 1
                inventory[name] = newItem
                
                print("Dispensing \(name)")
            }
        }

        let favoriteSnacks = [
            "Alice": "Chips",
            "Bob": "Licorice",
            "Eve": "Pretzels"
        ]

        func buyFavoriteSnacks(person: String, vendingMachine: VendingMachine) throws {
            let snackName = favoriteSnacks[person] ?? "Candy Bar"  // favoriteSnacks[person] 이것도
            try vendingMachine.vend(itemNamed: snackName)
        // Because the vend(itemNamed:) method can throw an error, it’s called with the try keyword in front of it.
        }

        // any errors that the vend(itemNamed:) method throws will propagate up to the point where the buyFavoriteSnack(person:vendingMachine:) function is called.
        // (vend(itemNamed:) 메소드에서 발생한 에러는 buyFavoriteSnack(person:vendingMachine:) 함수가 실행되는 곳에까지 전해집니다.)

        // Error Throwing initializers

        struct PurchasedSnack {
            let name: String
            init (name: String, vendingMachine: VendingMachine) throws {
                try vendingMachine.vend(itemNamed: name)
                self.name = name
            }
        }

        // the initializer for the PurchasedSnack structure in the listing below calls a throwing function as part of the initialization process, and it handles any errors that it encounters by propagating them to its caller.
        // PurchasedSnack 구조체의 초기자는 초기화 단계의 일부분으로써 에러를 발생시킬 수 있는 함수입니다. 그리고 초기자가 실행될 때 발생한 에러는 이 초기자를 호출한 곳에 전달 됩니다.

        // 방법-1 (the following code matches against all three cases of the VendingMachineError enumeration.)
        var vendingMachine = VendingMachine()  // 인스턴스 생성
        vendingMachine.coinsDeposited = 8

        do {
            try buyFavoriteSnacks(person: "Alice", vendingMachine: vendingMachine) // buyFavoriteSnacks 함수는 오류 발생 여지가 있으므로 (can throw an error) try expression 에서 호출됨
            print("Success! Yum.")
        } catch VendingMachineError.invalidSelection {
            print("Invalid Selection.")
        } catch VendingMachineError.outOfStock {
            print("Out of Stock.")
        } catch VendingMachineError.insufficientFunds(let coinsNeeded) {
            print("Insufficient Funds. Please insert an additional \(coinsNeeded) coins.")
        } catch {
            print("Unexpected error: \(error).")
        }
        // Prints "Insufficient funds. Please insert an additional 2 coins."

        // vendingMachine.coinsDeposited = 10
        // Prints Dispensing Chips, Success! Yum. (오류가 발생하지 않은 경우)

        // the buyFavoriteSnack(person:vendingMachine:) function is called in a try expression, because it can throw an error. 
        // If an error is thrown, execution immediately transfers to the catch clauses, which decide whether to allow propagation to continue.
        // If no pattern is matched, the error gets caught by the final catch clause and is bound to a local error constant. (만약 발생한 에러 종류에 해당하는 catch pattern 가 없다면 에러는 마지막 catch 구문에 걸리게 되서 지역 에러 상수인 error로 처리할 수 있습니다.)
        // If no error is thrown, the remaining statements in the do statement are executed.

        // 방법-2
        func nourish(with item: String) throws {
            do {
                try vendingMachine.vend(itemNamed: item)
            } catch is VendingMachineError {  // *is : Enum Error 3가지 경우에 해당하는지 체크
                print("Couldn't buy that from the the vending machine.")  // Error가 발생했는데 Enum Error 3가지 경우에 해당하는 경우
            }
        }

        do {
            try nourish(with: "Beef-Flavored Chips")
        } catch {
            print("Unexpected non-vending-machine-related error: \(error)")  // Error가 발생했는데 Enum Error 3가지 경우에 해당하지 않는 경우
        }

        // The catch clauses don’t have to handle every possible error that the code in the do clause can throw.
        // For example, the above example can be written so any error that isn’t a VendingMachineError is instead caught by the calling function: (<자동판매기 오류가 아닌 오류>는 다음의 호출 기능에 의해 대신 잡힙니다.)

        // In the nourish(with:) function, if vend(itemNamed:) throws an error that’s one of the cases of the VendingMachineError enumeration, nourish(with:) handles the error by printing a message.
        // Otherwise, nourish(with:) propagates the error to its call site. (nourish 함수는 그것을 호출한 곳에 에러를 발생시킨다.) The error is then caught by the general catch clause.

        // 방법-3
        func eat(item: String) throws {
            do {
                try vendingMachine.vend(itemNamed: item)
            } catch VendingMachineError.invalidSelection, VendingMachineError.insufficientFunds, VendingMachineError.outOfStock {
                print("Invalid Selection, out of stock, or not enough money.")
            }
        }

        // Another way to catch several related errors is to list them after catch, separated by commas.
        ```

- 특징
    - 오류는 `Error` 프로토콜을 준수하는 타입의 값을 통해 표현됩니다. `Error` 프로토콜은 사실상 요구사항이 없는 빈 프로토콜일 뿐이지만, 오류를 표현하기 위한 타입 (주로 열거형)은 이 프로토콜을 채택합니다.

        ```swift
        enum 오류종류이름: Error { 
        	case 종류1 
        	case 종류2 
        	case 종류3
        }
        ```

    - 열거형은 에러를 grouping하고, 연관 값 (associated values)을 통해 오류에 관한 부가 정보를 제공 가능합니다.
    - 함수 내부에서 오류가 발생할 경우, 함수는 즉시 오류를 자신을 호출한 곳에 던지고 (throw), 함수를 종료시킨다. (오류 발생 여지가 있는 함수는 throws로 명시한다.)
    - 오류를 던질 경우 던져진 오류를 처리하기 위한 코드도 작성해야 합니다. (예를 들어 던져진 오류가 무엇인지 판단하여 다시 문제를 해결한다든지, 다른 방법으로 시도해 본다든지, 사용자에게 오류를 알리고 사용자에게 선택 권한을 넘겨주어 다음 동작의 결정을 유도하는 등의 코드)
    - 오류 발생 여지가 있는 throws 함수는 `try`를 사용하여 호출해야 합니다. `try`와 `do-catch`, `try?`와 `try!` 등을 알아봅니다.

        ```swift
        do {
            try 오류 발생 가능한 throws 함수  // 1. 만약 여기서 오류가 발생하여 오류가 throw 되면
        } catch 오류 패턴 1 {  // 2. 여기서 오류를 catch하여 처리한다.
            처리
        } catch 오류 패턴 2 {
            처리
        }
        ```

        - [ ]  rethrows
        - [x]  defer
            - defer (Specifying Cleanup Actions) /연기하다/
                - use a defer statement to execute a set of statements just before code execution leaves the current block of code. 
                A defer statement defers execution until the current scope is exited.
                For example, to ensure that file descriptors (포인터와 유사한 개념) are closed and manually allocated memory is freed.
                현재 코드블럭이 종료될 때까지 특정 statement를 지연시켜서 필수적인 cleanup을 진행하도록 한다. 
                (*cleanup - 함수가 종료 된 후 파일 스트림을 닫거나, 사용했던 메모리를 해지하는 등)
                - Deferred actions are executed in the reverse of the order that they’re written in your source code.
                defer가 여러 개 있는 경우 가장 마지막 줄부터 실행된다.
                - ex) open 함수와 매치되는 close 함수를 실행
                    - used a defer statement to ensure that the open(*:) function has a corresponding call to close(*:).

                    ```swift
                    func processFile(filename: String) throws {
                    	if exists(filename) {
                    		let file = open(filename)
                    		defer {
                    			close(file)  // block 이 끝나기 직전에 실행
                    		}
                    		
                    		while let line = try file.readline() {
                    				// work with the file.
                    		}
                    		// close(file) is called here, at the end of the scope.
                    	}
                    }
                    ```

- 예제 - 자판기 작동 시 발생하는 오류상황을 구현
    - Error 표현

        ```swift
        enum VendingMachineError: Error {  // 열거형 VendingMachineError, Error 프로토콜으로 Error를 표현
            case invalidInput
            case insufficientFunds(moneyNeeded: Int)
            case outOfStock
        }
        ```

    - Error 던지기 
    - 오류 발생 여지가 있는 메서드는 throws 함수를 사용하여 오류를 내포하는 함수임을 표시합니다

        ```swift
        class VendingMachine {
            let itemPrice: Int = 100
            var itemCount: Int = 5
            var deposited: Int = 0
            
            // 돈 받기 메서드
            func receiveMoney(_ money: Int) throws {  // throws : 오류 발생 여지가 있음을 표시
                
                // 입력한 돈이 0이하면 오류를 던집니다
                guard money > 0 else {  // guard 로 빠른 종료를 유도하고
                    throw VendingMachineError.invalidInput  // 오류 (첫번째 case)를 던져줌
                }
                
                // 오류가 없으면 정상처리를 합니다
                self.deposited += money  
                print("\(money)원 받음")
            }
            
            // 물건 팔기 메서드
            func vend(numberOfItems numberOfItemsToVend: Int) throws -> String {  // 오류 발생 여지가 있음을 표시
                
                // 원하는 아이템의 수량이 잘못 입력되었으면 오류를 던집니다
                guard numberOfItemsToVend > 0 else {
                    throw VendingMachineError.invalidInput // 원래는 함수의 반환타입인 String을 반환해야 하는데, 오류가 발생하면 throw 하고 함수를 종료시킴
                }
                
                // 구매하려는 수량보다 미리 넣어둔 돈이 적으면 오류를 던집니다
                guard numberOfItemsToVend * itemPrice <= deposited else {
                    let moneyNeeded: Int
                    moneyNeeded = numberOfItemsToVend * itemPrice - deposited
                    
                    throw VendingMachineError.insufficientFunds(moneyNeeded: moneyNeeded)
                }
                
                // 재고보다 구매하려는 수량이 많으면 오류를 던집니다
                guard itemCount >= numberOfItemsToVend else {
                    throw VendingMachineError.outOfStock
                }
                
                // 오류가 없으면 정상처리를 합니다
                let totalPrice = numberOfItemsToVend * itemPrice
                
                self.deposited -= totalPrice
                self.itemCount -= numberOfItemsToVend
                
                return "\(numberOfItemsToVend)개 제공함"
            }
        }
        // 자판기 인스턴스
        let machine: VendingMachine = VendingMachine()

        // 판매 결과를 전달받을 변수
        var result: String?
        ```

- 예제 - do-catch, try
    - do-catch
        - 오류 발생 여지가 있는 throws 함수는 do-catch 구문을 활용하여 오류 발생에 대비합니다. (오류 발생의 여지가 있는 throws 함수는 try를 사용하여 호출해야 한다.)

            1) 모든 오류 케이스 (3가지 catch)에 대응하는 방식 (정석적인 방법)

            ```swift
            do {
                try machine.receiveMoney(0)  // *해당 부분 (입력한 돈이 0 이하)에서 오류가 발생하여 오류를 throw 했을 경우 (receiveMoney는 throws 함수임)
            } catch VendingMachineError.invalidInput {  // throw 된 오류를 (오류 패턴에 맞게) 여기서 catch 하여 실행해줌
                print("입력이 잘못되었습니다")
            } catch VendingMachineError.insufficientFunds(let moneyNeeded) { 
                print("\(moneyNeeded)원이 부족합니다")
            } catch VendingMachineError.outOfStock {
                print("수량이 부족합니다")
            } // 입력이 잘못되었습니다 - 출력
            ```

            2) 하나의 catch 블럭에서 switch 구문을 사용하여 오류를 분류해봅니다. 위 코드와 큰 차이가 없습니다.
            - If a catch clause doesn’t have a pattern, the clause matches any error and binds the error to a local constant named error.

            ```swift
            do {
                try machine.receiveMoney(300)
            } catch /*(let error)*/ {  // (let error를 통해 error 변수를 선언하지 않아도) 암시적 선언을 통해 switch문 내에서 사용 가능
                
                switch error {
                case VendingMachineError.invalidInput:
                    print("입력이 잘못되었습니다")
                case VendingMachineError.insufficientFunds(let moneyNeeded):
                    print("\(moneyNeeded)원이 부족합니다")
                case VendingMachineError.outOfStock:
                    print("수량이 부족합니다")
                default:
                    print("알수없는 오류 \(error)")   // enum 에서 정의한 3가지 에러 외에 다른 종류의 error가 발생한 경우
                }
            } // 300원 받음 - 출력
            ```

            3) 케이스별로 오류처리 할 필요가 없으면 catch 구문 내부를 간략화해도 무방합니다.

            ```swift
            do {
                result = try machine.vend(numberOfItems: 4)
            } catch {
                print(error)
            } 
            // insufficientFunds(moneyNeeded: 400) - 출력  // ?? guard 내용이 자동으로 넘어온건가?
            ```

            - [ ]  // insufficientFunds(moneyNeeded: 400)  // ?? guard 내용이 자동으로 넘어온건가?

            4) 케이스별로 오류처리 할 필요가 없으면 do 구문만 써도 무방합니다.

            ```swift
            do {
                result = try machine.vend(numberOfItems: 4)
            }

            -
            Playground execution terminated: An error was thrown and was not caught:
            ▿ VendingMachineError
              ▿ insufficientFunds : 1 element
                - moneyNeeded : 400
            ```

    - try? / try!
        - try? (Converting Errors to Optional Values)
            - try? : convert error to an optional value. 
            Using try? lets you write concise error handling code when you want to handle all errors in the same way.
                - 별도의 오류처리 결과를 통보받지 않고, 오류가 발생했으면 결과값을 nil 로 return 합니다.
                - 정상동작 시 결과값을 optional 타입으로 정상 반환값을 돌려 받습니다.

                ```swift
                // x and y have the same value and behavior:

                func someThrowingFunction() throws -> Int {
                	// ...
                }

                let y = Int?
                do {
                	y = try someThrowingFunction()
                } catch {
                	y = nil  // someThrowingFunction() 함수에서 오류 발생 시 nil을 결과값으로 return 한다. 
                }

                let x = try? someThrowingFunction()  // 위와 동일한 표현
                ```

            ```swift
            try machine.receiveMoney(300)  // 300원 넣었다고 가정 - 300원 받음 출력

            result = try? machine.vend(numberOfItems: 2)
            result // 2개 제공함 - 출력   ?? 왜 optional 아니지?
            print(result) // Optional("2개 제공함") - 출력

            result = try? machine.vend(numberOfItems: 2)
            print(result) // nil - 출력 (돈이 부족해서 오류 발생했으므로)
            ```

            - [ ]  result // 2개 제공함 - 출력   ?? 왜 optional 아니지?
        - try! (Disabling Error Propagation)
            - to disable error propagation and wrap the call in a runtime assertion that no error will be thrown.
            - 오류가 발생하지 않을 것을 확신할 때, try! (optional-forced unwrapping과 유사함)를 사용하면 정상동작 직후 결과값을 return 합니다.
            (오류가 발생하면 런타임 오류가 발생하여 애플리케이션 동작이 중지됩니다.)
            - ex) 앱 설치 시 이미 다운받은 이미지 파일을 load 할 때 (에러 발생 여지가 없음)

            ```swift
            try machine.receiveMoney(300)  // 300원 넣었다고 가정 - 300원 받음 출력

            result = try! machine.vend(numberOfItems: 2)
            result // 2개 제공함 - 출력
            print(result) // Optional("2개 제공함") - 출력
            print(result!) // 2개 제공함 - 출력

            // result = try! machine.vend(numberOfItems: 1)
            // print(result)
            // 런타임 오류 발생!
            ```

# 22. Higher-order Function (고차 함수)

- 특징
    - 고차 함수 (Higher-order Function) : 다른 함수를 전달인자로 받거나, 실행의 결과를 함수로 반환하는 함수
    - Swift의 함수/closure는 일급 시민 (일급 객체)이기 때문에 함수의 전달인자로 전달할 수 있으며, 함수의 결과값으로 반환할 수 있다.
    - Swift 표준라이브러리의 유용한 고차함수 - map, filter, reduce (Array, Set, Dictionary 등 컨테이너 타입에 구현되어 있음)
- map
    - Array, Set, Dictionary, Optional 등에서 사용 가능하다. (엄밀히는 Sequence, Collection 프로토콜을 따르는 type 및 Optional에서 사용 가능하다.)
    - 컨테이너 내부의 기존 데이터를 변형(transform)하여 새로운 컨테이너를 생성 및 반환한다.
    - 다중 스레드 환경일 때 대상 컨테이너의 값이 스레드에서 변경되는 시점에 다른 스레드에서도 동시에 값 변경이 되려고 할 때, 예상 외의 결과가 발생하는 부작용을 방지한다. ???
        - [ ]  ?
    - for 구문보다 성능이 좋다. (코드가 짧다. 또한 빈 배열을 생성하고, append 연산을 실행하는 등 작업이 불필요하다.)

        ```swift
        container1.map(f(x))
        => return f(container1의 각 요소) // 실행 결과 새로운 컨테이너를 반환한다.
        ```

        ```swift
        import Swift

        // 기존 데이터
        let numbers: [Int] = [0, 1, 2, 3, 4]

        // 변형 결과를 받을 변수
        var doubledNumbers: [Int]  // 변형 - 각 element를 2배한 새로운 Array
        var strings: [String]      // 변형 - 각 element를 Int에서 Sting type으로 변환한 새로운 Array

        // 방법1 - for 구문 사용
        doubledNumbers = [Int]()
        strings = [String]()

        for number in numbers {
            doubledNumbers.append(number * 2)  // array1.append() : array1에 element()를 추가하는 메서드
            strings.append("\(number)")
        }
        print(doubledNumbers) // [0, 2, 4, 6, 8]
        print(strings) // ["0", "1", "2", "3", "4"]

        // 방법2 - map 메서드 사용
        doubledNumbers = numbers.map({ (number: Int) -> Int in   // map()의 parameter 자리에 closure가 들어간다! 반환값 type을 명시하도록 되어있음
            return number * 2
        })

        strings = numbers.map({ (number: Int) -> String in
            return "\(number)"
        })
        print(doubledNumbers) // [0, 2, 4, 6, 8]
        print(strings) // ["0", "1", "2", "3", "4"]

        // 방법3 - map 메서드 + closure 표현방법 (매개변수, 반환 타입, return 생략, 후행 클로저)
        doubledNumbers = numbers.map({ $0 * 2 })
        strings = numbers.map({ "\($0)" }) // 이것도? 가능!

        doubledNumbers = numbers.map { $0 * 2 }  // 후행 클로저이므로 함수 외부에 구현 가능
        strings = numbers.map { "\($0)" } 

        print(doubledNumbers) // [0, 2, 4, 6, 8]
        print(strings) // ["0", "1", "2", "3", "4"]
        ```

    - 코드 재사용성 개선

        ```swift
        let evenNumbers: [Int] = [0,2,4,6,8]
        let oddNumbers: [Int] = [1,3,5,7,9]

        let multiplyTwo: (Int) -> Int = { $0 * 2 }

        let doubledEvenNumbers = evenNumbers.map(multiplyTwo) // 기능을 반복한다면, 하나의 클로저를 상수에 할당하여 여러 map 메서드에 적용하는 것이 좋다.
        print(doubledEvenNumbers) // [0, 4, 8, 12, 16]

        let doubledOddNumbers = oddNumbers.map(multiplyTwo) 
        print(doubledOddNumbers) // [2, 6, 10, 14, 18]
        ```

        - [x]  Dictionary는? (교재 p.297 참고)

            ```swift
            let dictionary: [String:String] = ["key1":"value1", "key2":"value2"]
            		
            		let keys = dictionary.map { $0.0 }
            		let values = dictionary.map { $0.1 }
            		
            		print(keys) //["key1", "key2"] - 변화 없음
            		print(values) //["value1", "value2"]
            		
            		let keys2 = dictionary.map { $0.0 + "a" }
            		let values2 = dictionary.map { $0.1 + "b" }
            		
            		print(keys2) //["key1a", "key2a"]
            		print(values2) //["value1b", "value2b"]
            ```

- x.map(함수)
    - x.map() 내부에 클로저 뿐만 아니라 함수도 들어갈 수 있다. `x.map(클로저)` `x.map(함수)`
    - 함수의 argument로 x (array, dictionary 등)의 element가 하나씩 전달된다.
    - map은 함수를 인자로 받는다.

    ```swift
    func addThree(_num: Int) -> Int {
    		return num + 3
    }

    // 일반적인 연산
    addThree(2) // 5
    addThree(Optional(2)) // int type과 int? type은 다르므로 오류 발생

    // 옵셔널 연산이 가능해짐
    optional(2).map(addThree) // Optional(5) - map 메서드를 통해 옵셔널 연산이 가능해진 addThree 함수
    ```

- filter
    - 컨테이너 내부의 값을 filtering (ture 해당 값만 추출)하여 새로운 컨테이너로 추출한다.
    - filter 함수의 parameter로 전달되는 함수의 return type은 Bool이다. 해당 콘텐츠의 값을 갖고 새로운 컨테이너에 포함될 항목이라고 판단되면 true를 반환하면 된다.

        ```swift
        // 기존 데이터
        let numbers: [Int] = [0, 1, 2, 3, 4]

        // 방법1 - for 문 사용
        var filtered: [Int] = [Int]()  // reduce와 달리 let 선언 불가

        for number in numbers {
            if number % 2 == 0 {
                filtered.append(number)
            }
        }
        print(filtered) // [0, 2, 4]

        // 방법2 - filter 메서드 사용
        // numbers의 요소 중 짝수를 걸러내어 새로운 배열로 반환
        let evenNumbers: [Int] = numbers.filter { (number: Int) -> Bool in   // filter()의 parameter 자리에 closure가 들어간다!
            return number % 2 == 0   // return 값의 Bool이 true 일때만 해당 number을 변수 evenNumbers 에 반환 (true/false를 return 하는 게 아님)
        }
        print(evenNumbers) // [0, 2, 4]

        // 방법3 - filter + closure 표현방법 (매개변수, 반환 타입, return 생략, 후행 클로저)
        let evenNumbers2: [Int] = numbers.filter { $0 % 2 == 0 } // {} 내용이 true인 경우만 변수 evenNumbers2 에 반환
        print(evenNumbers2) // [0, 2, 4]

        // 참고 - map 및 filter 연계 사용 가능
        let together: [Int] = numbers.map{ $0 + 3 }.filter{ $0 % 2 == 1 } // 3,4,5,6,7 중에서 홀수
        print(together) // [3, 5, 7]
        ```

- reduce
    - 컨테이너 내부의 콘텐츠를 하나로 통합 (Combine)한다.
        - 형태-1. reduce(_:_:) 클로저가 각 요소를 전달받아 연산한 이후 값을 다음 클로저 실행을 위해 반환하면서 컨테이너를 순환한다.

            ```swift
            public func reduce<Result>(_ initialResult: Result, _ nextPartialResult: (Result, Element) throws -> Result) rethrows -> Result

            // 1. 첫번째 parameter initialResult를 통해 초기값을 지정한다.
            // 2. 두번째 parameter nextPartialResult를 통해 클로저를 전달받는다. 
            //    이 클로저의 첫번째 parameter Result는 1의 초기값 또는 '이전 클로저의 결과값'이다. 모든 순회가 끝나면, reduce의 최종 결과값이 된다.
            //    이 클로저의 두번째 parameter Element는 reduce 메서드가 순환하는 컨테이너의 element이다.
            ```

            ```swift
            let numbers: [Int] = [1,2,3] // 이 값은 클로저의 두번째 parameter (next)에 차례로 전달됨

            // +
            var sum: Int = numbers.reduce(0, { (result: Int, next: Int) -> Int in
                print("\(result) + \(next)") // 0 + 1, 1 + 2, 3 + 3 출력 ('이전 클로저의 결과값'을 첫번째 parameter result로 전달받음)
                return result + next
            })
            print(sum) // 마지막 결과값을 반환 6

            // -
            let subtract1: Int = numbers.reduce(0, {
                print("\($0) - \($1)") // 0 - 1, -1 - 2, -3 - 3
                return $0 - $1
            })
            print(subtract1) // -6

            // String Array를 reduce 메서드를 사용하여 연결시킨다.
            let names: [String] = ["kevin","sam","mike"]

            let reducedNames1: String = names.reduce("Yagom's Friend : ") {
                return $0 + ", " + $1 }
            print(reducedNames1) // Yagom's Friend : , kevin, sam, mike
            ```

        - 형태-2. reduce(into:_:) 컨테이너를 순환하며 클로저가 실행되지만 클로저가 따로 결과값을 반환하지 않는다.
            - [ ]  첫번째 parameter Result를 inout parameter로 사용???한다. - inout이 argument label이 아닌가?

            ```swift
            public func reduce<Result>(into initialResult: Result, _ updateAccumulatingResult: (inout Result, Element) throws -> ()) rethrows -> Result

            // 1. 첫번째 parameter initialResult를 통해 초기값을 지정한다.
            // 2. 두번째 parameter updateAccumulatingResult를 통해 클로저를 전달받는다. 
            //    이 클로저의 첫번째 parameter Result를 inout parameter로 사용???한다. *inout parameter를 사용하여 초기값에 직접 연산을 실행한다.
            //    Result는 1의 초기값 또는 '이전에 실행된 클로저로 인해 변경된 결과값'이다. 모든 순회가 끝나면, reduce의 최종 결과값이 된다.
            //    이 클로저의 두번째 parameter Element는 reduce 메서드가 순환하는 컨테이너의 element이다.
            // 이 형태의 reduce는 map과 유사하게 사용 가능하다.
            ```

            ```swift
            let numbers: [Int] = [1,2,3]

            // +
            var sum2: Int = numbers.reduce(into: 0, { (result: inout Int, next: Int) in
                print("\(result) + \(next)") // 0 + 1, 1 + 2, 3 + 3 (이전 클로저 결과값을 반환하지 않고, 내부에서 직접 이전 값을 변경함)
                result += next // (형태-1) return result + next 과 다름
            })
            print(sum) // 6

            // -
            var subtract2: Int = numbers.reduce(into: 3, {
                print("\($0) - \($1)") // 3 - 1, 2 - 2, 0 - 3
                $0 -= $1
            })
            print(subtract2) // -3

            // 다른 컨테이너에 값을 변경하여 넣어줄 수 있다. map 또는 filter와 유사한 형태로 사용 가능하다.
            // numbers = [1,2,3] 중에서 홀수는 필터링하고, 짝수는 2배로 변경하여 배열에 직접 연산한다.
            var doubledNumbers: [Int] = numbers.reduce(into: [1,2]) { (result: inout [Int], next: Int) in
                print("result: \(result), next: \(next)") // 1. result: [1,2], next: 1 / 3. result: [1, 2], next: 2 / 6. result: [1, 2, 4], next: 3
                
                guard next.isMultiple(of: 2) else { // Returns true if this value is a multiple of the given value.
                    return // 2. 여기서 next: 1은 return 된다. / 7. next: 3은 return 된다.
                }
                
                print("\(result) append \(next) * 2") // 4. result: [1, 2], next: 2 가 전달되어 [1, 2] append 2 * 2 출력
                
                result.append(next * 2) // 5. append 실행 이후 result = [1, 2, 4]
            }
            // 차례로는
            // result: [1, 2], next: 1
            // result: [1, 2], next: 2
            // [1, 2] append 2 * 2
            // result: [1, 2, 4], next: 3
            print(doubledNumbers) // [1, 2, 4]

            // filter 및 map을 사용 (위와 동일한 결과)
            var doubledNumbers2: [Int] = [1,2] + numbers.filter{ $0.isMultiple(of: 2) }.map{ $0 * 2 }
            print(doubledNumbers2) // [1, 2, 4]

            // 참고 - map, filter, reduce 연계 사용
            let numbers2: [Int] = [1,2,3,4,5,6,7]

            var result: Int = numbers2.filter{ $0.isMultiple(of: 2) }.map{$0 * 3}.reduce(0, {$0 + $1})
            // 짝수인 2,4,6만 3배하여 총합을 구한다. 6+12+18 = 36
            print(result) // 36
            ```

        ```swift
        // 기존 데이터 
        let someNumbers: [Int] = [2, 8, 15]

        // 방법1 - for 문 사용
        var result: Int = 0  // reduce와 달리 let 선언 불가

        for number in someNumbers {
            result += number
        }
        print(result) // 25

        // 방법2 - reduce 메서드 사용
        // 초기값이 0 이고 someNumbers 내부의 모든 값을 더합니다.
        let sum: Int = someNumbers.reduce(0, { (result: Int, currentItem: Int) -> Int in   // reduce()의 parameter 자리에 초기값, closure가 들어간다!
         // print("\(result) + \(currentItem)")  
            return result + currentItem
        })
        //0 + 2   // 해당 결과값이 다음 line의 left(result) 값으로 들어간다! (최초 left는 초기값)
        //2 + 8   // 해당 결과값이 다음 line의 left(result) 값으로 들어간다!
        //10 + 15 // 최종 값을 return 한다
        print(sum)  // 25

        // 방법3 - reduce + closure 표현방법 (매개변수, 반환 타입, return 생략, 후행 클로저)
        // 초깃값이 3이고 someNumbers 내부의 모든 값을 더합니다.
        let sumFromThree = someNumbers.reduce(3) { $0 + $1 }
        print(sumFromThree) // 28

        // 방법4
        let sumFast = someNumbers.reduce(0,+)
        // 연산자는 중위 연산자로 왼쪽 값이 $0, 오른쪽 값이 $1임을 추론 가능하므로 생략 가능하다.
        ```

        - [x]  let sum: Int = someNumbers.reduce(0, { (first: Int, second: Int) -> Int in
            //print("\(first) + \(second)") //어떻게 동작하는지 확인해보세요
            - //0 + 2   // 해당 결과값이 다음 line의 left 값으로 들어간다! (최초 left는 초기값)
            //2 + 8   // 해당 결과값이 다음 line의 left 값으로 들어간다!
            //10 + 15 // 최종 값을 return 한다
- Quiz. 구조체 Friend

    ```swift
    struct Friend {
        let name: String
        let gender: Gender
        let location: String
        var age: UInt
    }

    enum Gender {
        case male, female, unknown
    }

    var friends: [Friend] = [Friend]() // 구조체 Friend type의 Array를 선언

    friends.append(Friend(name: "kevin", gender: .female, location: "서울", age: 26))
    friends.append(Friend(name: "sam", gender: .male, location: "시드니", age: 20))
    friends.append(Friend(name: "mike", gender: .male, location: "스톡홀름", age: 30))

    // Quiz. 구현 - 작년 정보인 friends array를 활용하여, 현재 기준 "서울 외에 거주하는 25세 이상의 친구"를 찾는다. (String 형태로 꺼내어 출력한다.)
    ```

    - Answer
        - Answer-1

            ```swift
            var filteredFriends: [Friend] = friends.filter{ $0.age >= 24 }.filter{ $0.location != "서울" } // *** $0에 Array의 element가 차례로 전달된다.
            print(filteredFriends) // [__lldb_expr_78.Friend(name: "mike", gender: __lldb_expr_78.Gender.male, location: "스톡홀름", age: 30)]
            ```

        - Answer-2

            ```swift
            // map - element를 하나씩 꺼내어 새로운 Array에 넣는다. (age만 +1 한다.)
            var result: [Friend] = friends.map{ Friend(name: $0.name, gender: $0.gender, location: $0.location, age: $0.age + 1) }

            result = result.filter{ $0.location != "서울" && $0.age >= 25 } // filter 내 조건에 && 사용 가능

            // reduce - String type으로 꺼내어 출력한다.
            let string1: String = result.reduce("서울 외에 거주하는 25세 이상의 친구 : ") {
                $0 + "\($1.name) \($1.gender) \($1.location) \($1.age)세"
            }
            print(string1) // 서울 외에 거주하는 25세 이상의 친구 : mike male 스톡홀름 31세
            ```

# 23. Access Control (접근 제어)

- 특징
    - 코드끼리 상호작용할 때 파일 간 또는 모듈 간에 접근을 제한하는 기능이다. 코드의 상세구현은 숨기고, 허용된 기능만 사용하는 interface를 제공한다.
    - 외부에서 접근하면 안되는 코드가 있을 때, 캡슐화 및 은닉화를 구현한다. (객체지향 프로그래밍에서 중요한 개념)
        - 은닉화 (Hiding) : 중요 사항이 외부로 드러나지 않게 감추는 것
        - 캡슐화 (Encapsulation) : 중요 사항을 감춘 상태에서 외부에서 그것을 사용할 수 있는 방법을 제공하고 외부와 소통하는 것
    - Swift는 모듈 및 소스파일을 기반으로 접근제어를 설계한다.
        - 모듈 (Module) : 배포할 코드의 묶음 단위이다. 보통 하나의 프레임워크 또는 라이브러리 또는 애플리케이션이 모듈 단위가 된다. (import 키워드로 불러옴)
        - 소스파일 : 하나의 Swift 소스코드 파일이다.
- 접근 제어는 접근수준 (Access Level)으로 구현한다.
    - 접근수준 (Access Level)의 종류
        - 타입 또는 타입 내부의 프로퍼티, 메서드, 이니셜라이저, 서브스크립트 각각에 접근수준 지정이 가능하다. (각 타입 또는 요소 앞에 키워드를 명시)
        - 접근수준 키워드 종류 (접근도 높은 순으로)
            1. open 개방 - 모듈 외부까지 (Class, Class 멤버에만 사용)
            - *public과의 차이점 : Class를 다른 모듈에서도 부모클래스로 사용할 목적으로 사용한다. (ex. Foundation 프레임워크에 정의된 NSString Class)
                - open 제외 모든 접근수준의 Class/Class 멤버는 해당 Class/Class 멤버가 정의된 모듈 안에서만 상속, override 가능하다.
                - open의 Class/Class 멤버는 해당 Class/Class 멤버가 정의된 모듈 외부의 다른 모듈에서도 상속, override 가능하다.
            2. public 공개 - 모듈 외부까지
            - 자신이 구현된 소스파일, 해당 소스파일이 소속된 모듈, 해당 모듈을 import한 모듈 등 모든 곳에서 사용 가능하다.
            - 주로 프레임워크에서 외부와 연결될 interface 구현에 사용한다. ??? (ex. Bool type 등 Swift의 기본 요소)
            3. internal 내부 - 모듈 내부
            - 모든 요소에 암묵적으로 지정하는 default 접근수준이다.
            - 소스파일이 소속된 모듈 내부에서 사용 가능하다. 단, 해당 모듈을 import한 외부 모듈에서는 접근 불가하다. (모듈 내부에서 광역적으로 사용할 경우 사용한다.)
            4. fileprivate 파일외부 비공개 - 파일 내부 
            - 해당 요소가 구현된 소스파일 내부에서만 사용 가능하다.
            - 해당 소스파일 외부에서 값이 변경되거나 함수를 호출하면 부작용이 생길 가능성이 있을 때 사용한다.
            5. private 비공개 - 기능 정의 내부
            - 해당 기능을 정의 및 구현한 범위 내에서만 사용한다. (해당 소스파일 내부에 구현한 다른 타입이나 기능에서 사용 불가하다.)
        - 하위 요소는 상위 요소보다 더 높은 접근수준을 가질 수 없다. ⇒ 상위는 하위보다 높아야 함 (*하위가 private이면 상위도 private이어야 함)
            - Struct/Class : 메서드, 프로퍼티는 Struct/Class보다 더 높은 접근수준을 가질 수 없다. (상위 Class-높음 public, 하위 메서드-낮음 private이면 가능하다.)
            - 함수 : 함수의 parameter로 접근수준이 부여된 type이 전달/반환되면, 함수는 parameter type보다 더 높은 접근수준을 가질 수 없다. (상위 parameter-높음 public, 하위 함수-낮음 private이면 가능하다.) 
            (상위 함수-높음, 하위 parameter-낮음이면 가능하다. X)
            - Tuple : Tuple은 Tuple의 내부 요소 type보다 더 높은 접근수준을 가질 수 없다. (상위 Tuple 내부요소-높음 public, 하위 Tuple-낮음 private이면 가능하다.)
                - [ ]  (상위 parameter-높음 public, 하위 함수-낮음 private이면 가능하다. O) (상위 함수-높음, 하위 parameter-낮음이면 가능하다. X)
                왜 함수, Tuple이 상위가 아니지??
            - Enum : 각 case는 개별 지정이 불가하며 Enum의 접근수준을 따른다. Enum은 rawValue 및 연관값 type보다 더 높은 수준을 가질 수 없다. (상위 rawValue, 연관값-높음 public, 하위 Enum-낮음 private이면 가능하다.)
                - [ ]  enum Point: PointValue { // *rawValue type이 private이므로 enum도 private이어야 함 (왜 fileprivate도 가능하지???)

            ```swift
            // 옳은 예
            public class OpenClass {
                    private func secretMethod() { // 상위 Class-높음 public, 하위 메서드-낮음 private이면 가능하다. 
                    }
            }

            private func openFuction2(b: OpenClass) -> OpenClass { // 상위 parameter-높음 public, 하위 함수-낮음 private이면 가능하다. 
                    return b
            }

            public func openFuction(a: OpenClass) -> OpenClass { // 동일한 수준도 가능
                    return a
            }
            ```

            ```swift
            // 잘못된 예
            private class SecretClass { // *Class가 private이므로 메서드도 private 이어야 함
            		public func openMethod() { // 잘못된 접근수준 - Class가 private이므로 method도 private 수준으로 취급된다. (에러가 발생하지는 않음) 
            		}
            }

            /*
            public func openFuction(a: SecretClass) -> SecretClass { // 잘못된 접근수준. 에러 발생 - Function cannot be declared public bc its parameter uses a private type.
            		return a // *parameter가 private이므로 함수도 private 이어야 함
            }
            */
            ```

            ```swift
            // Tuple

            internal class InternalClass {}
            private struct PrivateStruct {}

            private var publicTuple: (first: InternalClass, second: PrivateStruct) = (InternalClass(), PrivateStruct()) // 상위 Tuple 내부요소-높음 private, 하위 Tuple-높음 private 동일하므로 가능하다.

            // public var publicTuple: (first: InternalClass, second: PrivateStruct) = (InternalClass(), PrivateStruct()) // 잘못된 접근수준. 에러 발생 - Variable cannot be declared public because its type uses a private type
            // *Tuple의 내부요소가 private이므로 Tuple도 private 이어야 함

            ```

            ```swift
            // AClass.swift 파일과 Common.swift 파일이 동일한 모듈에 소속된 경우

            // AClass.swift 파일
            class AClass {
                func internalMethod() {}
                var internalProperty = 0
                
                fileprivate func filePrivateMethod() {}
                fileprivate var filePrivateProperty = 0
            }

            // Common.swift 파일
            let aInstance: AClass = AClass()
            aInstance.internalMethod()     // 동일한 모듈이므로 호출 가능
            aInstance.internalProperty = 1 // 동일한 모듈이므로 접근 가능

            aInstance.filePrivateMethod()     // 오류 발생. 다른 파일이므로 호출 불가
            aInstance.filePrivateProperty = 1 // 오류 발생. 다른 파일이므로 호출 불가

            // 따라서 프레임워크를 만들 때는 다른 모듈에서 특정 기능에 접근 가능하도록 API로 사용할 기능을 public으로 지정해야 함. 그 외 요소는 internal 또는 private 등으로 처리함
            ```

            ```swift
            private typealias PointValue = Int

            /*
            enum Point: PointValue { // (enum type은 default인 internal) 잘못된 접근수준. 에러 발생 - Enum must be declared private or fileprivate bc its raw type uses a private type
                case x, y            // *rawValue type이 private이므로 enum도 private이어야 함 (왜 fileprivate도 가능하지???)
            }
            */ 

            private enum Point: PointValue { // 가능
                case x, y
            }
            ```

    - private / fileprivate
    - 원래 private은 같은 파일 내부에 있어도 해당 기능 정의 내에서만 접근 가능하다. 
       단, 동일한 type의 Extension에서는 private 요소에 접근 가능하다.

        ```swift
        public struct AType {
            private var privateVariable = 0
            fileprivate var fileprivateVariable = 0
        }

        // 원래 private은 같은 파일 내부에 있어도 해당 기능 구현 내에서만 접근 가능하다. (fileprivate는 동일한 파일 내에서 접근 가능)
        // 단, 동일한 type의 Extension에서는 private 요소에 접근 가능하다.
        extension AType {
            public func publicMethod() {
                print("\(self.privateVariable), \(self.fileprivateVariable)")
            }
            
            private func privateMethod() {
                print("\(self.privateVariable), \(self.fileprivateVariable)")
            }
            
            fileprivate func fileprivateMethod() {
                print("\(self.privateVariable), \(self.fileprivateVariable)")
            }
        }

        struct BType {
            var aInstance: AType = AType() // AType의 인스턴스 생성
            
            mutating func someMethod() {
                self.aInstance.publicMethod() // (1) 0, 0 출력 - public 접근수준은 어디에서든 접근 가능
                
                // 동일한 파일 내부에 소속된 코드이므로 fileprivate 접근수준 요소에 접근 가능
                self.aInstance.fileprivateVariable = 100
                self.aInstance.fileprivateMethod() // (2) 0, 100 출력
                
                // 다른 타입 내부의 코드 (기능 정의 범위의 외부)이므로 private 요소에 접근 불가
        //      self.aInstance.privateVariable = 100 // 에러 발생 - 'privateVariable' is inaccessible due to 'private' protection level
        //      self.aInstance.privateMethod() // 에러 발생
            }
        }

        var bInstance: BType = BType()
        bInstance.someMethod() // (1) 0, 0, (2) 0, 100 출력
        ```

- 읽기 전용 구현
    - 설정자 (Setter)만 더 낮은 접근수준을 갖도록 제한할 수 있다. 접근수준 키워드 뒤에 (set)으로 표현한다. (ex. private(set))
    - 구조체, 클래스를 사용하여 저장 프로퍼티를 구현할 때 허용된 접근수준에서 프로퍼티 값을 가져갈 수 있는데 (읽기 get 가능), 값 변경이 불가하도록 (쓰기 set 불가) 설정된다.
    - 서브스크립트가 읽기 전용으로 제한된다.
    - 프로퍼티, 서브스크립트, 변수 등에 적용 가능하다. 해당 요소보다 같거나 낮은 수준으로 접근수준을 제한할 수 있다.
        - [ ]  서브스크립트? 하고나서 다시

        ```swift
        public struct aType {
            private var count: Int = 0
            
            public var publicStoredProperty: Int = 0
            
            // setter는 private 접근수준으로 설정
            public private(set) var publicGetOnlyStoredProperty: Int = 0
            
            internal var internalComputedProperty: Int {
                get {
                    return count
                }
                set {
                    count += 1
                }
            }
            
            // setter는 private 접근수준으로 설정
            internal private(set) var internalGetOnlyComputedProperty: Int {
                get {
                    return count
                }
                set {
                    count += 1
                }
            }
            
            public subscript() -> Int {
                get {
                    return count
                }
                set {
                    count += 1
                }
            }
            
            // setter는 internal 접근수준으로 설정
            public internal(set) subscript(some: Int) -> Int {
                get {
                    return count
                }
                set {
                    count += 1
                }
            }
        }

        var aInstance: aType = aType()

        print(aInstance.publicStoredProperty) // 0 출력 - 외부에서 getter, setter 모두 사용 가능
        aInstance.publicStoredProperty = 100

        print(aInstance.publicGetOnlyStoredProperty) // 0 출력 - 외부에서 getter만 사용 가능
        // aInstance.publicGetOnlyStoredProperty = 100 // 오류 발생 - Cannot assign to property 'publicGetOnlyStoredProperty' setter is inaccessible

        print(aInstance.internalComputedProperty) // 0 출력 - 외부에서 getter, setter 모두 사용 가능
        aInstance.internalComputedProperty = 100

        print(aInstance.internalGetOnlyComputedProperty) // 1 출력 - 외부에서 getter만 사용 가능
        //aInstance.internalGetOnlyComputedProperty = 100 // 오류 발생

        print(aInstance[]) // 1 출력 - 외부에서 getter, setter 모두 사용 가능
        aInstance[] = 100

        print(aInstance[0]) // 2 출력 - 외부에서 getter만, 동일한 모듈 내에서는 setter도 사용 가능
        aInstance[0] = 100
        ```

# 24. Monad

- Monad (모나드) : 순서가 있는 연산을 처리할 때 자주 활용하는 디자인 패턴 또는 자료구조이다. (모나딕 type, 모나드 함수) 특정 기능이 아니다.
- 수학의 /범주론/ 개념을 차용했다.
- 모나드 조건
    1. type을 인자로 받는 type 이다. (특정 type의 값을 포장한다.) ← type을 인자로 받는 : 제네릭 기능으로 구현
    2. 특정 type의 값을 포장한 것을 반환하는 함수가 존재한다.
    3. 포장된 값을 변환하여 동일한 형태로 포장하는 함수가 존재한다.
- 모나드 개념
    - 컨텍스트 (context) : 콘텐츠를 담고 있는 그릇이다. 또한 다른 type의 값들을 담을 수 있는 컨테이너 (Struct, Array 등???)는 컨텍스트의 역할을 수행 가능하다.
        - ex. 배추를 랩으로 포장해서, 락앤락으로 포장해서, 냉장고에 포장했다. → 콘텐츠=배추 / 3중 컨테이너 / 가장 바깥의 컨텍스트는 냉장고
    - 함수객체 (Functor) : map 함수를 적용 가능한 컨테이너 type이다. 즉, Array, Dictionary, Set 등 많은 Collection type은 함수객체이다.
        - 닫힌 함수객체 (Endofunctor) : 자신의 컨텍스트와 동일한 형태로 포장 (맵핑)할 수 있는 함수객체이다.
- 모나드 예
    - 옵셔널
        - 조건-1. 옵셔널은 Wrapped type을 인자로 받는 제네릭 type이다. (값이 있을지 없을지 모르는 상태를 포장하는 것이다. nil 가능성이 있는 상태를 포장하는 것이다.)
        - 조건-2. Optional<Int>.init(2) 처럼 다른 타입 (Int)의 값을 갖는 상태의 컨텍스트를 생성 가능하다. ???
        옵셔널은 Enum으로 구현되어 있다. (연관값을 통해 인스턴스 내부에 연관값을 갖는다.) 옵셔널에 값이 있으면 .some(value) case이고, 값이 없으면 .none case이다.
        → optional(2) : 컨텍스트 안에 2라는 콘텐츠가 들어있는 형태이다. "컨텍스트는 2라는 값을 가지고 있다."
            값이 없는 optional : "컨텍스트는 존재하지만, 내부에 값이 없다."
        - 조건-3. 옵셔널은 map 함수를 적용가능하다. 즉, 옵셔널은 함수객체이다.
        map 함수는 컨테이너의 값을 변형시킬 수 있는 고차함수이다. `x.map(클로저)` `x.map(함수)` 형태이다.
        (컨테이너가 담고 있는 값들을 *변형하고, 새로운 컨테이너를 생성/반환한다. - *(map이 함수를 parameter로 받는다.) 컨테이너의 값을 parameter로 받은 함수에 적용하여 변형함)
        옵셔널은 컨테이너와 값을 갖기 때문에 map 함수를 사용 가능하다.
        *map 함수를 사용하면 옵셔널을 일반 값처럼 연산 가능하다.

            ```swift
            func addThree(_num: Int) -> Int {
            		return num + 3
            }

            // 일반적인 연산
            addThree(2) // 5
            addThree(Optional(2)) // int type과 int? type은 다르므로 오류 발생

            // 옵셔널 연산이 가능해짐
            optional(2).map(addThree) // Optional(5) 출력 - *map 함수를 통해 옵셔널 연산이 가능해진 addThree 함수 (quick view에는 5라고 뜸)

            var check: Any? = Optional(2).map(addThree) // 확인용
            print(check) // Optional(5)
            ```

            ```swift
            // map 함수 및 클로저를 사용한 옵셔널 연산
            var value: Int? = 2  // Optional(2)
            value.map{ $0 + 3 } // 5
            check = value.map{ $0 + 3 }
            print(check) // Optional(5)

            value = nil
            value.map{ $0 + 3 } // nil
            check = value.map{ $0 + 3 }
            print(check) // nil (== Optional<Int>.none
            ```

            - 함수객체와 map 함수의 동작 모식도

                `map(a→b)` ⇒ `fa()` ⇒ `fb()`

                1. map이 함수를 인자로 받는다. (ex. addThree(_:)) 
                2. 함수객체에 map이 전달받은 함수를 적용한다. (ex. Optional(2))
                3. 새로운 함수객체를 반환한다. (ex. Optional(5))
            - 옵셔널의 map 함수 구현

                ```swift
                extension Optional {
                    func map<U>(f: (Wrapped) -> U) -> U? {
                        switch self {  // 옵셔널의 map 함수를 호출하면, 옵셔널 스스로 값이 있는지 switch문을 통해 판단한다.
                        case .some(let x): return f(x)  // 옵셔널에 값이 있으면, 전달받은 함수에 자신의 값을 적용한 결과값을 다시 컨텍스트에 넣어 반환한다.
                        case .none: return .none  // 옵셔널에 값이 없으면, 함수를 실행하지 않고 빈 컨텍스트를 반환한다.
                        }
                    }
                }
                ```

                - Optional(2).map(addThree)의 동작 모식도
                    1. 컨텍스트로부터 값을 추출한다.  Optional(2) → 2
                    2. 추출한 값에 전달받은 함수를 적용한다. addThree(2) = 5
                    3. 결과값을 다시 컨텍스트에 담아 반환한다. Optional(5)
                - 값이 없는 옵셔널 Optional.none.map(addThree)의 동작 모식도
                    1. 컨텍스트로부터 값을 추출하려는데, 값이 없다. Optional(nil)
                    2. 함수를 적용하지 않는다.
                    3. 그대로 빈 컨텍스트를 반환한다. nil

- 모나드
    - 모나드는 닫힌 함수객체이다.
    - 따라서 map 함수를 적용 가능하다. 이 맵핑의 결과가 함수객체와 동일한 컨텍스트를 반환하는 함수객체를 모나드라고 한다.
    - 이러한 맵핑을 수행하도록 flatMap 메서드를 활용한다.
        - flatMap 활용
            - map과 같이 함수를 매개변수로 받는다. 옵셔널은 모나드이므로 flatMap을 사용 가능하다.
            - flatMap은 컨텍스트 내부의 컨텍스트를 모두 같은 위상으로 평평하게 펴준다. (포장된 값 내부의 포장된 값을 풀어서 같은 위상으로 펴준다.)
            - 옵셔널과 관련하여 여러 컨테이너 값을 연쇄 처리할 때, 바인딩을 통해 Chain 형식으로 사용 가능하므로 flatMap이 map보다 유용할 수 있다.

                ```swift
                func doubledEven(_ num: Int) -> Int? { // return nil 가능성이 있으므로 return type이 Int? 이다.
                    if num.isMultiple(of: 2) {
                        return num * 2
                    }
                    return nil
                }

                Optional(3).map(doubledEven) // nil
                Optional(2).map(doubledEven) // Optional(Optional(4)) - 컨텍스트로부터 추출한 값 2를 함수에 전달한다. 함수의 결과값인 Optional(4)를 본래 컨텍스트 Optional에 담아서 Optional(Optional(4))이 된다.

                Optional(3).flatMap(doubledEven) // nil
                Optional(2).flatMap(doubledEven) // Optional(4) - *flatMap 및 map의 차이이다. flatMap은 컨텍스트 내부의 컨텍스트를 모두 같은 위상으로 평평하게 펴준다.
                ```

                - Optional(3).flatMap(doubledEven)의 동작
                    1. 컨텍스트로부터 값을 추출한다. Optional(3) → 3
                    2. 추출한 값을 함수에 전달한다. (추출한 값에 전달받은 함수를 적용한다.) doubledEven(3)
                    3. 결과값이 nil이므로 빈 컨텍스트를 반환한다. Optional(nil)
                - 값이 없는 옵셔널 Optional.none.flatMap(doubledEven의 동작
                    1. 빈 컨텍스트이다. (컨텍스트로부터 값을 추출하려는데, 값이 없다.) Optional(nil)
                    2. flatMap은 아무 일도 하지 않는다. (함수를 적용하지 않는다.)
                    3. 그대로 빈 컨텍스트를 반환한다.
            - flatMap을 Chain 형식으로 구현
            - flatMap은 항상 동일한 컨텍스트를 유지할 수 있으므로 연쇄 처리가 가능하다.
                - Int ↔ String

                    ```swift
                    func stringToInt(_ string: String) -> Int? { // String->Int : type 전환 실패 가능성이 있으므로 return type이 Optional이다.
                        return Int(string)
                    }

                    func intToString(_ int: Int) -> String? { // Int->String : 실패 가능성이 없지만 예시를 들고자 Optional로 지정했다.
                        return "\(int)"
                    }

                    var optionalString: String? = "2"
                    print(optionalString) // Optional("2")

                    // flatMap
                    let flatResult = optionalString.flatMap(stringToInt).flatMap(intToString).flatMap(stringToInt) // Chain 형식
                    print(flatResult) // Optional(2) - Optional로 포장된 값("2")를 추출하여, 함수로 처리하고("2"->2->"2"->2), 결과값을 본래 컨텍스트인 Optional로 포장한다. (2->Optional(2))

                    // map
                    let mappedResult = optionalString.map(stringToInt) // Chain 형태로 구현 불가
                    print(mappedResult) // Optional(Optional(2))
                    // let mappedResult2 = optionalString.map(stringToInt)?.map(intToString()?.map(stringToInt) // 컴파일 에러 발생 - Circular reference
                    ```

                - Chaining 도중에 연산에 실패하거나 값이 없어지는 경우 (.none OR nil) 별도의 예외처리 없이 빈 컨테이너를 반환한다.
                - 옵셔널 체이닝과 동일하게 동작한다. 옵셔널이 모나드이기 때문이다.

                    ```swift
                    func intToNil(param: Int) -> String? {
                        return nil
                    }

                    optionalString = "2"

                    var result1 = optionalString.flatMap(stringToInt).flatMap(intToNil)
                    var result2 = optionalString.flatMap(stringToInt).flatMap(intToNil).flatMap(stringToInt)

                    print(result1) // nil
                    print(result2) // nil
                    ```

        - flatMap 정의

            ```swift
            func map<U>(_ transform: (Wrapped) throws -> U) rethrows -> U? 

            func flatMap<U>(_ transform: (Wrapped) throws -> U?) rethrows -> U?
            ```

            ```swift
            func map<U>(_ transform: (Wrapped) throws -> U) rethrows -> U? 
            // map에서 전달받는 함수 transform은 포장된 값을 매개변수로 갖고 U를 반환한다. 최종 type은 U?이다.

            		// 예시
            		func stringToInt(_ string: String) -> Int? {  // 1) U == Int? 이다. 2) rethrows -> U? 이므로 최종 return type은 Int?? 이다. (U == Int? 이므로 U? == Int??)
            		    return Int(string)
            		}
            		

            func flatMap<U>(_ transform: (Wrapped) throws -> U?) rethrows -> U?
            // map에서 전달받는 함수 transform은 포장된 값을 매개변수로 갖고, U?를 반환한다. 최종 type은 U?이다.

            		// 예시
            		func stringToInt(_ string: String) -> Int? {  // 1) U? == Int? 이다. 2) 최종 return type은 Int? 이다. (U? == Int? 이므로 U? == Int?)
            		    return Int(string)
            		}
            ```

    - Sequence type이 Optional type의 Element를 포장한 경우, flatMap을 compactMap으로 사용한다.
        - [ ]  Sequence?
            - Sequence Protocol : A type that provides sequential, iterated access to its elements. 순차적 나열이 가능한 type이라는 의미의 프로토콜이다.

                ```swift
                protocol Sequence {
                    associatedtype Iterator : IteratorProtocol where Iterator.Element == Element
                    func makeIterator() -> Iterator
                }
                // Sequence 프로토콜은 2개의 요소로 구성됩니다. 
                // 1) 우선 Iterator라는 associated type이 있습니다. 이 associated type은 IteratorProtocol을 준수(conform)하고 있습니다. 
                // 2) 다른 하나는 Iterator를 만드는 makeIterator라는 함수이며, 앞서 선언한 Iterator 타입을 반환합니다.

                struct MyRange: SequenceType { 
                    let start: Int
                    let end: Int
                    
                    // 1
                    typealias Generator = MyRangeGenerator
                    
                    init(start: Int, end: Int) {
                        self.start = start
                        self.end = end
                    }
                    
                    // 2
                    func generate() -> MyRange.Generator { // SequenceType은 이 GeneratorType 프로토콜을 따르는 인스턴스를 반환하는 것이 목표이다.
                        return MyRangeGenerator(start: self.start, end: self.end)
                    }
                }
                ```

            - Sequence->Collection->Bidirectional Collection->Random Access Collection->Range Replaceable Collection
            - Sequence는 요소들의 목록이다. 한번만 interate 가능하다. (for-in loop, map 함수, filter 함수를 사용하여 iterate 가능하다.)
            - Array 타입을 사용할 때 Sequence가 대부분의 기능을 제공한다. 
            - Sequence를 이용하여 Linked List를 구현 가능하다.
            [https://academy.realm.io/kr/posts/try-swift-soroush-khanlou-sequence-collection/](https://academy.realm.io/kr/posts/try-swift-soroush-khanlou-sequence-collection/)
        - 이중 컨테이너 (compactMap)
        - compactMap을 통해 클로저를 실행하면, 알아서 내부 컨테이너 (Array로 포장된 값 내부의 Optional로 포장된 값)의 값을 추출한다.
            - [ ]  nil은 왜 자동삭제?

            ```swift
            let optionals: [Int?] = [1,2,nil,5]  // optionals는 이중 컨테이터 형태이다. (Array 내부에 Optional이 들어있고, 그 안에 값이 있다.)

            let mapped: [Int?] = optionals.map{ $0 }
            let compactedMapped1 = optionals.compactMap{ $0 } // [Int?]를 처리했는데 최종값이 [Int]가 된다.
            let compactedMapped2: [Int] = optionals.compactMap{ $0 } // [Int?]를 처리했는데 최종값이 [Int]가 된다.
            let compactedMapped3: [Int?] = optionals.compactMap{ $0 } // 참고 - 기존 컨테이너와 동일한 type으로 선언한 경우

            print(mapped) // [Optional(1), Optional(2), nil, Optional(5)] - Array로 포장된 값(Optional(1),Optional(2)...)을 추출하여 클로저에 전달 -> 결과값(Optional(1),Optional(2)...)을 하나씩 본래 컨텍스트인 Array로 포장한다. 
            print(compactedMapped1) // [1,2,5] - *Array로 포장된 Optional 내부의 값(1,2...)을 추출하여 -> 결과값(1,2...)을 하나씩 본래 컨텍스트인 Array로만 포장한다. (nil은 자동 삭제됐다.)
            print(compactedMapped2) // [1,2,5] 
            print(compactedMapped3) // [Optional(1), Optional(2), nil, Optional(5)] 
            ```

        - 삼중 컨테이너 (flatMap)

            ```swift
            let multipleContainer = [[1,2,Optional.none], [3,Optional.none], [4,5,Optional.none]]

            // 삼중
            let mappedMC = multipleContainer.map{ $0.map{ $0 } }
            let flatmappedMC = multipleContainer.flatMap{ $0.flatMap{ $0 } }
            // Array로 포장된 Optional 내부의 값(1,2...)을 추출하여 -> 결과값(1,2...)을 하나씩 본래 컨텍스트인 Array로만 포장한다.

            print(mappedMC) // [[Optional(1), Optional(2), nil], [Optional(3), nil], [Optional(4), Optional(5), nil]] - 처리 전과 동일하다.
            print(flatmappedMC) // [1,2,3,4,5] - nil은 자동 삭제

            // 중간과정 참고 - 이중
            let preMappedMC = multipleContainer.map{ $0 }
            let preFlatMappedMC = multipleContainer.flatMap{ $0 }

            print(preMappedMC) // [[Optional(1), Optional(2), nil], [Optional(3), nil], [Optional(4), Optional(5), nil]] - 처리 전과 동일하다.
            print(preFlatMappedMC) // [Optional(1), Optional(2), nil, Optional(3), nil, Optional(4), Optional(5), nil] - ***하나의 Array에 담겨있다.
            // Array로 포장된 Array 내부의 값(1,2,nil,3,nil,4,5,nil)을 추출하여 -> 결과값(1,2,nil,3,nil,4,5,nil)을 하나씩 본래 컨텍스트인 Array로만 포장한다.
            ```

# 25. Subscript

- L/G
    - Syntax

        ```swift
        struct TimesTable {
            let multiplier: Int

            subscript(index: Int) -> Int {
                return multiplier * index
            }
        }

        let threeTimesTable: TimesTable = TimesTable(multiplier: 3) // TimesTable의 인스턴스

        print("six times three is \(threeTimesTable[6])") // 인스턴스[index] 형태 - 서브스크립트에 index parameter로 6이 전달되고, multiplier(3)*6=18 연산 결과값 (threeTimesTable[6]에서 6번째 값???)인 18이 반환된다.
        // Prints "six times three is 18"
        ```

    - Usage
    - `Dictionary변수[key] = value`로 key:value를 추가 (numberOfLegs에 key=bird, value=2를 할당)하는 것은 서브스크립트 문법이다.
    - 참고 - 항상 Dictionary의 return type은 옵셔널이다. (특정 key 값이 없거나 nil일 수 있기 때문)

        ```swift
        var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]  // initializes numberOfLegs with a dictionary literal containing three key-value pairs.
        numberOfLegs["bird"] = 2  // key:value를 추가하기 위해 서브스크립트를 사용했다.
        ```

    - Subscript Option

        - parameter/return type에 따라 서브스크립트를 원하는 개수만큼 선언 가능하다.
        - 아래는 서브스크립트를 이용하여 2차원 행열을 선언 및 접근하는 example이다. (The Matrix structure’s subscript takes two integer parameters.)

        - [x]  grid = Array(repeating: 0.0, count: rows * columns)
            - Creating an Array with a Default Value

                ```swift
                var threeDoubles = Array(repeating: 0.0, count: 3)
                // threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0]
                ```

        - [ ]  var matrix = Matrix(rows: 2, columns: 2) // 서브스크립트 문법을 활용하여 2 * 2 행렬을 선언한다. - 그냥 구조체 프로퍼티를 통한 구조체 초기화 아닌가???
        - [ ]  matrix[0, 1] = 1.5 // (0*1)+1=1 이니까 1.0이 반환되어야 하는거 아닌가? → 행렬의 왼쪽 상단부터 순서대로 index 0,1,2,3 인가???

        ```swift
        struct Matrix { // Matrix=행렬
            let rows: Int, columns: Int
            var grid: [Double] // Double type의 Array

            init(rows: Int, columns: Int) {
                self.rows = rows
                self.columns = columns
                grid = Array(repeating: 0.0, count: rows * columns) // array’s size 및 initial value
            }

            func indexIsValid(row: Int, column: Int) -> Bool {
                return row >= 0 && row < rows && column >= 0 && column < columns
            }

            subscript(row: Int, column: Int) -> Double { // row, column 2개의 parameter를 받고, Double type을 반환하는 서브스크립트
                get {
                    assert(indexIsValid(row: row, column: column), "Index out of range") // 유효한 index가 아닌경우 프로그램이 바로 종료 되도록 assert를 호출
                    return grid[(row * columns) + column]  // grid Array의 해당 index에 들어있는 값을 반환한다.
                }
                set {
                    assert(indexIsValid(row: row, column: column), "Index out of range")
                    grid[(row * columns) + column] = newValue 
                }
            }
        }

        var matrix = Matrix(rows: 2, columns: 2) // 구조체 인스턴스 초기화. 서브스크립트 문법을 활용?????하여 2*2 행렬을 선언한다. - 그냥 구조체 프로퍼티를 통한  아닌가???
        // 초기화 과정에서 grid Array [0.0, 0.0, 0.0, 0.0]를 생성한다.

        // Values in the matrix can be set by passing row and column values into the subscript,
        matrix[0, 1] = 1.5 // 행렬의 값을 할당하기 위해 row/column 값을 서브스크립트에 전달한다. - (0*1)+1=1 이니까 grid[1]이 반환되어야 하는거 아닌가?
        matrix[1, 0] = 3.2 // (1*0)+2=2 이니까 grid[2]이 반환 ???
        ```

        - grid 배열은 서브스크립트에 의해 아래와 같이 row와 column을 갖는 행렬도 동작한다.

            ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2015.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2015.png)

        - row/column에 값을 할당한 결과이다.
            - [ ]  행렬의 왼쪽 상단부터 순서대로 index 0,1,2,3 인가???

            ![Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2016.png](Swift%20syntax%20db89af547ea44c838a71961695a68c01/Untitled%2016.png)

- 서브스크립트 (Subscript) : Collection, List, Sequence 등 타입의 element에 접근하는 '단축 문법'이다.
    - Class, Struct, Enum에 서브스크립트를 구현 가능하다.
    - 서브스크립트는 별도의 setter, getter 메서드 없이도 [index]를 통해 값을 설정/할당 가능하다. ex. `array1[index]`, `dictionary1[key]` 등으로 표현한다.
- 서브스크립트 문법
    - 서브스크립트는 `인스턴스[index]` 형태로 인스턴스 내부의 특정 값에 접근한다.
    - `subscript` 키워드를 통해 정의한다. parameter type/개수 및 return type을 지정하며, 읽고-쓰기 또는 읽기-전용으로 구현한다. (연산 프로퍼티 및 인스턴스 메서드의 syntax와 유사하다.)
    - 정의
    - 서브스크립트 정의 코드는 각 type의 구현부 또는 익스텐션 구현부에 위치한다.

        ```swift
        subscript(index: Int) -> Int {
        		get {
        		}

        		// 읽기전용이면 setter가 없다. 또한 get{}을 생략 가능하다.
        		set (newValue) {  // newValue의 type은 서브스크립트의 return type과 동일하다. (setter의 parameter를 지정하지 않으면, 암시적 전달인자 newValue를 사용한다. - 연산 프로퍼티와 동일)
        		}
        }
        ```

        - 참고 - 연산 프로퍼티 ☀️

            ```swift
            struct Student {
            		var koreanAge: Int = 0
            		    
            		var westernAge: Int {  
            				// read
            		    get { // 읽기전용은 get{}을 생략 가능하다.
            		        return koreanAge - 1  // koreanAge 값이 할당되는 즉시, 연산 결과값을 westernAge에 return 한다.
            		    }
            		    
            				// write
            		    set(newValue) {  // westernAge 새로운 값이 할당되는 즉시, 연산 결과값을 koreanAge에 할당한다.
            		        koreanAge = newValue + 1
            		    }
            		}
            }

            var yagom: Student = Student();
            yagom.koreanAge = 10 - koreanAge의 값이 할당되는 즉시, westernAge의 값을 지정한다. westernAge 입장에서 본인의 값을 get/read한다. (getter로 10-1 연산을 하고, 결과값이 westernAge의 값에 할당된다.)
            print(yagom.koreanAge) // 10
            print(yagom.westernAge) // 9 (getter 결과)

            yagom.westernAge = 30 - westernAge의 값이 할당되는 즉시, koreanAge의 값을 지정한다. westernAge 입장에서 남의 값을 set/write한다. (setter로 westernAge 본인 값을 newValue를 넣어서, 30+1 연산을 하고, 결과값이 koreanAge에 할당된다.)
            print(yagom.koreanAge) // 31 (setter 결과)
            print(yagom.westernAge) // 30
            ```

            ```swift
            // 읽기전용 - setter의 역할 참고
            struct Student {
            		var koreanAge: Int = 0
            		    
            		var westernAge: Int {  
            		    get { 
            		        return koreanAge - 1 
            		    }
            		}
            }

            var yagom: Student = Student();
            yagom.koreanAge = 10 
            print(yagom.koreanAge) // 10
            print(yagom.westernAge) // 9 (getter 결과)

            yagom.westernAge = 30 // setter가 없으므로 할당 불가하여 컴파일 에러 발생!!! - Cannot assign to property: 'westernAge' is a get-only property
            ```

- 서브스크립트 구현
    - 자신이 가지는 Collection, List, Sequence 등 타입의 element를 반환 및 설정한다.
    - 서브스크립트는 통상 1개 또는 여러 개의 parameter를 가진다. parameter type 및 return type은 제한이 없다.
    매개변수 기본값을 가질 수 있지만, 입출력 매개변수 (in-out parameter)는 가질 수 없다.
    - 서브스크립트 중복 정의 (Subscript Overloading) : 여러 서브스크립트를 1개 type에 구현하는 것이다.
    Class, Struct에 서브스크립트를 여러 개 구현하는 것이 가능하다. 외부에서 서브스크립트를 사용하는 경우, 서브스크립트 사용 시 전달한 값 type을 유추하여 적절한 서브스크립트를 실행한다.
        - [ ]  type 유추? 
        let aStudent: Student? = highSchool[1] // class School 인스턴스[index] 형태로 struct Student의 값에 접근한다. 
        - Case-1. highSchool.studentsList[1] - studentsList를 명시하지 않아도 type 유추를 통해 서브스크립트를 실행한 것???
        - Case-2. 서브스크립트가 type 유추를 통해 Int type인 totalNum를 선택하여 실행???
            - ???
            - 서브스크립트 중복 정의 (Subscript Overloading)에서
            - The appropriate subscript to be used will be inferred based on the types of the value or values that are contained within the subscript brackets at the point that the subscript is used.
            즉, 서브스크립트가 `인스턴스[index]` 형태로 사용되는 시점에서 index에 전달되는 값의 type에 따라 어떤 서브스크립트가 사용될 지 자동으로 유추된다.
        - [ ]  ***교재 - 학생번호(totalNum? OR ~~stNumber?~~)를 argument로 전달받아서 -> 자신의 studentsList의 index에 맞는 Student의 인스턴스 student를 반환한다.

        ```swift
        struct Student {
            var stName: String
            var stNumber: Int
        }

        class School {
            var totalNum: Int = 0
            var studentsList: [Student] = [Student]()
            
            func addStudent(addName: String) { // let student -> addStudent 함수가 호출/return 될 때까지 불변하므로 let 선언을 하나??? (+ 함수 3번 호출하면 각각 Stack에 쌓이니까 독립적이므로 let)
                let student: Student = Student(stName: addName, stNumber: self.totalNum) // struct Student 인스턴스를 생성 - 인스턴스 맞나???
                
                self.studentsList.append(student) // student 인스턴스를 studentsList Array에 추가
                self.totalNum += 1
                print(self.totalNum)
            }
            
            func addStudents(addNames: String...) {
                for name1 in addNames {
                    self.addStudent(addName: name1)
                }
            }
            
        		// 읽기전용 서브스크립트
            subscript(index: Int = 0) -> Student? { // 서브스크립트가 type 유추를 통해 Int type인 totalNum를 각각 선택하여 실행???
                if index < self.totalNum {
                    return self.studentsList[index] // *아래에서 Class School 인스턴스[index] 형태로 사용 -> studentsList Array에 접근 -> Struct Student의 인스턴스 student를 반환한다.
                }
                return nil
            }
        }

        let highSchool: School = School() // Class School 인스턴스 생성
        highSchool.addStudents(addNames: "kevin", "sam", "james") // 1) struct Student의 인스턴스인 student 생성, 2) studentsList Array에 student 인스턴스를 추가 (1 2 3 출력 - addStudent 함수를 3번 호출했으므로)
        // print(highSchool)

        print(highSchool.studentsList[0]) // Student(stName: "kevin", stNumber: 0) - studentsList Array에는 Student 인스턴스가 들어있다. 
        print(highSchool.studentsList[1]) // Student(stName: "sam", stNumber: 1)
        print(highSchool.studentsList[2]) // Student(stName: "james", stNumber: 2)
        print(highSchool.totalNum) // 3

        let aStudent: Student? = highSchool[1] // *School 인스턴스[index] 형태로 사용 -> studentsList Array에 접근 (type 유추) -> Student 인스턴스 student를 반환한다.
                              // highSchool.studentsList[1]로 명시하지 않아도 type 유추를 통해 studentsList를 선택하여 서브스크립트를 실행한 것이다. ???
        // ***교재 - 서브스크립트가 학생번호(totalNum? OR ~~stNumber?~~)를 argument로 전달받아서 -> 자신의 studentsList의 index에 맞는 -> Student 인스턴스 student를 반환한다.
        print("\(aStudent?.stNumber) \(aStudent?.stName)") // Optional(1) Optional("sam") 출력

        let bStudent: Student? = highSchool.studentsList[2] 
        print("\(bStudent?.stNumber) \(bStudent?.stName)") // Optional(2) Optional("james")

        print(highSchool[]?.stName) // Optional("kevin") - 매개변수 기본값 사용
        print(highSchool[0]?.stName) // Optional("kevin")
        print(highSchool[1]?.stName) // Optional("sam")
        print(highSchool[2]?.stName) // Optional("james")
        print(highSchool[3]?.stName) // nil

        ```

- 복수 서브스크립트
    - 다양한 parameter/return type의 여러 개의 subscript를 사용하여 다양한 목적에 따라 구현하는 데 용이하다.
        - [ ]  ???

        ```swift
        struct Student {
            var stName: String
            var stNumber: Int
        }

        class School {
            var totalNum: Int = 0
            var studentsList: [Student] = [Student]()
            
            func addStudent(addName: String) {
                let student: Student = Student(stName: addName, stNumber: self.totalNum) // struct Student 인스턴스를 생성
                
                self.studentsList.append(student)
                self.totalNum += 1
                print(self.totalNum)
            }
            func addStudents(addNames: String...) {
                for name1 in addNames {
                    self.addStudent(addName: name1)
                }
            }
            
            // 1번 서브스크립트 
            subscript(index: Int) -> Student? {
                get { // 본인의 값을 지정 - 위와 동일 (학생번호를 전달받아 해당 학생의 Student 인스턴스를 반환한다.)
                    if index < self.totalNum {
                        return self.studentsList[index] //*아래에서 Class School 인스턴스[index] 형태로 사용 -> studentsList Array에 접근 -> Struct Student의 인스턴스 student를 반환한다.
                    }
                    return nil
                }
                set { // 남의 값을 지정 - 특정 번호에 학생을 할당한다.
                    guard var newStudent1: Student = newValue else {
                        return
                    }
                    
                    var number1: Int = index
                    
                    if index > self.totalNum {
                        number1 = self.totalNum
                        self.totalNum += 1
                    }
                    
                    newStudent1.stNumber = number1
                    self.studentsList[number1] = newStudent1 // ???
                }
            }
            
            // 2번 서브스크립트
            subscript(name: String) -> Int? {
                get { // 학생이름을 전달받아 해당 학생이 있으면 학생번호를 반환한다.
                    return self.studentsList.filter{ $0.stName == name }.first?.stNumber // first 프로퍼티 - The first element of the collection
                }
                set { // 특정 이름의 학생을 해당 번호에 할당한다.
                    guard var number2: Int = newValue else {
                        return
                    }
                    
                    if number2 > self.totalNum {
                        number2 = self.totalNum
                        self.totalNum += 1
                    }
                    
                    let newStudent2: Student = Student(stName: name, stNumber: number2)
                    self.studentsList[number2] = newStudent2
                }
            }
            
            // 3번 서브스크립트
            subscript(name: String, number: Int) -> Student? { // 학생이름과 학생번호를 전달받아 해당 학생이 있으면 Student 인스턴스를 반환한다.
                return self.studentsList.filter{ $0.stName == name && $0.stNumber == number }.first
            }
        }

        let highSchool: School = School()
        highSchool.addStudents(addNames: "kevin", "sam", "james")

        // 1번 서브스크립트 getter가 사용됨
        var aStudent: Student? = highSchool[1] // class School 인스턴스[index] 형태로 struct Student의 값에 접근한다.
        print("\(aStudent?.stNumber) \(aStudent?.stName)") // Optional(1) Optional("sam") 출력

        highSchool[1] = Student(stName: "newYagom", stNumber: 100) // setter를 사용해보자?????
        aStudent = highSchool[1]
        print("New - \(aStudent?.stNumber) \(aStudent?.stName)") // New - Optional(1) Optional("newYagom")
        print(highSchool[1]!) // Student(stName: "newYagom", stNumber: 1)
        print(highSchool.studentsList[1]) // Student(stName: "newYagom", stNumber: 1) - 100이 아니다! -> setter 때문이지????

        // 2번 서브스크립트 getter가 사용됨
        print(highSchool["kevin"]) // Optional(0)
        print(highSchool["anSan"]) // nil

        // 3번 서브스크립트 getter가 사용됨
        highSchool[0] = Student(stName: "WhoIsThis", stNumber: 0)
        print(highSchool[0]!) // Student(stName: "WhoIsThis", stNumber: 0)

        print(highSchool["james", 2]!) // Student(stName: "james", stNumber: 2)
        // print(highSchool["No-james", 2]!) // nil - 컴파일 에러 발생

        // 2번 서브스크립트 setter가 사용됨 - newValue 1이 Int type이므로 (두번째 서브스크립트의 return type이 Int? 이다) 맞나???
        print(highSchool[1]!) // Student(stName: "newYagom", stNumber: 1)
        highSchool["Winner"] = 1 // setter
        print(highSchool[1]!) // Student(stName: "Winner", stNumber: 1)
        print(highSchool["newYagom"]) // nil - 값이 덮어씌워져서 삭제됨

        print(highSchool["Winner"]) // Optional(1)
        ```

- Type 서브스크립트
    - 인스턴스가 아니라 타입 자체에서 사용하는 서브스크립트이다.
    - static 키워드 또는 class 키워드 (Class인 경우)로 명시한다.
        - [x]  Self와 self의 차이
            - self : 해당 type의 인스턴스를 가르킨다.
            - Self : 해당 type 자체를 가르킨다.
            The Self type isn't a specific type, but rather lets you conveniently refer to the current type without repeating or knowing that type's name.

        ```swift
        enum School: Int {  // 원시값 지정한다고 명시
            case elementary = 1, middle, high, university
            
            static subscript(level: Int) -> School? {
                return Self(rawValue: level)  // self가 아님
                // return School(rawValue: level) 와 동일한 표현이다.
            }
        }

        let school: School? = School[2]  // Enum의 원시값을 index로 case에 접근한다.
        print(school!) // middle
        print(school) // Optional(__lldb_expr_207.School.middle)
        ```

        ```swift
        enum Planet: Int {
            case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
            
            static subscript(n: Int) -> Planet {
                return Planet(rawValue: n)!
            }
        }
        let planetA = Planet[4]
        print(planetA) // mars
        ```

        - 참고 - Enum rawValue
            - 원시값 할당은 Enum 선언 시에만 가능하다.
            - 원시값을 꺼낼 때는 `enum명.case명.rawValue`를 쓴다.

                ```swift
                // *Int type 원시값
                enum Fruit: Int {  // rawValue를 할당할 때는 타입을 지정해준다.
                    case apple = 0
                    case grape = 1  // 1을 지워도 자동으로 1이 할당된다.
                    case peach
                    case mango 
                }

                // raw value 값을 꺼낼 때는 enum명.case명.rawValue를 쓴다.
                print(Fruit.peach.rawValue)  // 2 출력

                // 참고 - rawValue를 지정한 경우 nil 가능성이 있으므로 Enum의 인스턴스는 항상 optional type이다. (실패가능한 이니셜라이저 참고)
                var fruitInstance: Fruit? = Fruit(rawValue: 0)
                print(fruitInstance) // optional(apple)
                var fruitInstanceImpossible: Fruit? = Fruit(rawValue: 10)
                print(fruitInstanceImpossible) // nil

                // *Int type 뿐만 아니라, Hashable 프로토콜을 따르는 모든 type을 원시값의 type으로 지정 가능하다.

                enum School: String {  // rawValue로 String 타입도 가능하다.
                    case elementary = "초등"
                    case middle = "중등"
                    case high = "고등"
                    case university
                }

                print(School.middle.rawValue)  // 중등 - 출력

                print(School.university.rawValue) // university - 출력
                // 열거형의 원시값 타입이 String일 때, 원시값이 지정되지 않았다면 "case명"을 원시값으로 사용함
                ```

# + 기타

- [ ]  
- 제네릭(Generics)
- ARC(Automatic Reference Counting)
- 중첩타입(Nested Types)
- 사용자정의 연산자(Custom Operators)
- 불명확 타입(Opaque Types)

- Contents-2
