# Swift Language Guide & Reference

Created: August 8, 2021 3:14 PM
Last Edited Time: October 1, 2021 4:55 AM
Property: Official

- Contents

# 🦜 Language Guide

# 1. The Basics (90%)

자세한 내용은 블로그에서 확인해주세요. [https://applecider2020.tistory.com/11](https://applecider2020.tistory.com/11)

## Numeric Type Conversion

The range of numbers that can be stored in an integer constant or variable is different for each numeric type. 

- An Int8 variable can store numbers between -128 and 127 (음수는 -128 ~ -1인 128개 (2^7), 나머지는 (0 및 양수) 0 ~ 127인 128개 (2^7)) → 총 256개 (2^8) 즉, 8비트 중에서 1비트는 부호 표현을 위해 사용되고, 남은 7비트로 숫자를 표현
- Whereas a UInt8 variable can store numbers between 0 and 255 (256개 (2^8)).

Because each numeric type can store a different range of values, you must opt in to numeric type conversion on a case-by-case basis. This opt-in approach prevents hidden conversion errors and helps make type conversion intentions explicit in your code.
*opt in to sth : ~에 동의/참여하기로 선택하다. opt out of sth : ~에 참여하지 않기로 선택하다.

To convert one specific number type to another, you initialize a new number of the desired type with the existing value. ('UInt16 type의 초기화'를 통해 새로운 number을 생성한 것이다.)

```swift
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + UInt16(one) 
// calls UInt16(one) to create a new UInt16 initialized with the value of one, and uses this value in place of the original. (원본 대신 이 값을 사용한다)
```

`SomeType(ofInitialValue)` is the default way to call the initializer of a Swift type and pass in an initial value.
Behind the scenes, UInt16 has an initializer that accepts a UInt8 value, ('UInt16 type의 이니셜라이저'를 통해 초기화했다. UInt8 type의 값을 전달받을 수 있는 것은 'UInt16 type의 이니셜라이저' 중에서 UInt8을 전달받는 것이 있기 때문이다.) and so this initializer is used to make a new UInt16 from an existing UInt8. 
You can’t pass in any type here, however—it has to be a type for which UInt16 provides an initializer. Extending existing types to provide initializers that accept new types (including your own type definitions) is covered in Extensions.

- ex. `init(_ source: Double)` - Creates an integer from the given floating-point value, rounding toward zero.
    
    ![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled.png)
    

# 2. Basic Operators (100%)

자세한 내용은 블로그에서 확인해주세요. [https://applecider2020.tistory.com/11](https://applecider2020.tistory.com/11)

# 3. Strings and Characters (90%)

Note: Swift’s String type is bridged with Foundation’s NSString class. Foundation also extends String to expose methods defined by NSString. This means, if you import Foundation, you can access those NSString methods on String without casting.

- [ ]  bridged

## String Literals

You can include predefined String values within your code as *string literals*. A string literal is a sequence of characters surrounded by double quotation marks (").

```swift
// Use a string literal as an initial value for a constant/variable:
let someString = "Some string literal value"
```

- Special Characters in String Literals
    
    An arbitrary Unicode scalar value, written as \u{n}, where n is a 1–8 digit hexadecimal number.
    
    ```swift
    let blackHeart = "\u{2665}"      // ♥,  Unicode scalar U+2665
    let sparklingHeart = "\u{1F496}" // 💖, Unicode scalar U+1F496
    ```
    
    Because multiline string literals use three double quotation marks ("""), you can include a double quotation mark (") inside of a multiline string literal without escaping it.
    To include the text """ in a multiline string, escape at least one of the quotation marks.
    
    ```swift
    let threeDoubleQuotationMarks = """
    Escaping the first quotation mark \"""
    Escaping all three quotation marks \"\"\"
    Just one or two quotation marks without escaping " ""
    """
    
    print(threeDoubleQuotationMarks)
    // Escaping the first quotation mark """
    // Escaping all three quotation marks """
    // Just one or two quotation marks without escaping " ""
    ```
    
- Extended String Delimiters (Delimiter : 구분자)
    
    You can place a string literal within extended delimiters to include special characters in a string without invoking their effect.
    You place your string within quotation marks (") and surround that with number signs (#).
    
    ```swift
    print(#"Line 1\nLine 2"#) // Line 1\nLine 2
    
    let threeMoreDoubleQuotationMarks = #"""
    Here are three more double quotes: """ // escaping 없이 특수문자를 사용 가능 (특수문자 효과 없이)
    """#
    
    print(threeDoubleQuotationMarks) // Here are three more double quotes: """
    ```
    

## Initializing an Empty String

빈 String을 생성하는 방법은 1) assign an empty string literal to a variable, 또는 2) initialize a new String instance with initializer syntax:

- [ ]  왜 String instance 라고 하지?????????????????

```swift
var emptyString = ""               // 1) empty string literal
var anotherEmptyString = String()  // 2) initializer syntax - empty 상태의 string type 이다.
```

## Unicode

### Extended Grapheme Clusters

*grapheme : 문자소 (의미를 나타내는 최소 문자 단위)

The letter é can be represented as the single Unicode scalar é (LATIN SMALL LETTER E WITH ACUTE, or U+00E9). However, the same letter can also be represented as a pair of scalars—a standard letter e (LATIN SMALL LETTER E, or U+0065), followed by the COMBINING ACUTE ACCENT scalar (U+0301). The COMBINING ACUTE ACCENT scalar is graphically applied to the scalar that precedes it, turning an e into an é when it’s rendered by a Unicode-aware text-rendering system. (Accent scalar가 그 앞의 scalar e에 적용되어 é로 변경된다.)

In both cases, the letter é is represented as a single Swift Character value that represents an extended grapheme cluster. (두 경우 모두, 문자 é는 확장된 그래프 분석 클러스터를 나타내는 단일 Swift 문자 값으로 표시된다.) In the first case, the cluster contains a single scalar; in the second case, it’s a cluster of two scalars: (첫 번째 경우, 클러스터는 단일 스칼라를 포함한다. 두 번째 경우, 클러스터는 2개의 스칼라로 구성된다.)

```swift
let eAcute: Character = "\u{E9}"                // é
let combinedEAcute: Character = "\u{65}\u{301}" // e followed by ́ => é
```

## Counting Characters

String 내부의 Character의 개수를 세기 위해 String의 count 프로퍼티를 사용한다.

Note that Swift’s use of extended grapheme clusters for Character values means that string concatenation and modification may not always affect a string’s character count. 
(Extended grapheme clusters를 사용하여 Character type의 값을 추가했을 때, Counting에 변화가 없을 수 있다. 예를 들어 accent를 추가하여 cafe가 café가 되는 경우이다.)

```swift
var word = "cafe"
print("the number of characters in \(word) is \(word.count)") // Prints "the number of characters in cafe is 4"

word += "\u{301}"    // COMBINING ACUTE ACCENT, U+0301

print("the number of characters in \(word) is \(word.count)") // Prints "the number of characters in café is 4" - counting 값에 변화가 없다.
```

- [ ]  NSString

```swift
var word = "cafe"
let nsStringLength1 = NSString(string: word).length // 이것도 가능

print("the number of characters in \(word) is \(word.count) and \(nsStringLength1)")
// Prints "the number of characters in cafe is 4 and 4"
```

Note: Extended grapheme clusters can be composed of multiple Unicode scalars. This means that different characters—and different representations of the same character—can require different amounts of memory to store. Because of this, characters in Swift don’t each take up the same amount of memory within a string’s representation. As a result, the number of characters in a string can’t be calculated without iterating through the string to determine its extended grapheme cluster boundaries.

If you are working with particularly long string values, be aware that the count property must iterate over the Unicode scalars in the entire string in order to determine the characters for that string.

- [ ]  ????

The count of the characters returned by the count property isn’t always the same as the length property of an NSString that contains the same characters. The length of an NSString is based on the number of 16-bit code units within the string’s UTF-16 representation and not the number of Unicode extended grapheme clusters within the string.

- [ ]  NSString의 length는 UTF-16 표현 방식으로 16bit 단위의 개수를 기반으로 하기 때문이다. (String의 count는 유니코드의 extended grapheme cluster의 개수를 기반으로 한다.) ???
- [ ]  손유석 답변 확인

## *Accessing and Modifying a String

- String Indices
    
    Each String value has an associated *Index type,* String.Index, which corresponds to the position of each Character in the string. 즉, String.Index도 하나의 type이다. 그 type은 Index type이다.
    
    - [ ]  ????
    
    As mentioned above, different characters can require different amounts of memory to store, so in order to determine which Character is at a particular position, you must iterate over each Unicode scalar from the start or end of that String. For this reason, Swift strings can’t be indexed by integer values. 즉, Swift에서는 int type으로 indexing이 불가하다.
    
    - [ ]  ?????????
    - `startIndex` property 는 String 첫번째 문자의 위치에 접근한다.
    - `endIndex` property 는 String 마지막 문자의 다음 위치에 접근한다. (따라서 endIndex는 Sting 서브스크립트에 대한 invalid argument이다.)
    단, 빈 문자열이면 startIndex 및 endIndex는 동일하다.
    - Sting 메서드인 `.index(before:)` / `.index(after:)` 를 통해 특정 index의 1칸 앞/뒤에 접근한다. 또는 `.index(_:offsetBy:)` 를 통해 특정 index에서 n칸 만큼 떨어진 위치에 접근한다. (+ 뒤, - 앞)
        
        ```swift
        // use subscript syntax to access the Character at a particular String index. instance[index]
        let greeting = "Guten Tag!"
        
        print(greeting.startIndex) // Index(_rawBits: 1) - 첫번째 문자 G의 위치에 접근 -> 이 자체가 Index type이고, 서브스크립트의 instance[index]에 전달된다. (아래 참고)
        print(greeting.endIndex)   // Index(_rawBits: 655361) - 마지막 문자 !의 다음 위치에 접근
        //print(greeting.index(before: 3)) // 컴파일 에러 - Cannot convert value of type 'Int' to expected argument type 'String.Index'
        //print(greeting.index(after: 3))
        
        var first = greeting.startIndex
        print(type(of: first)) // Index - 확인용
        
        greeting[greeting.startIndex] // G - 1) 첫번째 문자 G의 위치에 접근하고, 2) 해당 index를 통해 서브스크립트 문법을 사용 -> 첫번째 문자 G
        greeting[greeting.index(before: greeting.endIndex)] // ! - 1) 마지막 문자 !의 다음 위치에 접근하고, 2) 해당 index를 메서드의 argument로 전달하여 해당 위치의 1칸 앞에 접근, 3) 해당 index를 통해 서브스크립트 문법을 사용 -> !
        greeting[greeting.index(after: greeting.startIndex)] // u - 첫번째 문자 G의 위치에 접근하고, 해당 index를 메서드 argument로 전달하여 해당 위치의 1칸 뒤에 접근, 해당 index를 통해 서브스크립트 문법을 사용 -> u 
        
        greeting[greeting.index(greeting.startIndex, offsetBy: 7)] // a - 1) 첫번째 문자 G의 위치에 접근하고, 2) 해당 index를 메서드 argument로 전달하여 해당 위치의 +7칸 뒤에 접근, 3) 해당 index를 통해 서브스크립트 문법을 사용 -> a 
        
        let index = greeting.index(greeting.startIndex, offsetBy: 7) // 바로 위와 동일
        greeting[index] // a
        
        // strings range를 벗어난 index에 접근하면 런타임 에러가 발생한다.
        //greeting[greeting.endIndex] // Error
        //greeting.index(after: greeting.endIndex) // Error
        ```
        
        ```swift
        // greeting과 유사한 breezing 변수를 통해 실험
        let greeting = "Guten Tag!"
        var breezing = "12345 6789"
        
        print(greeting.startIndex) // Index(_rawBits: 1) - 첫번째 문자 G의 위치에 접근 
        print(greeting.endIndex)   // Index(_rawBits: 655361) - 마지막 문자 !의 다음 위치에 접근
        
        // 실험-1. 위치에 접근
        print(breezing.startIndex) // Index(_rawBits: 1) -> 똑같은 결과!
        print(breezing.endIndex) // Index(_rawBits: 655361)
        
        greeting[greeting.startIndex] // G - 첫번째 문자 G의 위치에 접근하고, 해당 index를 통해 서브스크립트 문법을 사용 -> 첫번째 문자 G
        greeting[greeting.index(before: greeting.endIndex)] // ! - 마지막 문자 !의 다음 위치에 접근하고, 해당 index를 메서드 argument로 전달하여 해당 위치의 1칸 앞에 접근, 해당 index를 통해 서브스크립트 문법을 사용 -> !
        greeting[greeting.index(after: greeting.startIndex)] // u - 첫번째 문자 G의 위치에 접근하고, 그것을 
        
        // 실험-2. 서브스크립트 [index] 내부 값 변경
        greeting[breezing.startIndex] // G -> 똑같은 결과!
        greeting[breezing.index(before: greeting.endIndex)] // !
        greeting[breezing.index(after: greeting.startIndex)] // u
        
        // 실험-3. 서브스크립트 instance 값 변경
        breezing[greeting.startIndex] // 1 -> 결론적으로 String 특정 위치에 접근하여 얻은 index type은 해당 String 값과 연결되어있지 않다.
        breezing[greeting.index(before: greeting.endIndex)] // 9
        breezing[greeting.index(after: greeting.startIndex)] // 2
        ```
        
    
    Note: startIndex/endIndex 프로토콜 및 index(before:)/index(after:)/index(_:offsetBy:) 메서드는 Collection protocol을 준수하는 모든 Type에 사용 가능하다. (String 포함 Array/Dictionary/Set 등)
    
- Inserting and Removing
    - `insert(_:at:)` 메서드를 통해 1개 문자를 String의 특정 index에 삽입한다.
    - `insert(contentsOf:at:)` 메서드를 통해 다른 String 콘텐츠를 String의 특정 index에 삽입한다.
    - `remove(at:)` 메서드를 통해 1개 문자를 String의 특정 index에서 삭제한다.
    - `removeSubrange(_:)` 메서드를 통해 Substring을 String의 특정 index range에서 삭제한다.
    
    Note: insert(_:at:)/insert(contentsOf:at:)/remove(at:)/removeSubrange(_:) 메서드는 RangeReplaceableCollection protocol을 준수하는 모든 Type에 사용 가능하다. (String 포함 Array/Dictionary/Set 등)
    

## Substrings

(서브스크립트 또는 prefix 메서드를 통해) String에서 Substring을 가져오면 다른 String이 아니라 새로운 Substring 인스턴스가 생성된다. (참조 type)
장기간 저장하여 사용하려면 Substring을 String으로 type 변환해야 한다.

```swift
// Substring 생성 방법-1. 서브스크립트
let name = "Marie Curie"
let firstSpace = name.firstIndex(of: " ") ?? name.endIndex // 앞에서부터 search for a space " " -> 해당 위치의 index type을 상수에 할당한다.
let firstName = name[..<firstSpace] // "Marie" - 서브스크립트 문법 인스턴스[index range]를 통해 Substring을 생성한다. name.prefix(upTo: firstSpace)와 동일하다!

// Type 확인
print(type(of: firstSpace)) // Index
print(type(of: firstName))  // Substring

// Substring 생성 방법-2. prefix 메서드 (prefix(_ maxLength:)) / prefix(through:) / prefix(upTo:)
let str = "Hello! Swift"
str.prefix(6) // "Hello!"
str.prefix(100) // "Hello! Swift" - parameter가 maxLength이므로 String length 보다 큰 값을 argument로 전달해도 crash가 발생하지 않는다. 

let index = str.index(str.startIndex, offsetBy: 5) // 첫번째 문자의 위치에서 +5칸 뒤의 index를 상수에 할당한다. (문자 !의 위치)
str.prefix(through: index) // "Hello!" - 서브스크립트 문법의 str[...index]와 동일하다.
str.prefix(upTo: index)    // "Hello", not include index specified (!) - str[..<index]와 동일하다.

// Type 확인
print(type(of: str.prefix(through: index))) // Substring
print(type(of: str.prefix(upTo: index))) // Substring
```

```swift
let greeting = "Hello, world!"
let index = greeting.firstIndex(of: ",") ?? greeting.endIndex // 앞에서부터 search for "," -> 해당 위치의 index type을 상수에 할당한다.
let beginning = greeting[..<index] // "Hello" - 서브스크립트 문법으로 Substring을 생성한다. greeting.prefix(upTo: index)와 동일하다!

// Convert the Substring to a String for long-term storage.
let newString = String(beginning)
```

- 참고 - prefix 메서드
    - `prefix(while:)` 메서드를 통해 return false가 나오기 전까지의 부분에 해당하는 prefix를 얻는다.
        
        ```swift
        let str = "Hello! Swift"
        str.prefix(while: { (character) -> Bool in
            return character != " " // space " "가 나오면 return false함
        }) // Hello!
        ```
        
    - `firstIndex(of:)` 메서드를 통해 특정 값을 앞에서부터 search for하여 해당 값이 나타난 위치의 index에 접근한다.
    `lastIndex(of:)` 메서드를 통해 특정 값을 뒤에서부터 search for하여 해당 값이 나타난 위치의 index에 접근한다.
        
        ```swift
        let str = "Hello! Swift"
        if let index = str.firstIndex(of: "l") {
            str.prefix(through: index) 
        } // "Hel"
        ```
        

성능 최적화로서 Substring은 original String을 저장하는 메모리의 일부를 재사용하거나, 또는 다른 Substring을 저장하는 메모리의 일부를 재사용한다.
이러한 성능 최적화를 통해 (String 또는 Substring을 수정하기 전에는) 메모리를 복사해도 성능이 저하되지 않는다. Substring을 사용하는 동안에는 전체 original String이 메모리에 들어있어야 한다.

- 메모리 구조
    
    ![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%201.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%201.png)
    
- [ ]  왜 12345로 안바뀌지? 변경된 String 값을 다른 메모리 공간에 저장했나?
    - String 또는 Substring을 수정한 직후에 메모리를 새롭게 할당하기 때문? 하지만 새로운 메모리를 할당받아도 여전히 String이 아니라 Substring인건가?

```swift
var greeting = "Hello, world!"
var index = greeting.firstIndex(of: ",") ?? greeting.endIndex
var beginning = greeting[..<index] // Hello - Substring 생성 (original String의 메모리 일부를 공유)

// original String의 값을 변경함
greeting = "12345, 789"
print(beginning) // Hello -> 왜 12345로 안바뀌지? 변경된 이후에는 변경된 String 값을 다른 메모리 공간에 저장했나?
```

Note: Both String and Substring conform to the StringProtocol protocol, which means it’s often convenient for string-manipulation functions to accept a StringProtocol value. You can call such functions with either a String value or Substring value.
String 및 Substring 모두 StringProtocol protocol을 준수한다. 따라서 Stirng을 다루는 함수를 사용할 때 StringProtocol의 값을 수용하는 것이 편리하다. ???? String 값 또는 Substring 값으로 이러한 함수를 호출한다.

# 4. Collection Types  (90%)

자세한 내용은 블로그에서 확인해주세요. [https://applecider2020.tistory.com/11](https://applecider2020.tistory.com/11)

Note: array, set, and dictionary types are implemented as generic collections.

- [ ]  generic Collection???

## Arrays

Note: Swift’s `Array` type is bridged to Foundation’s `NSArray` class.

- [ ]  [https://developer.apple.com/documentation/swift/array#2846730](https://developer.apple.com/documentation/swift/array#2846730)

### Accessing and Modifying an Array

You access/modify an array through 1) its methods and properties, or by 2) using subscript syntax.

- read-only `count` property를 통해 array의 item 개수를 확인한다.
- Boolean `isEmpty` property를 통해 축약으로 count property의 값이 0인지 확인한다. (array item이 0개인지 확인)
- array의 `append` method를 통해 array의 끝에 새로운 item을 추가한다.
- addition assignment operator (`+=`)를 통해 1개 또는 여러 개의 compatible item을 append 한다.
    
    ```swift
    print("The shopping list contains \(shoppingList.count) items.")  // Prints "The shopping list contains 2 items."
    
    if shoppingList.isEmpty {
        print("The shopping list is empty.")
    } else {
        print("The shopping list isn't empty.")  // Prints "The shopping list isn't empty." (shoppingList.count == 2 이므로)
    }  
    
    shoppingList.append("Flour")  // shoppingList now contains 3 items, and someone is making pancakes
    
    shoppingList += ["Baking Powder"]  // shoppingList now contains 4 items
    shoppingList += ["Chocolate Spread", "Cheese", "Butter"]  // shoppingList now contains 7 items
    ```
    
- Retrieve a value from the array by using subscript syntax, passing the index of the value you want to retrieve within square brackets immediately after the array name:
(서브스크립트 문법을 사용하여 array에서 값을 꺼낸다. 꺼내려는 값의 index를 array name 바로 뒤에 대괄호[] 내부에 전달한다. 즉, arrayName[index] 형태)
    
    ```swift
    var firstItem = shoppingList[0]  // firstItem is equal to "Eggs"
    ```
    
- You can use subscript syntax to change an existing value at a given index: (서브스크립트 문법을 사용하여 특정 index의 기존 값을 변경 가능하다.)
    
    ```swift
    shoppingList[0] = "Six eggs"  // the first item in the list is now equal to "Six eggs" rather than "Eggs"
    ```
    
    *When you use subscript syntax, the index you specify needs to be valid. For example, writing `shoppingList[shoppingList.count] = "Salt"` (index는 0부터 시작하므로 item 개수는 array.count -1 과 일치하므로) to try to append an item to the end of the array results in a runtime error. (서브스크립트 문법을 사용할 때, index가 valid 하지 않으면 런타임 에러가 발생한다.) 
    
    Dictionary와 달리 Array는 invalid index를 넣는 경우, 새로운 item이 추가되지 않는다.
    `shoppingList[100]= "new item"` // 런타임 에러 발생
    
- You can also use subscript syntax to change a range of values at once, even if the replacement set of values has a different length than the range you are replacing. 
(서브스크립트 문법을 통해 특정 범위의 값을 변경 가능하다. 이때 변경하려는 값과 변경할 값의 item의 개수가 달라도 가능하다.)
The following example replaces "Chocolate Spread", "Cheese", and "Butter" with "Bananas" and "Apples":
    
    ```swift
    print(shoppingList.count) // 7
    shoppingList[4...6] = ["Bananas", "Apples"]. // shoppingList now contains 6 items (3개 item을 2개 item으로 대체 가능하다.)
    print(shoppingList.count) // 6
    ```
    
- array의 `insert(_:at:)` method를 통해 특정 index에 item을 insert 한다. 해당 index에 있던 기존 item은 뒤로 밀린다.
- `remove(at:)` method를 통해 특정 index의 item을 삭제한다. 이때 삭제된 item이 return 된다. (return 값은 필요할 때만 사용한다.)
Any gaps in an array are closed when an item is removed, and so the value at index 0 is once again equal to "Six eggs": (삭제된 item의 index 자리는 바로 뒤의 item이 이동하여 Gap을 채움)
- `removeLast()` method를 통해 array의 마지막 item을 삭제한다. (remove(at:) 메서드를 사용하는 것보다 낫다.) 이때도 삭제된 item이 return 된다. 
Use the removeLast() method rather than the remove(at:) method to avoid the need to query the array’s count property.
    
    ```swift
    shoppingList.insert("Maple Syrup", at: 0)  // "Maple Syrup" is now the first item in the list, shoppingList now contains 7 items
    
    let mapleSyrup = shoppingList.remove(at: 0) // the item that was at index 0 has just been removed, shoppingList now contains 6 items, and no Maple Syrup
    // the mapleSyrup constant is now equal to the removed "Maple Syrup" string - 삭제된 item이 return 됨
    
    firstItem = shoppingList[0] // firstItem is now equal to "Six eggs" - 바로 뒤에 있던 item이 이동하여 Gap을 채움
    
    let apples = shoppingList.removeLast() // the last item in the array has just been removed, shoppingList now contains 5 items, and no apples
    // the apples constant is now equal to the removed "Apples" string - 삭제된 item이 return 됨
    ```
    

## Sets

Note: Swift’s Set type is bridged to Foundation’s NSSet class.

- [ ]  ???
- Hash Values for Set Types
    
    Note: 사용자 정의 타입 (custom types)이 Hashable Protocol을 준수하도록 설정하면, Set의 값, 그리고 Dictionary의 키로 사용할 수 있다. 단, 이 경우 직접 1) hash(into:) 메서드 및 2) == 연산자 함수를 구현 (implement)해야 한다.
    
    Hashable protocol - [https://developer.apple.com/documentation/swift/hashable](https://developer.apple.com/documentation/swift/hashable)
    
    - 예시는 이해가 된다. hasher의 기능에 대해서 찾아보자
        - [ ]  'Hashing a value' means feeding its essential components into a hash function, represented by the Hasher type. Essential components are those that contribute to the type’s implementation of Equatable. Two instances that are equal must feed the same values to Hasher in hash(into:), in the same order. ???

## Dictionaries

### Accessing and Modifying a Dictionary

1) methods and properties를 사용하거나 2) subscript syntax를 사용하여 Dictionary에 접근/수정이 가능하다.

- subscript syntax를 통해 Dictionary의 특정 key의 value를 꺼낼 수 있다. 
Because it’s possible to request a key for which no value exists, a dictionary’s subscript returns an optional value of the dictionary’s value type.
Dictionary에 request한 key의 value가 있으면 subscript는 그 value를 Optional type으로 반환하고, value가 없으면??? subscript는 nil을 반환한다.
    - [ ]  질문 - key는 있지만, value가 nil인 경우
    
    ```swift
    if let airportName = airports["DUB"] {
        print("The name of the airport is \(airportName).") // Prints "The name of the airport is Dublin Airport." - request한 key가 있다.
    } else {
        print("That airport isn't in the airports dictionary.")
    }
    
    // request한 key가 없는 경우
    if let airportName = airports["Key-doesn't exist"] {
        print("The name of the airport is \(airportName).")
    } else {
        print("That airport isn't in the airports dictionary.") // That airport isn't in the airports dictionary. - request한 key가 존재하지 않으므로 nil 반환
    }
    
    // key는 있지만, value가 nil인 경우
    var airports: [String: Any?] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"] // value에 nil을 할당하기 위해 Dictionary 선언부에서 type을 바꿔야 한다.
    
    var valueX: Any? = nil
    airports["test"] = valueX
    print(airports) // ["test": nil, "DUB": Optional("Dublin Airport"), "LHR": Optional("London Heathrow"), "YYZ": Optional("Toronto Pearson"), "New Key": Optional("set this value")]
    
    if let airportName = airports["test"] { // key는 있지만, value가 nil인 상황
        print("The name of the airport is \(airportName).") // The name of the airport is nil. - ???????????????? nil이 그대로 반환된다.
    } else {
        print("That airport isn't in the airports dictionary.")
    }
    ```
    

# 5. Control Flows (95%)

자세한 내용은 블로그에서 확인해주세요. [https://applecider2020.tistory.com/11](https://applecider2020.tistory.com/11)

### For-In Loops

You use the `for-in` loop to iterate over a sequence, such as items in an array, ranges of numbers, or characters in a string.
*Sequence Protocol : A type that provides sequential, iterated access to its elements. (Swift 기본 type에서는 Collection이 이에 해당된다.)

이외에도, you can use this syntax to iterate any collection, including your own classes and collection types, as long as those types conform to the Sequence protocol.

- [ ]  class를 어떻게 iterate???

```swift
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
} // 1 times 5 is 5, 2 times 5 is 10, 3 times 5 is 15, 4 times 5 is 20, 5 times 5 is 25,
```

- 실험
    
    ```swift
    var closedRange = 3.0...5.0 // 3...5 는 가능하다
        
    for n in closedRange { // 컴파일 에러 - Protocol 'Sequence' requires that 'Double.Stride' (aka 'Double') conform to 'SignedInteger'
        print(n)
    } // 3, 4, 5 출력
    ```
    

If you don’t need each value from a sequence, you can ignore the values by using `_` (underscore).  ← Wildcard Pattern

For this calculation, the individual counter values each time through the loop are unnecessary—the code simply executes the loop the correct number of times. The underscore character (_) used in place of a loop variable ??? (왜 default는 상수인데, loop variable이라고 하지?) causes the individual values to be ignored and doesn’t provide access to the current value during each iteration of the loop.

```swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")  // Prints "3 to the power of 10 is 59049"
```

구현 - Consider drawing the tick marks for every minute on a watch face.

```swift
// 1) draw 60 tick marks, starting with the 0 minute.
let minutes = 60
for tickMark in 0..<minutes { // Use the half-open range operator (..<) to include the lower bound but not the upper bound.
    // render the tick mark each minute (60 times)
}

// 2) prefer one mark every 5 minutes
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) { // Use the stride(from:to:by:) function to skip the unwanted marks.
    // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55) <- 60 불포함
}

// 3) prefer one mark every 3 hours
let hours = 12
let hourInterval = 3
for tickMark in stride(from: 3, through: hours, by: hourInterval) { // Closed ranges are also available, by using stride(from:through:by:)
    // render the tick mark every 3 hours (3, 6, 9, 12) <- 12 포힘
}
```

### While Loops

While 

구현 - This example plays a simple game of Snakes and Ladders.

![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%202.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%202.png)

```swift
let finalSquare = 25 // constant finalSquare is used to initialize the array and also to check for a win condition later in the example.
var board = [Int](repeating: 0, count: finalSquare + 1) // The game board is represented by an array of Int values. - Because the players start off the board, the board is initialized with 26 size, not 25.

// Some squares are then set to have more specific values for the snakes and ladders. (labber base: +, snake head: -)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08

// To align the values and statements, the unary plus operator (+i) is explicitly used with the unary minus operator (-i) 
// (값과 statement를 일치시키기 위해 단항 덧셈 연산자 (+i) 및 단항 뺄셈 연산자 (-i)를 명시적으로 사용하며)
// and numbers lower than 10 are padded with zeros. (1이면 01로 표기했다는 뜻) (Neither stylistic technique is strictly necessary, but they lead to neater code.) (2가지 stylistic technique 모두 엄격히 필요한 것은 아니지만, 더 깔끔한 코드를 만든다.)

var square = 0 // player의 현재 위치
var diceRoll = 0

while square < finalSquare {
    // roll the dice
    diceRoll += 1 // The result is a sequence of diceRoll values that’s always 1, 2, 3, 4, 5, 6, 1, 2... (used a simple approach)
    if diceRoll == 7 { diceRoll = 1 }
    // move by the rolled amount
    square += diceRoll

    if square < board.count { // board.count == 26 <- 왜 finalSquare가 아니지??????
        // if we're still on the board, move up/down for a snake/ladder
        square += board[square]
    }
}
print("Game over!")
```

- [x]  if square < board.count { // board.count == 26 <- 왜 finalSquare가 아니지??????
    - if문 내부의 board[square]이 런타임 에러를 발생시키지 않을 조건만 체크한 것이라서 인듯 (note 부분 참고)

Repeat-While

```swift
// while loop와 동일함
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)

board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08

var square = 0
var diceRoll = 0

// repeat-while문 사용 - the first action in the loop is to check for a ladder/snake.
repeat {
    // move up/down for a snake/ladder
    square += board[square]  // while문의 'if square < board.count {...}' statement가 필요 없다! (= 'array bounds check'이 필요 없다)

    // roll the dice
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    // move by the rolled amount
    square += diceRoll
} while square < finalSquare
print("Game over!")
```

The structure of the repeat-while loop is better suited to this game than the while loop in the previous example.

## Conditional Statements

- [x]  패턴 매칭 (pattern matching)이란?
    
    특정 형태의 패턴을 찾아내는 것이다. 예를 들어 튜플 (1, "일")의 패턴은 (Int, String)이다. (2, "이")의 패턴과 패턴 매칭을 하면, 두 패턴은 일치하다는 것을 알 수 있다.
    
- 참고 - if/switch 차이점
    
    컴파일할 때 동작에 차이가 있다.
    if-else문은 true가 나올 때까지 순차적으로 모든 조건을 확인하고,
    switch문은 조건문에 해당하는 case로 바로 이동 (jump 기반)한다. → 따라서 둘다 적절한 경우, switch문을 사용하는 게 성능 측면에서 좋다.
    
    [https://pongsoyun.tistory.com/121](https://pongsoyun.tistory.com/121)
    
    - [ ]  jump table이 뭐지?

### *Switch

each case is a separate branch of code execution. The switch statement determines which branch(case) should be selected. ← This procedure is known as *switching* on the value that’s being considered.

- [x]  왜 switch문의 case는 들여쓰기를 안할까? switch indentation style swift / why switch case should be indented swift
    - (여러 가지 설이 있음)
    - Backing the cases to the same indentation level as the "switch" is a common style for all C-inspired languages.
    - switch ... case의 의미는 기본적으로 if...else if 와 같다. 따라서 동일한 indent에 switch와 case를 배치한다.
        
        ```swift
        if condition {
            // ...
        } else if {
            // ...
        }
        ```
        
    - 원하면 Xcode setting을 변경 가능함 - [https://stackoverflow.com/questions/42376988/xcode-changing-the-indentation-rules-for-switch-statements](https://stackoverflow.com/questions/42376988/xcode-changing-the-indentation-rules-for-switch-statements)

## Control Transfer Statements

- continue
    
    The continue statement tells a loop to stop what it’s doing and start again at the beginning of the next iteration through the loop. (반복문의 다음 iteration으로 넘어감)
    
    ```swift
    let puzzleInput = "great minds think alike"
    var puzzleOutput = ""
    let charactersToRemove: [Character] = ["a", "e", "i", "o", "u", " "]
    
    for character in puzzleInput {
        if charactersToRemove.contains(character) {
            continue // calls the continue keyword whenever it matches a vowel or a space, causing the current iteration of the loop to end immediately and to jump straight to the start of the next iteration.
        }
        puzzleOutput.append(character)
    }
    print(puzzleOutput)
    // Prints "grtmndsthnklk"
    ```
    
- break
    
    The break statement ends execution of an entire control flow statement immediately. The break statement can be used inside a switch or loop statement when you want to terminate the execution of the switch/loop statement earlier than would otherwise be the case. (switch문 또는 반복문 자체를 즉시 종료함)
    
- Labeled Statements
    
    To achieve these aims, you can mark a loop statement/conditional statement with a *statement label*.
    
    - With a conditional statement, you can use a statement label with the break statement to end the execution of the labeled statement.
    - With a loop statement, you can use a statement label with the break or continue statement to end or continue the execution of the labeled statement.
    
    즉, break 또는 continue를 적용할 대상이 labeled statement 이다.
    
    ```swift
    label name: while condition {  // the principle is the same for all loops and switch statements.
        statements
    }
    ```
    
    The example uses the break and continue statements with a labeled while loop for an adapted version of the Snakes and Ladders game. This time around, the game has an extra rule:
    
    - To win, you must land *exactly* on square 25. (you must roll again until you roll the exact number needed to land on square 25.)
    - [ ]  label을 지우면 무한루프 돌아가듯이 실행됨;;; 왜지?
    
    ```swift
    let finalSquare = 25  // initialized in the same way as before:
    var board = [Int](repeating: 0, count: finalSquare + 1)
    
    board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
    board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
    
    var square = 0  // player의 현재 위치
    var diceRoll = 0
    
    // a labeled while loop (정확히 square == 25 이면 종료됨)
    gameLoop: while square != finalSquare {
        diceRoll += 1
        if diceRoll == 7 { diceRoll = 1 }
    
        switch square + diceRoll {  // uses a switch statement to consider the result of the move and to determine whether the move is allowed.
        case finalSquare:
            // diceRoll will move us to the final square, so the game is over
            break gameLoop
        case let newSquare where newSquare > finalSquare:  // switch문의 value binding 기능을 사용 (square + diceRoll의 값이 newSquare에 할당됨) - A switch case can name the value or values it matches to temporary constants/variables for use in the body of the case.
            // diceRoll will move us beyond the final square, so roll again
            continue gameLoop
        default:
            // this is a valid move, so find out its effect
            square += diceRoll
            square += board[square]
        }
    }
    print("Game over!")
    ```
    
    - 참고 - repeat-while
        
        ```swift
        let finalSquare = 25
        var board = [Int](repeating: 0, count: finalSquare + 1)
        
        board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
        board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
        
        var square = 0
        var diceRoll = 0
        
        // repeat-while문 사용 - the first action in the loop is to check for a ladder/snake.
        repeat {
            // move up/down for a snake/ladder
            square += board[square]  // 위의 'if square < board.count {}' statement가 필요 없다! (= 'array bounds check'이 필요 없다)
        
            // roll the dice
            diceRoll += 1
            if diceRoll == 7 { diceRoll = 1 }
            // move by the rolled amount
            square += diceRoll
        } while square < finalSquare
        print("Game over!")
        ```
        
    
- [ ]  return - Functions 파트 참고
- [ ]  throw - Propagating Errors Using Throwing Functions 파트 참고

## Early Exit

else branch must transfer control to exit the code block in which the guard statement appears. It can do this with 1) a control transfer statement such as return, break, continue, or throw, or it can 2) call a function that doesn’t return, such as fatalError(_:file:line:).

guard문의 조건이 거짓이면, else 구문이 실행된다. else 구문은 guard문이 존재하는 코드 블럭을 종료하기 위해 반드시 제어을 이동 (transfer control)시켜야 한다. 즉, 1) 제어 전달 구문 (continue, break, return, throw) 또는 2) 반환하지 않는 함수 (fatalError(_:file:line:))를 사용해야 한다.

- [ ]  실제로는 else 구문을 종료하기 위해서 같아 보이는데????????? 
(guard문이 존재하는 큰 덩어리 코드가 아니라)

```swift
func greet(person: [String: String]) {
    guard let name = person["name"] else { // if-let과 차이점 : Any variables/constants that were assigned values using an optional binding as part of the condition ??? are available for the rest of the code block that the guard statement appears in. ('gaurd문이 들어있는 코드 블럭' 내부에서 사용 가능하다.)
        return // else branch는 'guard문이 들어 있는 코드 블럭'의 외부로 control을 이동시켜야 한다. (1) control transfer statement를 사용하거나, 2) return 값이 없는 함수를 호출한다.)
//      fatalError("this is wrong") // 2) return 대신 return 값이 없는 함수를 호출한 예시
    }
    print("Hello \(name)!") 

// guard 1>2, let location = person["location"] else {  // 조건을 추가 (조건1 && 조건2 불가) -> 항상 false이므로 else block을 실행시킴 (print 이후 함수 greet가 종료됨)
    guard let location = person["location"] else {
        print("I hope the weather is nice near you.")
        return
    }
    print("I hope the weather is nice in \(location).")
}

greet(person: ["name": "John"])
// Prints "Hello John!"
// Prints "I hope the weather is nice near you."
greet(person: ["name": "Jane", "location": "Cupertino"])
// Prints "Hello Jane!"
// Prints "I hope the weather is nice in Cupertino."
```

## Checking API Availability

Swift has built-in support for checking API availability, which ensures that you don’t accidentally use APIs that are unavailable on a given deployment target. 

- [x]  deployment?
    - Software deployment refers to the process of running an application on a server or device. A software update or application may be deployed to a test server, a testing machine, or into the live environment, and it may be deployed several times during the development process to verify its proper functioning and check for errors. Another example of software deployment could be when a user downloads a mobile application from the App Store and installs it onto their mobile device.

The compiler uses availability information in the SDK (Software Development Kit) to verify that all of the APIs used in your code are available on the deployment target specified by your project. Swift reports an error at compile time if you try to use an API that isn’t available.

컴파일러는 *SDK에 들어있는 사용 가능한 API 정보를 확인하고, 개발이 완료된 코드 내부의 모든 API가 적절한지 확인한다. 사용 불가한 (unavailable) API를 사용하면 컴파일 오류가 발생한다.

*SDK (Software Development Kit)란?

개발자가 쉽게 개발을 하도록 도와주는 개발 도구 모음이다. 특정 기능을 구현하기 위해 필요한 클래스/함수/프로토콜 등을 개발자가 모두 구현하지 않고, Swift 표준 라이브러리와 같이 외부에서 구현한 코드 패키지를 사용하는 방식이다.

- [ ]  이 내용이 맞나????

- [ ]  API가 플랫폼 & 배전 정보 맞나?????

```swift
// availability condition

if #available(iOS 10, macOS 10.12, *) {
    // Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS
} else {
    // Fall back to earlier iOS and macOS APIs
}
```

- Use iOS 10 APIs on iOS
    - [ ]  iOS 10 API ??????
- The last argument, *, is required (필수 인자) and specifies that on any other platform, the body of the if executes on the minimum deployment target specified by your target. ???
    - [ ]  minimum deployment target????

# 6. Functions (95%)

Functions are self-contained chunks of code that perform a specific task. You give a function a name that identifies what it does (함수의 기능을 식별하는 이름을 지정한다), and this name is used to “call” the function to perform its task when needed. (함수를 호출할 때 함수이름을 사용한다.)

Swift’s unified function syntax is flexible enough to express anything from a simple C-style function (with no parameter names) to a complex Objective-C-style method (with names and argument labels for each parameter). Parameters can provide default values to simplify function calls and can be passed as in-out parameters, which modify a passed variable once the function has completed its execution.

Every function has a type, consisting of the function’s parameter types and return type. You can use this type like any other type in Swift, which makes it easy to 1) pass functions as parameters to other functions, and to 2) return functions from functions. Functions can also be 3) written within other functions to encapsulate useful functionality within a nested function scope. (내포된 함수 범위 내에서 유용하게 기능한다.) 내포된 함수 범위 내에서 ????

- [ ]  
- Defining and Calling Functions
    
    When you define a function, you can optionally define one or more named, typed values that the function takes as input, known as `parameters.` You can also optionally define a type of value that the function will pass back as output when it’s done, known as its `return type`.
    
    Every function has a function name, which describes the task that the function performs. To use a function, you “call” that function with its name and pass it input values (known as `arguments`) that match the types of the function’s parameters. A function’s arguments must always be provided in the same order as the function’s parameter list.
    
    ```swift
    func greet(person: String) -> String { // define one input parameter—a String value called person
        let greeting = "Hello, " + person + "!"
        return greeting
    }
    ```
    
    ```swift
    // 축약형 
    func greetAgain(person: String) -> String {
        return "Hello again, " + person + "!"  // you can combine the message creation and the return statement into one line
    }
    ```
    
    In the line of code that says `return greeting`, the function finishes its execution and returns the current value of `greeting`.
    
    All of this information is rolled up into the `function’s definition`, which is prefixed with the `func` keyword. You indicate the function’s return type with the `return arrow` -> (a hyphen followed by a right angle bracket), which is followed by the name of the type to return. 
    
    ```swift
    print(greet(person: "Anna")) // Prints "Hello, Anna!" - You call the greet(person:) function by passing it a String value after the person argument label
    print(greet(person: "Brian")) // Prints "Hello, Brian!" 
    ```
    
    You call the greet(person:) function by passing it a String value after the person argument label. Because the function returns a String value, greet(person:) can be wrapped in a call to the print function (greet 함수는 'print 함수를 호출하는 구문' 내부에 들어갈 수 있다.) to print that string and see its return value.
    
    - [x]  함수 정의에서 parameter를 정의할 때 argument label을 추가로 붙이지 않아도, 함수호출 시 parameter name이 argument label이라고 불린다!
    즉, 함수 호출 시점에서 argument가 전달되는 부분의 앞쪽은 무조건 'argument label'인건가?
        - 함수 호출 시점에서 argument가 전달되는 부분의 앞쪽은 무조건 'argument label'이 맞다. 선언 시 argument label을 따로 지정해주지 않으면 자동으로 parameter name이 argument label이 된다.
    
    Note: print(_:separator:terminator:) function doesn’t have a label for its first argument, and its other arguments are optional because they have a default value.
    

### Function Parameters and Return Values

You can define anything from a simple utility function (with a single unnamed parameter) to a complex function.

- [x]  utility function?
    - Utility methods perform common, often re-used functions which are helpful for accomplishing routine programming tasks. Examples might include methods connecting to databases, performing string manipulations, sorting and searching of collections of data, writing/reading data to/from files and so on.
        
        [https://www.quora.com/What-is-a-utility-method](https://www.quora.com/What-is-a-utility-method)
        
- Functions Without Parameters
    
    Functions aren’t required to define input parameters.
    
    ```swift
    func sayHelloWorld() -> String { // parameter를 정의하지 않더라도 함수 정의 시 ()가 필요하다.
        return "hello, world"
    }
    print(sayHelloWorld()) // Prints "hello, world" - 마찬가지로 함수 호출 시 ()가 필요하다. 
    ```
    
- Functions With Multiple Parameters
    
    ```swift
    func greet(person: String, alreadyGreeted: Bool) -> String {
        if alreadyGreeted {
            return greetAgain(person: person)
        } else {
            return greet(person: person)
        }
    }
    print(greet(person: "Tim", alreadyGreeted: true)) // Prints "Hello again, Tim!" - passing the function both a String argument value labeled person and a Bool argument value labeled alreadyGreeted.
    ```
    
    Note that this function `greet(person:alreadyGreeted)` is distinct (별개의 함수이다) from the `greet(person:)` function. (Although both functions have names that begin with greet, the greet(person:alreadyGreeted:) function takes two arguments but the greet(person:) function takes only one.)
    
- Functions Without Return Values
    
    Functions aren’t required to define a return type.
    
    ```swift
    func greet(person: String) {
        print("Hello, \(person)!") // prints its own String value rather than returning it
    }
    greet(person: "Dave") // Prints "Hello, Dave!"
    
    var specialValueOfTypeVoid: Void = greet(person: "test") // Hello, test! 출력 - 함수가 할당됨과 동시에 호출된다. ????
    //print(specialValueOfTypeVoid)  // ()
    ```
    
    Note: Strictly speaking, this version of the greet(person:) function does still return a value, even though no return value is defined. Functions without a defined return type return a special value of type `Void`. This is simply an empty tuple, written as `()`.
    (엄밀히는 return type을 정의하지 않은 함수도 return value를 가진다. 그것은 Void type이고, empty tuple () 이다.)
    
    - [ ]  var specialValueOfTypeVoid: Void = greet(person: "test") // Hello, test! 출력 - 함수가 할당됨과 동시에 호출된다. ????
    
    The return value of a function can be ignored when it’s called:
    
    ```swift
    func printAndCount(string: String) -> Int {
        print(string)
        return string.count
    }
    func printWithoutCounting(string: String) {
        let _ = printAndCount(string: string) // calls the first function, but ignores its return value.
    }
    printAndCount(string: "hello, world") // prints "hello, world" and returns a value of 12
    printWithoutCounting(string: "hello, world") // prints "hello, world" - The message is still printed by the first function, but the returned value isn’t used.
    ```
    
    Note: Return values can be ignored, but a function that says it will return a value must always do so. A function with a defined return type can’t allow control to fall out of the bottom of the function without returning a value, and attempting to do so will result in a compile-time error. 
    (함수 정의 시 return type이 Void가 아닌 경우, 항상 특정 값을 return 해야 한다. (반환값은 꼭 사용하지 않아도 된다.) 아니면 런타임 에러가 발생한다.)
    
    - [ ]  let _ = printAndCount(string: string) // calls the first function, but ignores its return value.
    
- Functions with Multiple Return Values
    
    You can use a tuple type as the return type for a function to return multiple values as part of one compound return value.
    
    ```swift
    // a function, which finds the smallest and largest numbers in an array
    func minMax(array: [Int]) -> (min: Int, max: Int) {
        var currentMin = array[0]
        var currentMax = array[0]
    
        for value in array[1..<array.count] {
            if value < currentMin {
                currentMin = value
            } else if value > currentMax {
                currentMax = value
            }
        }
        return (currentMin, currentMax) // 이미 return type을 정의할 때 tuple element의 name을 지정했으므로 return (min: currentMin, max: currentMax)으로 다시 명시하지 않아도 된다.
    }
    
    // Because the tuple’s member values are named as part of the function’s return type, they can be accessed with dot syntax to retrieve the values:
    let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
    print("min is \(bounds.min) and max is \(bounds.max)") // Prints "min is -6 and max is 109"
    ```
    
    The function returns a tuple containing two values. These values are labeled min/max so that they can be accessed by name when querying the function’s return value.
    
    - [ ]  단순히 labeled 라는 표현을 named와 동일하게 사용하는건가?
    - [x]  Query?
        - 특정데이터베이스에서 원하는 조건의 데이터를 조작하기 위한 문장 덩어리이다.
        - 데이터베이스에 보내는 요청(request) 또는 질문이다.
        - 데이터베이스나 파일의 특정 내용을 검색하기 위해 코드(code) 또는 키(Key)를 기초로 조회하는 것이다.
        
        [http://wiki.hash.kr/index.php/쿼리](http://wiki.hash.kr/index.php/%EC%BF%BC%EB%A6%AC)
        

- Optional Tuple Return Types
    
    함수의 return type이 tuple인 경우, tuple 전체가 nil 일 가능성이 있다. 이때 return type을 Optional tuple type 으로 명시한다. 예를 들면 `(Int, Int)?`  이다.
    
    Note: `(Int?, Int?)`와 다르다. (Int?, Int?)은 a tuple that contains values of optional types 이다. 반면, optional tuple type `(Int, Int)?`은 the entire tuple is optional 이라는 의미이다.
    
    ```swift
    //minMax(array: []) // 런타임 에러 발생 - Fatal error: Index out of range (empty array의 index 0의 값은 없으므로)
    ```
    
    minMax 함수는 argument로 전달하는 array에 대해 safety checks를 하지 않았다. 만약 argument로 empty array를 전달하면, array[0]에 접근하는 과정에서 런타임 에러가 발생한다.
    safety checks를 거쳐서 함수를 안전하게 사용하려면, return type을 optional tuple로 수정하고, argument에 empty array가 전달되면 nil을 반환해야 한다.
    
    ```swift
    func minMax(array: [Int]) -> (min: Int, max: Int)? {
        if array.isEmpty { return nil }  // empty array가 전달되면 nil을 반환하고, 함수를 종료한다.
        var currentMin = array[0]
        var currentMax = array[0]
        for value in array[1..<array.count] {
            if value < currentMin {
                currentMin = value
            } else if value > currentMax {
                currentMax = value
            }
        }
        return (currentMin, currentMax)
    }
    
    if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) { // Use optional binding to check whether the function returns an actual tuple value or nil
        print("min is \(bounds.min) and max is \(bounds.max)")
    } // Prints "min is -6 and max is 109"
    ```
    
- Functions With an Implicit Return
    
    If the entire body of the function is a single expression, the function implicitly returns that expression. 
    Any function that you write as just one return line can omit the return. (함수 구현부가 1줄이면 `return` 키워드를 생략 가능하다.) 
    
    참고 - property `getter` can also use an implicit return.
    
    ```swift
    func greeting(for person: String) -> String {
        "Hello, " + person + "!"  // 함수 구현부가 1줄이므로 return 키워드를 생략 가능하다.
    }
    print(greeting(for: "Dave")) // Prints "Hello, Dave!"
    
    func anotherGreeting(for person: String) -> String {
        return "Hello, " + person + "!"
    }
    print(anotherGreeting(for: "Dave")) // Prints "Hello, Dave!"
    ```
    
    Note: return type을 명시했으므로 반드시 return value가 있어야 한다. 따라서 You can’t use `fatalError("Oh no!")` or `print(13)` as an implicit return value.
    

### Function Argument Labels and Parameter Names

Each function parameter has both an argument label and a parameter name. The argument label is used when calling the function; each argument is written in the function call with its argument label before it. (함수 호출 시, argument 앞에 있는 것이 무조건 argument label이다!) The parameter name is used in the implementation of the function. (parameter name은 함수를 실행하는 과정에서 사용된다. 함수 구현부에서 언급됨) 
By default, parameters use their parameter name as their argument label. (함수 선언 시 argument label을 따로 정의하지 않으면, default로 parameter name을 argument label로 사용한다!)

함수 호출 시, argument 앞에 있는 것이 무조건 argument label이다!
함수 선언 시, argument label을 따로 정의하지 않으면, default로 parameter name을 argument label로 사용한다!

parameter가 여러 개인 경우, parameter name과 달리 argument label은 동일하게 중복사용이 가능하다. 단, 가독성을 위해 argument label을 중복하지 않는 것이 좋다.

```swift
func someFunction(sameLabel firstParameterName: Int, sameLabel secondParameterName: Int) {
    // In the function body, firstParameterName/secondParameterName refer to the argument values for the first/second parameters.
    print(firstParameterName + secondParameterName)
}
someFunction(sameLabel: 1, sameLabel: 2) // 3 출력
```

- Specifying Argument Labels
    
    The use of argument labels can allow a function to be called in an expressive, sentence-like manner, while still providing a function body that’s readable and clear in intent.
    (argument label의 장점은 함수 호출부는 표현이 쉽고, 문장과 같은 표현방식으로 사용이 가능하며, 동시에 함수 구현부는 가독성 있고 의도에 맞게 구성할 수 있다는 것이다.)
    
    ```swift
    func someFunction(argumentLabel parameterName: Int) {
        // ...
    }
    ```
    
- Omitting Argument Labels
    
    parameter에 argument label를 지정하고 싶지 않다면, `_` (underscore) 를 사용한다.
    단, parameter에 argument label가 있다면, 반드시 함수 호출 시 argument label을 사용해야 한다.
    
    ```swift
    func someFunction(_ firstParameterName: Int, secondParameterName: Int) {
        // ...
    }
    someFunction(1, secondParameterName: 2)
    ```
    
- Default Parameter Values (매개변수 기본값)
    
    You can define a default value for any parameter by assigning a value to the parameter. default value를 정의하면, 함수 호출 시 parameter를 생략 가능하다.
    
    함수 정의 시, default value가 있는 parameter를 list의 뒷쪽에 배치한다. default value가 없는 parameter가 더 중요할 가능성이 높기 때문이다. (중요한 parameter를 앞에 배치하면 가독성이 좋다.)
    
    ```swift
    func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
        // If you omit the second argument when calling this function, then the value of parameterWithDefault is 12 inside the function body.
    }
    someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6) // parameterWithDefault is 6
    someFunction(parameterWithoutDefault: 4) // parameterWithDefault is 12 - parameter 생략 가능
    ```
    
- Variadic Parameters (가변 매개변수)
    
    *Variadic : (CS용어) Taking a arbitrarily many arguments.
    
    A variadic parameter accepts zero or more values of a specified type. A variadic parameter specifies that the parameter can be passed a varying number of input values when the function is called. Write variadic parameters by inserting `...` (three period characters) after the parameter’s type name.
    
    The values passed to a variadic parameter are made available (within the function’s body) as an array.
    
    ```swift
    func arithmeticMean(_ numbers: Double...) -> Double {
        var total: Double = 0
        for number in numbers { // 전달된 arguments는 Array로 사용된다. ([Double] type)
            total += number
        }
        return total / Double(numbers.count)
    }
    arithmeticMean(1, 2, 3, 4, 5) // returns 3.0, which is the arithmetic mean of these five numbers
    arithmeticMean(3, 8.25, 18.75) // returns 10.0, which is the arithmetic mean of these three numbers
    
    print(arithmeticMean()) // nan 
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
    
    //print(arrayParameter()) // 컴파일 에러 발생 - Missing argument for parameter #1 in call <- array는 parameter 생략이 불가하다.
    print(variadicParameter()) // [] <- 가변 매개변수는 parameter 생략이 가능하다.
    ```
    
    - [x]  nil 이 아니라 nan?
        - nan : Type property - A quiet NaN (“not a number”). 
        A NaN compares not equal, not greater than, and not less than every value, including itself. Passing a NaN to an operation generally results in NaN.
            
            ```swift
            let x = 1.21
            // x > Double.nan == false
            // x < Double.nan == false
            // x == Double.nan == false
            ```
            
            Because a NaN always compares not equal to itself, to test whether a floating-point value is NaN, use its isNaN property instead of the equal-to operator (==). In the following example, y is NaN.
            
            ```swift
            let y = x + Double.nan
            print(y == Double.nan) // Prints "false"
            print(y.isNaN) // Prints "true"
            ```
            
    
    A function can have multiple variadic parameters. The first parameter that comes after a variadic parameter must have an argument label. (가변 매개변수 바로 뒤에오는 매개변수는 반드시 argument label이 있어야 한다. 함수 호출 시 arguments가 어떤 parameter로 전달될지 구분해야 하기 때문이다.)
    
    ```swift
    func arithmeticMean(label1 numbers1: Double..., label2 numbers2: Double...) -> Double {
    //func arithmeticMean(label1 numbers1: Double..., _ numbers2: Double...) -> Double { // 컴파일 에러 발생 - A parameter following a variadic parameter requires a label.
        var total1: Double = 0
        var total2: Double = 0
        
        for number1 in numbers1 {
            total1 += number1
        }
        for number2 in numbers2 {
            total2 += number2
        }
        
        return total1 + total2
    }
    
    print(arithmeticMean(label1: 1,2,3, label2: 7,8,9)) // 30.0 출력
    ```
    
- In-Out Parameters
    
    Function parameters are constants by default. Trying to change the value of a function parameter from within the body of that function results in a compile-time error. 
    If you want a function to modify a parameter’s value, and you want those changes to persist after the function call has ended, define that parameter as an *in-out parameter* instead.
    (함수가 parameter의 값을 수정하고, 함수가 종료된 이후에도 해당 값을 유지하려면 해당 parameter를 in-out parameter로 정의해야 한다.)
    
    An in-out parameter has a value that’s passed *in* to the function, is modified by the function, and is passed back *out* of the function to replace the original value.
    (in-out parameter는 값을 함수에 전달 (in)하고, 함수 내부에서 값을 수정한 이후에 함수 밖으로 다시 전달 (out)하여 기존의 값을 대체한다.
    
    - [ ]  behavior of in-out parameters and associated compiler optimizations, see [In-Out Parameters](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#ID545).
        - in-out parameters are passed as follows:
            1. When the function is called, the value of the argument is copied.
            2. In the body of the function, the copy is modified.
            3. When the function returns, the copy’s value is assigned to the original argument.
        - This behavior is known as copy-in copy-out or call by value result.
    
    You can only pass a variable as the argument for an in-out parameter. You can’t pass a constant or a literal value as the argument, because constants and literals can’t be modified. You place an `&` (ampersand) directly before a variable’s name when you pass it as an argument to an in-out parameter, to indicate that it can be modified by the function.
    
    Note: In-out parameters는 매개변수 기본값을 가질 수 없다. 가변 매개변수 (variadic parameters)는 in-out parameter가 될 수 없다.
    
    ```swift
    func swapTwoInts(_ a: inout Int, _ b: inout Int) { // in-out parameter가 아니라면, 함수 종료 시 argument 및 로컬변수가 Stack에서 pop되므로 함수 외부에 영향을 미치지 않는다.
        let temporaryA = a  // 임시 상수 temporaryA를 통해 in-out parameter인 a와 b의 값을 swap 한다.
        a = b
        b = temporaryA
    }
    
    var someInt = 3
    var anotherInt = 107
    
    swapTwoInts(&someInt, &anotherInt) // in-out parameter로 전달되는 argument는 &로 표시한다. (각 변수의 주소를 함수에 전달하므로 함수 외부에도 영향을 미친다. CS50 swap 참고)
    print("someInt is now \(someInt), and anotherInt is now \(anotherInt)") // Prints "someInt is now 107, and anotherInt is now 3" - 함수 실행 이후 swap 된 상태이다.
    ```
    
    Note: In-out parameters는 어떤 값을 return 하는 것 외에 함수 외부에 영향을 미치는 또 다른 방법이다.
    

### Function Types

함수의 type은 parameter type 및 return type으로 구성된다.

```swift
func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}
func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}
```

두 함수의 type은 `(Int, Int) -> Int` 이다. “A function that has two parameters, both of type Int, and that returns a value of type Int.” 라는 뜻이다.

```swift
func printHelloWorld() {
    print("hello, world")
}
```

이 함수의 type은 `() -> Void` 이다. “a function that has no parameters, and returns Void.”라는 뜻이다.

- Using Function Types
    
    함수 type을 Swift의 다른 type과 동일하게 사용 가능하다. 1) 상수/변수의 type을 함수로 정의 가능하다. 또한 해당 상수/변수에 함수를 할당 가능하다.
    
    ```swift
    var mathFunction: (Int, Int) -> Int = addTwoInts
    
    print("Result: \(mathFunction(2, 3))") // Prints "Result: 5" - You can call the assigned function with the name mathFunction. (변수 자체가 함수이므로 parameter a,b 없이 호출 가능하다.)
    ```
    
    변수 mathFuntion을 함수 type으로 정의한다. 그리고 Set this variable to refer to the addTwoInts function. (함수 addTwoInts를 참조하도록 이 변수를 설정한다.)” 라는 뜻이다. *함수는 참조 type 이므로
    
    A different function with the same matching type can be assigned to the same variable, in the same way as for nonfunction types: (해당 변수에 동일한 type의 함수를 할당 가능하다.)
    
    ```swift
    mathFunction = multiplyTwoInts
    print("Result: \(mathFunction(2, 3))") // Prints "Result: 6"
    ```
    
    Swift의 다른 type과 마찬가지로 type inference 기능을 통해 이미 type 정보가 있는 함수는 상수/변수에 할당할 때, 상수/변수의 type 정의를 생략 가능하다.
    
    ```swift
    let anotherMathFunction = addTwoInts
    // anotherMathFunction is inferred to be of type (Int, Int) -> Int 
    ```
    

- Function Types as Parameter Types
    
    2) 함수 type을 다른 함수의 parameter type으로 사용 가능하다. This enables you to leave some aspects of a function’s implementation for the function’s caller to provide when the function is called. (이렇게 하면 함수 호출자가 함수를 호출할 때 제공할 수 있도록 함수 구현의 일부 측면을 남겨둘 수 있다.) 
    = This enables `printMathResult` to hand off some of its functionality to the caller of the function.
    
    ```swift
    func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
        print("Result: \(mathFunction(a, b))") // \()를 이렇게도 사용 가능하다.
    }
    printMathResult(addTwoInts, 3, 5) // Prints "Result: 8"
    // printMathResult 함수가 호출되면, addTwoInts 함수 및 3,5 가 argument로 전달되고, 전달받은 함수를 3,5로 호출하여 결과적으로 8을 출력한다.
    ```
    
- Function Types as Return Types
    
    3) 함수 type을 다른 함수의 return type으로 사용 가능하다. Write a complete function type immediately after the  `->` `(return arrow)` of the returning function.
    
    ```swift
    func stepForward(_ input: Int) -> Int {  // 함수의 type은 (Int) -> Int 이다.
        return input + 1
    }
    func stepBackward(_ input: Int) -> Int {
        return input - 1
    }
    
    func chooseStepFunction(backward: Bool) -> (Int) -> Int {  // 쉽게 표현하면 -> (Int) -> Int (즉, 함수의 return type이 (Int) -> Int 이다.)
        return backward ? stepBackward : stepForward 
    }
    
    var currentValue = 3
    let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)  // arugument가 true이므로 -> moveNearerToZero now refers to the stepBackward() function.
    
    // Now that moveNearerToZero refers to the correct function, it can be used to count to zero:
    while currentValue != 0 {
        print("\(currentValue)... ")
        currentValue = moveNearerToZero(currentValue)
    }
    print("zero!") 
    // 3... 2... 1... zero!
    ```
    

### Nested Functions

지금까지 Functions Chapter에서 다룬 함수는 모두 `global functions` 이다. global function은 global scope에서 정의된다. 
이외에도 다른 함수의 내부에 함수를 정의할 수 있다. 이것은 `nested functions`이다.

Nested functions are hidden from the outside world by default, but can still be called and used by their enclosing function. An enclosing function can also return its nested function to allow the nested function to be used in another scope. (Nested Function은 return 되어 Enclosing function 외부에서 사용될 수 있다!) Enclosing Function 내부에서만 사용할 수 있는 게 아니다!

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(inputNumber: Int) -> Int { return inputNumber + 1 }  // 함수 내부에 함수를 정의한다.
    func stepBackward(inputNumber: Int) -> Int { return inputNumber - 1 }

    return backward ? stepBackward : stepForward  // Nested Function을 return 한다. Enclosing Function 외부에서도 호출될 수 있다.
}

var currentValue = -4
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0) // (초기화 시점) arugument가 false이므로 -> moveNearerToZero now refers to the nested stepForward() function.

while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue) // Eclosing Function의 외부이지만 stepForward 함수를 호출한다. (정확히는 stepForward 함수의 reference를 통해 -> stepForward 함수의 parameter inputNumber에 argument currentValue를 전달한다!) 
}
print("zero!")
// -4... -3... -2... -1... zero!

print(moveNearerToZero(5))  // 6 출력  <- arugument 5는 stepForward의 parameter inputNumber로 전달된다.
print(moveNearerToZero(-5)) // -4 출력

// 참고 - Enclosing Function 실행에 영향을 미치는 변수 currentValue의 값이 1 이상이 되면, stepForward가 아니라 stepBackward를 호출한다고 착각할 수 있지만 아니다!
// currentValue 값이 1 이상이어도 여전히 stepForward를 호출한다. 
currentValue = 1
print(moveNearerToZero(5))  // 6 출력
print(moveNearerToZero(-5)) // -4 출력
```

- [ ]  Enclosing Function 실행에 영향을 미치는 변수 currentValue의 값이 1 이상이 되면, stepForward가 아니라 stepBackward를 호출하는거 아닌가?

moveNearerToZero가 상수이지만 함수 stepForward의 참조를 할당했으니까, 변수 currentValue의 값에 따라 함수 stepBackward의 참조를 할당하게 될 수도 있는거 아닌가...?
⇒ 아니구나. 함수 stepForward의 참조를 할당했으니까, 해당 주소에서 뭔가가 바뀌면 그걸 반영할 수 있다는 뜻이구나. 참조니까 다른 함수의 참조를 가르키도록 변경 가능한게 아니라!

***그렇다면, 상수 moveNearerToZero를 초기화할 때, 그 "초기화하는 시점"에서 할당한 함수의 참조를 계속해서 저장하고 있는건가? 변수 currentValue의 값에 상관 없이?

# 7. Closure -

# 8. Enumerations -

# 9. Structures and Classes (90%)

Structures and classes are general-purpose, flexible constructs that become the building blocks of your program’s code. You define properties and methods to add functionality to your structures and classes.

Unlike other programming languages, Swift doesn’t require you to create separate interface and implementation files for custom structures and classes. (다른 언어는 별도의 인터페이스 및 실행파일을 생성해야 한다.) In Swift, you define a structure or class in a single file, and the external interface to that class or structure is automatically made available for other code to use.

- [ ]  external interface? 예를 들면?

Note: An instance of a class is traditionally known as an *object.* However, Swift structures and classes are much closer in functionality than in other languages, and much of this chapter describes functionality that applies to instances of either a class or a structure type. Because of this, the more general term *instance* is used.
(통상적으로 클래스의 인스턴스를 객체 (object)라고 하지만, Swift에서는 다른 언어에 비해 구조체와 클래스의 기능이 유사하다. 따라서 객체보다 포괄적으로 구조체의 인스턴스, 클래스의 인스턴스 등으로 사용되는 용어인 인스턴스 (instance)라는 표현을 사용한다.)

- [ ]  Swift의 object는 'class의 인스턴스'를 말하는 게 아니었나? structure 및 class의 instance를 통칭하는 거였나? ← 프리코스 과제 참고
    - Swift에서 객체가 될 수 있는 존재는 3가지이다. struct, class, enum 이다. ?????
    (objective-c에서는 class 또는 class 인스턴스만 객체이다.)
    - 야곰님 책 <스위프트 프로그래밍>
        
        "객체라는 표현 대신 인스턴스라는 표현을 사용한다. 인스턴스는 구조체의 인스턴스, 열거형의 인스턴스도 있을 수 있기 때문에 객체와 인스턴스는 같은 표현이 아니다. 클래스의 인스턴트를 객체라고 부른다. 객체는 클래스의 인스턴스만을 가리키는 한정적인 의미이다."
        
    
- [ ]  서브스크립트 설명의 객체도 다시 확인하자...

## Comparing Structures and Classes

Structures and classes have many things in common. Both can:

- Define properties to store values
- Define methods to provide functionality (보통 다른 언어에서는 구조체에 메서드를 생성할 수 없다.)
- Define subscripts to provide access to their values using subscript syntax
- Define initializers to set up their initial state
- Be extended to expand their functionality beyond a default implementation - Extension 기능
- Conform to protocols to provide standard functionality of a certain kind

Classes have additional capabilities that structures don’t have:

- Inheritance enables one class to inherit the characteristics of another.
- Type casting enables you to check and interpret the type of a class instance at runtime. ???
- Deinitializers enable an instance of a class to free up any resources it has assigned.
- Reference counting allows more than one reference to a class instance. ???
    - [ ]  Automatic Reference Counting - [https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)

Class에 추가 기능이 있는 만큼 프로그램의 복잡성이 증가한다. 따라서 일반적인 상황에는 추론하기 쉬운 structure를 사용하고, 꼭 필요할 때만 class를 사용한다.
실질적으로 사용자정의 type의 대부분이 structure 및 enumeration 이다.

- [x]  Choosing Between Structures and Classes - [https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes)
    - Use structures by default. - structure는 인스턴스의 변경사항을 명시적으로 확인 가능하므로 추론하기 쉽다. 다른 언어 대비 메서드 구현, 프로토콜 채택 가능 등 structure 활용도가 높다.
    - Use classes when you need Objective-C interoperability. - 데이터 처리를 위해 Objective-C API를 사용한다면, 또는 데이터 모델을 Objective-C framework에서 정의한 기존의 클래스 hierarchy에 맞춰야 하는 상황이라면, class를 써야할 수 있다.
    - Use classes when you need to control the identity of the data you're modeling. - 코드 전체에서 변경사항이 즉각 반영되어야 하는 경우에 class를 사용한다.
    - identity operator (===)로 동일해야 하는 경우에 사용한다.
    - When you share a class instance across your app, changes you make to that instance are visible to every part of your code that holds a reference to that instance. Use classes when you need your instances to have this kind of identity. Common use cases are file handles, network connections, and shared hardware intermediaries.
    - Use structures along with protocols to adopt behavior by sharing implementations. - 상속 hierarchy 구조는 protocol 상속을 통해 structure에서도 모델링 가능하다. (class는 class 끼리만 상속 가능하지만, protocol을 통한 상속은 class/enum/structure 모두에서도 가능하다는 장점이 있다.)

Note: Classes and actors (행위자) share many of the same characteristics and behaviors. For information about actors, see Concurrency (동시성).

- [ ]  Concurrency - [https://docs.swift.org/swift-book/LanguageGuide/Concurrency.html](https://docs.swift.org/swift-book/LanguageGuide/Concurrency.html)  어렵다... 나중에

- Definition Syntax
    
    structure 및 class를 정의하는 것은 새로운 Swift type을 정의하는 것이므로 UpperCamelCase에 따라 이름을 짓는다. (type이름은 항상 대문자로 시작)
    
    ```swift
    struct Resolution {  // describe a pixel-based display resolution.
        var width = 0
        var height = 0
    }
    class VideoMode {  // describe a specific video mode for video display.
        var resolution = Resolution() // initialized with a new Resolution structure instance // var resolution: Resolution 이랑 다른가?
        var interlaced = false
        var frameRate = 0.0
        var name: String? // name property is automatically given a default value of nil, or “no name value”, because it’s of an optional type.
    }
    ```
    
- Structure and Class Instances
    
    The Resolution structure definition and the VideoMode class definition only describe what a Resolution or VideoMode will look like. In order to describe a specific resolution or video mode. To do that, you need to create an instance of the structure/class.
    
    ```swift
    let someResolution = Resolution()  // both use initializer syntax for new instances.
    let someVideoMode = VideoMode()
    ```
    
    The simplest form of initializer syntax uses the type name of the class/structure followed by empty parentheses, such as Resolution() or VideoMode(). This creates a new instance of the class/structure, with any properties initialized to their default values. (프로퍼티 기본값으로 초기화된다.)
    
- Accessing Properties
    
    You can access the properties of an instance using *dot syntax.*
    
    You can drill down into subproperties, such as the width property in the resolution property of a VideoMode:
    
    ```swift
    print("The width of someResolution is \(someResolution.width)") // refers to the width property of someResolution, and returns its default initial value of 0.
    // Prints "The width of someResolution is 0"
    
    print("The width of someVideoMode is \(someVideoMode.resolution.width)")
    // Prints "The width of someVideoMode is 0"
    ```
    
- Memberwise Initializers for Structure Types
    
    All structures have an automatically generated *memberwise initializer.* (클래스는 아님 - Unlike structures, class instances don’t receive a default memberwise initializer.)
    
    Initial values for the properties of the new instance can be passed to the memberwise initializer by name:
    
    ```swift
    let vga = Resolution(width: 640, height: 480)
    // Resolution structure는 모든 프로퍼티에 기본값이 있다. -> initializer는 () 그리고 (width: , height: ) 모두 사용 가능하다.
    ```
    

## Structures and Enumerations Are Value Types

A *value type* is a type whose value is *copied* when it’s assigned to a variable/constant, or when it’s passed to a function.

all of the basic types in Swift—integers, floating-point numbers, Booleans, strings, arrays and dictionaries—are value types, and are implemented as structures behind the scenes. 
(Swift의 기본 data type은 structure로 구현되었으므로)

All structures and enumerations are value types in Swift. This means that any structure and enumeration instances you create—and any value types they have as properties—are always *copied* when they’re passed around in your code.

Note: Collections defined by the standard library (like arrays, dictionaries), and strings use an optimization to reduce the performance cost of copying. Instead of making a copy immediately, these collections share the memory where the elements are stored between the original instance and any copies. If one of the copies of the collection is modified, the elements are copied just before the modification. (Collection 및 String type의 경우, 복사로 인한 성능 손실을 최소화하고자 최적화 기능를 사용한다. 처음부터 복사본을 위한 메모리를 새로 할당하지 않고, 일단 원본의 메모리를 공유한다. 그러다가 복사본이 수정되는 상황이 발생하면, 수정 직전에 복사본에 새로운 메모리를 할당한다!)

값 type을 상수/변수에 할당할 때

```swift
let hd = Resolution(width: 1920, height: 1080) // declares a constant called hd, sets it to a Resolution instance initialized with the width/height of full HD video.
var cinema = hd // declares a variable called cinema, sets it to the current value of hd. (*Resolution이 값 type이므로 -> 상수 hd의 현재 값이 할당된다.)
// Because Resolution is a structure, a copy of the existing instance is made, and this new copy is assigned to cinema.
```

When cinema was given the current value of hd, the values stored in hd were copied into the new cinema instance.
Even though hd and cinema have the same width/height, they’re two completely different instances.

![Untitled](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%203.png)

cinema는 변수이므로 Resolution 프로퍼티를 변경 가능하다. cinema는 값이 복사되면서 생성된 새로운 인스턴스를 담고 있으므로 hd의 인스턴스에 아무 영향을 미치지 않는다.
(참고 - hd는 상수이므로 변경 불가하다.)

```swift
cinema.width = 2048

print("hd is still \(hd.width) pixels wide") // Prints "hd is still 1920 pixels wide"
print("cinema is now \(cinema.width) pixels wide") // Prints "cinema is now 2048 pixels wide"
```

Enum도 동일하게 작동한다.

```swift
enum CompassPoint {
    case north, south, east, west
    mutating func turnNorth() {
        self = .north
    }
}
var currentDirection = CompassPoint.west
let rememberedDirection = currentDirection // When rememberedDirection is assigned the value of currentDirection, it’s actually set to a copy of that value.
currentDirection.turnNorth() // currentDirection의 case가 변경되는 것은 복사본의 원본인 rememberedDirection에 아무런 영향이 없다.

print("The current direction is \(currentDirection)") // Prints "The current direction is north"
print("The remembered direction is \(rememberedDirection)") // Prints "The remembered direction is west"
```

## ***Classes Are Reference Types

*reference types* are *not copied* when they’re assigned to a variable/constant, or when they’re passed to a function. 
Rather than a copy, a reference to the same existing instance is used.

참조 type을 상수/변수에 할당할 때

```swift
let tenEighty = VideoMode() // declares a constant called tenEighty, sets it to refer to a VideoMode instance. - 클래스 인스턴스의 참조가 할당되었다. (클래스는 참조 type이므로)
tenEighty.resolution = hd  // Now it’s set to be interlaced (전체 프로퍼티가 변경됨)
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0

// tenEighty is assigned to a new constant, called alsoTenEighty, and the frame rate of alsoTenEighty is modified:
let alsoTenEighty = tenEighty // *tenEighty에 들어있던 클래스 인스턴스의 참조가 alsoTenEighty에 할당되었다. 동일한 참조를 공유하고 있다. 
alsoTenEighty.frameRate = 30.0

// 참조를 공유하므로 한 쪽에서 변경한 내용은 다른 쪽에서 확인해도 변경되어 있다.
print("\(alsoTenEighty.frameRate)") // 30.0
print("The frameRate property of tenEighty is now \(tenEighty.frameRate)") // Prints "The frameRate property of tenEighty is now 30.0"
```

```swift
// 참고 - 비교용
let hd = Resolution() // declares a constant called hd, sets it to a Resolution instance. - 구조체 인스턴스의 값이 할당되었다. (구조체는 값 type이므로)
```

Because classes are reference types, tenEighty and alsoTenEighty actually both refer to the same VideoMode instance. Effectively, they’re just two different names for the same single instance.

![Untitled](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%204.png)

This example also shows how reference types can be harder to reason about. Wherever you use tenEighty, you also have to think about the code that uses alsoTenEighty, and vice versa. 
(참조 type은 추론하기 어렵다. Class instance tenEighty를 쓸 때마다, (참조를 공유하고 있는) alsoTenEighty를 사용하는 코드에 대해서도 생각해야 하고, 그 반대도 동일하다.)

클래스의 let 선언 인스턴스의 프로퍼티 값을 변경 가능한 이유는?

(참고 - Class 인스턴스이므로 let으로 선언해도 '가변 프로퍼티' 값을 변경 가능하다. 단, 참조 정보는 변경 불가하다.)
Note that tenEighty and alsoTenEighty are declared as constants, rather than variables. However, you can still change `tenEighty.frameRate` and `alsoTenEighty.frameRate` because the values of the tenEighty and alsoTenEighty constants themselves don’t actually change. tenEighty and alsoTenEighty themselves don’t “store” the VideoMode instance—instead, they both refer to a VideoMode instance behind the scenes. It’s the frameRate property of the underlying VideoMode that’s changed, not the values of the constant references to that VideoMode.
(상수 tenEighty/alsoTenEighty는 인스턴스의 프로퍼티 값을 저장하고 있지 않다. 참조 (주소)를 가지고 있다.
따라서 `tenEighty.frameRate`를 해도 참조 정보는 변하지 않는다. 변하는 것은 프로퍼티 값이다.)

- Identity Operators
    
    Because classes are reference types, it’s possible for multiple constants/variables to refer to the same single instance of a class. It can be useful to find out whether two constants/variables refer to exactly the same instance of a class.
    
    identity operators: Identical to `===`, Not identical to `!==`
    
    - [ ]  왜 !===가 아니지?
    
    ```swift
    if tenEighty === alsoTenEighty {
        print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
    } // Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
    ```
    
    === : *Identical* to means that two instances (two constants/variables) of class type refer to exactly the same class instance. 
    ==   : *Equal* to means that two instances are considered equal in value.
    
- Pointers
    
    If you have experience with C, C++, or Objective-C, you may know that these languages use pointers to refer to addresses in memory. 
    A Swift constant/variable that refers to an instance of some reference type is similar to a pointer in C, but isn’t a direct pointer to an address in memory, and doesn’t require you to write an asterisk (*) to indicate that you are creating a reference. Instead, these references are defined like any other constant/variable in Swift. 
    
    The standard library provides pointer and buffer types that you can use if you need to interact with pointers directly—see Manual Memory Management.
    
    - [ ]  Swift에서 참조 type은 클래스 밖에 없는거 맞나? 함수, 클로저도 참조 type 이구나...
    - [ ]  Manual Memory Management - [https://developer.apple.com/documentation/swift/swift_standard_library/manual_memory_management](https://developer.apple.com/documentation/swift/swift_standard_library/manual_memory_management)

# 10. Properties -

# 11. Methods -

# 12. Subscripts -

# 13. Inheritance -

# 14. Initialization -

# 15. Deinitialization -

# 16. Optional Chaining -

# 17. Error Handling

# 18. Concurrency

# 19. Type Casting

# 20. Nested Types

# 21. Extensions

# 22. Protocols

# 23. Generics

# 24. Opaque types

# 25. Automatic Reference Counting -

Swift uses *Automatic Reference Counting (ARC)* to track and manage your app’s memory usage. ARC automatically frees up the memory used by class instances when those instances are no longer needed.

## How ARC Works

ARC in Action

Strong Reference Cycles Between Class Instances

Resolving Strong Reference Cycles Between Class Instances

- Weak References
- Unowned References
- Unowned Optional References
- Unowned References and Implicitly Unwrapped Optional Properties

Strong Reference Cycles for Closures

Resolving Strong Reference Cycles for Closures

- Defining a Capture List
- Weak and Unowned References
- 

---

# 🦜 Language Reference

# 1. Lexical Structure

# 2. Types -

- Type 구분 - Named Type / Compound Type
    
    Swift는 전체 Type을 두 가지로 구분한다. 또한 Type 앞뒤로 ()를 붙일 수 있고, 아무런 효과가 없다.
    
    - Named Type (명명된 타입) : 정의할 때 특정한 이름을 붙여서 선언하는 타입이다. Swift standard library에서 정의한, Structure 기반의 기본 Data Type (Int, String 등), 사용자-정의-Type (Class, Structure, Enumeration, Protocol) 그리고 Swift standard library에서 정의한 Arrays, Dictionaries, and Optional 등이 속한다. 
    예를 들어 사용자 정의 Class type인 MyClass의 인스턴스가 있다면, 해당 인스턴스의 타입 이름은 MyClass 타입이다. 
    Extension을 사용하여 기능을 확장 가능하다.
    - Compound Type (복합 타입) : 별도의 명명 없이 Swift 언어로 정의되는 타입이다. Function type과 Tuple type이 속한다.
    Named Type 또는 다른 Compound Type을 포함할 수 있다. 
    예를 들어 Tuple type의 (Int, (Int, Int))이 있다. (첫번째 element는 Int type, 두번째 element는 다른 Compound Type인 (Int, Int)이다.)
- Type Annotation / Type Identifier / Type Inference
    - Type Annotation
        
        A `type annotation` explicitly specifies the type of a variable or expression.
        Write a type annotation by placing a colon(:) after the constant/variable name, followed by the type name.
        
        Type annotations can contain an optional list of type attributes before the type. `: attributes opt inoutopt type`
        
        - [x]  Type Attribute?
            - An attribute provides additional information about the 1) declaration or 2) type. (2가지 Attribute가 있음)
                
                1) Declaration - Specify an attribute by the attribute’s name (and any arguments that the attribute accepts). `@attribute name(attribute arguments)`
                
                2) Type Attribute - 예를 들어 `@autoclosure`
                
    - Type Identifier
        
        A `type identifier` refers to either a named type or a type alias of a named or compound type.
        
        ```swift
        // ex-1. named type을 refer하는 경우
        var exam1a: Int = 0
        var exam1b: Dictionary<String, Int> = ["key1":100]
        
        // ex-2. type alias를 refer하는 경우 
        typealias Point = (Int, Int)  // tuple type의 type alias
        let origin: Point = (0, 0)
        
        // ex-3. named types declared in other modules or nested within other types를 refer하는 경우 
        var someValue: ExampleModule.MyType  // the named type MyType that’s declared in the ExampleModule module을 refer함
        ```
        
    - Type Inference
        - L/G
            
            `Type inference` enables a compiler to deduce the type of a particular expression automatically when it compiles your code, simply by examining the values you provide.
            It’s rare that you need to write type annotations in practice. If you provide an initial value (*literal value or literal) for a constant or variable at the point that it’s defined, Swift can almost always infer the type to be used for that constant or variable, as described in Type Safety and Type Inference. 
            (변수 선언 시 초기값을 할당하면, type annotation을 하지 않아도 Swift가 알아서 type을 추론해준다.) - 단, type을 명시해야 버그 대응이 쉽다.
            
            ```swift
            let meaningOfLife = 42  // meaningOfLife is inferred to be of type Int (왜냐하면 소수점이 없으니까!)
            
            let pi = 3.14159  // pi is inferred to be of type Double
            If you don’t specify a type for a floating-point literal, Swift infers that you want to create a Double:
            Swift always chooses Double (rather than Float) when inferring the type of floating-point numbers.
            
            typealias MyNewInt = Int  // Type Alias (타입 별칭)을 활용 가능함 (Int의 별칭을 지정한 것과 같음)
            var age: MyNewInt = 20    // Int로 Type 을 지정한 것과 동일함
            var year: Int = 100 // 기존 Int도 사용 가능
            age = year
            ```
            
        - L/R
            
            Swift uses `type inference` extensively, allowing you to omit the type or part of the type of many variables and expressions in your code.
            you can omit part of a type when the full type can be inferred from context.
            
            타입 추론은 잎에서 루트 방향으로 이루어질 수도 있고, 루트에서 잎 방향으로 이루어질 수도 있다.
            
            - 잎→루트
                
                `var x: Int = 0` // the type of x is inferred by first checking the type of 0 and then passing this type information up to the root (the variable x).
                
            - 루트→잎
                
                the explicit type annotation (: Float) on the constant eFloat causes the numeric literal 2.71828 to have an inferred type of Float instead of Double. ???
                
                - [ ]  Double.???
                
                ```swift
                let e = 2.71828 // The type of e is inferred to be Double.???
                let eFloat: Float = 2.71828 // The type of eFloat is Float. 
                ```
                
            
            Type inference operates at the level of a single expression or statement. (단일 표현식 또는 단일 구문 수준에서 작동한다.)
            This means that all of the information needed to infer an omitted type or part of a type in an expression must be accessible from type-checking the expression or one of its subexpressions. (타입 유추에 필요한 정보는 표현식, 또는 하위 표현식 중 하나를 type-checking하여 접근 가능해야 한다.)
            
- Tuple Type
    - A tuple type is a comma-separated list of types, enclosed in parentheses.
    All tuple types contain two or more types, except for `Void` which is a type alias for the empty tuple type, (). ← Void는 Tuple type이다!
    - When an element of a tuple type has a name, that name is part of the type.
        
        ```swift
        var someTuple = (top: 10, bottom: 12)  // someTuple is of Tuple type (top: Int, bottom: Int)
        someTuple = (top: 4, bottom: 42) // OK: names match
        someTuple = (9, 99)              // OK: names are inferred
        someTuple = (left: 5, right: 5)  // Error: names don't match
        ```
        
- Function Type (밑으로 더 추가 필요)
    - A function type represents the type of a function/method/closure and consists of a parameter and return type separated by an arrow (->) `(parameter type) -> return type`
    - A parameter of the function type `() -> T` (where T is any type) can apply the autoclosure attribute to implicitly create a closure at its call sites.
        - [ ]  autoclosure attribute?
- Metatype Type (밑으로 더 추가 필요)
    
    A *metatype type* refers to the type of any type, including class types, structure types, enumeration types, and protocol types.
    
    ... 지금은 이해 안감
    
- Self Type (해석 필요)
    
    cf. self 프로퍼티와 다름
    
    - The `Self` type isn’t a specific type, but rather lets you conveniently refer to the current type without repeating or knowing that type’s name.
        1. In a protocol declaration or a protocol member declaration, the `Self` type refers to the eventual type that conforms to the protocol. (프로토콜도 타입이다.)
        2. In a structure/class/enumeration declaration, the `Self` type refers to the type introduced by the declaration. 
    - Inside the declaration for a member of a type, the `Self` type refers to that type. (member = 해당 타입의 프로퍼티 및 메서드)
    In the members of a class declaration, `Self` can appear only as follows:
    - As the return type of a method 
    - As the return type of a read-only subscript
    - As the type of a read-only computed property
    - In the body of a method
        - [ ]  type(of:) ?
        
        ```swift
        class Superclass {
            func f() -> Self { return self } // f 메소드의 return type은 Self이다. self (Class 인스턴스 자기 자신)를 return 한다.
        }
        
        let x = Superclass()  // 인스턴스 생성
        print(type(of: x.f())) // Prints "Superclass"
        
        class Subclass: Superclass { }  // Superclass의 자식클래스 Subclass 정의
        
        let y = Subclass()  // 인스턴스 생성
        print(type(of: y.f())) // Prints "Subclass"
        
        let z: Superclass = Subclass()  // 인스턴스 생성
        print(type(of: z.f())) // Prints "Subclass"
        ```
        
        The last part of the example above shows that Self refers to the runtime type Subclass of the value of z, not the compile-time type Superclass of the variable itself.
        
        Inside a nested type declaration, the Self type refers to the type introduced by the innermost type declaration.
        
        The Self type refers to the same type as the type(of:) function in the Swift standard library. Writing Self.someStaticMember to access a member of the current type is the same as writing type(of: self).someStaticMember. ??????????
        

# 3. Expressions

# 4. Statements

# 5.Declarations

# 6. Attributes

# 7. Patterns (90%)

패턴은 값의 구조이다.

패턴은 단일 값 (single value) 또는 복합 값 (composite value)의 구조를 나타낸다. 예를 들어 tuple `(1, 2)` 의 구조는 a comma-separated list of two elements 이다. 
패턴은 '특정 값'이 아니라 '값의 구조'를 나타내므로 다양한 값들과 match 할 수 있다. 패턴 `(x, y)` 은 tuple `(1, 2)` 또는 2개의 element로 구성된 tuple과 match 된다. 
패턴은 값과 매치하는 것 외에도, 복합 값의 일부 또는 전체를 추출하여 각 부분을 상수/변수이름에 bind 할 수 있다.

*Patter matching : The process of checking whether a specific sequence of characters/tokens/data exists among the given data.
Pattern matching is used to determine whether source files of high-level languages are syntactically correct. It is also used to find and replace a matching pattern in a code with another code.

Swift에는 2가지 기본 패턴이 있다. 

1. 모든 종류의 값과 match 되는 패턴 
    
    - 단순 변수/상수, optional binding에서 값을 분해 (destructuring values) 하는데 사용한다. 
    - wildcard patterns, identifier patterns 그리고 이 패턴을 포함하는 모든 value-binding patterns 또는 tuple patterns이 이에 속한다. 
    - 이러한 패턴에 type annotation을 하면, 특정 type의 값만 match 되도록 제한할 수 있다.
    
2. 런타임에서 특정 값에 match 되지 않을 수도 있는 (may fail to match) 패턴
    
    - full pattern matching에 사용하며, match 하려는 값이 런타임 시 없을 수 있다. ???? 
    - enumeration case patterns, optional patterns, expression patterns, and type-casting patterns이 이에 속한다. 
    - a case label of a `switch` statement (switch문의 case), a `catch` clause of a `do` statement (do문의 catch절), or in the case condition of an `if`, `while`, `guard`, `for`-`in` statement (if/while/guard/for-in문의 조건부)에 이 패턴을 사용한다.
    

- Wildcard Pattern
    
    A wildcard pattern matches and ignores any value. (모든 값을 match 하고, 무시한다.) 
    match 할 값을 신경쓰지 않아도 될 때 사용한다.
    
    ```swift
    for _ in 1...3 {
        // Do something three times. - ignoring 'the current value of the range (1...3)' on each iteration of the loop
    }
    ```
    
- Identifier Pattern
    
    An identifier pattern matches any value and binds the matched value to a variable/constant name. (모든 값을 match 하고, match된 값을 상수/변수이름에 bind 한다.)
    
    `let someValue = 42` 상수 선언에서 someValue는 'Int type의 값 42'에 match 되는 identifier pattern이다. match에 성공할 경우, 값 42는 상수이름 someValue에 할당 (assigned, bound) 된다.
    
    변수/상수 선언의 왼쪽 패턴이 identifier pattern인 경우, identifier pattern은 암시적으로 value-binding pattern의 subpattern 이다.
    
- Value-Binding Pattern
    
    A value-binding pattern binds matched values to variable/constant names. (match된 값을 상수/변수이름에 bind 한다.) 
    match 된 값을 상수/변수이름에 bind하는 Value-binding pattern은 `let/var` 키워드로 시작한다.
    
    value-binding pattern 내부의 identifiers pattern은 새로운 상수/변수이름을 match하는 값에 bind 한다. 
    예를 들어 tuple의 element를 분해하고, 각 element의 값을 해당 identifier pattern에 bind 한다. (You can decompose the elements of a tuple and bind the value of each element to a corresponding identifier pattern.)
    
    ```swift
    let point = (3, 2)
    
    switch point {
    case let (x, y): // Bind x and y to the elements of point. <- value-binding pattern let이 내부의 identifiers pattern x, y에 분해된 element의 값 3, 2를 각각 전달/분배한다.
        print("The point is at (\(x), \(y)).")
    } // Prints "The point is at (3, 2)."
    ```
    
    In the example above,`let` distributes to each identifier pattern in the tuple pattern (x, y). (`let`은 각각의 identifier pattern x, y에 분해된 element의 값 3, 2를 전달/분배한다.) 따라서 switch문의 `caselet(x, y):` 및 `case (letx,lety):` 은 동일하게 동작한다.
    
- Tuple Pattern
    
    A tuple pattern is a comma-separated list of zero or more patterns (0개 이상의 패턴의 목록), enclosed in parentheses. 
    Tuple patterns match values of corresponding tuple types. (해당 tuple type의 값과 match 한다.)
    
    Type annotion을 통해 특정한 tuple type에 match 하도록 tuple pattern을 제한할 수 있다. 예를 들어, 상수 선언 `let (x, y): (Int, Int) = (1, 2)`에서 tuple pattern `(x, y): (Int, Int)`은 2개 element가 Int type인 tuple type에만 match 한다.
    
    for-in문 또는 상수/변수 선언에서 tuple pattern이 사용될 경우, 해당 tuple pattern은 wildcard patterns, identifier patterns, optional patterns 또는 이를 포함하는 다른 tuple pattern만 가질 수 있다. 예를 들어 아래 코드을 보면, tuple pattern (x, 0)의 element 0이 expression pattern 이므로 유효하지 않다. 
    
    ```swift
    let points = [(0, 0), (1, 0), (1, 1), (2, 0), (2, 1)]
    
    for (x, 0) in points { // This code isn't valid.
        /* ... */
    }
    ```
    
    tuple pattern의 ()에 single element가 들어있는 경우, 아무런 효과가 없다. tuple pattern은 해당 single element type의 값을 match 한다.
    
    ```swift
    let a = 2        // a: Int = 2  <- 3개 표현은 동일하다.
    let (a) = 2      // a: Int = 2
    let (a): Int = 2 // a: Int = 2
    ```
    
- Enumeration Case Pattern
    
    An enumeration case pattern matches a case of an existing enumeration type. (기존의 Enum type의 case에 match 한다.) 
    switch 문의 case label, if/while/guard/for-in 문의 case condition에서 사용한다.
    
    match 하려는 enumeration case에 연관값이 있는 경우, 해당 enumeration case pattern은 개별 연관값에 대한 element를 가지는 tuple pattern 명시해야 한다.
    
    ```swift
    enum Barcode {
        case upc(Int, Int, Int, Int)
        case qrCode(String)
    }
    ```
    
    또한 enumeration case pattern은 optional에 warpped된 case의 값을 match 할 수 있다. 이러한 축약 문법으로 optional pattern을 생략할 수 있다. 
    optional은 enum으로 구현된다. 따라서 `.none` 및 `.some`은 동일한 swtich 문 내부에 'enum type의 case'로 나타날 수 있다. 
    
    - [x]  What's the difference between optional none (.none for short) and nil?
        - There is no difference, as `.none` (basically it's `Optional.none` ) and `nil` are equivalent to each other. The use of nil is more common and is the recommended convention.
        - `.none` is an enum representation of the `nil` (absence of value) implemented by the Optional<T> enum.
    
    ```swift
    enum SomeEnum { case left, right }
    
    let x: SomeEnum? = .left  // optional에 warpped된 case
    
    switch x {
    case .left:
        print("Turn left")
    case .right:
        print("Turn right")
    case nil:  // case .none: 와 동일함
        print("Keep going straight")
    } // Prints "Turn left"
    ```
    
- Optional Pattern
    
    An optional pattern matches values wrapped in a `some(Wrapped)` case of an `Optional<Wrapped>` enumeration. (optional enum type의 .some case에 wrapped된 값에 match 한다.) 
    
    identifier pattern 뒤에 `?`를 붙인 형태이며, enumeration case pattern과 동일한 위치에서 사용된다. (switch 문의 case label, if/while/guard/for-in 문의 case condition)
    
    *optional pattern은 optional enumeration case pattern에 대한 syntactic sugar이다. 아래 코드는 동일하다.
    
    - [ ]  if case?
    - [ ]  // 이건 무슨 패턴이지?
    if let x = someOptional {
        print(x)
    }
    
    ```swift
    let someOptional: Int? = 42
    
    // Match using an enumeration case pattern.
    if case .some(let x) = someOptional {  // optional은 enum으로 구현되므로 -> enum의 wrapped value (.some)에 대해 case 키워드를 사용 가능하다.
        print(x)
    } // 42
    
    // Match using an optional pattern.
    if case let x? = someOptional {  
        print(x)
    } // 42
    
    // 이건 무슨 패턴이지?
    if let x = someOptional {
        print(x)
    } // 42
    ```
    
    optional pattern은 for-in문에서 optional value가 들어있는 array를 interate 할 때 사용하면 편리하다. non-nil element에 대해서만 loop의 내용을 실행하기 때문이다.
    
    - [ ]  for case in?
    
    ```swift
    let arrayOfOptionalInts: [Int?] = [nil, 2, 3, nil, 5]
    
    for case let number? in arrayOfOptionalInts {  // Match only non-nil values.
        print("Found a \(number)")
    } // Found a 2, Found a 3, Found a 5
    ```
    
- Type-Casting Patterns
    
    type-casting patterns은 `is` pattern 및 `as` pattern 2가지가 있다. 
    is pattern은 switch문의 case label에만 사용한다. is 및 as pattern의 형태는 아래와 같다.
    
    ```swift
    is type
    pattern as type
    ```
    
    The is pattern matches a value if the type of that value at runtime is the same as the type specified in the right-hand side of the is pattern—or a subclass of that type. ??? (is pattern은 런타임에서 해당 값의 type이 'is pattern의 오른편에 명시된 type'과 동일한 경우, 값을 match 한다. (또는 해당 type의 subclass과 동일한 경우 ???))
    
    is pattern behaves like the is operator in that they both perform a type cast but discard the returned type. (is 패턴은 type cast를 수행하지만, return type은 버린다는 점에서 is operator처럼 동작한다.)
    
    as pattern은 런타임에서 해당 값의 type이 'as pattern의 오른편에 명시된 type'과 동일한 경우, 값을 match 한다. (또는 해당 type의 subclass과 동일한 경우 ???)
    
    match가 성공하면, match된 값의 type은 pattern (as pattern의 오른편에 명시된 pattern)에 cast 된다.
    
    - an example that uses a switch statement to match values with is and as patterns.
        
        ```swift
        // Type Casting for Any and AnyObject
        class Movie {
            var name: String
            var director: String
            init(name: String, director: String) { // 자식클래스 본인의 init을 정의했으므로 부모 init이 자동 상속되지 않았다.
                self.name = name
                self.director = director
            }
        }
        
        var things: [Any] = []
        
        things.append(0)
        things.append(0.0)
        things.append(42)
        things.append(3.14159)
        things.append("hello")
        things.append((3.0, 5.0))
        things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman")) // Movie Class의 인스턴스
        things.append({ (name: String) -> String in "Hello, \(name)" }) // 클로저 - 호출하지 않은 상태의 클로저???
        
        print(things) // [0, 0.0, 42, 3.14159, "hello", (3.0, 5.0), SomeName.Movie, (Function)]
        
        for thing in things {
            switch thing {
            case 0 as Int:
                print("zero as an Int") // zero as an Int
            case 0 as Double:
                print("zero as a Double") // zero as a Double
            case let someInt as Int:
                print("an integer value of \(someInt)") // an integer value of 42
            case let someDouble as Double where someDouble > 0:
                print("a positive double value of \(someDouble)") // a positive double value of 3.14159
            case is Double:
                print("some other double value that I don't want to print")
            case let someString as String:
                print("a string value of \"\(someString)\"") // a string value of "hello"
            case let (x, y) as (Double, Double):
                print("an (x, y) point at \(x), \(y)") // an (x, y) point at 3.0, 5.0
            case let movie as Movie:
                print("a movie called \(movie.name), dir. \(movie.director)") // a movie called Ghostbusters, dir. Ivan Reitman
            case let stringConverter as (String) -> String:
                print(stringConverter("Michael")) // Hello, Michael - 여기서 클로저를 호출
            default:
                print("something else")
            }
        }
        ```
        
    
- Expression Pattern
    
    An expression pattern represents the value of an expression. (수식의 값을 나타낸다.) switch문의 case label에서만 사용한다.
    *expression (수식, evaluate를 통해 값으로 환원됨)
    *statement (구문, 실행가능한 독립적인 코드 조각, statement은 expression을 포함할 수 있음)
    
    expression pattern을 통해 나타낸 expression은 Swift 표준 라이브러리의 `~=` operator를 사용하여 input expression의 값과 비교된다. ??? 
    ~= operator가 true를 반환하면, match에 성공한다. Default로 ~= operator는 == operator를 사용하여 동일한 type의 2개 값을 비교한다. 
    
    또한, 아래 예시와 같이 특정 값이 range 내에 포함되었는지 확인하여, 해당 값을 range에 match 할 수 있다.
    
    ```swift
    let point = (1, 2)
    
    switch point { // 여기서 expression은 point 이다 ??? 맞나?
    case (0, 0):
        print("(0, 0) is at the origin.") 
    case (-2...2, -2...2):
        print("(\(point.0), \(point.1)) is near the origin.")  // Prints "(1, 2) is near the origin."
    default:
        print("The point is at (\(point.0), \(point.1)).")
    } 
    ```
    
    You can overload the ~= operator to provide custom expression matching behavior. ??? For example, you can rewrite the above example to compare the point expression with a string representations of points. point가 아니라 points???
    
    - [ ]  ??? int랑 string을 비교한건가 지금???
    
    ```swift
    // Overload the ~= operator to match a string with an integer.
    func ~= (pattern: String, value: Int) -> Bool { // ~= operator가 true를 반환하면, match에 성공함...........???
        return pattern == "\(value)"
    }
    
    switch point {
    case ("0", "0"):  // (0, 0) 이 아니라 ("0", "0")
        print("(0, 0) is at the origin.")
    default:
        print("The point is at (\(point.0), \(point.1)).")  // Prints "The point is at (1, 2)."
    }
    ```
    

# 8. Generic Parameters and Arguments

# 9. Summary Of the Grammar

---

# 🦜 API Design Guidelines

출처: [https://swift.org/documentation/api-design-guidelines](https://swift.org/documentation/api-design-guidelines/)

*API : Application Programming Interface

## Fundamentals

1. 사용 시점을 기준으로 명확하게 작성하는 것이 가장 중요한 목표이다. (Clarity at the point of use is your most important goal.) 메서드 및 프로퍼티와 같은 Entities는 한 번 선언하면 반복적으로 사용한다. 이러한 Entities를 명확하고 간결하게 사용하도록 API를 설계해야 한다. 디자인을 검토할 때 선언부를 읽는 것만으로는 부족하다. 항상 사용 시점의 상황을 검토하여 문맥상 명확한지 확인해야 한다.
Entities such as methods and properties are declared only once but used repeatedly. Design APIs to make those uses clear and concise. When evaluating a design, reading a declaration is seldom sufficient; always examine a use case to make sure it looks clear in context. 
    
    *Entity : 저장되고, 관리되어야 하는 데이터의 집합이다.
    An entity is represented by an instance of the NSEntityDescription class. This class provides access to a wide range of properties, such as its name, the data model it is defined in, and the name of the class the entity is represented by. ???
    

1. 명확성이 간결성보다 중요하다. (Clarity is more important than brevity.) 단순히 글자수를 줄여서 가장 짧은 코드를 만드는 것이 목표가 아니다. Swift 코드의 간결성은 강력한 type 시스템과 자연스럽게 반복적으로 재사용하는 코드를 줄이는 기능으로 인해 나타난 부수적인 효과일 뿐이다. 
Although Swift code can be compact, it is a *non-goal* to enable the smallest possible code with the fewest characters. Brevity in Swift code, where it occurs, is a side-effect of the strong type system and features that naturally reduce boilerplate. 
    
    *boilerplate : 표준 형식. boilerplate는 반복적으로 재사용하는 코드를 의미한다.
    

1. 모든 선언에 대해 문서화 주석을 작성한다. 문서 작성을 통해 얻는 통찰력은 디자인에 큰 영향을 미칠 수 있다. 
Write a documentation comment for every declaration. Insights gained by writing documentation can have a profound impact on your design, so don’t put it off. 
    
    Note: API의 기능에 대해 간단히 설명할 수 없으면, API를 잘못 설계했을 가능성이 있다.
    
    - Xcode 자동완성에서 확인 가능하도록 Swift Markdown을 사용한다.
        - [ ]  [https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497-CH2-SW1](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497-CH2-SW1)
    - 먼저, 요약을 통해 선언한 Entity에 대해 설명한다. API 자체는 선언 부분과 요약만으로도 완벽히 이해할 수 있다.
        
        ```swift
        /// Returns a "view" of `self` containing the same elements in reverse order.
        func reversed() -> ReverseCollection
        ```
        
        - 가능하면 1개의 문구 (a single sentence fragment)로 작성하고, 마침표로 끝낸다. 완전한 문장 (complete sentence)을 사용하지 않는다.
        - 함수가 수행하는 작업과 반환하는 내용을 설명한다. null 효과 및 Void 반환은 설명을 생략한다.
            
            ```swift
            /// Inserts `newHead` at the beginning of `self`.
            mutating func prepend(_ newHead: Int)
            
            /// Returns a `List` containing `head` followed by the elements of `self`.
            func prepending(_ head: Element) -> List
            
            /// Removes and returns the first element of `self` if non-empty; returns `nil` otherwise.
            mutating func popFirst() -> Element?
            // Note: 드물게 popFirst 함수와 같이 요약문구가 여러 문장인 경우, 문장을 ;으로 구분한다.
            ```
            
        - Subscript가 접근하는 대상을 설명한다.
        - 이니셜라이저가 생성하는 대상을 설명한다.
        - 그 외의 경우, 선언된 Entity(프로퍼티 및 메서드 등)가 무엇인지 설명한다.
            
            ```swift
            /// Accesses the `index`th element.
            subscript(index: Int) -> Element { get set }
            
            /// Creates an instance containing `n` repetitions of `x`.
            init(count n: Int, repeatedElement x: Element)
            
            /// A collection that supports equally efficient insertion/removal at any position.
            struct List {
            
              /// The element at the beginning of `self`, or `nil` if self is empty.
              var first: Element?
              ...
            ```
            
        
    - 필요 시 1개 이상의 문단 (paragraphs)과 글머리 기호 (bullet items)를 추가한다. 문단은 한 줄을 띄어쓰고 (separated by blank), 완전한 문장 (complete sentences)을 사용한다.
        
        ```swift
        /// Writes the textual representation of each    ← Summary
        /// element of `items` to the standard output. 
        /// (각 'items' element의 텍스트 표현을 표준 출력에다 적는다.) 
        ///                                              ← Blank line
        /// The textual representation for each item `x` ← Additional discussion (추가 설명)
        /// is generated by the expression `String(x)`.
        ///
        /// - Parameter separator: text to be printed    ⎫
        ///   between items.                             ⎟
        /// - Parameter terminator: text to be printed   ⎬ Parameters section
        ///   at the end.                                ⎟
        ///                                              ⎭
        /// - Note: To print without a trailing          ⎫
        ///   newline, pass `terminator: ""`             ⎟
        ///                                              ⎬ Symbol commands (기타 참고사항)
        /// - SeeAlso: `CustomDebugStringConvertible`,   ⎟
        ///   `CustomStringConvertible`, `debugPrint`.   ⎭
        public func print(
          _ items: Any..., separator: String = " ", terminator: String = "\n") // 가변 매개변수니까 argument items는 array 형태로 전달된다.
        ```
        
        - 요약 외의 추가 정보는 symbol documentation markup을 사용하여 제공한다.
            - [ ]  [https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW1](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW1)
        - symbol command syntax를 사용한다. Xcode는 아래 키워드로 시작하는 글머리 기호 (bullet items)를 취급하는 약속된 방식이 있다.
        Attention	Author Authors Bug Complexity Copyright Date Experiment Important Invariant Note Parameter Parameters	Postcondition	Precondition Remark Requires Returns	SeeAlso Since Throws ToDo Version Warning

## Naming (이름 짓기)

### Promote Clear Usage (이름으로 명확한 사용법을 제시하기)

- 코드를 읽는 사람이 명확히 이해하도록 필요한 모든 단어를 사용한다.
    
    ex. Collection 내부의 주어진 위치에 있는 element를 제거하는 메서드를 고려한다면,
    
    ```swift
    extension List {
      public mutating func remove(at position: Index) -> Element
    }
    employees.remove(at: x) - 좋은 예시. x가 삭제할 element의 위치를 나타내는 것이 명확하다.
    employees.remove(x) - 나쁜 예시. x라는 element를 제거한다는 의미로 오인할 수 있다.
    ```
    
- 불필요한 단어는 생략한다. 이름의 모든 단어는 사용 시점에서 핵심적인 정보를 전달해야 한다. 특히 단순히 type 정보를 중복으로 제시하는 단어는 생략한다.
    
    ```swift
    public mutating func remove(member: Element) -> Element? 
    allViews.remove(cancelButton) - 좋은 예시. 명확하다.
    
    public mutating func removeElement(member: Element) -> Element?
    allViews.removeElement(cancelButton) - 나쁜 예시. 함수 호출 시 Element라는 단어는 핵심 정보가 아니다.
    ```
    
- 변수/매개변수/연관 type의 이름은 type 제약 조건이 아니라 "역할"에 따라 지정한다.
    
    ```swift
    var greeting = "Hello" // 좋은 예시. Entity의 역할을 표현하는 이름을 사용하는 것이 좋다.
    protocol ViewController {
    	  associatedtype ContentView : View
    }
    class ProductionLine {
    	  func restock(from supplier: WidgetFactory)
    }
    
    var string = "Hello" // 나쁜 예시. 이처럼 type 이름을 반복해서 사용 (type name의 용도 변경 ???) 하는 것은 명확성과 표현력을 떨어트린다.
    protocol ViewController {
    	  associatedtype ViewType : View
    }
    class ProductionLine {
    	  func restock(from widgetFactory: WidgetFactory)
    }
    ```
    
    If an associated type is so tightly bound to its protocol constraint that the protocol name is the role, avoid collision by appending `Protocol` to the protocol name:
    (연관 type이 protocol 제약사항과 밀접한 연관이 있어 protocol 이름이 해당 역할 자체인 경우, protocol 이름에 `Protocol`을 붙여서 충돌을 방지한다.)
    
    - [ ]  protocol constraint ???
    
    ```swift
    protocol Sequence {
      associatedtype Iterator : IteratorProtocol
    }
    protocol IteratorProtocol { ... }
    ```
    
- Compensate for weak type information to clarify a parameter’s role. (parameter의 역할을 명시하기 위해 부족한 type 정보를 보완한다.) weak type ???
parameter type이 NSObject, Any, AnyObject, or a fundamental type (Int, String 등)인 경우, 사용 위치에서 type 정보가 불명확하여 해당 문맥이 의도를 충분히 나타내기 어려울 수 있다.
    
    ```swift
    func addObserver(_ observer: NSObject, forKeyPath path: String) // 좋은 예시. weak type parameter인 path의 역할을 설명하는 명사(forKeyPath)를 붙여서 정보를 보완했다.
    grid.addObserver(self, forKeyPath: graphics) // clear
    
    func add(_ observer: NSObject, for keyPath: String) // 나쁜 예시. 선언은 명확하지만, 사용 시점 (use site)에서 모호하다.
    grid.add(self, for: graphics) // vague
    ```
    

### *Strive for Fluent Usage (쉽고 명확하게 읽히도록 작성하기)

- 함수를 사용하는 위치에서 함수 이름이 '영어 문장의 형태'가 되도록 한다. Prefer function names that make use sites form grammatical English phrases.
    
    ```swift
    x.insert(y, at: z)          “x, insert y at z” (x에 y를 z위치에 삽입)  // 좋은 예시
    x.subViews(havingColor: y)  “x's subviews having color y” (x의 subviews는 색상 y을 가짐)
    x.capitalizingNouns()       “x, capitalizing nouns” (x의 명사를 대문자화)
    
    x.insert(y, position: z)  // 나쁜 예시
    x.subViews(color: y)
    x.nounCapitalize()
    ```
    
    첫번째 또는 두번째 argument 다음에 전달하는 값들이 중요하지 않으면, 가독성을 위해  해당 값들 앞에 줄 바꿈하는 것을 허용한다.
    
    ```swift
    AudioUnit.instantiate(
      with: description, 
      options: [.inProcess], completionHandler: stopProgressBar)  // 중요도가 낮은 arguments이므로 의도적으로 줄바꿈 한다.
    ```
    
- factory method의 이름은 `make`로 시작한다. ex. `x.makeIterator()`
    - [ ]  factory method??? 설명 이해 안됨
- The first argument to initializer and factory methods calls should not form a phrase starting with the base name. ex. `x.makeWidget(cogCount: 47)`
(앞서 함수를 사용하는 시점에서 함수 이름이 '영어 문장의 형태'가 되어야 한다고 했다. 하지만 그렇다고해서 (이니셜라이저 및 factory method 호출 시) 첫번째 argument를 포함하여 영어 문장을 구성해서는 안된다.)
    
    ```swift
    let foreground = Color(red: 32, green: 64, blue: 128) // 좋은 예시. 이니셜라이저 및 factory method 호출 시, 첫번째 argument를 문장의 일부로 만들지 않았다. 
    let newPart = factory.makeWidget(gears: 42, spindles: 14)
    
    let foreground = Color(havingRGBValuesRed: 32, green: 64, andBlue: 128) // 나쁜 예시. 첫번째 argument에 추가 설명을 덧붙여 문장을 만들었다.
    let newPart = factory.makeWidget(havingGearCount: 42, andSpindleCount: 14)
    ```
    
    In practice, this guideline along with those for argument labels means the first argument will have a label unless the call is performing a value preserving type conversion. ???
    
    실제로 이 가이드라인과 argument label에 관한 가이드라인은??? 첫번째 argument가 argument label을 가진다는 것을 의미한다. 함수 호출을 통해 값을 유지하는 type 변환을 실행하지 않는다면.
    
    ```swift
    let rgbForeground = RGBColor(cmykForeground)
    ```
    
- 함수 이름은 Side-effects (부작용 ???) 여부에 따라 지정한다.
    - [ ]  부작용?
    - side-effects가 없는 경우 명사구로 지정한다. ex. `x.distance(to: y)`, `i.successor()`
    - side-effects가 있는 경우 명령형 동사구 (imperative verb phrases)로 지정한다. ex. `print(x)`, `x.sort()`, `x.append(y)`
    - 가변 (mutating) 및 불변 (nonmutating) 메서드 pair의 이름은 일관성 있게 지정한다. 
    A mutating method will often have a nonmutating variant with similar semantics, but that returns a new value rather than updating an instance in-place. ????
        
        ```swift
        // 참고 - mutating 키워드 : 구조체의 메서드가 구조체 내부에서 데이터 수정 할떄는 Mutating 키워드를 선언 해주어야함
        struct Point {
        var x = 0 
        var y = 0 
        
        mutating func moveTo(x: Int, y: Int) {
        	self.x = x  // mutating 키워드가 없으면 컴파일 에러 발생
        	self.y = y 
          }
        }
        ```
        
        - 메서드의 동작이 동사에 의해 자연스럽게 설명될 때, *가변 메서드의 이름으로 명령형 동사를 사용한다. *불변 메서드의 이름에 "ed" 또는 "ing" suffix를 붙인다.
            
            - 가변 메서드 :  `x.sort()`, `x.append(y)`
            
            - 불변 메서드 :  `z = x.sorted()`, `z = x.appending(y)`
            
            - 동사의 과거분사 (보통 "ed"를 붙임)를 사용하여 불변 메서드의 이름을 지정한다.
                
                ```swift
                /// Reverses `self` in-place. (뒤집음) <- 비교용
                mutating func reverse()
                
                /// Returns a reversed copy of `self`. (뒤집힌 상태의 복사본을 반환함)
                func reversed() -> Self
                ...
                x.reverse()
                let y = x.reversed()
                ```
                
            - 동사가 목적어를 가지므로 "ed"를 붙이는 것이 문법에 맞지 않을 경우, 동사의 현재분사 (보통 "ing"를 붙임)를 사용하여 불변 메서드의 이름을 지정한다.
                
                ```swift
                /// Strips all the newlines from `self` (줄바꿈을 제거함) <- 비교용
                mutating func stripNewlines()
                
                /// Returns a copy of `self` with all the newlines stripped. (줄바꿈을 제거한 상태의 복사본을 반환함)
                func strippingNewlines() -> String
                ...
                s.stripNewlines()
                let oneLine = t.strippingNewlines()
                ```
                
        - 메서드의 동작이 명사에 의해 자연스럽게 설명될 때, *불변 메서드의 이름으로 명사를 사용하고, *가변 메서드의 이름에 "from" prefix를 붙인다.
            
            - 불변 메서드 : `x = y.union(z)`, `j = c.successor(i)`
            
            - 가변 메서드 : `y.formUnion(z)`, `c.formSuccessor(&i)`
            
        - 불변 메서드/프로퍼티일 때, Boolean 메서드 및 프로퍼티는 receiver (반환값)에 대한 주장 (Assertion) 형태로 이름을 지정한다. ex. `x.isEmpty`, `line1.intersects(line2)`
        - 무언가를 설명하는 Protocol은 명사로 이름을 지정한다. ex. `Collection`
        - 기능 (capability)을 설명하는 Protocol은 "able, ible, ing" suffix를 붙인다. ex. `Equatable`, `ProgressReporting`
        - 이외 type, 프로퍼티, 변수/상수는 명사로 이름을 지정한다.

### Use Terminology Well (용어를 제대로 사용하기)

*Term of Art : 특정 분야에서 통용되는 전문적인 용어를 의미한다.

- 일반적인 단어가 의미를 잘 전달할 수 있다면 모호한 용어는 피한다. “피부(skin)”가 목적에 맞는 용어라면 “표피(epidermis)"를 사용하지 않는다. Terms of art는 중요한 커뮤니케이션 수단이지만, 의미를 엄밀히 구분할 필요가 있을 때만 사용한다.
- Term of Art를 사용한다면 기존의 의미에 충실해야 한다.
    - API는 받아들여지는 의미에 따라 엄격하게 용어를 사용해야 한다.
        
        - 전문가를 놀라게 하지 않는다: 기존 용어에 새로운 의미를 지어내면, 이미 해당 용어에 익숙한 사람이 당황할 수 있다.
        
        - 초보자를 혼란스럽게 하지 않는다: 초보자는 웹 검색을 통해 기존에 통용되는 의미를 받아들인다.
        
- 약어 (abbreviations)를 피한다. Abbreviations, especially non-standard ones, are effectively terms-of-art, because understanding depends on correctly translating them into their non-abbreviated forms. ??? (특히 표준이 아닌 약어는 풀어쓰는 과정에서 오인될 수 있다.) 약어를 사용한다면 웹 검색을 통해 쉽게 찾을 수 있어야 한다.
- 선례 (precedent)를 따른다.  초보자를 위해 용어를 최적화하지 않는다.
    
    - 예시-1. 초보자는 List를 더 쉽게 이해할 수 있을지라도 Array를 사용하는 게 좋다. Array는 모든 프로그래머가 익숙한 기초 용어이기 때문이다.
    
    - 예시-2. 수학 등 특정 프로그래밍 Domain에서도 프로그래머/수학자에게 익숙한 용어인 "sin(x)"을 사용하는 게 좋다. 원칙적으로는 "sine"이 complete word 임에도 불구하고, 약어가 더 많이 통용되기 때문이다. (In this case, precedent outweighs the guideline to avoid abbreviations.)
    

## Conventions (규칙)

### General Conventions (일반적인 규칙)

- complexity가 O(1)이 아닌 연산 프로퍼티는 모두 문서화하여 설명한다. (Document the complexity of any computed property that is not O(1).) 종종 프로퍼티 접근이 중요한 연산을 거치지 않는다고 생각한다. 이는 사람들이 저장 프로퍼티를 mental model로 여기기 때문이다. 하지만 그렇지 않을 때가 있다.
    
    *mental model : 어떤 사물을 기억하기 위해 중요한 특징이라고 인식하는 것이다. 여기서는 프로퍼티를 기억할 때 보통 저장 프로퍼티를 먼저 떠올린다는 의미이다.
    
- free function (자유 함수???) 보다 메서드 및 프로퍼티가 선호된다. free function은 특수한 상황에서만 사용한다.
    1. 명확히 self가 없을 때
    2. 함수가 제약 없는 제네릭 (unconstrained generic)일 때 ???
    3. 함수 문법이 기존 도메인 표기법을 따를 때
        
        ```swift
        min(x, y, z) // 1
        print(x)     // 2
        sin(x)       // 3
        ```
        
- 대소문자 표기법 (Case conventions)을 따른다. Type 및 Protocol의 이름은 UpperCamelCase를 사용하고, 그외는 모두 lowerCamelCase를 사용한다.
    
    미국식 영어에서 대문자로 표현하는 Acronyms (두문자어, 머리글자만 따서 만든 단어) 및 Initialisms (이니셜)은 일관되게 대문자 또는 소문자로 사용한다.
    
    ```swift
    var utf8Bytes: [UTF8.CodeUnit]
    var isRepresentableAsASCII = true
    var userSMTPServer: SecureSMTPServer
    ```
    
    그외 Acronyms은 일반적인 단어로 취급한다.
    
    ```swift
    var radarDetector: RadarScanner
    var enjoysScubaDiving = true
    ```
    
- 동일한 기본 의미를 갖고 있거나, 특정 domain에서만 동작할 때 메서드는 base name을 공유할 수 있다. 
*iff : (수학 용어) if and only if (필요충분조건을 나타냄)
    
    ```swift
    // 메서드가 본질적으로 동일하게 동작하므로 base name을 공유한다. (parameter type은 다르지만)
    extension Shape {  
      /// Returns `true` iff `other` is within the area of `self`.
      func contains(_ other: Point) -> Bool { ... }
    
      /// Returns `true` iff `other` is entirely within the area of `self`.
      func contains(_ other: Shape) -> Bool { ... }
    
      /// Returns `true` iff `other` is within the area of `self`.
      func contains(_ other: LineSegment) -> Bool { ... }
    }
    ```
    
    geometric types ??? 및 collections은 별도의 domain 이므로 동일한 프로그램 내에서 메서드 이름이 동일해도 된다.
    
    ```swift
    extension Collection where Element : Equatable {
      /// Returns `true` iff `self` contains an element equal to `sought`.
      func contains(_ sought: Element) -> Bool { ... }
    }
    ```
    
    index 메서드는 다른 의미를 가지므로 서로 다른 이름으로 지정해야 한다.
    
    ```swift
    extension Database {  // 나쁜 예시. 메서드의 의미 및 동작이 다르므로 다른 이름을 지정해야 한다.
      /// Rebuilds the database's search index
      func index() { ... }
    
      /// Returns the `n`th row in the given table.
      func index(_ n: Int, inTable: TableID) -> TableRow { ... }
    }
    ```
    
    type inference 기능으로 인한 모호성이 발생하므로 “overloading on return type”을 피한다. (return type이 다른 경우 overloading이 가능하지만, 함수이름을 다르게 지정하는 것이 낫다.)
    
    ```swift
    extension Box {  // 나쁜 예시. return type이 다르므로 다른 이름을 지정하는게 좋다.
      /// Returns the `Int` stored in `self`, if any, and `nil` otherwise.
      func value() -> Int? { ... }
    
      /// Returns the `String` stored in `self`, if any, and `nil` otherwise.
      func value() -> String? { ... }
    }
    ```
    

### Parameters

```swift
func move(from start: Point, to end: Point)
```

- 문서의 가독성을 높이는 parameter 이름을 지정한다. 함수 사용 시점에 parameter 이름이 보이지 않을 수 있지만, 함수를 설명하는 중요한 역할을 한다.
    
    ```swift
    /// Return an `Array` containing the elements of `self` that satisfy `predicate`.  // 좋은 예시. 자연스럽게 읽힌다.
    func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]
    
    /// Replace the given `subRange` of elements with `newElements`.
    mutating func replaceRange(_ subRange: Range, with newElements: [E])
    
    /// Return an `Array` containing the elements of `self` that satisfy `includedInResult`.  // 나쁜 예시. 어색하거나 문법에 맞지 않는다.
    func filter(_ includedInResult: (Element) -> Bool) -> [Generator.Element]
    
    /// Replace the range of elements indicated by `r` with the contents of `with`.
    mutating func replaceRange(_ r: Range, with: [E])
    ```
    
- 매개변수 기본값 (defaulted parameter)을 사용하여 함수 사용을 단순화한다. 흔히 사용하는 값 1개를 전달받는 parameter는 매개변수 기본값을 가질 확률이 높다.
    
    ```swift
    let order = lastName.compare(royalFamilyName)  // 좋은 예시. 함수 사용이 간단하다.
    
    let order = lastName.compare(  // 나쁜 예시. 관련 없는 정보를 숨기지 않아서 가독성이 나쁘다.
      royalFamilyName, options: [], range: nil, locale: nil)
    ```
    
    매개변수 기본값은 보통 method family (메서드 집합) 보다 선호되는데, API를 읽을 때 부담이 적기 때문이다.
    
    ```swift
    extension String {  // 좋은 예시. 읽을 때 부담이 적다.
      /// ...description...
      public func compare(
         _ other: String, options: CompareOptions = [],
         range: Range? = nil, locale: Locale? = nil
      ) -> Ordering
    }
    
    extension String {  // 나쁜 예시. method family는 읽기 복잡하다. (다른 개수의 parameter로 overload)
      /// ...description 1...
      public func compare(_ other: String) -> Ordering
      /// ...description 2...
      public func compare(_ other: String, options: CompareOptions) -> Ordering
      /// ...description 3...
      public func compare(_ other: String, options: CompareOptions, range: Range) -> Ordering
      /// ...description 4...
      public func compare(_ other: String, options: StringCompareOptions, range: Range, locale: Locale) -> Ordering
    }
    ```
    
    사용자 입장에서는 method family의 모든 member에 대한 개별적인 설명을 읽고 이해해야만 메서드를 사용할 수 있다. method family 중에서 사용할 메서드를 결정하려면, 모든 메서드를 이해해야 하기 때문이다. 
    또한 가끔 예상치 못한 상황에서 (예를 들어 `foo(bar: nil)` 및 `foo()`는 항상 동의어가 아님 ???) 미세한 차이를 찾기 위해 전체 문서를 확인해야 하는 불편함을 초래한다. 따라서 매개변수 기본값을 사용하는 단일 메서드를 사용하면 보다 나은 개발이 가능하다.
    
- 매개변수 기본값이 있는 parameter는 parameter list의 끝부분에 배치한다. 매개변수 기본값이 없는 parameter가 메서드의 의미상 더 중요하고, 메서드 호출 위치에서 안정적인 초기화 형태를 제공하기 때문이다.

### Argument Labels

```swift
func move(from start: Point, to end: Point)
x.move(from: x, to: y)
```

- argument label이 argument를 구분하는데 유용하지 않으면, argument label을 생략한다. ex. `min(number1, number2)`, `zip(sequence1, sequence2)`
- 이니셜라이저가 value 보존 type 변환 (value preserving type conversions ???)을 수행할 때, 첫번째 argument label을 생략한다. ex. `Int64(someUInt32)`
    
    ```swift
    extension String {  // 좋은 예시. type 변환 시, 첫번째 argument는 항상 변환 대상 (source)이다.
      // Convert `x` into its textual representation in the given radix. (*radix : 진법)
      init(_ x: BigInt, radix: Int = 10)   ← Note the initial underscore 
    }
    
    text = "The value is: "
    text += String(veryLargeNumber)
    text += " and in hexadecimal, it's"
    text += String(veryLargeNumber, radix: 16)
    ```
    
    단, “narrowing” type 변환 시, narrowing을 설명하는 argument label을 사용한다. 
    *정밀한 (narrowing) type 변환은 레이블에 정밀도를 표현한다. (아래 코드에서 64비트에서 32비트로 그냥 줄어들 때는 truncating를, 64비트에서 32비트 근사값을 처리할 때는 saturating로 표기하고 있다.)
    
    ```swift
    extension UInt32 {
      /// Creates an instance having the specified `value`.
      init(_ value: Int16)            ← Widening, so no label
      /// Creates an instance having the lowest 32 bits of `source`.
      init(truncating source: UInt64)
      /// Creates an instance having the nearest representable approximation of `valueToApproximate`. (표현 가능한 가장 가까운 근사치) *approximation : 근사치
      init(saturating valueToApproximate: UInt64)
    }
    ```
    
    값 보존 type 변환 (a value preserving type conversion)은 *monomorphism (단형성)이다. 즉, 변환 대상 (source)인 값의 차이는 결과값의 차이를 초래한다. ???
    예를 들어, Int8에서 Int64로의 type 변환은 값을 보존한다. 반면, 반대 방향으로의 type 변환은 값 보존이 불가하다. Int64의 값은 Int8로 표현 가능한 값보다 더 많기 때문이다.
    
    Note: 원래 값을 얻는 수 있는지 (the ability to retrieve the original value)는 해당 type 변환이 값을 보존하는지 여부와 관련이 없다. ????? (값을 보존하지 않더라도 원래 값을 그대로 가져올 수 있는 경우도 있고 값에 따라 다를 수 있다.)
    
    *(OOP 용어) 다형성 (polymorphism)이란 프로그램 언어의 각 요소들 (상수, 변수, 객체, 함수 등)이 다양한 data type에 속하는 것이 허용되는 성질이다. 
    - 단형성 : 함수는 고유의 이름으로 식별되며, 한 가지 의미를 가진다. 따라서 다른 기능을 구현하려면 다른 이름을 사용해야 한다. 
    - 다형성 : 다형성 체계에서는 범용 메소드 이름을 정의하여 형태에 따라 각각 적절한 변환 방식을 정의해두어서 객체의 종류와 상관없이 추상화를 높일 수 있다. (화면에 뿌린다 (render)는 개념과 일맥상통하여 부모클래스가 일괄적으로 가지는 것이 합리적이다.)
    
    ```java
    // 단형성
    string = StringFromNumber(number);
    string = stringFromDate(date);
    
    // 다형성 
    string = number.stringValue();
    string = date.StringValue();
    ```
    
- 첫번째 argument가 전치사구(prepositional phrase)의 일부인 경우, argument label을 사용한다. 이때 argument label은 보통 전치사로 시작한다. ex. `x.removeBoxes(havingLength: 12)`
    
    단, 첫번째 및 두번째 argument가 동일한 추상화 수준인 경우 (when the first two arguments represent parts of a single abstraction. ???), 예외적으로 argument label 앞에 전치사를 적는다.
    
    ```swift
    a.moveTo(x: b, y: c)  // 좋은 예시. 예외 상황에 해당하므로 argument label 앞에 전치사를 적는다.
    a.fadeFrom(red: b, green: c, blue: d)
    
    a.move(toX: b, y: c)  // 나쁜 예시. 
    a.fade(fromRed: b, green: c, blue: d)
    ```
    
- 그와 달리, 첫번째 argument가 문법에 맞는 문장의 일부인 경우, argument label을 생략하고, base name (기본 함수 이름)에 선행 단어 (preceding word)를 붙인다. ex. `x.addSubview(y)`
    
    이는 다른 말로 하면, 첫번째 argument가 문법에 맞는 문장의 일부가 아닐 경우에는 argument label을 사용해야 한다는 뜻이다. 
    
    ```swift
    view.dismiss(animated: false)  // 좋은 예시. argument label을 사용하여 문법에 맞는 문장을 만들었다.
    let text = words.split(maxSplits: 12)
    let studentsByName = students.sorted(isOrderedBefore: Student.namePrecedes)
    x.addSubView(y)  // arugument label 없이도 문법에 맞는 문장이며, 선행 단어 (SubView)를 붙여서 의미를 명확히 했다.
    
    view.dismiss(false)   Don't dismiss? Dismiss a Bool?  // 나쁜 예시. 문법에는 맞지만, 잘못된 의미로 오인될 수 있다. *정확한 의미를 전달할 수 있는 문장을 만드는 것이 중요하다. 
    words.split(12)       Split the number 12?
    ```
    
    Note: 매개변수 기본값이 있는 parameter는 생략 가능하며, 이 경우 해당 parameter를 문장에 포함시키지 않는다. 만약 포함시키면 해당 argument label을 항상 사용해야 하기 때문이다.
    
- 그외의 경우, 모든 argument에 argument label을 지정한다.

## Special Instructions

- API에서 나타나는 위치에 Tuple member의 argument label을 지정하고, 클로저 parameter의 이름을 지정한다. 
이렇게 지정된 이름은 설명 기능을 가지며, 문서화 주석에서 참고할 수 있고, tuple member에 대한 접근성을 높인다.
    
    ```swift
    /// Ensure that we hold uniquely-referenced storage for at least `requestedCapacity` elements. (적어도 `requestedCapacity` element에 대해 uniquely-referenced 저장공간을 확보함을 보장한다.)
    ///
    /// If more storage is needed, `allocate` is called with `byteCount` equal to the number of maximally-aligned bytes to allocate. (추가 저장공간이 필요하면, maximally-aligned 바이트 개수와 동일한 `byteCount`를 사용하여 `allocate`를 호출한다.)
    ///
    /// - Returns:
    ///   - reallocated: `true` iff a new block of memory was allocated.
    ///   - capacityChanged: `true` iff `capacity` was updated.
    mutating func ensureUniqueStorage(
      minimumCapacity requestedCapacity: Int, 
      allocate: (_ byteCount: Int) -> UnsafePointer<Void>
    ) -> (reallocated: Bool, capacityChanged: Bool)
    ```
    
    클로저 parameter의 이름은 상위 함수의 parameter 이름처럼 지정한다. 호출 위치에서 클로저의 argument label은 지원되지 않는다. ???
    
- unconstrained polymorphism (제약되지 않은 다형성)은 overload 하면 모호하게 표현될 수 있으므로 특히 주의한다.
unconstrained polymorphism의 예는 Any, AnyObject, and unconstrained generic parameters 등이다.
    
    ```swift
    struct Array {  // 좋은 예시. overload 시 argument label을 지정하여 의미 및 type을 명확히 한다.
      /// Inserts `newElement` at `self.endIndex`.
      public mutating func append(_ newElement: Element)
    
      /// Inserts the contents of `newElements`, in order, at `self.endIndex`.  <- 주석과 argument label이 일치한다. (이처럼 주석을 작성하는 것은 API 작성자에게 영향을 미친다.)
      public mutating func append(contentsOf newElements: S)
        where S.Generator.Element == Element
    }
    ```
    
    ```swift
    	struct Array {  // overload set의 나쁜 예시. 
      /// Inserts `newElement` at `self.endIndex`.
      public mutating func append(_ newElement: Element)
    
      /// Inserts the contents of `newElements`, in order, at `self.endIndex`.
      public mutating func append(_ newElements: S)
        where S.Generator.Element == Element
    }
    
    // 이 메서드는 semantic family (의미 집합)을 형성하며, 처음에는 parameter type이 뚜렷히 구분되는 것처럼 보인다. 
    // 하지만 Element가 Any type인 경우, '1개 element의 type' 및 'a sequence of elements의 type'이 동일할 수도 있다. ???
    
    var values: [Any] = [1, "a"]
    values.append([2, 3, 4]) // [1, "a", [2, 3, 4]] or [1, "a", 2, 3, 4] ?  <- element의 type이 모호하다.
    ```
    
- 참고 - [https://gist.github.com/godrm/d07ae33973bf71c5324058406dfe42dd](https://www.notion.so/d07ae33973bf71c5324058406dfe42dd)

# 🦜 Coding Guidelines for Cocoa - Naming Methods -

- [ ]  [https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html)

General Rules 

- Start the name with a lowercase letter. Don’t use prefixes.
- two specific exceptions: You may begin a method name with a well-known acronym in uppercase (such as TIFF or PDF)), 
and you may use prefixes to group and identify private methods (see Private Methods).
- For methods that represent actions an object takes, start the name with a verb:
    
    Do not use “do” or “does” as part of the name because these auxiliary verbs rarely add meaning. Also, never use adverbs or adjectives before the verb.
    
    ```swift
    (void)invokeWithTarget:(id)target; ????
    
    (void)selectTabViewItem:(NSTabViewItem *)tabViewItem
    ```
    

Accessor Methods

Delegate Methods

Collection Methods

Method Arguments

Private Methods

---

- Contents
