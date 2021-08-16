# Swift Language Guide & Reference

Created: August 8, 2021 3:14 PM
Created By: 손효주
Last Edited Time: August 16, 2021 10:21 PM
Property: Official

- Contents

# 🦜 Language Guide

# 1. The Basics (70%)

## Integers

Integers are whole numbers with no fractional component, such as 42 and -23. Integers are either signed (positive, zero, or negative) or unsigned (positive or zero).

Swift provides signed and unsigned integers in 8, 16, 32, and 64 bit forms. 
These integers follow a naming convention similar to C (~라는 점에서 C와 유사한 명명 규칙을 따른다.), in that an 8-bit unsigned integer is of type `UInt8`, and a 32-bit signed integer is of type `Int32`.

- Integer Bounds

    You can access the minimum/maximum values of each integer type with its min/max properties:

    ```swift
    let minValue = UInt8.min  // minValue is equal to 0, and is of type UInt8
    let maxValue = UInt8.max  // maxValue is equal to 255, and is of type UInt8
    ```

- Int

    In most cases, you don’t need to pick a specific size of integer to use in your code. Swift provides an additional integer type, `Int`, which has the same size as the current platform’s native word size (CPU architecture 32-bit/64-bit):

    - On a 32-bit platform, `Int` is the same size as `Int32`.
    - On a 64-bit platform, `Int` is the same size as `Int64`.

    Even on 32-bit platforms, Int can store any value between -2,147,483,648 and 2,147,483,647, and is large enough for many integer ranges.

## Floating-Point Numbers

Floating-point numbers are numbers with a fractional component, such as 3.14159, 0.1, and -273.15.

Swift provides two signed floating-point number types:

- `Double` represents a 64-bit floating-point number.
- `Float` represents a 32-bit floating-point number.

Note: Double has a precision of at least 15 decimal digits, whereas the precision of Float can be as little as 6 decimal digits. The appropriate floating-point type to use depends on the nature and range of values you need to work with in your code. In situations where either type would be appropriate, Double is preferred.

## Numeric Literals

- [ ]  Literal?

    예를 들어 문자열 리터럴(String Literal)은 "HelloSwift", "ABCDE" 등이 있다. 정수형 리터럴은 123, 41234, 52394, 2147483647 등이 있다.

Integer literals can be written as:

- A *decimal* number, with no prefix
- A *binary* number, with a `0b` prefix
- An *octal* number, with a `0o` prefix
- A *hexadecimal* number, with a `0x` prefix

Decimal floats can also have an optional exponent (지수가 있거나 없음), indicated by an uppercase/lowercase e; 
hexadecimal floats must have an exponent, indicated by an uppercase/lowercase p.

For decimal numbers with an exponent of `exp`, the base number is multiplied by 10^exp:

- `1.25e2` means 1.25 x 10^2, or `125.0`.
- `1.25e-2` means 1.25 x 10^(-2), or `0.0125`.

For hexadecimal numbers with an exponent of `exp`, the base number is multiplied by 2^exp:

- `0xFp2` means 15 x 2^2, or `60.0`.
- `0xFp-2` means 15 x 2^(-2), or `3.75`.

```swift
// All of these floating-point literals have a decimal value of 12.1875:
let decimalDouble = 12.1875
let exponentDouble = 1.21875e1
```

Numeric literals can contain extra formatting to make them easier to read. 
Both integers and floats can be padded with extra zeros (0을 추가로 채움) and can contain underscores to help with readability. Neither type of formatting affects the underlying value of the literal (리터럴의 기본 값에 영향을 미치지 않는다.):

```swift
let paddedDouble = 000123.456
let oneMillion = 1_000_000
let justOverOneMillion = 1_000_000.000_000_1
```

## Numeric Type Conversion

The range of numbers that can be stored in an integer constant or variable is different for each numeric type. - An Int8 variable can store numbers between -128 and 127 (음수는 -128 ~ -1인 128개 (2^7), 나머지는 (0 및 양수) 0 ~ 127인 128개 (2^7)) → 총 256개 (2^8) 즉, 8비트 중에서 1비트는 부호 표현을 위해 사용되고, 남은 7비트로 숫자를 표현
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

SomeType(ofInitialValue) is the default way to call the initializer of a Swift type and pass in an initial value.
Behind the scenes, UInt16 has an initializer that accepts a UInt8 value, ('UInt16 type의 이니셜라이저'를 통해 초기화했다. UInt8 type의 값을 전달받을 수 있는 것은 'UInt16 type의 이니셜라이저' 중에서 UInt8을 전달받는 것이 있기 때문이다.) and so this initializer is used to make a new UInt16 from an existing UInt8. 
You can’t pass in any type here, however—it has to be a type for which UInt16 provides an initializer. Extending existing types to provide initializers that accept new types (including your own type definitions) is covered in Extensions.

- ex. `init(_ source: Double)` - Creates an integer from the given floating-point value, rounding toward zero.

    ![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled.png)

- Integer and Floating-Point Conversion

    Conversions between integer and floating-point numeric types must be made explicit:

    ```swift
    // Int -> Double
    let three = 3
    let pointOneFourOneFiveNine = 0.14159
    let pi = Double(three) + pointOneFourOneFiveNine // create a new value of type Double
    // pi equals 3.14159, and is inferred to be of type Double

    // Double -> Int
    let integerPi = Int(pi) // Floating-point values are always truncated when used to initialize a new integer value in this way. This means that 3.14159 becomes 3.
    // integerPi equals 3, and is inferred to be of type Int
    ```

    Note: The rules for combining numeric constants and variables are different from the rules for numeric literals. The literal value 3 can be added directly to the literal value 0.14159, because number literals don’t have an explicit type in and of themselves. (숫자 리터럴 자체에는 명시적 형식이 없으므로) ???
    Their type is inferred only at the point that they’re evaluated by the compiler. (컴파일러에 의해 평가되는 시점에서만 type 추론이 가능하다.) 

## Tuple

Tuples group multiple values into a single compound value. The values within a tuple can be of any type and don’t have to be of the same type as each other.

You can create tuples from any permutation (순열) of types. (Int, Int, Int), or (String, Bool)...

Tuples are useful as the return values of functions.

```swift
// a tuple that describes an HTTP status code. (HTTP status code : a special value returned by a web server whenever you request a web page.) - A status code of 404 Not Found is returned if you request a webpage that doesn’t exist.
let http404Error = (404, "Not Found") // http404Error is of type (Int, String). a tuple of type (Int, String)
```

- Tuple 분해 및 접근

    You can decompose a tuple’s contents into separate constants/variables, which you then access as usual: (Tuple의 콘텐츠를 분해 가능하다.)

    ```swift
    // 분해 및 접근-1. 변수/상수이름 지정
    let (statusCode, statusMessage) = http404Error // (Int, String) type의 tuple을 분해하여 각각의 콘텐츠에 대한 상수이름을 지정한다.
    print("The status code is \(statusCode)") // Prints "The status code is 404"
    print("The status message is \(statusMessage)") // Prints "The status message is Not Found"
    ```

    If you only need some of the tuple’s values, ignore parts of the tuple with an underscore (_):

    ```swift
    // 분해 및 접근-2. 일부 지정 및 _ 처리
    let (justTheStatusCode, _) = http404Error // 접근이 필요 없는 부분은 _ 처리한다.
    print("The status code is \(justTheStatusCode)") // Prints "The status code is 404"
    ```

    Alternatively, access the individual element values in a tuple using index numbers starting at zero:

    ```swift
    // 분해 및 접근-3. index
    print("The status code is \(http404Error.0)") // Prints "The status code is 404"
    print("The status message is \(http404Error.1)") // Prints "The status message is Not Found"
    ```

    You can name the individual elements in a tuple when the tuple is defined:

    ```swift
    // 분해 및 접근-4. 선언 시 개별 element에 변수/상수이름 지정
    let http200Status = (statusCode: 200, description: "OK")

    print("The status code is \(http200Status.statusCode)")
    // Prints "The status code is 200"
    print("The status message is \(http200Status.description)")
    // Prints "The status message is OK"
    ```

    *참고 - Tuple은 Type Alias와 함께 사용하면 좋다.

    ```swift
    typealias CoordinateTuple = (x: Int, y: Int)
    ```

## Optionals -

You use optionals in situations where a value may be absent. 
An optional represents two possibilities: Either there is a value, and you can unwrap the optional to access that value, or there isn’t a value at all.

- nil
- If Statements and Forced Unwrapping
- Optional Binding
- Implicitly Unwrapped Optionals

## Error Handling -

## Assertions and Preconditions -

- Debugging with Assertions
- Enforcing Preconditions

# 2. Basic Operators -

# 3. Strings and Characters (90%)

Note: Swift’s String type is bridged with Foundation’s NSString class. Foundation also extends String to expose methods defined by NSString. This means, if you import Foundation, you can access those NSString methods on String without casting.

## String Literals

You can include predefined String values within your code as *string literals*. A string literal is a sequence of characters surrounded by double quotation marks (").

```swift
// Use a string literal as an initial value for a constant/variable:
let someString = "Some string literal value"
```

- Multiline String Literals

    If you need a string that spans several lines, use a multiline string literal—a sequence of characters surrounded by three double quotation marks (""") :
    - neither of the strings below start or end with a line break: (아래의 String은 줄 바꿈으로 시작하거나 끝나지 않는다.)

    ```swift
    let singleLineString = "These are the same."
    let multilineString = """
    These are the same.
    """

    print(singleLineString + multilineString) // These are the same.These are the same.
    print(singleLineString, multilineString)  // These are the same. These are the same.
    ```

    ```swift
    // If you want to use line breaks to make your source code easier to read, but you don’t want the line breaks to be part of the string’s value, 
    // write a backslash (\) at the end (즉, \를 사용하면 소스코드 내에서 line breaks처럼 나타낼 수 있지만 (가독성을 위해 사용), String의 값에는 반영시키지 않을 수 있다.)

    let softWrappedQuotation = """
    The White Rabbit put on his spectacles.  "Where shall I begin, \
    please your Majesty?" he asked.

    "Begin at the beginning," the King said gravely, "and go on \
    till you come to the end; then stop."
    """

    print(softWrappedQuotation)
    // The White Rabbit put on his spectacles.  "Where shall I begin, please your Majesty?" he asked.
    //
    // "Begin at the beginning," the King said gravely, "and go on till you come to the end; then stop."
    ```

    A multiline string can be indented to match the surrounding code. (multiline string이 주변 코드와 일치하도록 들여쓰기 가능하다.)
    - The whitespace before the closing quotation marks tells Swift what whitespace to ignore before all of the other lines. (닫는 따옴표 앞에 공백이 있으면 다른 모든 행 앞에 무시해야 할 공백이 표시된다.)
    - However, if you write whitespace at the beginning of a line in addition to what’s before the closing quotation marks, that whitespace is included. (닫힘 따옴표 앞의 공백에 추가하여 줄의 시작 부분에 공백이 있으면 해당 공백은 string value에 포함된다.)

    ![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%201.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%201.png)

    ```swift
    let linesWithIndentation = """
    		This line doesn't begin with whitespace.    
    				This line begins with four space.
    		This line doesn't begin with whitespace.
    		"""

    //This line doesn't begin with whitespace.
    //    This line begins with four space.
    //This line doesn't begin with whitespace.
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

```swift
var emptyString = ""               // 1) empty string literal
var anotherEmptyString = String()  // 2) initializer syntax - empty 상태의 string type 이다.
```

## String Mutability / Strings Are Value Types

You indicate whether a particular String can be modified (or mutated) by assigning it to a variable

```swift
var variableString = "Horse"
variableString += " and carriage" // variableString is now "Horse and carriage"
```

Note: This approach is different from string mutation in Objective-C and Cocoa, where you choose between two classes (NSString and NSMutableString) to indicate whether a string can be mutated. - Objective-C의 NSString Class는 immutable 하다. (string mutation이 불가하다.)

함수에 전달되거나 변수에 할당될 때, String의 복사 값이 생성되어 전달된다.

cf. C에서 String은 Char의 Array이며, "참조 type"이다.

## Working with Characters

You can access the individual Character values for a String by iterating over the string with a for-in loop:

```swift
for character in "Dog!🐶" {
    print(character)
}
// D  -> 실제로 String으로 for-in loop를 실행하면, Character type이 하나씩 출력된다!
// o
// g
// !
// 🐶
```

- String values can be constructed by passing an array of Character values as an argument to its initializer:

```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
let catString = String(catCharacters) // String initializer에 Character type Array를 전달한다. -> String이 생성된다.
print(catString) // Prints "Cat!🐱"
```

- Concatenating Strings and Characters

    String values can be added together (or concatenated) with +/+= operators, append method.

    ```swift
    let string1 = "hello"
    let string2 = " there"
    var welcome = string1 + string2 // "hello there"

    var instruction = "look over"
    instruction += string2 // "look over there"

    let exclamationMark: Character = "!"
    welcome.append(exclamationMark) // "hello there!"
    ```

- String Interpolation

    String interpolation is a way to construct a new String value from a mix of variables, literals, and expressions by including their values inside a string literal.
    You can use string interpolation in both single-line and multiline string literals.

    ```swift
    let multiplier = 3
    let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)" // \() 내부에서 type 변환도 가능하다.
    // message is "3 times 2.5 is 7.5"

    // The value of multiplier is inserted into a string literal as \(multiplier). 
    // This placeholder is replaced with the actual value of multiplier when the string interpolation is evaluated to create an actual string. 
    // 즉, \() 내부를 evaluate 하여 구한 결과값을 String으로 생성하여 대체한다.
    ```

    ```swift
    // extended string delimiters(##)
    print(#"Write an interpolated string in Swift using \(multiplier)."#) // Prints "Write an interpolated string in Swift using \(multiplier)."
    ```

    To use string interpolation inside a string that uses extended delimiters, match the number of number signs after the backslash to the number of number signs at the beginning and end of the string. ## 내부에 \()를 사용하려면, String 앞뒤에 붙인 #의 개수와 \() 사이의 # 개수를 일치시킨다.

    ```swift
    print(#"6 times 7 is \#(6 * 7)."#)       // 6 times 7 is 42.
    print(##"6 times 7 is \##(6 * 7)."##)    // 6 times 7 is 42.
    print(###"6 times 7 is \###(6 * 7)."###) // 6 times 7 is 42.

    print(##"6 times 7 is \#(6 * 7)."##)     // 6 times 7 is \#(6 * 7). - 참고
    print(###"6 times 7 is \##(6 * 7)."###)  // 6 times 7 is \##(6 * 7). - 참고
    ```

## Unicode ???

Unicode is an international standard for encoding, representing, and processing text in different writing systems.
Swift’s String and Character types are fully Unicode-compliant. (유니코드-호환)

### Unicode Scalar Values

Behind the scenes, Swift’s native String type is built from Unicode scalar values. A Unicode scalar value is a unique 21-bit number for a character or modifier, such as U+0061 for LATIN SMALL LETTER A ("a"), or U+1F425 for FRONT-FACING BABY CHICK ("🐥"). 

Note that not all 21-bit Unicode scalar values are assigned to a character—some scalars are reserved for future assignment or for use in UTF-16 encoding. (일부 Unicode scalar value는 나중에 할당하거나 UTF-16 인코딩에 사용된다.) Scalar values that have been assigned to a character typically also have a name, such as LATIN SMALL LETTER A and FRONT-FACING BABY CHICK in the examples above.

### Extended Grapheme Clusters

*grapheme : 문자소 (의미를 나타내는 최소 문자 단위)

Every instance of Character type represents a single extended grapheme cluster. (문자 유형의 모든 인스턴스는 단일 확장 그래픽 클러스터를 나타낸다.) 
An extended grapheme cluster is a sequence of one or more Unicode scalars that (when combined) produce a single human-readable character. (사람이 읽을 수 있는 단일 문자를 생성하는 하나 이상의 Unicode scalars 시퀀스이다.) ????

- [ ]  ?

The letter é can be represented as the single Unicode scalar é (LATIN SMALL LETTER E WITH ACUTE, or U+00E9). However, the same letter can also be represented as a pair of scalars—a standard letter e (LATIN SMALL LETTER E, or U+0065), followed by the COMBINING ACUTE ACCENT scalar (U+0301). The COMBINING ACUTE ACCENT scalar is graphically applied to the scalar that precedes it, turning an e into an é when it’s rendered by a Unicode-aware text-rendering system. (Accent scalar가 그 앞의 scalar e에 적용되어 é로 변경된다.)

In both cases, the letter é is represented as a single Swift Character value that represents an extended grapheme cluster. (두 경우 모두, 문자 é는 확장된 그래프 분석 클러스터를 나타내는 단일 Swift 문자 값으로 표시된다.) In the first case, the cluster contains a single scalar; in the second case, it’s a cluster of two scalars: (첫 번째 경우, 클러스터는 단일 스칼라를 포함한다. 두 번째 경우, 클러스터는 2개의 스칼라로 구성된다.)

```swift
let eAcute: Character = "\u{E9}"                // é
let combinedEAcute: Character = "\u{65}\u{301}" // e followed by ́ => é
```

Extended grapheme clusters are a flexible way to represent many complex script characters as a single Character value. (여러 복잡한 스크립트 문자를 하나의 문자 값으로 유연하게 나타낼 수 있는 방법이다.) For example, Hangul syllables (음절) from the Korean alphabet can be represented as either a precomposed or decomposed sequence. Both of these representations qualify as a single Character value in Swift: 
즉, '한' 및 'ㅎ', 'ㅏ', 'ㄴ'은 모두 a single Character value 이다.

```swift
let precomposed: Character = "\u{D55C}"                  // 한
let decomposed: Character = "\u{1112}\u{1161}\u{11AB}"   // ᄒ, ᅡ, ᆫ => 한
```

Extended grapheme clusters enable scalars for enclosing marks (such as COMBINING ENCLOSING CIRCLE, or U+20DD) to enclose other Unicode scalars as part of a single Character value:

```swift
let enclosedEAcute: Character = "\u{E9}\u{20DD}" // enclosedEAcute is é⃝  <- é가 원 안에 들어있는 형태임
```

Unicode scalars for regional indicator symbols can be combined in pairs to make a single Character value, such as this combination of REGIONAL INDICATOR SYMBOL LETTER U (U+1F1FA) and REGIONAL INDICATOR SYMBOL LETTER S (U+1F1F8):

```swift
let regionalIndicatorForUS1: Character = "\u{1F1FA}"
let regionalIndicatorForUS2: Character = "\u{1F1F8}"
print(regionalIndicatorForUS1) // 🇺
print(regionalIndicatorForUS2) // 🇸

let regionalIndicatorForUS: Character = "\u{1F1FA}\u{1F1F8}"
print(regionalIndicatorForUS)  // 🇺🇸  <- 2개의 regional indicator symbols를 합치면, 국기 모양의 1개 character value가 된다.
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

Note: 

Extended grapheme clusters can be composed of multiple Unicode scalars. This means that different characters—and different representations of the same character—can require different amounts of memory to store. (다른 방법을 통해 동일한 문자를 나타낼 수 있다. 따라서 두 방법은 메모리 소모량이 다를 수 있다.) Because of this, characters in Swift don’t each take up the same amount of memory within a string’s representation. (Swift의 문자는 문자열 표현 내에서 각각 동일한 양의 메모리를 차지하지 않는다.) As a result, the number of characters in a string can’t be calculated without iterating through the string to determine its extended grapheme cluster boundaries. (문자열 iterate를 통해 extended grapheme cluster의 경계를 결정해야 String의 문자 수를 계산할 수 있다.) ?????? If you are working with particularly long string values, be aware that the count property must iterate over the Unicode scalars in the entire string in order to determine the characters for that string. 

The count of the characters returned by the count property isn’t always the same as the length property of an NSString that contains the same characters. The length of an NSString is based on the number of 16-bit code units within the string’s UTF-16 representation and not the number of Unicode extended grapheme clusters within the string.

## *Accessing and Modifying a String

- String Indices

    Each String value has an associated *Index type,* String.Index, which corresponds to the position of each Character in the string. 즉, String.Index도 하나의 type이다. 그 type은 Index type이다.

    As mentioned above, different characters can require different amounts of memory to store, so in order to determine which Character is at a particular position, you must iterate over each Unicode scalar from the start or end of that String. For this reason, Swift strings can’t be indexed by integer values. 즉, Swift에서는 int type으로 indexing이 불가하다.

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

        greeting[greeting.startIndex] // G - 첫번째 문자 G의 위치에 접근하고, 해당 index를 통해 서브스크립트 문법을 사용 -> 첫번째 문자 G
        greeting[greeting.index(before: greeting.endIndex)] // ! - 마지막 문자 !의 다음 위치에 접근하고, 해당 index를 메서드 argument로 전달하여 해당 위치의 1칸 앞에 접근, 해당 index를 통해 서브스크립트 문법을 사용 -> !
        greeting[greeting.index(after: greeting.startIndex)] // u - 첫번째 문자 G의 위치에 접근하고, 해당 index를 메서드 argument로 전달하여 해당 위치의 1칸 뒤에 접근, 해당 index를 통해 서브스크립트 문법을 사용 -> u 

        greeting[greeting.index(greeting.startIndex, offsetBy: 7)] // a - 첫번째 문자 G의 위치에 접근하고, 해당 index를 메서드 argument로 전달하여 해당 위치의 +7칸 뒤에 접근, 해당 index를 통해 서브스크립트 문법을 사용 -> a 

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

    - `.indices` 프로퍼티를 통해 String 내부 문자의 모든 index에 접근한다.

        ```swift
        for index in greeting.indices {
            print("\(greeting[index]) ", terminator: "") // greeting[greeting.startIndex] 형태
        } // Prints "G u t e n   T a g ! "
        ```

    Note: startIndex/endIndex 프로토콜 및 index(before:)/index(after:)/index(_:offsetBy:) 메서드는 Collection protocol을 준수하는 모든 Type에 사용 가능하다. (String 포함 Array/Dictionary/Set 등)

- Inserting and Removing
    - `insert(_:at:)` 메서드를 통해 1개 문자를 String의 특정 index에 삽입한다.
    - `insert(contentsOf:at:)` 메서드를 통해 다른 String 콘텐츠를 String의 특정 index에 삽입한다.

        ```swift
        var welcome = "hello"

        welcome.insert("!", at: welcome.endIndex) // welcome now equals "hello!" - 마지막 문자의 다음 위치에 "!"를 삽입
        welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex)) // welcome now equals "hello there!" - 마지막 문자의 다음 위치의 1칸 앞에 "there"를 삽입
        ```

    - `remove(at:)` 메서드를 통해 1개 문자를 String의 특정 index에서 삭제한다.
    - `removeSubrange(_:)` 메서드를 통해 Substring을 String의 특정 index range에서 삭제한다.

        ```swift
        var welcome = "hello there!"

        welcome.remove(at: welcome.index(before: welcome.endIndex)) // "hello there" - 마지막 문자의 다음 위치에서 1칸 앞에 있는 !를 삭제

        let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex // 범위 (마지막 문자의 다음 위치에서 -6칸 앞에 있는 빈칸의 index ..< 마지막 문자의 다음 위치의 index)
        //let range = welcome.index(welcome.endIndex, offsetBy: -6)...welcome.index(before: welcome.endIndex) // 위와 동일 
        welcome.removeSubrange(range) // "hello" - 해당 range의 Substring을 삭제
        ```

    Note: insert(_:at:)/insert(contentsOf:at:)/remove(at:)/removeSubrange(_:) 메서드는 RangeReplaceableCollection protocol을 준수하는 모든 Type에 사용 가능하다. (String 포함 Array/Dictionary/Set 등)

## Substrings

(서브스크립트 또는 prefix 메서드를 통해) String에서 Substring을 가져오면 다른 String이 아니라 새로운 Substring 인스턴스가 생성된다. (참조 type)
Substring의 메서드는 String의 메서드와 대부분 동일하므로 String처럼 사용 가능하다.

You use substrings for only a short amount of time while performing actions on a string. When you’re ready to store the substring for a longer time, you convert the substring to an instance of String. (장기간 저장하여 사용하려면 Substring을 String으로 type 변환해야 한다.)

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

성능 최적화로서 Substring can reuse part of the memory that’s used to store the original string, or part of the memory that’s used to store another substring. 
(Substring은 original String을 저장하는 메모리의 일부를 재사용하거나, 또는 다른 Substring을 저장하는 메모리의 일부를 재사용한다.
이러한 성능 최적화를 통해 (String 또는 Substring을 수정하기 전에는) 메모리를 복사해도 성능이 저하되지 않는다. Substring을 사용하는 동안에는 전체 original String이 메모리에 들어있어야 한다.

- 메모리 구조

    ![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%202.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%202.png)

- [ ]  왜 12345로 안바뀌지? 변경된 String 값을 다른 메모리 공간에 저장했나?

```swift
var greeting = "Hello, world!"
var index = greeting.firstIndex(of: ",") ?? greeting.endIndex
var beginning = greeting[..<index] // Hello - Substring 생성 (original String의 메모리 일부를 공유)

// original String의 값을 변경함
greeting = "12345, 789"
print(beginning) // Hello -> 왜 12345로 안바뀌지? 변경된 String 값을 다른 메모리 공간에 저장했나?
```

Note: Both String and Substring conform to the StringProtocol protocol, which means it’s often convenient for string-manipulation functions to accept a StringProtocol value. You can call such functions with either a String value or Substring value.
String 및 Substring 모두 StringProtocol protocol을 준수한다. 따라서 Stirng을 다루는 함수를 사용할 때 StringProtocol의 값을 수용하는 것이 편리하다. ???? String 값 또는 Substring 값으로 이러한 함수를 호출한다.

## Comparing Strings

Swift에서 String을 비교하는 방법은 3가지이다. 1) string and character equality, 2) prefix equality, 3) suffix equality

- 1) String and Character Equality

    "equal to" operator (==) 및 “not equal to” operator (!=)를 통해 체크한다.

    ```swift
    let quotation = "We're a lot alike, you and I."
    let sameQuotation = "We're a lot alike, you and I."
    if quotation == sameQuotation {
        print("These two strings are considered equal")
    } // Prints "These two strings are considered equal"
    ```

    Two String values are considered equal if their extended grapheme clusters are *canonically equivalent.* ??? Extended grapheme clusters are canonically equivalent if they have the same linguistic meaning and appearance, even if they’re composed from different Unicode scalars behind the scenes. 다른 Unicode scalars을 통해 표현된 동일한 문자 (*단, 의미/모양이 동일해야 함)는 String 비교에서 동일하다고 판단한다.

    ```swift
    // \u{E9} 및 \u{65}\u{301}는 다른 Unicode scalars 값을 가지지만, 동일한 문자 é를 나타내므로 String 비교에서 같다고 판단한다.
    let eAcuteQuestion = "Voulez-vous un caf\u{E9}?" // "Voulez-vous un café?" using LATIN SMALL LETTER E WITH ACUTE
    let combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?" // "Voulez-vous un café?" using LATIN SMALL LETTER E and COMBINING ACUTE ACCENT

    if eAcuteQuestion == combinedEAcuteQuestion {
        print("These two strings are considered equal")
    } // Prints "These two strings are considered equal"
    ```

    *참고 - 모양은 동일하나 의미가 다른 문자이므로 String 비교에서 동일하지 않다고 판단한 예시

    ```swift
    let latinCapitalLetterA: Character = "\u{41}" // 영어의 A
    let cyrillicCapitalLetterA: Character = "\u{0410}" // 러시아어의 A -> The characters are visually similar, but don’t have the same linguistic meaning

    if latinCapitalLetterA != cyrillicCapitalLetterA {
        print("These two characters aren't equivalent.")
    } // Prints "These two characters aren't equivalent."
    ```

- 2) prefix equality & 3) suffix equality

    hasPrefix(*:) 및 hasSuffix(*:) 메서드를 통해 String에 특정 prefix/suffix의 포함 여부를 알 수 있다. (parameter: String, return type: Bool)

    ```swift
    let romeoAndJuliet = [ // Array of String
        "Act 1 Scene 1: Verona, A public place",
        "Act 1 Scene 2: Capulet's mansion",
        "Act 1 Scene 3: A room in Capulet's mansion",
        "Act 1 Scene 4: A street outside Capulet's mansion",
        "Act 1 Scene 5: The Great Hall in Capulet's mansion",
        "Act 2 Scene 1: Outside Capulet's mansion",
        "Act 2 Scene 2: Capulet's orchard",
        "Act 2 Scene 3: Outside Friar Lawrence's cell",
        "Act 2 Scene 4: A street in Verona",
        "Act 2 Scene 5: Capulet's mansion",
        "Act 2 Scene 6: Friar Lawrence's cell"
    ]

    var act1SceneCount = 0
    for scene in romeoAndJuliet {
        if scene.hasPrefix("Act 1 ") { // Array 내부 String에 Prefix "Act 1 "가 몇 개 있는지 확인한다.
            act1SceneCount += 1
        }
    }
    print("There are \(act1SceneCount) scenes in Act 1") // Prints "There are 5 scenes in Act 1"

    var mansionCount = 0
    var cellCount = 0
    for scene in romeoAndJuliet {
        if scene.hasSuffix("Capulet's mansion") { // Array 내부 String에 Suffix "Capulet's mansion"가 몇 개 있는지 확인한다.
            mansionCount += 1
        } else if scene.hasSuffix("Friar Lawrence's cell") {
            cellCount += 1
        }
    }
    print("\(mansionCount) mansion scenes; \(cellCount) cell scenes") // Prints "6 mansion scenes; 2 cell scenes"
    ```

    Note: hasPrefix/hasSuffix methods perform a character-by-character canonical equivalence comparison between the extended grapheme clusters in each string.
    hasPrefix/hasSuffix 메서드는 extended grapheme clusters를 비교할 때 개별 문자에 대한 canonical equivalence 비교를 수행한다. 

## Unicode Representations of Strings ???

When a Unicode string is written to a text file or some other storage, the Unicode scalars in that string are encoded in one of several Unicode-defined encoding forms.
(유니코드 String이 텍스트 파일이나 다른 저장공간에 기록되면, String의 유니코드 scalar는 여러 가지 유니코드-정의 인코딩 양식 중 하나에 인코딩된다.) Each form encodes the string in small chunks known as code units. These include the UTF-8/16/32 encoding form (which encodes a string as 8/16/32-bit code units)

for-in loop를 통해 Unicode extended grapheme clusters 형태의 Character 값에 접근한다.
또는 3가지 유니코드-호환 표현 중 하나를 통해 String 값에 접근한다.

- A collection of UTF-8 code units (accessed with the string’s `utf8` property)
- A collection of UTF-16 code units (accessed with the string’s `utf16` property)
- A collection of 21-bit Unicode scalar values, equivalent to the string’s UTF-32 encoding form (accessed with the string’s `unicodeScalars` property)

```swift
// 아래 예시를 통해 동일한 String에 대한 다른 표현 (different representation)을 확인한다.
let dogString = "Dog‼🐶"
    // ‼ (DOUBLE EXCLAMATION MARK, or Unicode scalar U+203C)
		// 🐶 (DOG FACE, or Unicode scalar U+1F436):
```

- UTF-8 Representation

    `utf8` 프로퍼티를 iterate 하여 String의 UTF-8 표현에 접근한다. ??? 이 프로퍼티는 `String.UTF8View` type이다. `String.UTF8View` type은 UInt8 (unsigned 8-bit) 값의 collection이다.

    ![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%203.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%203.png)

    ```swift
    let dogString = "Dog‼🐶"

    for codeUnit in dogString.utf8 {
        print("\(codeUnit) ", terminator: "")
    } // Prints "68 111 103 226 128 188 240 159 144 182 "
    ```

    - The first three decimal codeUnit values (68, 111, 103) represent the characters D, o, and g, whose UTF-8 representation is the same as their ASCII representation.
    - The next three decimal codeUnit values (226, 128, 188) are a three-byte UTF-8 representation of the DOUBLE EXCLAMATION MARK character.
    - The last four codeUnit values (240, 159, 144, 182) are a four-byte UTF-8 representation of the DOG FACE character.
- UTF-16 Representation

    `utf16` 프로퍼티를 iterate 하여 String의 UTF-16 표현에 접근한다. ??? 이 프로퍼티는 `String.UTF16View` type이다. `String.UTF16View` type은 UInt16 (unsigned 16-bit) 값의 collection이다.

    ![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%204.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%204.png)

    ```swift
    for codeUnit in dogString.utf16 {
        print("\(codeUnit) ", terminator: "")
    } // Prints "68 111 103 8252 55357 56374 "
    ```

    - 마찬가지로, the first three codeUnit values (68, 111, 103) represent the characters D, o, and g, whose UTF-16 code units have the same values as in the string’s UTF-8 representation (because these Unicode scalars represent ASCII characters).
    - The fourth codeUnit value (8252) is a decimal equivalent of the hexadecimal value 203C, which represents the Unicode scalar U+203C for the DOUBLE EXCLAMATION MARK character. This character can be represented as a single code unit in UTF-16.
    - The fifth and sixth codeUnit values (55357 and 56374) are a UTF-16 surrogate (대리인) pair representation of the DOG FACE character. 
    These values are a high-surrogate value of U+D83D (decimal value 55357) and a low-surrogate value of U+DC36 (decimal value 56374). ???
- Unicode Scalar Representation

    `unicodeScalars` 프로퍼티를 iterate 하여 String의 Unicode scalar 표현에 접근한다. ??? 이 프로퍼티는 `UnicodeScalarView` type이다. `UnicodeScalarView` type은 UnicodeScalar type 값의 collection이다.

    Each `UnicodeScalar` has a `value` property that returns the scalar’s 21-bit value, represented within a UInt32 value:

    ![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%205.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%205.png)

    ```swift
    for scalar in dogString.unicodeScalars {
        print("\(scalar.value) ", terminator: "")
    } // Prints "68 111 103 8252 128054 "
    ```

    - The value properties for the first three UnicodeScalar values (68, 111, 103) once again represent the characters D, o, and g.
    - The fourth codeUnit value (8252) is again a decimal equivalent of the hexadecimal value 203C, which represents the Unicode scalar U+203C for the DOUBLE EXCLAMATION MARK character. ???
    - The value property of the fifth UnicodeScalar (128054), is a decimal equivalent of the hexadecimal value 1F436, which represents the Unicode scalar U+1F436 for the DOG FACE character.

    As an alternative to querying their value properties, each UnicodeScalar value can also be used to construct a new String value, such as with string interpolation: ???

    ```swift
    for scalar in dogString.unicodeScalars {
        print("\(scalar) ") 
    }
    // D
    // o
    // g
    // ‼
    // 🐶
    ```

# 4. Collection Types  (90%)

Swift의 3가지 primary collection types은 arrays, sets, and dictionaries 이다.
Arrays are ordered collections of values. Sets are unordered collections of unique values. Dictionaries are unordered collections of key-value associations.

![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%206.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%206.png)

Note: array, set, and dictionary types are implemented as generic collections.

- Mutability of Collections

    If you create an array, a set, or a dictionary, and assign it to a variable, the collection that’s created will be mutable. 
    If you assign an array, a set, or a dictionary to a constant, that collection is immutable. (its size and contents can’t be changed.)
    var 선언을 통해 할당한 collection은 mutable하고, let 선언을 통해 할당한 collection은 immutable하다.
    let 선언을 통해 생성한 collection을 변수에 할당하면, 해당 변수에 새로 생성된 collection은 mutable 하다.

    ```swift
    var varArray: [Int] = [1, 2, 3] 
    varArray[0] = 100 // [100, 2, 3, 4] - mutable

    let letArray: [Int] = [1, 2, 3] 
    //letArray.append(4) // 컴파일 에러 발생  - immutable 

    var someVariable = letArray // 변수에 let 선언 collection을 할당하면
    someVariable.append(4) // [1, 2, 3, 4] - mutable
    ```

    Note: collection 변경이 필요 없는 경우, immutable collection (let 선언 collection)을 생성하는 것이 좋다. 코드에 대해 쉽게 추론할 수 있고, 컴파일러의 성능 최적화가 가능하기 때문이다.

## Arrays

An array stores values of the same type in an ordered list.

Note: Swift’s `Array` type is bridged to Foundation’s `NSArray` class.

- [ ]  [https://developer.apple.com/documentation/swift/array#2846730](https://developer.apple.com/documentation/swift/array#2846730)
- Array Type Shorthand Syntax (축약 문법)

    1) array is written in full as `Array<Element>`
    2) an array in shorthand form as `[Element]`

    두 형식은 기능적으로 동일하지만, 축약형이 선호된다.

- Creating an Empty Array

    initializer syntax를 사용하여 특정 type의 empty Array를 생성 가능하다.

    ```swift
    var someInts: [Int] = []
    print("someInts is of type [Int] with \(someInts.count) items.")  // Prints "someInts is of type [Int] with 0 items."
    ```

    Note that the type of the `someInts` variable is inferred to be `[Int]` from the type of the initializer. ??? 왜 inferred? 선언 시 : [Int]로 type 명시해뒀는데...?

    ```swift
    someInts.append(3)  // someInts now contains 1 value of type Int
    someInts = []  // someInts is now an empty array, but is still of type [Int] - 변수 선언 시 이미 type을 명시했으므로 []만으로 할당 가능하다.
    ```

    Alternatively, if the context already provides type information, such as a function argument or an already typed variable/constant, you can create an empty array with an empty array literal, which is written as `[]` 

- Creating an Array with a Default Value

    Array type provides an initializer for creating an array of 'a certain size' with all of its values 'set to the same default value'. ('동일한 기본값'을 '특정 size 만큼 반복'하는 Array를 생성하는 initializer를 제공한다.)
    You pass this initializer a default value of the appropriate type (called `repeating`): and the number of times that value is repeated in the new array (called `count`):

    ```swift
    var threeDoubles = Array(repeating: 0.0, count: 3)  
    // threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0] - Array(repeating:count:) initializer를 사용하여 생성했다. (Array<>가 아님)
    ```

- Creating an Array by Adding Two Arrays Together

    You can create a new array by adding together two existing arrays with compatible types with the addition operator (+).  (연산자 +와 호환되는 type의 array)
    The new array’s type is inferred from the type of the two arrays you add together:

    ```swift
    var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
    // anotherThreeDoubles is of type [Double], and equals [2.5, 2.5, 2.5]

    var sixDoubles = threeDoubles + anotherThreeDoubles
    // sixDoubles is inferred as [Double], and equals [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
    ```

- Creating an Array with an Array Literal

    You can also initialize an array with an array literal, which is a shorthand way to write one or more values as an array collection. 
    An array literal is written as a list of values, separated by commas, surrounded by a pair of square brackets: `[value1, value2, value3]`

    ```swift
    var shoppingList: [String] = ["Eggs", "Milk"]  // <- array literal (*String type이 맞으므로 이를 통한 initialize가 가능하다.)
    // shoppingList has been initialized with two initial items
    ```

    The shoppingList variable is declared as “an array of string type values”, written as `[String]`.
    The shoppingList array is initialized with two String values ("Eggs" and "Milk"), written within an array literal.

    Swift의 type inference를 통해 array literal 만으로도 array initialization이 가능하다. (type을 명시할 필요가 없다.)

    ```swift
    var shoppingList = ["Eggs", "Milk"]  // <- array literal (*element가 모두 String type이므로 [String]으로 type inference 된다.)
    ```

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

Note: The first item in the array has an index of 0, not 1. Arrays in Swift are always zero-indexed.

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

- Iterating Over an Array
    - `for-in` loop 사용

        ```swift
        for item in shoppingList {
            print(item)
        } // Six eggs, Milk, Flour, Baking Powder, Bananas
        ```

    - `enumerated` 메서드를 통해 array를 interate하면, 각 item의 int type index 및 value를 얻을 수 있다.
    - enumerated 정의 : Returns a sequence of pairs (n, x), where n represents a consecutive integer starting at zero and x represents an element of the sequence.

        ```swift
        for (index, value) in shoppingList.enumerated() { // Enumerated() method returns a tuple composed of an integer and the item. Integers start at zero and count up by one for each item
            print("Item \(index + 1): \(value)") // You can decompose the tuple into temporary constants/variables as part of the iteration
        } 
        // Item 1: Six eggs, Item 2: Milk, Item 3: Flour, Item 4: Baking Powder, Item 5: Bananas
        ```

## Sets

A set stores distinct values (고유의 값) of the same type in a collection with no defined ordering.
You can use a set instead of an array when the order of items isn’t important, or when you need to ensure that an item only appears once.

Note: Swift’s Set type is bridged to Foundation’s NSSet class.

- Hash Values for Set Types

    A type must be hashable in order to be stored in a set—that is, the type must provide a way to compute a hash value for itself. A hash value is an Int value that’s the same for all objects that compare equally, such that if `a == b`, the hash value of a is equal to the hash value of b.
    (Set에 저장하려면 type이 hashable 해야 한다. 즉, type이 hash value를 연산하는 방법을 제공해야 한다는 뜻이다. hash Value는 동일하게 비교되는 모든 객체의 int 값이다. ??? 
    예를 들어 a == b 인 경우, a의 hash value는 b의 hash value와 동일하다.)

    Swift의 기본 type (String, Int, Double, Bool 등)은 default로 hashable 하다. 따라서 Set의 value type 또는 Dictionary의 key type으로 사용 가능하다. 
    Set 또한 default로 hashable 하다. 
    Enumeration의 case value (연관값이 없는 경우) 또한 default로 hashable이다.

    Note: 사용자 정의 type이 Hashable protocol을 준수한다면, Set의 value type 또는 Dictionary의 key type으로 사용 가능하다.

    - [ ]  Hashable protocol - [https://developer.apple.com/documentation/swift/hashable](https://developer.apple.com/documentation/swift/hashable)
        - A type that can be hashed into a Hasher to produce an integer hash value. (Hasher로 hash 하여 int hash value를 생성할 수 있는 type이다.)
        - 내용

            You can use any type that conforms to the Hashable protocol in a set or as a dictionary key. Many types in the standard library conform to Hashable: Strings, integers, floating-point and Boolean values, and even sets are hashable by default. Some other types, such as optionals, arrays and ranges automatically become hashable when their type arguments implement the same.

            Your own custom types can be hashable as well. When you define an enumeration without associated values, it gains Hashable conformance automatically (자동으로 Hashable 준수 상태가 되며), and you can add Hashable conformance to your other custom types by implementing the `hash(into:)` method. For structs whose stored properties are all Hashable, and for enum types that have all-Hashable associated values, the compiler is able to provide an implementation of hash(into:) automatically.

            'Hashing a value' means feeding its essential components into a hash function, represented by the Hasher type. Essential components are those that contribute to the type’s implementation of Equatable. Two instances that are equal must feed the same values to Hasher in hash(into:), in the same order. ???

            - [x]  Equatable
                - A type that can be compared for value equality.
- Set Type Syntax

    The type of set is written as `Set<Element>`, where Element is the type that the set is allowed to store. Unlike arrays, sets don’t have an equivalent shorthand form. (Set는 축약형이 없다.)

- Creating and Initializing an Empty Set

    initializer syntax를 사용하여 특정 type의 empty set을 생성한다.

    ```swift
    var letters = Set<Character>()
    print("letters is of type Set<Character> with \(letters.count) items.") // Prints "letters is of type Set<Character> with 0 items."
    ```

    Note: The type of the letters variable is inferred to be Set<Character>, from the type of the initializer.

    Alternatively, if the context already provides type information (such as a function argument or an already typed variable/constant), you can create an empty set with an empty array literal:

    ```swift
    letters.insert("a") // letters now contains 1 value of type Character
    letters = [] // letters is now an empty set, but is still of type Set<Character> - {}이 아님!
    ```

- Creating a Set with an Array Literal

    You can also initialize a set with an array literal, as a shorthand way to write one or more values as a set collection.

    ```swift
    var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"] // initialized with three initial items
    // The favoriteGenres variable is declared as “a set of String values”
    // Because this set has specified a value type of String, it’s only allowed to store String values.
    ```

    Note: The favoriteGenres set is declared as a variable (with the `var` introducer) and not a constant (with the `let` introducer)

    이때, `: Set<String>`로 type을 명시하지 않으면 Set가 아니라 Array가 된다. 단, 값 type이 1가지인 array literal로 initialize하는 경우, type inference 기능을 통해 Set element의 type을 명시하지 않는 것은 가능하다. 

    ```swift
    var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"] // array literal의 모든 값의 type이 동일하므로 Set<String>로 명시하지 않아도 type inference 된다.
    ```

### Accessing and Modifying a Set

메서드 및 프로퍼티를 통해 Set를 접근 및 수정한다.

- read-only `count` property를 통해 item의 개수를 확인한다.
- Boolean `isEmpty` property를 통해 count 프로퍼티가 0인지 축약으로 확인한다.
- Set의 `insert(_:)` method를 통해 item을 추가한다.
- Set의 `remove(_:)` method를 통해 item을 삭제한다. 해당 item이 set의 member가 맞으면 item을 삭제 및 반환하고, member에 속하지 않으면 nil을 반환한다.
Set는 순서가 없으므로 parameter at: index가 필요없다.
- Set의 `removeAll()` method를 통해 모든 item을 삭제한다.
- `contains(_:)` method를 통해 특정 item이 있는지 확인한다. (return type Bool)

    ```swift
    print("I have \(favoriteGenres.count) favorite music genres.") // Prints "I have 3 favorite music genres."

    if favoriteGenres.isEmpty {
        print("As far as music goes, I'm not picky.")
    } else {
        print("I have particular music preferences.") // Prints "I have particular music preferences."
    }

    favoriteGenres.insert("Jazz") // favoriteGenres now contains 4 items

    if let removedGenre = favoriteGenres.remove("Rock") {
        print("\(removedGenre)? I'm over it.") // Prints "Rock? I'm over it." - member에 속하므로 삭제 및 반환된다.
    } else {
        print("I never much cared for that.")
    }

    if favoriteGenres.contains("Funk") {
        print("I get up on the good foot.")
    } else {
        print("It's too funky in here.") // Prints "It's too funky in here." - 해당 item이 없으므로 false를 반환한다.
    }

    ```

- Iterating Over a Set

    for-in loop 사용

    ```swift
    for genre in favoriteGenres {
        print("\(genre)")
    } // Classical, Jazz, Hip hop
    ```

    Set type은 순서가 없다. Set의 값을 특정 순서대로 iterate 하려면, `sort()` 메서드를 사용한다. (sort 메서드는 Set의 element를 < 연산자를 사용하여 정렬한 Array로 바꾸어 반환한다.) 
    Use the sorted() method, which returns the set’s elements as an array sorted using the < operator.

    ```swift
    for genre in favoriteGenres.sorted() {
        print("\(genre)")
    } // Classical, Hip hop, Jazz - < operator로 sorted 된 상태이다. (A-Z 순 또는 1-9 순으로 정렬됨)
    ```

- Performing Set Operations

    You can efficiently perform fundamental set operations, such as combining two sets together, determining which values two sets have in common (2개 Set가 공통적으로 갖는 값을 확인하거나), or determining whether two sets contain all/some/none of the same values.

    - Fundamental Set Operations

        The illustration below depicts two sets. 여러 Set 연산의 결과를 그림자 영역으로 나타낸다.

        ![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%207.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%207.png)

        - Use the `intersection(_:)` method to create a new set with only the values common to both sets. - 교집합
        - Use the `symmetricDifference(_:)` method to create a new set with values in either set, but not both. - (합집합-교집합)
        - Use the `union(_:)` method to create a new set with all of the values in both sets. - 합집합
        - Use the `subtracting(_:)` method to create a new set with values not in the specified set. - 차집합

            ```swift
            let oddDigits: Set = [1, 3, 5, 7, 9]
            let evenDigits: Set = [0, 2, 4, 6, 8]
            let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]

            oddDigits.union(evenDigits).sorted() // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
            oddDigits.intersection(evenDigits).sorted() // []
            oddDigits.subtracting(singleDigitPrimeNumbers).sorted() // [1, 9]
            oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted() // [1, 2, 9]
            ```

    - Set Membership and Equality

        The illustration below depicts three sets with overlapping regions representing elements shared among sets. 

        Set a is a superset of set b, because a contains all elements in b. (a가 b를 감싼다.) Conversely, set b is a subset of set a, because all elements in b are also contained by a. 
        Set b and set c are disjoint with one another, because they share no elements in common. (공통 부분이 없다.)

        ![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%208.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%208.png)

        - Use the “is equal” operator (`==`) to determine whether two sets contain all of the same values.
        - Use the `isSubset(of:)` method to determine whether all of the values of a set are contained in the specified set.
        - Use the `isSuperset(of:)` method to determine whether a set contains all of the values in a specified set.
        - Use the `isStrictSubset(of:)` or `isStrictSuperset(of:)` methods to determine whether a set is a subset or superset, but not equal to, a specified set.
        - Use the `isDisjoint(with:)` method to determine whether two sets have no values in common.

            ```swift
            let houseAnimals: Set = ["🐶", "🐱"]
            let farmAnimals: Set = ["🐶", "🐱", "🐔", "🐑"]
            let cityAnimals: Set = ["🐦", "🐭"]

            houseAnimals.isSubset(of: farmAnimals) // true
            farmAnimals.isSuperset(of: houseAnimals) // true
            farmAnimals.isDisjoint(with: cityAnimals) // true

            let myPets: Set = ["🐶", "🐱"] // Strict-메서드 확인용
            myPets.isStrictSubset(of: houseAnimals) // false - Subset/Superset는 맞지만, element가 동일하므로 false 이다.
            ```

## Dictionaries

A dictionary stores associations between keys of the same type and values of the same type in a collection with no defined ordering. Each value is associated with a unique key, which acts as an identifier for that value within the dictionary. (key는 identifier 이므로 중복 불가하다.)
Unlike items in an array, items in a dictionary don’t have a specified order. You use a dictionary when you need to look up values based on their identifier, in much the same way that a real-world dictionary is used to look up the definition for a particular word. (실제 사전의 개념에서 따온 것이다!)

Note: Swift’s Dictionary type is bridged to Foundation’s NSDictionary class.

- Dictionary Type Shorthand Syntax

    written in full as `Dictionary<Key, Value>`, where Key is the type of value that can be used as a dictionary key, and Value is the type of value that the dictionary stores for those keys.

    축약형은 `[Key: Value]` 이다. 기능적으로 동일하지만 축약형이 선호된다. 

    Note: Dictionary의 Key type은 반드시 Hashable protocol을 준수해야 한다. (Set의 value type과 마찬가지로)

- Creating an Empty Dictionary

    initializer syntax를 사용하여 empty Dictionary를 생성한다.

    ```swift
    var namesOfIntegers: [Int: String] = [:]  // namesOfIntegers is an empty [Int: String] dictionary
    ```

    If the context already provides type information, you can create an empty dictionary with an empty dictionary literal, which is written as `[:]` (a colon inside a pair of square brackets):

    ```swift
    namesOfIntegers[16] = "sixteen" // namesOfIntegers now contains 1 key-value pair [16: "sixteen"]
    namesOfIntegers = [:] // namesOfIntegers is once again an empty dictionary of type [Int: String]
    ```

- Creating a Dictionary with a Dictionary Literal

    You can also initialize a dictionary with a dictionary literal. A dictionary literal is a shorthand way to write one or more key-value pairs as a Dictionary collection.
    A key-value pair is a combination of a key and a value. In a dictionary literal, the key and value in each key-value pair are separated by a colon. The key-value pairs are written as a list, separated by commas, surrounded by a pair of square brackets: `[key1: value1, key2: value2, key3: value3]`

    ```swift
    var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"] // initialized with a dictionary literal containing two key-value pairs.
    // declared as having a type of [String: String], which means “a Dictionary whose keys are of type String, and whose values are also of type String”.
    ```

    Array와 마찬가지로 key와 value의 type이 일정하면, type inference 기능을 통해 type을 명시하지 않아도 initialize 가능하다.

    ```swift
    var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
    ```

### Accessing and Modifying a Dictionary

1) methods and properties를 사용하거나 2) subscript syntax를 사용하여 Dictionary에 접근/수정이 가능하다.

- read-only `count` property를 통해 item의 개수를 확인한다.
- Boolean `isEmpty` property를 통해 count property가 0인지 축약으로 확인한다.

    ```swift
    print("The airports dictionary contains \(airports.count) items.") // Prints "The airports dictionary contains 2 items."

    if airports.isEmpty {
        print("The airports dictionary is empty.")
    } else {
        print("The airports dictionary isn't empty.") // Prints "The airports dictionary isn't empty."
    }
    ```

- subscript syntax를 통해 item을 추가한다. 
Use a new key of the appropriate type as the subscript index, and assign a new value of the appropriate type. (subscript index로 새로운 key를 사용하고, 새로운 value를 할당한다.)

    ```swift
    airports["LHR"] = "London" // the airports dictionary now contains 3 items - item 추가
    ```

- subscript syntax를 사용하여 특정 key의 value를 변경한다.

    ```swift
    airports["LHR"] = "London Heathrow" // the value for "LHR" has been changed to "London Heathrow" - item 변경
    ```

- Dictionary의 `updateValue(_:forKey:)` method를 통해 특정 key에 대한 value를 설정(set)하거나 업데이트(update)한다. 해당 key가 기존 key에 없으면 value를 설정하고, 있으면 value를 업데이트한다.

    하지만 서브스크립트 문법과 달리, updateValue 메서드는 업데이트 이후 old value (기존의 값)을 반환한다. ***업데이트된 값이 아님!
    또한 updateValue 메서드의 return type은 Optional이다. 이 Optional value는 해당 key에 대한 old value가 있으면 (value를 업데이트 하기 전에) old value를 반환하고, 없으면 nil을 반환한다.

    ```swift
    // set
    if let oldValue = airports.updateValue("set this value", forKey: "New Key") {  // 기존에 없던 key -> 값을 set 
        print("The old value for DUB was \(oldValue).")
    } else {
        print("This is nil.")  // old value가 없으므로 nil을 반환한다.
    } // This is nil.

    print(airports) // ["DUB": "Dublin Airport", "YYZ": "Toronto Pearson", "LHR": "London Heathrow", "New Key": "set this value"]

    // update
    if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {  // 기존에 있던 key -> 값을 update
        print("The old value for DUB was \(oldValue).")
    } // Prints "The old value for DUB was Dublin."  // old value가 있으므로 기존 값이 반환된다.

    print(airports) // ["LHR": "London Heathrow", "DUB": "Dublin Airport", "YYZ": "Toronto Pearson"] // key DUB에 대한 value는 업데이트 되었다.
    ```

- subscript syntax를 통해 Dictionary의 특정 key의 value를 꺼낼 수 있다. 
Because it’s possible to request a key for which no value exists, a dictionary’s subscript returns an optional value of the dictionary’s value type. (value가 없는 key를 (존재하지 않는 key???) request 할 수 있으므로 subscript의 return type은 Optional 이다.)
Dictionary에 request한 key의 value가 있으면 subscript는 그 value를 Optional type으로 반환하고, value가 없으면??? subscript는 nil을 반환한다.
    - [ ]  질문

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

- subscript syntax를 통해 특정 key의 value에 nil을 할당하여 key-value pair를 삭제한다. (key는 남겨두고 value만 삭제하는 것이 아님)
- `removeValue(forKey:)` method를 통해 key-value pair를 삭제한다. key-value pair가 있으면 삭제한 value를 반환하고, 없으면 nil을 반환한다.

    ```swift
    airports["APL"] = "Apple International"  // "Apple International" isn't the real airport for APL, so delete it
    airports["APL"] = nil  // APL has now been removed from the dictionary

    if let removedValue = airports.removeValue(forKey: "DUB") {
        print("The removed airport's name is \(removedValue).") // Prints "The removed airport's name is Dublin Airport." - 삭제한 value를 반환
    } else {
        print("The airports dictionary doesn't contain a value for DUB.")
    }

    // key가 없는 경우
    if let removedValue = airports.removeValue(forKey: "Key-doesn't exist") {
        print("The removed airport's name is \(removedValue).")
    } else {
        print("The airports dictionary doesn't contain a value for DUB.") // The airports dictionary doesn't contain a value for DUB.
    }

    // key는 있지만, value가 nil인 경우
    var airports: [String: Any?] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"] // value에 nil을 할당하기 위해 Dictionary 선언부에서 type을 바꿔야 한다.

    var valueX: Any? = nil
    airports["test"] = valueX
    print(airports) // ["test": nil, "DUB": Optional("Dublin Airport"), "LHR": Optional("London Heathrow"), "YYZ": Optional("Toronto Pearson"), "New Key": Optional("set this value")]

    if let removedValue = airports.removeValue(forKey: "test") { // key는 있지만, value가 nil인 상황
        print("The removed airport's name is \(removedValue).") // The removed airport's name is nil. - ???????????????? nil이 그대로 반환된다.
    } else {
        print("The airports dictionary doesn't contain a value for DUB.")  DUB.
    }

    ```

- Iterating Over a Dictionary

    for-in loop를 사용하여 iterate한다.

    Dictionary의 각 item은 (key, value) tuple type으로 반환된다. iteration 과정에서 이러한 tuple의 member를 분해하여 임시 상수/변수에 할당 가능하다.

    ```swift
    for (airportCode, airportName) in airports {
        print("\(airportCode): \(airportName)")
    }
    // LHR: London Heathrow
    // YYZ: Toronto Pearson
    ```

    `keys` property 및 `values` property에 접근하여 iterable collection인 Dictionary의 key 또는 value를 꺼낼 수 있다.

    ```swift
    for airportCode in airports.keys {
        print("Airport code: \(airportCode)")
    }
    // Airport code: LHR
    // Airport code: YYZ

    for airportName in airports.values {
        print("Airport name: \(airportName)")
    }
    // Airport name: London Heathrow
    // Airport name: Toronto Pearson
    ```

    If you need to use a dictionary’s keys or values with an API that takes an Array instance, initialize a new array with the keys property or values property. 

    - [ ]  API ???? Array instance ???

    ```swift
    let airportCodes = [String](airports.keys)
    // airportCodes is ["LHR", "YYZ"]

    let airportNames = [String](airports.values)
    // airportNames is ["London Heathrow", "Toronto Pearson"]
    ```

    Dictionary type은 순서가 없으므로 key 또는 value를 특정 순서로 iterate 하려면, keys property 및 values property에 대해 `sorted()` method를 사용한다.

# 5. Control Flows -

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

    - [ ]  함수 정의에서 parameter를 정의할 때 argument label을 추가로 붙이지 않아도, 함수호출 시 parameter name이 argument label이라고 불린다!
    즉, 함수 호출 시점에서 argument가 전달되는 부분의 앞쪽은 무조건 'argument label'인건가? (함수 정의 시점에서 parameter name만 있든, argument label 및 parameter name이 둘다 있든 상관없이)

    Note: The print(_:separator:terminator:) function doesn’t have a label for its first argument, and its other arguments are optional because they have a default value.

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

parameter가 여러 개인 경우, parameter name과 달리 argument label은 동일하게 중복사용이 가능하다. 단, 가독성을 위해 argument label도 중복하지 않는 것이 선호된다.

```swift
func someFunction(sameLabel firstParameterName: Int, sameLabel secondParameterName: Int) {
    // In the function body, firstParameterName/secondParameterName refer to the argument values for the first/second parameters.
    print(firstParameterName + secondParameterName)
}
someFunction(sameLabel: 1, sameLabel: 2) // 3 출력
```

- Specifying Argument Labels

    You write an argument label before the parameter name, separated by a space:

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

    print(arithmeticMean()) // nan - ??????
    ```

    - [ ]  nil 이 아니라 nan 이다?????

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

---

# 🦜 Language Reference

---

# 🦜 API Design Guidelines

*API : Application Programming Interface

## Fundamentals

1. Clarity at the point of use is your most important goal. (가장 중요한 목표는 사용 시점에서의 명료성이다.) Entities such as methods and properties are declared only once but used repeatedly. Design APIs to make those uses clear and concise. When evaluating a design, reading a declaration is seldom sufficient; always examine a use case to make sure it looks clear in context. (메서드 및 프로퍼티와 같은 Entities는 한 번 선언하면 반복적으로 사용한다. 이러한 Entities를 명확하고 간결하게 사용하도록 API를 설계해야 한다.??? 디자인을 검토할 때 선언부를 읽는 것만으로는 충분하지 않다. 항상 사용 사례를 검토하여 문맥상 명확하게 파악되는지 확인한다.)

    *Entity : 저장되고, 관리되어야 하는 데이터의 집합이다.
    An entity is represented by an instance of the NSEntityDescription class. This class provides access to a wide range of properties, such as its name, the data model it is defined in, and the name of the class the entity is represented by. ???

1. Clarity is more important than brevity. (명료함이 간결함보다 더 중요하다.) Although Swift code can be compact, it is a *non-goal* to enable the smallest possible code with the fewest characters. (가장 적은 문자로 가능한 최소의 코드를 활성화하는 것이 목표가 아니다.) Brevity in Swift code, where it occurs, is a side-effect of the strong type system and features that naturally reduce boilerplate. Swift 코드의 간결함은 강력한 type 시스템과 boilerplate 코드를 줄이는 기능으로 인한 부수적인 효과이다. 
    - [ ]  (boilerplate : 표준 형식) ???

1. Write a documentation comment for every declaration. Insights gained by writing documentation can have a profound impact on your design, so don’t put it off. (모든 선언에 대해 문서화 주석을 작성한다. 문서 작성을 통해 얻은 통찰력은 디자인에 큰 영향을 미칠 수 있다.)

    Note: API의 기능에 대해 간단히 설명할 수 없으면, API를 잘못 설계했을 가능성이 있다.

    - Swift의 Markdown을 사용한다.
        - [ ]  [https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497-CH2-SW1](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497-CH2-SW1)
    - 먼저 요약을 통해 선언할 Entity를 설명한다. 요약은 매우 중요하다.

        ```swift
        /// Returns a "view" of `self` containing the same elements in reverse order.
        func reversed() -> ReverseCollection
        ```

        - 가능하면 1개의 문구 (a single sentence fragment)로 작성하고, 마침표로 끝낸다. 완전한 문장을 사용하지 않는다.
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

    - 경우에 따라 1개 이상의 단락 (paragraphs)과 글머리 기호 (bullet items)를 사용한다. 단락은 한 줄을 띄어쓰고 (separated by blank), 완전한 문장 (complete sentences)을 사용한다.

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

## Naming (이름 지정)

### Promote Clear Usage (명확한 사용을 지향하기)

- 코드를 읽는 사람이 명확히 이해하도록 필요한 모든 단어를 사용한다.

    ex. Collection 내부의 주어진 위치에 있는 element를 제거하는 메서드를 고려한다면,

    ```swift
    extension List {
      public mutating func remove(at position: Index) -> Element
    }
    employees.remove(at: x) - 좋은 예시. x가 삭제할 element의 위치를 나타내는 것이 명확하다.
    employees.remove(x) - 나쁜 예시. x라는 element를 제거한다는 의미로 오인할 수 있다.
    ```

- 불필요한 단어는 생략한다. 이름의 모든 단어는 사용 시점에서 핵심적인 정보를 전달해야 한다.
특히 단순히 type 정보를 반복하는 단어는 생략한다.

    ```swift
    public mutating func remove(member: Element) -> Element? 
    allViews.remove(cancelButton) - 좋은 예시. 명확하다.

    public mutating func removeElement(member: Element) -> Element?
    allViews.removeElement(cancelButton) - 나쁜 예시. 함수 호출 시 Element라는 단어는 핵심적인 정보가 아니다.
    ```

- type 제약 조건이 아니라 "역할"에 따라 변수/매개변수/관련 type에 대해 이름을 지정한다.

    ```swift
    var greeting = "Hello" // 좋은 예시. Entity의 역할을 표현하는 이름을 사용하는 것이 좋다.
    protocol ViewController {
    	  associatedtype ContentView : View
    }
    class ProductionLine {
    	  func restock(from supplier: WidgetFactory)
    }

    var string = "Hello" // 나쁜 예시. 이러한 type name의 용도 변경 ??? 은 명확성과 표현력이 낮다. 
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
부족한 type 정보는 역할을 명시하는 명사 (아래에서는 argument label)를 사용한다.

    특히 parameter type이 NSObject, Any, AnyObject, or a fundamental type (Int, String 등)인 경우, type 정보와 사용 시점에서의 문맥이 의도를 충분히 나타내기 어려울 수 있다.
    Precede each weakly typed parameter with a noun describing its role. (이때, weak type의 parameter 앞에 해당 parameter의 역할을 설명하는 명사를 붙인다.)

    ```swift
    func addObserver(_ observer: NSObject, forKeyPath path: String) // 좋은 예시. weak type parameter인 path의 역할을 설명하는 명사(forKeyPath)를 붙여서 정보를 보완했다.
    grid.addObserver(self, forKeyPath: graphics) // clear

    func add(_ observer: NSObject, for keyPath: String) // 나쁜 예시. 선언은 명확하지만, 사용 시점 (use site)에서 모호하다.
    grid.add(self, for: graphics) // vague
    ```

### *Strive for Fluent Usage (쉽고 명확하게 읽히도록 작성하기)

- Prefer function names that make use sites form grammatical English phrases. (사용 위치에서 (함수를 사용하는 시점에서) 함수 이름이 '영어 문장의 형태'가 되도록 한다.)

    ```swift
    x.insert(y, at: z)          “x, insert y at z” (x에 y를 z위치에 삽입)  // 좋은 예시
    x.subViews(havingColor: y)  “x's subviews having color y” (x의 subviews는 색상 y을 가짐)
    x.capitalizingNouns()       “x, capitalizing nouns” (x의 명사를 대문자화)

    x.insert(y, position: z)  // 나쁜 예시
    x.subViews(color: y)
    x.nounCapitalize()
    ```

    첫번째 또는 두번째 argument 다음의 argument들이 함수 호출에 있어 중요한 의미가 없으면, 가독성을 위해 argument들 앞에 줄 바꿈하는 것을 허용한다.

    ```swift
    AudioUnit.instantiate(
      with: description, 
      options: [.inProcess], completionHandler: stopProgressBar)  // 중요도가 낮은 arguments이므로 의도적으로 줄바꿈 한다.
    ```

- factory method의 이름은 `make`로 시작한다. ex. `x.makeIterator()`
    - [ ]  factory method
- The first argument to initializer and factory methods calls should not form a phrase starting with the base name. ex. `x.makeWidget(cogCount: 47)`
(앞서 함수를 사용하는 시점에서 함수 이름이 '영어 문장의 형태'가 되어야 한다고 했다. 하지만 그렇다고해서 (이니셜라이저 및 factory method 호출 시) 첫번째 argument를 포함하여 영어 문장을 구성해서는 안된다.)

    For example, the first arguments to these calls do not read as part of the same phrase as the base name: (이러한 호출에 대해 첫번째 argument는 영어 문장의 일부로 읽히지 않는다.)

    ```swift
    let foreground = Color(red: 32, green: 64, blue: 128) // 좋은 예시
    let newPart = factory.makeWidget(gears: 42, spindles: 14)

    let foreground = Color(havingRGBValuesRed: 32, green: 64, andBlue: 128) // 나쁜 예시. API 작성자는 첫번째 argument를 통해 문법적인 연속성을 만들고자 한다.
    let newPart = factory.makeWidget(havingGearCount: 42, andSpindleCount: 14)
    ```

    In practice, this guideline along with those for argument labels means the first argument will have a label unless the call is performing a value preserving type conversion. ???

    실제로 이 가이드라인과 argument label에 관한 가이드라인은??? 첫번째 argument가 argument label을 가진다는 것을 의미한다. 함수 호출을 통해 값을 유지하는 type 변환을 실행하지 않는다면.

    ```swift
    let rgbForeground = RGBColor(cmykForeground)
    ```

- Side-effects (부수적인 효과???) 에 따라 함수 이름을 지정한다.
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

        - Boolean 메서드 및 프로퍼티는 receiver에 대한 주장 (Assertion) 형태로 이름을 지정한다. (불변 메서드/프로퍼티일 때???) ex. `x.isEmpty`, `line1.intersects(line2)`
        - 무언가를 설명하는 Protocol은 명사로 이름을 지정한다. ex. `Collection`
        - 기능 (capability)을 설명하는 Protocol은  "able, ible, ing" suffix를 붙인다. ex. `Equatable`, `ProgressReporting`
        - 이외 type, 프로퍼티, 변수/상수는 명사로 이름을 지정한다.

### Use Terminology Well (용어를 제대로 사용하기)

*Term of Art : 특정 분야에서 통용되는 전문적인 용어를 의미한다.

- 일반적인 단어가 의미를 잘 전달할 수 있다면 모호한 용어는 피한다. “피부(skin)”가 목적에 맞는 용어라면 “표피(epidermis)"를 사용하지 않는다. Terms of art는 중요한 커뮤니케이션 수단이지만, 의미를 엄밀히 구분할 필요가 있을 때만 사용해야 한다.
- Term of Art를 사용한다면 기존의 의미에 충실해야 한다.
    - API는 받아들여지는 의미에 따라 엄격하게 용어를 사용해야 한다.

        - 전문가를 놀라게 하지 않는다. 기존 용어에 새로운 의미를 지어내면, 이미 해당 용어에 익숙한 사람이 당황할 수 있다.

        - 초보자를 혼란스럽게 하지 않는다. 초보자는 웹 검색을 통해 기존에 통용되는 의미를 받아들인다.

- 약어 (abbreviations)를 피한다. Abbreviations, especially non-standard ones, are effectively terms-of-art, because understanding depends on correctly translating them into their non-abbreviated forms. ??? (특히 표준이 아닌 약어는 풀어쓰는 과정에서 오인될 수 있다.) 약어를 사용한다면 웹 검색을 통해 쉽게 찾을 수 있어야 한다.
- 선례 (precedent)를 따른다.  초보자를 위해 용어를 최적화하지 않는다.

    - 예시-1. 초보자는 List를 더 쉽게 이해할 수 있을지라도 Array를 사용하는 게 좋다. Array는 모든 프로그래머가 익숙한 기초 용어이기 때문이다.

    - 예시-2. 수학 등 특정 프로그래밍 Domain 내에서도 프로그래머/수학자에게 익숙한 용어인 "sin(x)"을 사용하는 게 좋다. 원칙적으로는 "sine"이 complete word 임에도 불구하고, 약어가 더 많이 통용되기 때문이다. (In this case, precedent outweighs the guideline to avoid abbreviations.)

## Conventions (규칙)

### General Conventions (일반적인 규칙)

- Document the complexity of any computed property that is not O(1). (complexity가 O(1)이 아닌 연산 프로퍼티는 모두 문서화한다.) 종종 프로퍼티 접근이 중요한 연산을 거치지 않는다고 생각한다. 이는 저장 프로퍼티를 mental model로 갖기 때문이다. 하지만 그렇지 않을 때가 있다. ???

    *mental model : 어떤 사물을 기억하기 위해 중요한 특징이라고 인식하는 것이다. 여기서는 프로퍼티를 기억할 때 보통 저장 프로퍼티를 먼저 떠올린다는 의미이다.

- free function (자유 함수???) 보다 메서드 및 프로퍼티가 선호된다. free function은 특수한 상황에서만 사용한다.
    1. 명백한 self가 없을 때
    2. 함수가 제약 없는 제네릭 (unconstrained generic)일 때 ???
    3. 함수 문법이 기존 도메인 표기의 일부일 때

        ```swift
        min(x, y, z)
        print(x)
        sin(x)
        ```

- Case conventions를 따른다. Type 및 Protocol의 이름은 UpperCamelCase를 사용하고, 그외는 모두 lowerCamelCase를 사용한다.

    보통 미국식 영어에서 대문자로 표현하는 Acronyms (두문자어, 머리글자만 따서 만든 단어) 및 Initialisms (이니셜)은 case conventions에 따라 대문자 또는 소문자로 일관되게 사용한다.

    ```swift
    var utf8Bytes: [UTF8.CodeUnit]
    var isRepresentableAsASCII = true
    var userSMTPServer: SecureSMTPServer
    ```

    그외 Acronyms은 일반 단어로 취급한다.

    ```swift
    var radarDetector: RadarScanner
    var enjoysScubaDiving = true
    ```

- 동일한 기본 의미를 공유하거나, 특정 domain에서 동작할 때 메서드는 base name을 공유할 수 있다.

    메서드가 본질적으로 동일하게 동작하므로 base name을 공유하는 예시이다. (parameter type은 다르지만)
    *iff : (수학 용어) if and only if (필요충분조건을 나타냄)

    ```swift
    extension Shape {
      /// Returns `true` iff `other` is within the area of `self`.
      func contains(_ other: Point) -> Bool { ... }

      /// Returns `true` iff `other` is entirely within the area of `self`.
      func contains(_ other: Shape) -> Bool { ... }

      /// Returns `true` iff `other` is within the area of `self`.
      func contains(_ other: LineSegment) -> Bool { ... }
    }
    ```

    geometric types ??? and collections은 별도의 domain 이므로 동일한 프로그램 내에서 base name을 공유하는 예시이다. ???

    ```swift
    extension Collection where Element : Equatable {
      /// Returns `true` iff `self` contains an element equal to `sought`.
      func contains(_ sought: Element) -> Bool { ... }
    }
    ```

    index 메서드는 다른 의미를 가지므로 각각 다른 이름으로 지정해야 한다.

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

- 문서의 가독성을 높이는 parameter 이름을 지정한다. 함수 사용 시점에 parameter 이름이 드러나지 않을 수 있지만, 함수를 설명하는 중요한 역할을 수행한다.

    ```swift
    /// Return an `Array` containing the elements of `self` that satisfy `predicate`.  // 좋은 예시. 자연스럽게 읽힌다.
    func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]

    /// Replace the given `subRange` of elements with `newElements`.
    mutating func replaceRange(_ subRange: Range, with newElements: [E])
    ```

    ```swift
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

    사용자 입장에서는 method family의 모든 member에 대한 개별적인 설명을 읽고 이해해야 메서드를 사용할 수 있다. method family 중에서 사용할 메서드를 결정하려면, 모든 메서드를 이해해야 하기 때문이다. 또한 가끔 예상치 못한 관계 (예를 들어 `foo(bar: nil)` 및 `foo()`는 항상 동의어가 아님)는 거의 동일한 문서 안에서 미세한 차이를 찾아내야 하는 지루한 상황을 만든다. 매개변수 기본값을 사용하는 단일 메서드를 사용하면 훨씬 뛰어난 프로그래밍이 가능하다.

- 매개변수 기본값이 있는 parameter는 parameter list의 끝부분에 배치한다. 매개변수 기본값이 없는 parameter가 메서드의 의미상 더 중요하고, 메서드 호출 장소에서 안정적인 초기화 패턴을 제공하기 때문이다.

### Argument Labels

```swift
func move(from start: Point, to end: Point)
x.move(from: x, to: y)
```

- argument를 구분하는 것이 무의미한 경우 argument label을 생략한다. ex. `min(number1, number2)`, `zip(sequence1, sequence2)`
- 이니셜라이저가 value를 유지하는 type 변환 (value preserving type conversions ???)을 수행할 때, 첫번째 argument label을 생략한다. ex. `Int64(someUInt32)`

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

    ㅇ

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

## Special Instructions
