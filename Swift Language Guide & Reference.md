# Swift Language Guide & Reference

Created: August 8, 2021 3:14 PM
Last Edited Time: October 2, 2021 6:39 AM
Property: Official

- Contents

# π¦ Language Guide

# 1. The Basics (90%)

μμΈν λ΄μ©μ λΈλ‘κ·Έμμ νμΈν΄μ£ΌμΈμ. [https://applecider2020.tistory.com/11](https://applecider2020.tistory.com/11)

## Numeric Type Conversion

The range of numbers that can be stored in an integer constant or variable is different for each numeric type. 

- An Int8 variable can store numbers between -128 and 127 (μμλ -128 ~ -1μΈ 128κ° (2^7), λλ¨Έμ§λ (0 λ° μμ) 0 ~ 127μΈ 128κ° (2^7)) β μ΄ 256κ° (2^8) μ¦, 8λΉνΈ μ€μμ 1λΉνΈλ λΆνΈ ννμ μν΄ μ¬μ©λκ³ , λ¨μ 7λΉνΈλ‘ μ«μλ₯Ό νν
- Whereas a UInt8 variable can store numbers between 0 and 255 (256κ° (2^8)).

Because each numeric type can store a different range of values, you must opt in to numeric type conversion on a case-by-case basis. This opt-in approach prevents hidden conversion errors and helps make type conversion intentions explicit in your code.
*opt in to sth : ~μ λμ/μ°Έμ¬νκΈ°λ‘ μ ννλ€. opt out of sth : ~μ μ°Έμ¬νμ§ μκΈ°λ‘ μ ννλ€.

To convert one specific number type to another, you initialize a new number of the desired type with the existing value. ('UInt16 typeμ μ΄κΈ°ν'λ₯Ό ν΅ν΄ μλ‘μ΄ numberμ μμ±ν κ²μ΄λ€.)

```swift
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + UInt16(one) 
// calls UInt16(one) to create a new UInt16 initialized with the value of one, and uses this value in place of the original. (μλ³Έ λμ  μ΄ κ°μ μ¬μ©νλ€)
```

`SomeType(ofInitialValue)` is the default way to call the initializer of a Swift type and pass in an initial value.
Behind the scenes, UInt16 has an initializer that accepts a UInt8 value, ('UInt16 typeμ μ΄λμλΌμ΄μ 'λ₯Ό ν΅ν΄ μ΄κΈ°ννλ€. UInt8 typeμ κ°μ μ λ¬λ°μ μ μλ κ²μ 'UInt16 typeμ μ΄λμλΌμ΄μ ' μ€μμ UInt8μ μ λ¬λ°λ κ²μ΄ μκΈ° λλ¬Έμ΄λ€.) and so this initializer is used to make a new UInt16 from an existing UInt8. 
You canβt pass in any type here, howeverβit has to be a type for which UInt16 provides an initializer. Extending existing types to provide initializers that accept new types (including your own type definitions) is covered in Extensions.

- ex. `init(_ source: Double)` - Creates an integer from the given floating-point value, rounding toward zero.
    
    ![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled.png)
    

# 2. Basic Operators (100%)

μμΈν λ΄μ©μ λΈλ‘κ·Έμμ νμΈν΄μ£ΌμΈμ. [https://applecider2020.tistory.com/11](https://applecider2020.tistory.com/11)

# 3. Strings and Characters (90%)

Note: Swiftβs String type is bridged with Foundationβs NSString class. Foundation also extends String to expose methods defined by NSString. This means, if you import Foundation, you can access those NSString methods on String without casting.

- [ ]  bridged

## String Literals

You can include predefined String values within your code as *string literals*. A string literal is a sequence of characters surrounded by double quotation marks (").

```swift
// Use a string literal as an initial value for a constant/variable:
let someString = "Some string literal value"
```

- Special Characters in String Literals
    
    An arbitrary Unicode scalar value, written as \u{n}, where n is a 1β8 digit hexadecimal number.
    
    ```swift
    let blackHeart = "\u{2665}"      // β₯,  Unicode scalar U+2665
    let sparklingHeart = "\u{1F496}" // π, Unicode scalar U+1F496
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
    
- Extended String Delimiters (Delimiter : κ΅¬λΆμ)
    
    You can place a string literal within extended delimiters to include special characters in a string without invoking their effect.
    You place your string within quotation marks (") and surround that with number signs (#).
    
    ```swift
    print(#"Line 1\nLine 2"#) // Line 1\nLine 2
    
    let threeMoreDoubleQuotationMarks = #"""
    Here are three more double quotes: """ // escaping μμ΄ νΉμλ¬Έμλ₯Ό μ¬μ© κ°λ₯ (νΉμλ¬Έμ ν¨κ³Ό μμ΄)
    """#
    
    print(threeDoubleQuotationMarks) // Here are three more double quotes: """
    ```
    

## Initializing an Empty String

λΉ Stringμ μμ±νλ λ°©λ²μ 1) assign an empty string literal to a variable, λλ 2) initialize a new String instance with initializer syntax:

- [ ]  μ String instance λΌκ³  νμ§?????????????????

```swift
var emptyString = ""               // 1) empty string literal
var anotherEmptyString = String()  // 2) initializer syntax - empty μνμ string type μ΄λ€.
```

## Unicode

### Extended Grapheme Clusters

*grapheme : λ¬Έμμ (μλ―Έλ₯Ό λνλ΄λ μ΅μ λ¬Έμ λ¨μ)

The letter Γ© can be represented as the single Unicode scalar Γ© (LATIN SMALL LETTER E WITH ACUTE, or U+00E9). However, the same letter can also be represented as a pair of scalarsβa standard letter e (LATIN SMALL LETTER E, or U+0065), followed by the COMBINING ACUTE ACCENT scalar (U+0301). The COMBINING ACUTE ACCENT scalar is graphically applied to the scalar that precedes it, turning an e into an Γ© when itβs rendered by a Unicode-aware text-rendering system. (Accent scalarκ° κ·Έ μμ scalar eμ μ μ©λμ΄ Γ©λ‘ λ³κ²½λλ€.)

In both cases, the letter Γ© is represented as a single Swift Character value that represents an extended grapheme cluster. (λ κ²½μ° λͺ¨λ, λ¬Έμ Γ©λ νμ₯λ κ·Έλν λΆμ ν΄λ¬μ€ν°λ₯Ό λνλ΄λ λ¨μΌ Swift λ¬Έμ κ°μΌλ‘ νμλλ€.) In the first case, the cluster contains a single scalar; in the second case, itβs a cluster of two scalars: (μ²« λ²μ§Έ κ²½μ°, ν΄λ¬μ€ν°λ λ¨μΌ μ€μΉΌλΌλ₯Ό ν¬ν¨νλ€. λ λ²μ§Έ κ²½μ°, ν΄λ¬μ€ν°λ 2κ°μ μ€μΉΌλΌλ‘ κ΅¬μ±λλ€.)

```swift
let eAcute: Character = "\u{E9}"                // Γ©
let combinedEAcute: Character = "\u{65}\u{301}" // e followed by Μ => Γ©
```

## Counting Characters

String λ΄λΆμ Characterμ κ°μλ₯Ό μΈκΈ° μν΄ Stringμ count νλ‘νΌν°λ₯Ό μ¬μ©νλ€.

Note that Swiftβs use of extended grapheme clusters for Character values means that string concatenation and modification may not always affect a stringβs character count. 
(Extended grapheme clustersλ₯Ό μ¬μ©νμ¬ Character typeμ κ°μ μΆκ°νμ λ, Countingμ λ³νκ° μμ μ μλ€. μλ₯Ό λ€μ΄ accentλ₯Ό μΆκ°νμ¬ cafeκ° cafeΜκ° λλ κ²½μ°μ΄λ€.)

```swift
var word = "cafe"
print("the number of characters in \(word) is \(word.count)") // Prints "the number of characters in cafe is 4"

word += "\u{301}"    // COMBINING ACUTE ACCENT, U+0301

print("the number of characters in \(word) is \(word.count)") // Prints "the number of characters in cafeΜ is 4" - counting κ°μ λ³νκ° μλ€.
```

- [ ]  NSString

```swift
var word = "cafe"
let nsStringLength1 = NSString(string: word).length // μ΄κ²λ κ°λ₯

print("the number of characters in \(word) is \(word.count) and \(nsStringLength1)")
// Prints "the number of characters in cafe is 4 and 4"
```

Note: Extended grapheme clusters can be composed of multiple Unicode scalars. This means that different charactersβand different representations of the same characterβcan require different amounts of memory to store. Because of this, characters in Swift donβt each take up the same amount of memory within a stringβs representation. As a result, the number of characters in a string canβt be calculated without iterating through the string to determine its extended grapheme cluster boundaries.

If you are working with particularly long string values, be aware that the count property must iterate over the Unicode scalars in the entire string in order to determine the characters for that string.

- [ ]  ????

The count of the characters returned by the count property isnβt always the same as the length property of an NSString that contains the same characters. The length of an NSString is based on the number of 16-bit code units within the stringβs UTF-16 representation and not the number of Unicode extended grapheme clusters within the string.

- [ ]  NSStringμ lengthλ UTF-16 νν λ°©μμΌλ‘ 16bit λ¨μμ κ°μλ₯Ό κΈ°λ°μΌλ‘ νκΈ° λλ¬Έμ΄λ€. (Stringμ countλ μ λμ½λμ extended grapheme clusterμ κ°μλ₯Ό κΈ°λ°μΌλ‘ νλ€.) ???
- [ ]  μμ μ λ΅λ³ νμΈ

## *Accessing and Modifying a String

- String Indices
    
    Each String value has an associated *Index type,* String.Index, which corresponds to the position of each Character in the string. μ¦, String.Indexλ νλμ typeμ΄λ€. κ·Έ typeμ Index typeμ΄λ€.
    
    - [ ]  ????
    
    As mentioned above, different characters can require different amounts of memory to store, so in order to determine which Character is at a particular position, you must iterate over each Unicode scalar from the start or end of that String. For this reason, Swift strings canβt be indexed by integer values. μ¦, Swiftμμλ int typeμΌλ‘ indexingμ΄ λΆκ°νλ€.
    
    - [ ]  ?????????
    - `startIndex` property λ String μ²«λ²μ§Έ λ¬Έμμ μμΉμ μ κ·Όνλ€.
    - `endIndex` property λ String λ§μ§λ§ λ¬Έμμ λ€μ μμΉμ μ κ·Όνλ€. (λ°λΌμ endIndexλ Sting μλΈμ€ν¬λ¦½νΈμ λν invalid argumentμ΄λ€.)
    λ¨, λΉ λ¬Έμμ΄μ΄λ©΄ startIndex λ° endIndexλ λμΌνλ€.
    - Sting λ©μλμΈ `.index(before:)` / `.index(after:)` λ₯Ό ν΅ν΄ νΉμ  indexμ 1μΉΈ μ/λ€μ μ κ·Όνλ€. λλ `.index(_:offsetBy:)` λ₯Ό ν΅ν΄ νΉμ  indexμμ nμΉΈ λ§νΌ λ¨μ΄μ§ μμΉμ μ κ·Όνλ€. (+ λ€, - μ)
        
        ```swift
        // use subscript syntax to access theΒ CharacterΒ at a particularΒ StringΒ index. instance[index]
        let greeting = "Guten Tag!"
        
        print(greeting.startIndex) // Index(_rawBits: 1) - μ²«λ²μ§Έ λ¬Έμ Gμ μμΉμ μ κ·Ό -> μ΄ μμ²΄κ° Index typeμ΄κ³ , μλΈμ€ν¬λ¦½νΈμ instance[index]μ μ λ¬λλ€. (μλ μ°Έκ³ )
        print(greeting.endIndex)   // Index(_rawBits: 655361) - λ§μ§λ§ λ¬Έμ !μ λ€μ μμΉμ μ κ·Ό
        //print(greeting.index(before: 3)) // μ»΄νμΌ μλ¬ - Cannot convert value of type 'Int' to expected argument type 'String.Index'
        //print(greeting.index(after: 3))
        
        var first = greeting.startIndex
        print(type(of: first)) // Index - νμΈμ©
        
        greeting[greeting.startIndex] // G - 1) μ²«λ²μ§Έ λ¬Έμ Gμ μμΉμ μ κ·Όνκ³ , 2) ν΄λΉ indexλ₯Ό ν΅ν΄ μλΈμ€ν¬λ¦½νΈ λ¬Έλ²μ μ¬μ© -> μ²«λ²μ§Έ λ¬Έμ G
        greeting[greeting.index(before: greeting.endIndex)] // ! - 1) λ§μ§λ§ λ¬Έμ !μ λ€μ μμΉμ μ κ·Όνκ³ , 2) ν΄λΉ indexλ₯Ό λ©μλμ argumentλ‘ μ λ¬νμ¬ ν΄λΉ μμΉμ 1μΉΈ μμ μ κ·Ό, 3) ν΄λΉ indexλ₯Ό ν΅ν΄ μλΈμ€ν¬λ¦½νΈ λ¬Έλ²μ μ¬μ© -> !
        greeting[greeting.index(after: greeting.startIndex)] // u - μ²«λ²μ§Έ λ¬Έμ Gμ μμΉμ μ κ·Όνκ³ , ν΄λΉ indexλ₯Ό λ©μλ argumentλ‘ μ λ¬νμ¬ ν΄λΉ μμΉμ 1μΉΈ λ€μ μ κ·Ό, ν΄λΉ indexλ₯Ό ν΅ν΄ μλΈμ€ν¬λ¦½νΈ λ¬Έλ²μ μ¬μ© -> u 
        
        greeting[greeting.index(greeting.startIndex, offsetBy: 7)] // a - 1) μ²«λ²μ§Έ λ¬Έμ Gμ μμΉμ μ κ·Όνκ³ , 2) ν΄λΉ indexλ₯Ό λ©μλ argumentλ‘ μ λ¬νμ¬ ν΄λΉ μμΉμ +7μΉΈ λ€μ μ κ·Ό, 3) ν΄λΉ indexλ₯Ό ν΅ν΄ μλΈμ€ν¬λ¦½νΈ λ¬Έλ²μ μ¬μ© -> a 
        
        let index = greeting.index(greeting.startIndex, offsetBy: 7) // λ°λ‘ μμ λμΌ
        greeting[index] // a
        
        // strings rangeλ₯Ό λ²μ΄λ indexμ μ κ·Όνλ©΄ λ°νμ μλ¬κ° λ°μνλ€.
        //greeting[greeting.endIndex] // Error
        //greeting.index(after: greeting.endIndex) // Error
        ```
        
        ```swift
        // greetingκ³Ό μ μ¬ν breezing λ³μλ₯Ό ν΅ν΄ μ€ν
        let greeting = "Guten Tag!"
        var breezing = "12345 6789"
        
        print(greeting.startIndex) // Index(_rawBits: 1) - μ²«λ²μ§Έ λ¬Έμ Gμ μμΉμ μ κ·Ό 
        print(greeting.endIndex)   // Index(_rawBits: 655361) - λ§μ§λ§ λ¬Έμ !μ λ€μ μμΉμ μ κ·Ό
        
        // μ€ν-1. μμΉμ μ κ·Ό
        print(breezing.startIndex) // Index(_rawBits: 1) -> λκ°μ κ²°κ³Ό!
        print(breezing.endIndex) // Index(_rawBits: 655361)
        
        greeting[greeting.startIndex] // G - μ²«λ²μ§Έ λ¬Έμ Gμ μμΉμ μ κ·Όνκ³ , ν΄λΉ indexλ₯Ό ν΅ν΄ μλΈμ€ν¬λ¦½νΈ λ¬Έλ²μ μ¬μ© -> μ²«λ²μ§Έ λ¬Έμ G
        greeting[greeting.index(before: greeting.endIndex)] // ! - λ§μ§λ§ λ¬Έμ !μ λ€μ μμΉμ μ κ·Όνκ³ , ν΄λΉ indexλ₯Ό λ©μλ argumentλ‘ μ λ¬νμ¬ ν΄λΉ μμΉμ 1μΉΈ μμ μ κ·Ό, ν΄λΉ indexλ₯Ό ν΅ν΄ μλΈμ€ν¬λ¦½νΈ λ¬Έλ²μ μ¬μ© -> !
        greeting[greeting.index(after: greeting.startIndex)] // u - μ²«λ²μ§Έ λ¬Έμ Gμ μμΉμ μ κ·Όνκ³ , κ·Έκ²μ 
        
        // μ€ν-2. μλΈμ€ν¬λ¦½νΈ [index] λ΄λΆ κ° λ³κ²½
        greeting[breezing.startIndex] // G -> λκ°μ κ²°κ³Ό!
        greeting[breezing.index(before: greeting.endIndex)] // !
        greeting[breezing.index(after: greeting.startIndex)] // u
        
        // μ€ν-3. μλΈμ€ν¬λ¦½νΈ instance κ° λ³κ²½
        breezing[greeting.startIndex] // 1 -> κ²°λ‘ μ μΌλ‘ String νΉμ  μμΉμ μ κ·Όνμ¬ μ»μ index typeμ ν΄λΉ String κ°κ³Ό μ°κ²°λμ΄μμ§ μλ€.
        breezing[greeting.index(before: greeting.endIndex)] // 9
        breezing[greeting.index(after: greeting.startIndex)] // 2
        ```
        
    
    Note: startIndex/endIndex νλ‘ν μ½ λ° index(before:)/index(after:)/index(_:offsetBy:) λ©μλλ Collection protocolμ μ€μνλ λͺ¨λ  Typeμ μ¬μ© κ°λ₯νλ€. (String ν¬ν¨ Array/Dictionary/Set λ±)
    
- Inserting and Removing
    - `insert(_:at:)` λ©μλλ₯Ό ν΅ν΄ 1κ° λ¬Έμλ₯Ό Stringμ νΉμ  indexμ μ½μνλ€.
    - `insert(contentsOf:at:)` λ©μλλ₯Ό ν΅ν΄ λ€λ₯Έ String μ½νμΈ λ₯Ό Stringμ νΉμ  indexμ μ½μνλ€.
    - `remove(at:)` λ©μλλ₯Ό ν΅ν΄ 1κ° λ¬Έμλ₯Ό Stringμ νΉμ  indexμμ μ­μ νλ€.
    - `removeSubrange(_:)` λ©μλλ₯Ό ν΅ν΄ Substringμ Stringμ νΉμ  index rangeμμ μ­μ νλ€.
    
    Note: insert(_:at:)/insert(contentsOf:at:)/remove(at:)/removeSubrange(_:) λ©μλλ RangeReplaceableCollection protocolμ μ€μνλ λͺ¨λ  Typeμ μ¬μ© κ°λ₯νλ€. (String ν¬ν¨ Array/Dictionary/Set λ±)
    

## Substrings

(μλΈμ€ν¬λ¦½νΈ λλ prefix λ©μλλ₯Ό ν΅ν΄) Stringμμ Substringμ κ°μ Έμ€λ©΄ λ€λ₯Έ Stringμ΄ μλλΌ μλ‘μ΄ Substring μΈμ€ν΄μ€κ° μμ±λλ€. (μ°Έμ‘° type)
μ₯κΈ°κ° μ μ₯νμ¬ μ¬μ©νλ €λ©΄ Substringμ StringμΌλ‘ type λ³νν΄μΌ νλ€.

```swift
// Substring μμ± λ°©λ²-1. μλΈμ€ν¬λ¦½νΈ
let name = "Marie Curie"
let firstSpace = name.firstIndex(of: " ") ?? name.endIndex // μμμλΆν° search for a space " " -> ν΄λΉ μμΉμ index typeμ μμμ ν λΉνλ€.
let firstName = name[..<firstSpace] // "Marie" - μλΈμ€ν¬λ¦½νΈ λ¬Έλ² μΈμ€ν΄μ€[index range]λ₯Ό ν΅ν΄ Substringμ μμ±νλ€. name.prefix(upTo: firstSpace)μ λμΌνλ€!

// Type νμΈ
print(type(of: firstSpace)) // Index
print(type(of: firstName))  // Substring

// Substring μμ± λ°©λ²-2. prefix λ©μλ (prefix(_ maxLength:)) / prefix(through:) / prefix(upTo:)
let str = "Hello! Swift"
str.prefix(6) // "Hello!"
str.prefix(100) // "Hello! Swift" - parameterκ° maxLengthμ΄λ―λ‘ String length λ³΄λ€ ν° κ°μ argumentλ‘ μ λ¬ν΄λ crashκ° λ°μνμ§ μλλ€. 

let index = str.index(str.startIndex, offsetBy: 5) // μ²«λ²μ§Έ λ¬Έμμ μμΉμμ +5μΉΈ λ€μ indexλ₯Ό μμμ ν λΉνλ€. (λ¬Έμ !μ μμΉ)
str.prefix(through: index) // "Hello!" - μλΈμ€ν¬λ¦½νΈ λ¬Έλ²μ str[...index]μ λμΌνλ€.
str.prefix(upTo: index)    // "Hello", not include index specified (!) - str[..<index]μ λμΌνλ€.

// Type νμΈ
print(type(of: str.prefix(through: index))) // Substring
print(type(of: str.prefix(upTo: index))) // Substring
```

```swift
let greeting = "Hello, world!"
let index = greeting.firstIndex(of: ",") ?? greeting.endIndex // μμμλΆν° search for "," -> ν΄λΉ μμΉμ index typeμ μμμ ν λΉνλ€.
let beginning = greeting[..<index] // "Hello" - μλΈμ€ν¬λ¦½νΈ λ¬Έλ²μΌλ‘ Substringμ μμ±νλ€. greeting.prefix(upTo: index)μ λμΌνλ€!

// Convert the Substring to a String for long-term storage.
let newString = String(beginning)
```

- μ°Έκ³  - prefix λ©μλ
    - `prefix(while:)` λ©μλλ₯Ό ν΅ν΄ return falseκ° λμ€κΈ° μ κΉμ§μ λΆλΆμ ν΄λΉνλ prefixλ₯Ό μ»λλ€.
        
        ```swift
        let str = "Hello! Swift"
        str.prefix(while: { (character) -> Bool in
            return character != " " // space " "κ° λμ€λ©΄ return falseν¨
        }) // Hello!
        ```
        
    - `firstIndex(of:)` λ©μλλ₯Ό ν΅ν΄ νΉμ  κ°μ μμμλΆν° search forνμ¬ ν΄λΉ κ°μ΄ λνλ μμΉμ indexμ μ κ·Όνλ€.
    `lastIndex(of:)` λ©μλλ₯Ό ν΅ν΄ νΉμ  κ°μ λ€μμλΆν° search forνμ¬ ν΄λΉ κ°μ΄ λνλ μμΉμ indexμ μ κ·Όνλ€.
        
        ```swift
        let str = "Hello! Swift"
        if let index = str.firstIndex(of: "l") {
            str.prefix(through: index) 
        } // "Hel"
        ```
        

μ±λ₯ μ΅μ νλ‘μ Substringμ original Stringμ μ μ₯νλ λ©λͺ¨λ¦¬μ μΌλΆλ₯Ό μ¬μ¬μ©νκ±°λ, λλ λ€λ₯Έ Substringμ μ μ₯νλ λ©λͺ¨λ¦¬μ μΌλΆλ₯Ό μ¬μ¬μ©νλ€.
μ΄λ¬ν μ±λ₯ μ΅μ νλ₯Ό ν΅ν΄ (String λλ Substringμ μμ νκΈ° μ μλ) λ©λͺ¨λ¦¬λ₯Ό λ³΅μ¬ν΄λ μ±λ₯μ΄ μ νλμ§ μλλ€. Substringμ μ¬μ©νλ λμμλ μ μ²΄ original Stringμ΄ λ©λͺ¨λ¦¬μ λ€μ΄μμ΄μΌ νλ€.

- λ©λͺ¨λ¦¬ κ΅¬μ‘°
    
    ![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%201.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%201.png)
    
- [ ]  μ 12345λ‘ μλ°λμ§? λ³κ²½λ String κ°μ λ€λ₯Έ λ©λͺ¨λ¦¬ κ³΅κ°μ μ μ₯νλ?
    - String λλ Substringμ μμ ν μ§νμ λ©λͺ¨λ¦¬λ₯Ό μλ‘­κ² ν λΉνκΈ° λλ¬Έ? νμ§λ§ μλ‘μ΄ λ©λͺ¨λ¦¬λ₯Ό ν λΉλ°μλ μ¬μ ν Stringμ΄ μλλΌ SubstringμΈκ±΄κ°?

```swift
var greeting = "Hello, world!"
var index = greeting.firstIndex(of: ",") ?? greeting.endIndex
var beginning = greeting[..<index] // Hello - Substring μμ± (original Stringμ λ©λͺ¨λ¦¬ μΌλΆλ₯Ό κ³΅μ )

// original Stringμ κ°μ λ³κ²½ν¨
greeting = "12345, 789"
print(beginning) // Hello -> μ 12345λ‘ μλ°λμ§? λ³κ²½λ μ΄νμλ λ³κ²½λ String κ°μ λ€λ₯Έ λ©λͺ¨λ¦¬ κ³΅κ°μ μ μ₯νλ?
```

Note: Both String and Substring conform to the StringProtocol protocol, which means itβs often convenient for string-manipulation functions to accept a StringProtocol value. You can call such functions with either a String value or Substring value.
String λ° Substring λͺ¨λ StringProtocol protocolμ μ€μνλ€. λ°λΌμ Stirngμ λ€λ£¨λ ν¨μλ₯Ό μ¬μ©ν  λ StringProtocolμ κ°μ μμ©νλ κ²μ΄ νΈλ¦¬νλ€. ???? String κ° λλ Substring κ°μΌλ‘ μ΄λ¬ν ν¨μλ₯Ό νΈμΆνλ€.

# 4. Collection Types  (90%)

μμΈν λ΄μ©μ λΈλ‘κ·Έμμ νμΈν΄μ£ΌμΈμ. [https://applecider2020.tistory.com/11](https://applecider2020.tistory.com/11)

Note: array, set, and dictionary types are implemented as generic collections.

- [ ]  generic Collection???

## Arrays

Note: Swiftβs `Array` type is bridged to Foundationβs `NSArray` class.

- [ ]  [https://developer.apple.com/documentation/swift/array#2846730](https://developer.apple.com/documentation/swift/array#2846730)

### Accessing and Modifying an Array

You access/modify an array through 1) its methods and properties, or by 2) using subscript syntax.

- read-only `count` propertyλ₯Ό ν΅ν΄ arrayμ item κ°μλ₯Ό νμΈνλ€.
- Boolean `isEmpty` propertyλ₯Ό ν΅ν΄ μΆμ½μΌλ‘ count propertyμ κ°μ΄ 0μΈμ§ νμΈνλ€. (array itemμ΄ 0κ°μΈμ§ νμΈ)
- arrayμ `append` methodλ₯Ό ν΅ν΄ arrayμ λμ μλ‘μ΄ itemμ μΆκ°νλ€.
- addition assignment operator (`+=`)λ₯Ό ν΅ν΄ 1κ° λλ μ¬λ¬ κ°μ compatible itemμ append νλ€.
    
    ```swift
    print("The shopping list contains \(shoppingList.count) items.")  // Prints "The shopping list contains 2 items."
    
    if shoppingList.isEmpty {
        print("The shopping list is empty.")
    } else {
        print("The shopping list isn't empty.")  // Prints "The shopping list isn't empty." (shoppingList.count == 2 μ΄λ―λ‘)
    }  
    
    shoppingList.append("Flour")  // shoppingList now contains 3 items, and someone is making pancakes
    
    shoppingList += ["Baking Powder"]  // shoppingList now contains 4 items
    shoppingList += ["Chocolate Spread", "Cheese", "Butter"]  // shoppingList now contains 7 items
    ```
    
- Retrieve a value from the array by using subscript syntax, passing the index of the value you want to retrieve within square brackets immediately after the array name:
(μλΈμ€ν¬λ¦½νΈ λ¬Έλ²μ μ¬μ©νμ¬ arrayμμ κ°μ κΊΌλΈλ€. κΊΌλ΄λ €λ κ°μ indexλ₯Ό array name λ°λ‘ λ€μ λκ΄νΈ[] λ΄λΆμ μ λ¬νλ€. μ¦, arrayName[index] νν)
    
    ```swift
    var firstItem = shoppingList[0]  // firstItem is equal to "Eggs"
    ```
    
- You can use subscript syntax to change an existing value at a given index: (μλΈμ€ν¬λ¦½νΈ λ¬Έλ²μ μ¬μ©νμ¬ νΉμ  indexμ κΈ°μ‘΄ κ°μ λ³κ²½ κ°λ₯νλ€.)
    
    ```swift
    shoppingList[0] = "Six eggs"  // the first item in the list is now equal to "Six eggs" rather than "Eggs"
    ```
    
    *When you use subscript syntax, the index you specify needs to be valid. For example, writing `shoppingList[shoppingList.count] = "Salt"` (indexλ 0λΆν° μμνλ―λ‘ item κ°μλ array.count -1 κ³Ό μΌμΉνλ―λ‘) to try to append an item to the end of the array results in a runtime error. (μλΈμ€ν¬λ¦½νΈ λ¬Έλ²μ μ¬μ©ν  λ, indexκ° valid νμ§ μμΌλ©΄ λ°νμ μλ¬κ° λ°μνλ€.) 
    
    Dictionaryμ λ¬λ¦¬ Arrayλ invalid indexλ₯Ό λ£λ κ²½μ°, μλ‘μ΄ itemμ΄ μΆκ°λμ§ μλλ€.
    `shoppingList[100]= "new item"` // λ°νμ μλ¬ λ°μ
    
- You can also use subscript syntax to change a range of values at once, even if the replacement set of values has a different length than the range you are replacing. 
(μλΈμ€ν¬λ¦½νΈ λ¬Έλ²μ ν΅ν΄ νΉμ  λ²μμ κ°μ λ³κ²½ κ°λ₯νλ€. μ΄λ λ³κ²½νλ €λ κ°κ³Ό λ³κ²½ν  κ°μ itemμ κ°μκ° λ¬λΌλ κ°λ₯νλ€.)
The following example replaces "Chocolate Spread", "Cheese", and "Butter" with "Bananas" and "Apples":
    
    ```swift
    print(shoppingList.count) // 7
    shoppingList[4...6] = ["Bananas", "Apples"]. // shoppingList now contains 6 items (3κ° itemμ 2κ° itemμΌλ‘ λμ²΄ κ°λ₯νλ€.)
    print(shoppingList.count) // 6
    ```
    
- arrayμ `insert(_:at:)` methodλ₯Ό ν΅ν΄ νΉμ  indexμ itemμ insert νλ€. ν΄λΉ indexμ μλ κΈ°μ‘΄ itemμ λ€λ‘ λ°λ¦°λ€.
- `remove(at:)` methodλ₯Ό ν΅ν΄ νΉμ  indexμ itemμ μ­μ νλ€. μ΄λ μ­μ λ itemμ΄ return λλ€. (return κ°μ νμν  λλ§ μ¬μ©νλ€.)
Any gaps in an array are closed when an item is removed, and so the value at index 0 is once again equal to "Six eggs": (μ­μ λ itemμ index μλ¦¬λ λ°λ‘ λ€μ itemμ΄ μ΄λνμ¬ Gapμ μ±μ)
- `removeLast()` methodλ₯Ό ν΅ν΄ arrayμ λ§μ§λ§ itemμ μ­μ νλ€. (remove(at:) λ©μλλ₯Ό μ¬μ©νλ κ²λ³΄λ€ λ«λ€.) μ΄λλ μ­μ λ itemμ΄ return λλ€. 
Use the removeLast() method rather than the remove(at:) method to avoid the need to query the arrayβs count property.
    
    ```swift
    shoppingList.insert("Maple Syrup", at: 0)  // "Maple Syrup" is now the first item in the list, shoppingList now contains 7 items
    
    let mapleSyrup = shoppingList.remove(at: 0) // the item that was at index 0 has just been removed, shoppingList now contains 6 items, and no Maple Syrup
    // the mapleSyrup constant is now equal to the removed "Maple Syrup" string - μ­μ λ itemμ΄ return λ¨
    
    firstItem = shoppingList[0] // firstItem is now equal to "Six eggs" - λ°λ‘ λ€μ μλ itemμ΄ μ΄λνμ¬ Gapμ μ±μ
    
    let apples = shoppingList.removeLast() // the last item in the array has just been removed, shoppingList now contains 5 items, and no apples
    // the apples constant is now equal to the removed "Apples" string - μ­μ λ itemμ΄ return λ¨
    ```
    

## Sets

Note: Swiftβs Set type is bridged to Foundationβs NSSet class.

- [ ]  ???
- Hash Values for Set Types
    
    Note: μ¬μ©μ μ μ νμ (custom types)μ΄ Hashable Protocolμ μ€μνλλ‘ μ€μ νλ©΄, Setμ κ°, κ·Έλ¦¬κ³  Dictionaryμ ν€λ‘ μ¬μ©ν  μ μλ€. λ¨, μ΄ κ²½μ° μ§μ  1) hash(into:) λ©μλ λ° 2) == μ°μ°μ ν¨μλ₯Ό κ΅¬ν (implement)ν΄μΌ νλ€.
    
    Hashable protocol - [https://developer.apple.com/documentation/swift/hashable](https://developer.apple.com/documentation/swift/hashable)
    
    - μμλ μ΄ν΄κ° λλ€. hasherμ κΈ°λ₯μ λν΄μ μ°Ύμλ³΄μ
        - [ ]  'Hashing a value' means feeding its essential components into a hash function, represented by the Hasher type. Essential components are those that contribute to the typeβs implementation of Equatable. Two instances that are equal must feed the same values to Hasher in hash(into:), in the same order. ???

## Dictionaries

### Accessing and Modifying a Dictionary

1) methods and propertiesλ₯Ό μ¬μ©νκ±°λ 2) subscript syntaxλ₯Ό μ¬μ©νμ¬ Dictionaryμ μ κ·Ό/μμ μ΄ κ°λ₯νλ€.

- subscript syntaxλ₯Ό ν΅ν΄ Dictionaryμ νΉμ  keyμ valueλ₯Ό κΊΌλΌ μ μλ€. 
Because itβs possible to request a key for which no value exists, a dictionaryβs subscript returns an optional value of the dictionaryβs value type.
Dictionaryμ requestν keyμ valueκ° μμΌλ©΄ subscriptλ κ·Έ valueλ₯Ό Optional typeμΌλ‘ λ°ννκ³ , valueκ° μμΌλ©΄??? subscriptλ nilμ λ°ννλ€.
    - [ ]  μ§λ¬Έ - keyλ μμ§λ§, valueκ° nilμΈ κ²½μ°
    
    ```swift
    if let airportName = airports["DUB"] {
        print("The name of the airport is \(airportName).") // Prints "The name of the airport is Dublin Airport." - requestν keyκ° μλ€.
    } else {
        print("That airport isn't in the airports dictionary.")
    }
    
    // requestν keyκ° μλ κ²½μ°
    if let airportName = airports["Key-doesn't exist"] {
        print("The name of the airport is \(airportName).")
    } else {
        print("That airport isn't in the airports dictionary.") // That airport isn't in the airports dictionary. - requestν keyκ° μ‘΄μ¬νμ§ μμΌλ―λ‘ nil λ°ν
    }
    
    // keyλ μμ§λ§, valueκ° nilμΈ κ²½μ°
    var airports: [String: Any?] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"] // valueμ nilμ ν λΉνκΈ° μν΄ Dictionary μ μΈλΆμμ typeμ λ°κΏμΌ νλ€.
    
    var valueX: Any? = nil
    airports["test"] = valueX
    print(airports) // ["test": nil, "DUB": Optional("Dublin Airport"), "LHR": Optional("London Heathrow"), "YYZ": Optional("Toronto Pearson"), "New Key": Optional("set this value")]
    
    if let airportName = airports["test"] { // keyλ μμ§λ§, valueκ° nilμΈ μν©
        print("The name of the airport is \(airportName).") // The name of the airport is nil. - ???????????????? nilμ΄ κ·Έλλ‘ λ°νλλ€.
    } else {
        print("That airport isn't in the airports dictionary.")
    }
    ```
    

# 5. Control Flows (95%)

μμΈν λ΄μ©μ λΈλ‘κ·Έμμ νμΈν΄μ£ΌμΈμ. [https://applecider2020.tistory.com/11](https://applecider2020.tistory.com/11)

### For-In Loops

You use the `for-in` loop to iterate over a sequence, such as items in an array, ranges of numbers, or characters in a string.
*Sequence Protocol : A type that provides sequential, iterated access to its elements. (Swift κΈ°λ³Έ typeμμλ Collectionμ΄ μ΄μ ν΄λΉλλ€.)

μ΄μΈμλ, you can use this syntax to iterate any collection, including your own classes and collection types, as long as those types conform to the Sequence protocol.

- [ ]  classλ₯Ό μ΄λ»κ² iterate???

```swift
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
} // 1 times 5 is 5, 2 times 5 is 10, 3 times 5 is 15, 4 times 5 is 20, 5 times 5 is 25,
```

- μ€ν
    
    ```swift
    var closedRange = 3.0...5.0 // 3...5 λ κ°λ₯νλ€
        
    for n in closedRange { // μ»΄νμΌ μλ¬ - Protocol 'Sequence' requires that 'Double.Stride' (aka 'Double') conform to 'SignedInteger'
        print(n)
    } // 3, 4, 5 μΆλ ₯
    ```
    

If you donβt need each value from a sequence, you can ignore the values by using `_` (underscore).  β Wildcard Pattern

For this calculation, the individual counter values each time through the loop are unnecessaryβthe code simply executes the loop the correct number of times. The underscore character (_) used in place of a loop variable ??? (μ defaultλ μμμΈλ°, loop variableμ΄λΌκ³  νμ§?) causes the individual values to be ignored and doesnβt provide access to the current value during each iteration of the loop.

```swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")  // Prints "3 to the power of 10 is 59049"
```

κ΅¬ν - Consider drawing the tick marks for every minute on a watch face.

```swift
// 1) draw 60 tick marks, starting with the 0 minute.
let minutes = 60
for tickMark in 0..<minutes { // Use the half-open range operator (..<) to include the lower bound but not the upper bound.
    // render the tick mark each minute (60 times)
}

// 2) prefer one mark every 5 minutes
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) { // Use the stride(from:to:by:) function to skip the unwanted marks.
    // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55) <- 60 λΆν¬ν¨
}

// 3) prefer one mark every 3 hours
let hours = 12
let hourInterval = 3
for tickMark in stride(from: 3, through: hours, by: hourInterval) { // Closed ranges are also available, by using stride(from:through:by:)
    // render the tick mark every 3 hours (3, 6, 9, 12) <- 12 ν¬ν
}
```

### While Loops

While 

κ΅¬ν - This example plays a simple game of Snakes and Ladders.

![Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%202.png](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%202.png)

```swift
let finalSquare = 25 // constant finalSquare is used to initialize the array and also to check for a win condition later in the example.
var board = [Int](repeating: 0, count: finalSquare + 1) // The game board is represented by an array of Int values. - Because the players start off the board, the board is initialized with 26 size, not 25.

// Some squares are then set to have more specific values for the snakes and ladders. (labber base: +, snake head: -)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08

// To align the values and statements, the unary plus operator (+i) is explicitly used with the unary minus operator (-i) 
// (κ°κ³Ό statementλ₯Ό μΌμΉμν€κΈ° μν΄ λ¨ν­ λ§μ μ°μ°μ (+i) λ° λ¨ν­ λΊμ μ°μ°μ (-i)λ₯Ό λͺμμ μΌλ‘ μ¬μ©νλ©°)
// and numbers lower than 10 are padded with zeros. (1μ΄λ©΄ 01λ‘ νκΈ°νλ€λ λ») (Neither stylistic technique is strictly necessary, but they lead to neater code.) (2κ°μ§ stylistic technique λͺ¨λ μκ²©ν νμν κ²μ μλμ§λ§, λ κΉλν μ½λλ₯Ό λ§λ λ€.)

var square = 0 // playerμ νμ¬ μμΉ
var diceRoll = 0

while square < finalSquare {
    // roll the dice
    diceRoll += 1 // The result is a sequence of diceRoll values thatβs always 1, 2, 3, 4, 5, 6, 1, 2... (used a simple approach)
    if diceRoll == 7 { diceRoll = 1 }
    // move by the rolled amount
    square += diceRoll

    if square < board.count { // board.count == 26 <- μ finalSquareκ° μλμ§??????
        // if we're still on the board, move up/down for a snake/ladder
        square += board[square]
    }
}
print("Game over!")
```

- [x]  if square < board.count { // board.count == 26 <- μ finalSquareκ° μλμ§??????
    - ifλ¬Έ λ΄λΆμ board[square]μ΄ λ°νμ μλ¬λ₯Ό λ°μμν€μ§ μμ μ‘°κ±΄λ§ μ²΄ν¬ν κ²μ΄λΌμ μΈλ― (note λΆλΆ μ°Έκ³ )

Repeat-While

```swift
// while loopμ λμΌν¨
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)

board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08

var square = 0
var diceRoll = 0

// repeat-whileλ¬Έ μ¬μ© - the first action in the loop is to check for a ladder/snake.
repeat {
    // move up/down for a snake/ladder
    square += board[square]  // whileλ¬Έμ 'if square < board.count {...}' statementκ° νμ μλ€! (= 'array bounds check'μ΄ νμ μλ€)

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

- [x]  ν¨ν΄ λ§€μΉ­ (pattern matching)μ΄λ?
    
    νΉμ  ννμ ν¨ν΄μ μ°Ύμλ΄λ κ²μ΄λ€. μλ₯Ό λ€μ΄ νν (1, "μΌ")μ ν¨ν΄μ (Int, String)μ΄λ€. (2, "μ΄")μ ν¨ν΄κ³Ό ν¨ν΄ λ§€μΉ­μ νλ©΄, λ ν¨ν΄μ μΌμΉνλ€λ κ²μ μ μ μλ€.
    
- μ°Έκ³  - if/switch μ°¨μ΄μ 
    
    μ»΄νμΌν  λ λμμ μ°¨μ΄κ° μλ€.
    if-elseλ¬Έμ trueκ° λμ¬ λκΉμ§ μμ°¨μ μΌλ‘ λͺ¨λ  μ‘°κ±΄μ νμΈνκ³ ,
    switchλ¬Έμ μ‘°κ±΄λ¬Έμ ν΄λΉνλ caseλ‘ λ°λ‘ μ΄λ (jump κΈ°λ°)νλ€. β λ°λΌμ λλ€ μ μ ν κ²½μ°, switchλ¬Έμ μ¬μ©νλ κ² μ±λ₯ μΈ‘λ©΄μμ μ’λ€.
    
    [https://pongsoyun.tistory.com/121](https://pongsoyun.tistory.com/121)
    
    - [ ]  jump tableμ΄ λ­μ§?

### *Switch

each case is a separate branch of code execution. The switch statement determines which branch(case) should be selected. β This procedure is known as *switching* on the value thatβs being considered.

- [x]  μ switchλ¬Έμ caseλ λ€μ¬μ°κΈ°λ₯Ό μν κΉ? switch indentation style swift / why switch case should be indented swift
    - (μ¬λ¬ κ°μ§ μ€μ΄ μμ)
    - Backing the cases to the same indentation level as the "switch" is a common style for all C-inspired languages.
    - switch ... caseμ μλ―Έλ κΈ°λ³Έμ μΌλ‘ if...else if μ κ°λ€. λ°λΌμ λμΌν indentμ switchμ caseλ₯Ό λ°°μΉνλ€.
        
        ```swift
        if condition {
            // ...
        } else if {
            // ...
        }
        ```
        
    - μνλ©΄ Xcode settingμ λ³κ²½ κ°λ₯ν¨ - [https://stackoverflow.com/questions/42376988/xcode-changing-the-indentation-rules-for-switch-statements](https://stackoverflow.com/questions/42376988/xcode-changing-the-indentation-rules-for-switch-statements)

## Control Transfer Statements

- continue
    
    The continue statement tells a loop to stop what itβs doing and start again at the beginning of the next iteration through the loop. (λ°λ³΅λ¬Έμ λ€μ iterationμΌλ‘ λμ΄κ°)
    
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
    
    The break statement ends execution of an entire control flow statement immediately. The break statement can be used inside a switch or loop statement when you want to terminate the execution of the switch/loop statement earlier than would otherwise be the case. (switchλ¬Έ λλ λ°λ³΅λ¬Έ μμ²΄λ₯Ό μ¦μ μ’λ£ν¨)
    
- Labeled Statements
    
    To achieve these aims, you can mark a loop statement/conditional statement with a *statement label*.
    
    - With a conditional statement, you can use a statement label with the break statement to end the execution of the labeled statement.
    - With a loop statement, you can use a statement label with the break or continue statement to end or continue the execution of the labeled statement.
    
    μ¦, break λλ continueλ₯Ό μ μ©ν  λμμ΄ labeled statement μ΄λ€.
    
    ```swift
    label name: while condition {  // the principle is the same for all loops and switch statements.
        statements
    }
    ```
    
    The example uses the break and continue statements with a labeled while loop for an adapted version of the Snakes and Ladders game. This time around, the game has an extra rule:
    
    - To win, you must landΒ *exactly*Β on square 25. (you must roll again until you roll the exact number needed to land on square 25.)
    - [ ]  labelμ μ§μ°λ©΄ λ¬΄νλ£¨ν λμκ°λ―μ΄ μ€νλ¨;;; μμ§?
    
    ```swift
    let finalSquare = 25  // initialized in the same way as before:
    var board = [Int](repeating: 0, count: finalSquare + 1)
    
    board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
    board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
    
    var square = 0  // playerμ νμ¬ μμΉ
    var diceRoll = 0
    
    // a labeled while loop (μ νν square == 25 μ΄λ©΄ μ’λ£λ¨)
    gameLoop: while square != finalSquare {
        diceRoll += 1
        if diceRoll == 7 { diceRoll = 1 }
    
        switch square + diceRoll {  // uses a switch statement to consider the result of the move and to determine whether the move is allowed.
        case finalSquare:
            // diceRoll will move us to the final square, so the game is over
            break gameLoop
        case let newSquare where newSquare > finalSquare:  // switchλ¬Έμ value binding κΈ°λ₯μ μ¬μ© (square + diceRollμ κ°μ΄ newSquareμ ν λΉλ¨) - A switch case can name the value or values it matches to temporary constants/variables for use in the body of the case.
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
    
    - μ°Έκ³  - repeat-while
        
        ```swift
        let finalSquare = 25
        var board = [Int](repeating: 0, count: finalSquare + 1)
        
        board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
        board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
        
        var square = 0
        var diceRoll = 0
        
        // repeat-whileλ¬Έ μ¬μ© - the first action in the loop is to check for a ladder/snake.
        repeat {
            // move up/down for a snake/ladder
            square += board[square]  // μμ 'if square < board.count {}' statementκ° νμ μλ€! (= 'array bounds check'μ΄ νμ μλ€)
        
            // roll the dice
            diceRoll += 1
            if diceRoll == 7 { diceRoll = 1 }
            // move by the rolled amount
            square += diceRoll
        } while square < finalSquare
        print("Game over!")
        ```
        
    
- [ ]  return - Functions ννΈ μ°Έκ³ 
- [ ]  throw - Propagating Errors Using Throwing Functions ννΈ μ°Έκ³ 

## Early Exit

else branch must transfer control to exit the code block in which the guard statement appears. It can do this with 1) a control transfer statement such as return, break, continue, or throw, or it can 2) call a function that doesnβt return, such as fatalError(_:file:line:).

guardλ¬Έμ μ‘°κ±΄μ΄ κ±°μ§μ΄λ©΄, else κ΅¬λ¬Έμ΄ μ€νλλ€. else κ΅¬λ¬Έμ guardλ¬Έμ΄ μ‘΄μ¬νλ μ½λ λΈλ­μ μ’λ£νκΈ° μν΄ λ°λμ μ μ΄μ μ΄λ (transfer control)μμΌμΌ νλ€. μ¦, 1) μ μ΄ μ λ¬ κ΅¬λ¬Έ (continue, break, return, throw) λλ 2) λ°ννμ§ μλ ν¨μ (fatalError(_:file:line:))λ₯Ό μ¬μ©ν΄μΌ νλ€.

- [ ]  μ€μ λ‘λ else κ΅¬λ¬Έμ μ’λ£νκΈ° μν΄μ κ°μ λ³΄μ΄λλ°????????? 
(guardλ¬Έμ΄ μ‘΄μ¬νλ ν° λ©μ΄λ¦¬ μ½λκ° μλλΌ)

```swift
func greet(person: [String: String]) {
    guard let name = person["name"] else { // if-letκ³Ό μ°¨μ΄μ  : Any variables/constants that were assigned values using an optional binding as part of the condition ??? are available for the rest of the code block that the guard statement appears in. ('gaurdλ¬Έμ΄ λ€μ΄μλ μ½λ λΈλ­' λ΄λΆμμ μ¬μ© κ°λ₯νλ€.)
        return // else branchλ 'guardλ¬Έμ΄ λ€μ΄ μλ μ½λ λΈλ­'μ μΈλΆλ‘ controlμ μ΄λμμΌμΌ νλ€. (1) control transfer statementλ₯Ό μ¬μ©νκ±°λ, 2) return κ°μ΄ μλ ν¨μλ₯Ό νΈμΆνλ€.)
//      fatalError("this is wrong") // 2) return λμ  return κ°μ΄ μλ ν¨μλ₯Ό νΈμΆν μμ
    }
    print("Hello \(name)!") 

// guard 1>2, let location = person["location"] else {  // μ‘°κ±΄μ μΆκ° (μ‘°κ±΄1 && μ‘°κ±΄2 λΆκ°) -> ν­μ falseμ΄λ―λ‘ else blockμ μ€νμν΄ (print μ΄ν ν¨μ greetκ° μ’λ£λ¨)
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

Swift has built-in support for checking API availability, which ensures that you donβt accidentally use APIs that are unavailable on a given deployment target. 

- [x]  deployment?
    - Software deployment refers to the process of running an application on a server or device. A software update or application may be deployed to a test server, a testing machine, or into the live environment, and it may be deployed several times during the development process to verify its proper functioning and check for errors. Another example of software deployment could be when a user downloads a mobile application from the App Store and installs it onto their mobile device.

The compiler uses availability information in the SDK (Software Development Kit) to verify that all of the APIs used in your code are available on the deployment target specified by your project. Swift reports an error at compile time if you try to use an API that isnβt available.

μ»΄νμΌλ¬λ *SDKμ λ€μ΄μλ μ¬μ© κ°λ₯ν API μ λ³΄λ₯Ό νμΈνκ³ , κ°λ°μ΄ μλ£λ μ½λ λ΄λΆμ λͺ¨λ  APIκ° μ μ νμ§ νμΈνλ€. μ¬μ© λΆκ°ν (unavailable) APIλ₯Ό μ¬μ©νλ©΄ μ»΄νμΌ μ€λ₯κ° λ°μνλ€.

*SDK (Software Development Kit)λ?

κ°λ°μκ° μ½κ² κ°λ°μ νλλ‘ λμμ£Όλ κ°λ° λκ΅¬ λͺ¨μμ΄λ€. νΉμ  κΈ°λ₯μ κ΅¬ννκΈ° μν΄ νμν ν΄λμ€/ν¨μ/νλ‘ν μ½ λ±μ κ°λ°μκ° λͺ¨λ κ΅¬ννμ§ μκ³ , Swift νμ€ λΌμ΄λΈλ¬λ¦¬μ κ°μ΄ μΈλΆμμ κ΅¬νν μ½λ ν¨ν€μ§λ₯Ό μ¬μ©νλ λ°©μμ΄λ€.

- [ ]  μ΄ λ΄μ©μ΄ λ§λ????

- [ ]  APIκ° νλ«νΌ & λ°°μ  μ λ³΄ λ§λ?????

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
- The last argument, *, is required (νμ μΈμ) and specifies that on any other platform, the body of the if executes on the minimum deployment target specified by your target. ???
    - [ ]  minimum deployment target????

# 6. Functions (95%)

- Defining and Calling Functions
    
    ```swift
    print(greet(person: "Anna")) // Prints "Hello, Anna!" - You call the greet(person:) function by passing it a String value after the person argument label
    print(greet(person: "Brian")) // Prints "Hello, Brian!" 
    ```
    
    You call the greet(person:) function by passing it a String value after the person argument label. Because the function returns a String value, greet(person:) can be wrapped in a call to the print function (greet ν¨μλ 'print ν¨μλ₯Ό νΈμΆνλ κ΅¬λ¬Έ' λ΄λΆμ λ€μ΄κ° μ μλ€.) to print that string and see its return value.
    
    - [x]  ν¨μ μ μμμ parameterλ₯Ό μ μν  λ argument labelμ μΆκ°λ‘ λΆμ΄μ§ μμλ, ν¨μνΈμΆ μ parameter nameμ΄ argument labelμ΄λΌκ³  λΆλ¦°λ€!
    μ¦, ν¨μ νΈμΆ μμ μμ argumentκ° μ λ¬λλ λΆλΆμ μμͺ½μ λ¬΄μ‘°κ±΄ 'argument label'μΈκ±΄κ°?
        - ν¨μ νΈμΆ μμ μμ argumentκ° μ λ¬λλ λΆλΆμ μμͺ½μ λ¬΄μ‘°κ±΄ 'argument label'μ΄ λ§λ€. μ μΈ μ argument labelμ λ°λ‘ μ§μ ν΄μ£Όμ§ μμΌλ©΄ μλμΌλ‘ parameter nameμ΄ argument labelμ΄ λλ€.

### Function Parameters and Return Values

You can define anything from a simple utility function (with a single unnamed parameter) to a complex function.

- [x]  utility function?
    - Utility methods perform common, often re-used functions which are helpful for accomplishing routine programming tasks. Examples might include methods connecting to databases, performing string manipulations, sorting and searching of collections of data, writing/reading data to/from files and so on.
        
        [https://www.quora.com/What-is-a-utility-method](https://www.quora.com/What-is-a-utility-method)
        
- Functions Without Return Values
    
    ```swift
    func greet(person: String) {
        print("Hello, \(person)!") // prints its own String value rather than returning it
    }
    greet(person: "Dave") // Prints "Hello, Dave!"
    
    var specialValueOfTypeVoid: Void = greet(person: "test") // Hello, test! μΆλ ₯ - ν¨μκ° ν λΉλ¨κ³Ό λμμ νΈμΆλλ€.
    //print(specialValueOfTypeVoid)  // ()
    ```
    
    Note: Strictly speaking, this version of the greet(person:) function does still return a value, even though no return value is defined. Functions without a defined return type return a special value of type `Void`. This is simply an empty tuple, written as `()`.
    (μλ°νλ return typeμ μ μνμ§ μμ ν¨μλ return valueλ₯Ό κ°μ§λ€. κ·Έκ²μ Void typeμ΄κ³ , empty tuple () μ΄λ€.)
    
    - [x]  var specialValueOfTypeVoid: Void = greet(person: "test") // Hello, test! μΆλ ₯?
        - ν¨μκ° ν λΉλ¨κ³Ό λμμ νΈμΆλλ€.
    
    ```swift
    func printAndCount(string: String) -> Int {
        print(string)
        return string.count
    }
    func printWithoutCounting(string: String) {
        let _ = printAndCount(string: string) // calls the first function, but ignores its return value.
    }
    printAndCount(string: "hello, world") // prints "hello, world" and returns a value of 12
    printWithoutCounting(string: "hello, world") // prints "hello, world" - The message is still printed by the first function, but the returned value isnβt used.
    ```
    
    - [ ]  let _ = printAndCount(string: string) // calls the first function, but ignores its return value. μ΄κ²λ κ·Έλ₯ μμΌλμΉ΄λ ν¨ν΄μ΄λΌκ³  λ³΄λ©΄λλ???
    
    Note: Return values can be ignored, but a function that says it will return a value must always do so. A function with a defined return type canβt allow control to fall out of the bottom of the function without returning a value, and attempting to do so will result in a compile-time error. 
    
    λ°νκ°μ λ¬΄μλ  μ μμ§λ§, λ°ν νμμ΄ μλ ν¨μλ ν­μ κ°μ λ°ννλ€.βοΈλ°ν νμμ μ μν ν¨μλ λ°νκ°μ λ°ννκΈ° μ μ ν¨μμμ λΉ μ Έλκ° μ μλ€. λ°νκ°μ λ°ννκΈ° μ μ ν¨μλ₯Ό μ’λ£νλ©΄, μ»΄νμΌ μλ¬κ° λ°μνλ€.
    
    - [ ]  μ€μ - μλ₯Ό λ€λ©΄????
    
- Functions with Multiple Return Values
    
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
        return (currentMin, currentMax) // μ΄λ―Έ return typeμ μ μν  λ tuple elementμ nameμ μ§μ νμΌλ―λ‘ return (min: currentMin, max: currentMax)μΌλ‘ λ€μ λͺμνμ§ μμλ λλ€.
    }
    // Because the tupleβs member values are named as part of the functionβs return type, they can be accessed with dot syntax to retrieve the values:
    let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
    print("min is \(bounds.min) and max is \(bounds.max)") // Prints "min is -6 and max is 109"
    ```
    
    The function returns a tuple containing two values. These values are labeled min/max so that they can be accessed by name when querying the functionβs return value.
    
    - [x]  Query?
        - νΉμ λ°μ΄ν°λ² μ΄μ€μμ μνλ μ‘°κ±΄μ λ°μ΄ν°λ₯Ό μ‘°μνκΈ° μν λ¬Έμ₯ λ©μ΄λ¦¬μ΄λ€.
        - λ°μ΄ν°λ² μ΄μ€μ λ³΄λ΄λ μμ²­(request) λλ μ§λ¬Έμ΄λ€.
        - λ°μ΄ν°λ² μ΄μ€λ νμΌμ νΉμ  λ΄μ©μ κ²μνκΈ° μν΄ μ½λ(code) λλ ν€(Key)λ₯Ό κΈ°μ΄λ‘ μ‘°ννλ κ²μ΄λ€.
        
        [http://wiki.hash.kr/index.php/μΏΌλ¦¬](http://wiki.hash.kr/index.php/%EC%BF%BC%EB%A6%AC)
        

- Optional Tuple Return Types
    
    ν¨μμ return typeμ΄ tupleμΈ κ²½μ°, tuple μ μ²΄κ° nil μΌ κ°λ₯μ±μ΄ μλ€. μ΄λ return typeμ Optional tuple type μΌλ‘ λͺμνλ€. μλ₯Ό λ€λ©΄ `(Int, Int)?`  μ΄λ€.
    
    Note: `(Int?, Int?)`μ λ€λ₯΄λ€. (Int?, Int?)μ a tuple that contains values of optional types μ΄λ€. λ°λ©΄, optional tuple type `(Int, Int)?`μ the entire tuple is optional μ΄λΌλ μλ―Έμ΄λ€.
    
    ```swift
    func minMax(array: [Int]) -> (min: Int, max: Int)? {
        if array.isEmpty { return nil }  // empty arrayκ° μ λ¬λλ©΄ nilμ λ°ννκ³ , ν¨μλ₯Ό μ’λ£νλ€.
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
    

### Function Argument Labels and Parameter Names

ν¨μ νΈμΆ μ, argument μμ μλ κ²μ΄ λ¬΄μ‘°κ±΄ argument labelμ΄λ€!
ν¨μ μ μΈ μ, argument labelμ λ°λ‘ μ μνμ§ μμΌλ©΄, defaultλ‘ parameter nameμ argument labelλ‘ μ¬μ©νλ€!

parameterκ° μ¬λ¬ κ°μΈ κ²½μ°, parameter nameκ³Ό λ¬λ¦¬ argument labelμ λμΌνκ² μ€λ³΅μ¬μ©μ΄ κ°λ₯νλ€. λ¨, κ°λμ±μ μν΄ argument labelμ μ€λ³΅νμ§ μλ κ²μ΄ μ’λ€.

- Variadic Parameters (κ°λ³ λ§€κ°λ³μ)
    
    *Variadic : (CSμ©μ΄) Taking a arbitrarily many arguments.
    
    ```swift
    func arithmeticMean(_ numbers: Double...) -> Double {
        var total: Double = 0
        for number in numbers { // μ λ¬λ argumentsλ Arrayλ‘ μ¬μ©λλ€. ([Double] type)
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
    
    //print(arrayParameter()) // μ»΄νμΌ μλ¬ λ°μ - Missing argument for parameter #1 in call <- arrayλ parameter μλ΅μ΄ λΆκ°νλ€.
    print(variadicParameter()) // [] <- κ°λ³ λ§€κ°λ³μλ parameter μλ΅μ΄ κ°λ₯νλ€.
    ```
    
    - [x]  nil μ΄ μλλΌ nan?
        - nan : Type property - A quiet NaN (βnot a numberβ). 
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
            
            - [ ]  ?????????????
- In-Out Parameters
    
    An in-out parameter has a value thatβs passedΒ *in*Β to the function, is modified by the function, and is passed backΒ *out*Β of the function to replace the original value.
    (in-out parameterλ κ°μ ν¨μμ μ λ¬ (in)νκ³ , ν¨μ λ΄λΆμμ κ°μ μμ ν μ΄νμ ν¨μ λ°μΌλ‘ λ€μ μ λ¬ (out)νμ¬ κΈ°μ‘΄μ κ°μ λμ²΄νλ€.
    
    - [ ]  behavior of in-out parameters and associated compiler optimizations, seeΒ [In-Out Parameters](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#ID545).
        - in-out parameters are passed as follows:
            1. When the function is called, the value of the argument is copied.
            2. In the body of the function, the copy is modified.
            3. When the function returns, the copyβs value is assigned to the original argument.
        - This behavior is known as copy-in copy-out or call by value result.

### Function Types

- Using Function Types
    
    ```swift
    var mathFunction: (Int, Int) -> Int = addTwoInts
    
    print("Result: \(mathFunction(2, 3))") // Prints "Result: 5" - You can call the assigned function with the name mathFunction. (λ³μ μμ²΄κ° ν¨μμ΄λ―λ‘ parameter a,b μμ΄ νΈμΆ κ°λ₯νλ€.)
    ```
    
    λ³μ mathFuntionμ ν¨μ typeμΌλ‘ μ μνλ€. κ·Έλ¦¬κ³  Set this variable to refer to the addTwoInts function. (ν¨μ addTwoIntsλ₯Ό μ°Έμ‘°νλλ‘ μ΄ λ³μλ₯Ό μ€μ νλ€.)β λΌλ λ»μ΄λ€. *ν¨μλ μ°Έμ‘° type μ΄λ―λ‘
    
- Function Types as Parameter Types
    
    2) ν¨μ typeμ λ€λ₯Έ ν¨μμ parameter typeμΌλ‘ μ¬μ© κ°λ₯νλ€. This enables you to leave some aspects of a functionβs implementation for the functionβs caller to provide when the function is called. (μ΄λ κ² νλ©΄ ν¨μ νΈμΆμκ° ν¨μλ₯Ό νΈμΆν  λ μ κ³΅ν  μ μλλ‘ ν¨μ κ΅¬νμ μΌλΆ μΈ‘λ©΄μ λ¨κ²¨λ μ μλ€.) 
    = This enables `printMathResult` to hand off some of its functionality to the caller of the function.
    

### Nested Functions

Nested functions are hidden from the outside world by default, but can still be called and used by their enclosing function. An enclosing function can also return its nested function to allow the nested function to be used in another scope. (Nested Functionμ return λμ΄ Enclosing function μΈλΆμμ μ¬μ©λ  μ μλ€!) Enclosing Function λ΄λΆμμλ§ μ¬μ©ν  μ μλ κ² μλλ€!

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(inputNumber: Int) -> Int { return inputNumber + 1 }  // ν¨μ λ΄λΆμ ν¨μλ₯Ό μ μνλ€.
    func stepBackward(inputNumber: Int) -> Int { return inputNumber - 1 }

    return backward ? stepBackward : stepForward  // Nested Functionμ return νλ€. Enclosing Function μΈλΆμμλ νΈμΆλ  μ μλ€.
}

var currentValue = -4
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0) // (μ΄κΈ°ν μμ ) arugumentκ° falseμ΄λ―λ‘ -> moveNearerToZero now refers to the nested stepForward().

while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue) // Eclosing Functionμ μΈλΆμ΄μ§λ§ stepForward ν¨μλ₯Ό νΈμΆνλ€. (μ ννλ stepForward ν¨μμ referenceλ₯Ό ν΅ν΄ -> stepForward ν¨μμ parameter inputNumberμ argument currentValueλ₯Ό μ λ¬νλ€!) 
}
print("zero!")
// -4... -3... -2... -1... zero!

print(moveNearerToZero(5))  // 6 μΆλ ₯  <- arugument 5λ stepForwardμ parameter inputNumberλ‘ μ λ¬λλ€.
print(moveNearerToZero(-5)) // -4 μΆλ ₯

// μ°Έκ³  - Enclosing Function μ€νμ μν₯μ λ―ΈμΉλ λ³μ currentValueμ κ°μ΄ 1 μ΄μμ΄ λλ©΄, stepForwardκ° μλλΌ stepBackwardλ₯Ό νΈμΆνλ€κ³  μ°©κ°ν  μ μμ§λ§ μλλ€!
// currentValue κ°μ΄ 1 μ΄μμ΄μ΄λ μ¬μ ν stepForwardλ₯Ό νΈμΆνλ€. 
currentValue = 1
print(moveNearerToZero(5))  // 6 μΆλ ₯
print(moveNearerToZero(-5)) // -4 μΆλ ₯
```

- [ ]  Enclosing Function μ€νμ μν₯μ λ―ΈμΉλ λ³μ currentValueμ κ°μ΄ 1 μ΄μμ΄ λλ©΄, stepForwardκ° μλλΌ stepBackwardλ₯Ό νΈμΆνλκ±° μλκ°?

moveNearerToZeroκ° μμμ΄μ§λ§ ν¨μ stepForwardμ μ°Έμ‘°λ₯Ό ν λΉνμΌλκΉ, λ³μ currentValueμ κ°μ λ°λΌ ν¨μ stepBackwardμ μ°Έμ‘°λ₯Ό ν λΉνκ² λ  μλ μλκ±° μλκ°...?
β μλκ΅¬λ. ν¨μ stepForwardμ μ°Έμ‘°λ₯Ό ν λΉνμΌλκΉ, ν΄λΉ μ£Όμμμ λ­κ°κ° λ°λλ©΄ κ·Έκ±Έ λ°μν  μ μλ€λ λ»μ΄κ΅¬λ. μ°Έμ‘°λκΉ λ€λ₯Έ ν¨μμ μ°Έμ‘°λ₯Ό κ°λ₯΄ν€λλ‘ λ³κ²½ κ°λ₯νκ² μλλΌ!

***κ·Έλ λ€λ©΄, μμ moveNearerToZeroλ₯Ό μ΄κΈ°νν  λ, κ·Έ "μ΄κΈ°ννλ μμ "μμ ν λΉν ν¨μμ μ°Έμ‘°λ₯Ό κ³μν΄μ μ μ₯νκ³  μλκ±΄κ°? λ³μ currentValueμ κ°μ μκ΄ μμ΄?

# 7. Closure -

# 8. Enumerations -

# 9. Structures and Classes (90%)

Structures and classes are general-purpose, flexible constructs that become the building blocks of your programβs code. You define properties and methods to add functionality to your structures and classes.

Unlike other programming languages, Swift doesnβt require you to create separate interface and implementation files for custom structures and classes. (λ€λ₯Έ μΈμ΄λ λ³λμ μΈν°νμ΄μ€ λ° μ€ννμΌμ μμ±ν΄μΌ νλ€.) In Swift, you define a structure or class in a single file, and the external interface to that class or structure is automatically made available for other code to use.

- [ ]  external interface? μλ₯Ό λ€λ©΄?

Note: An instance of a class is traditionally known as an *object.* However, Swift structures and classes are much closer in functionality than in other languages, and much of this chapter describes functionality that applies to instances of either a class or a structure type. Because of this, the more general term *instance* is used.
(ν΅μμ μΌλ‘ ν΄λμ€μ μΈμ€ν΄μ€λ₯Ό κ°μ²΄ (object)λΌκ³  νμ§λ§, Swiftμμλ λ€λ₯Έ μΈμ΄μ λΉν΄ κ΅¬μ‘°μ²΄μ ν΄λμ€μ κΈ°λ₯μ΄ μ μ¬νλ€. λ°λΌμ κ°μ²΄λ³΄λ€ ν¬κ΄μ μΌλ‘ κ΅¬μ‘°μ²΄μ μΈμ€ν΄μ€, ν΄λμ€μ μΈμ€ν΄μ€ λ±μΌλ‘ μ¬μ©λλ μ©μ΄μΈ μΈμ€ν΄μ€ (instance)λΌλ ννμ μ¬μ©νλ€.)

- [ ]  Swiftμ objectλ 'classμ μΈμ€ν΄μ€'λ₯Ό λ§νλ κ² μλμλ? structure λ° classμ instanceλ₯Ό ν΅μΉ­νλ κ±°μλ? β νλ¦¬μ½μ€ κ³Όμ  μ°Έκ³ 
    - Swiftμμ κ°μ²΄κ° λ  μ μλ μ‘΄μ¬λ 3κ°μ§μ΄λ€. struct, class, enum μ΄λ€. ?????
    (objective-cμμλ class λλ class μΈμ€ν΄μ€λ§ κ°μ²΄μ΄λ€.)
    - μΌκ³°λ μ± <μ€μννΈ νλ‘κ·Έλλ°>
        
        "κ°μ²΄λΌλ νν λμ  μΈμ€ν΄μ€λΌλ ννμ μ¬μ©νλ€. μΈμ€ν΄μ€λ κ΅¬μ‘°μ²΄μ μΈμ€ν΄μ€, μ΄κ±°νμ μΈμ€ν΄μ€λ μμ μ μκΈ° λλ¬Έμ κ°μ²΄μ μΈμ€ν΄μ€λ κ°μ ννμ΄ μλλ€. ν΄λμ€μ μΈμ€ν΄νΈλ₯Ό κ°μ²΄λΌκ³  λΆλ₯Έλ€. κ°μ²΄λ ν΄λμ€μ μΈμ€ν΄μ€λ§μ κ°λ¦¬ν€λ νμ μ μΈ μλ―Έμ΄λ€."
        
    
- [ ]  μλΈμ€ν¬λ¦½νΈ μ€λͺμ κ°μ²΄λ λ€μ νμΈνμ...

## Comparing Structures and Classes

Structures and classes have many things in common. Both can:

- Define properties to store values
- Define methods to provide functionality (λ³΄ν΅ λ€λ₯Έ μΈμ΄μμλ κ΅¬μ‘°μ²΄μ λ©μλλ₯Ό μμ±ν  μ μλ€.)
- Define subscripts to provide access to their values using subscript syntax
- Define initializers to set up their initial state
- Be extended to expand their functionality beyond a default implementation - Extension κΈ°λ₯
- Conform to protocols to provide standard functionality of a certain kind

Classes have additional capabilities that structures donβt have:

- Inheritance enables one class to inherit the characteristics of another.
- Type casting enables you to check and interpret the type of a class instance at runtime. ???
- Deinitializers enable an instance of a class to free up any resources it has assigned.
- Reference counting allows more than one reference to a class instance. ???
    - [ ]  Automatic Reference Counting - [https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)

Classμ μΆκ° κΈ°λ₯μ΄ μλ λ§νΌ νλ‘κ·Έλ¨μ λ³΅μ‘μ±μ΄ μ¦κ°νλ€. λ°λΌμ μΌλ°μ μΈ μν©μλ μΆλ‘ νκΈ° μ¬μ΄ structureλ₯Ό μ¬μ©νκ³ , κΌ­ νμν  λλ§ classλ₯Ό μ¬μ©νλ€.
μ€μ§μ μΌλ‘ μ¬μ©μμ μ typeμ λλΆλΆμ΄ structure λ° enumeration μ΄λ€.

- [x]  Choosing Between Structures and Classes - [https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes)
    - Use structures by default. - structureλ μΈμ€ν΄μ€μ λ³κ²½μ¬ν­μ λͺμμ μΌλ‘ νμΈ κ°λ₯νλ―λ‘ μΆλ‘ νκΈ° μ½λ€. λ€λ₯Έ μΈμ΄ λλΉ λ©μλ κ΅¬ν, νλ‘ν μ½ μ±ν κ°λ₯ λ± structure νμ©λκ° λλ€.
    - Use classes when you need Objective-C interoperability. - λ°μ΄ν° μ²λ¦¬λ₯Ό μν΄ Objective-C APIλ₯Ό μ¬μ©νλ€λ©΄, λλ λ°μ΄ν° λͺ¨λΈμ Objective-C frameworkμμ μ μν κΈ°μ‘΄μ ν΄λμ€ hierarchyμ λ§μΆ°μΌ νλ μν©μ΄λΌλ©΄, classλ₯Ό μ¨μΌν  μ μλ€.
    - Use classes when you need to control the identity of the data you're modeling. - μ½λ μ μ²΄μμ λ³κ²½μ¬ν­μ΄ μ¦κ° λ°μλμ΄μΌ νλ κ²½μ°μ classλ₯Ό μ¬μ©νλ€.
    - identity operator (===)λ‘ λμΌν΄μΌ νλ κ²½μ°μ μ¬μ©νλ€.
    - When you share a class instance across your app, changes you make to that instance are visible to every part of your code that holds a reference to that instance. Use classes when you need your instances to have this kind of identity. Common use cases are file handles, network connections, and shared hardware intermediaries.
    - Use structures along with protocols to adopt behavior by sharing implementations. - μμ hierarchy κ΅¬μ‘°λ protocol μμμ ν΅ν΄ structureμμλ λͺ¨λΈλ§ κ°λ₯νλ€. (classλ class λΌλ¦¬λ§ μμ κ°λ₯νμ§λ§, protocolμ ν΅ν μμμ class/enum/structure λͺ¨λμμλ κ°λ₯νλ€λ μ₯μ μ΄ μλ€.)

Note: Classes and actors (νμμ) share many of the same characteristics and behaviors. For information about actors, see Concurrency (λμμ±).

- [ ]  Concurrency - [https://docs.swift.org/swift-book/LanguageGuide/Concurrency.html](https://docs.swift.org/swift-book/LanguageGuide/Concurrency.html)  μ΄λ ΅λ€... λμ€μ

- Definition Syntax
    
    structure λ° classλ₯Ό μ μνλ κ²μ μλ‘μ΄ Swift typeμ μ μνλ κ²μ΄λ―λ‘ UpperCamelCaseμ λ°λΌ μ΄λ¦μ μ§λλ€. (typeμ΄λ¦μ ν­μ λλ¬Έμλ‘ μμ)
    
    ```swift
    struct Resolution {  // describe a pixel-based display resolution.
        var width = 0
        var height = 0
    }
    class VideoMode {  // describe a specific video mode for video display.
        var resolution = Resolution() // initialized with a new Resolution structure instance // var resolution: Resolution μ΄λ λ€λ₯Έκ°?
        var interlaced = false
        var frameRate = 0.0
        var name: String? // name property is automatically given a default value of nil, or βno name valueβ, because itβs of an optional type.
    }
    ```
    
- Structure and Class Instances
    
    The Resolution structure definition and the VideoMode class definition only describe what a Resolution or VideoMode will look like. In order to describe a specific resolution or video mode. To do that, you need to create an instance of the structure/class.
    
    ```swift
    let someResolution = Resolution()  // both use initializer syntax for new instances.
    let someVideoMode = VideoMode()
    ```
    
    The simplest form of initializer syntax uses the type name of the class/structure followed by empty parentheses, such as Resolution() or VideoMode(). This creates a new instance of the class/structure, with any properties initialized to their default values. (νλ‘νΌν° κΈ°λ³Έκ°μΌλ‘ μ΄κΈ°νλλ€.)
    
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
    
    All structures have an automatically generated *memberwise initializer.* (ν΄λμ€λ μλ - Unlike structures, class instances donβt receive a default memberwise initializer.)
    
    Initial values for the properties of the new instance can be passed to the memberwise initializer by name:
    
    ```swift
    let vga = Resolution(width: 640, height: 480)
    // Resolution structureλ λͺ¨λ  νλ‘νΌν°μ κΈ°λ³Έκ°μ΄ μλ€. -> initializerλ () κ·Έλ¦¬κ³  (width: , height: ) λͺ¨λ μ¬μ© κ°λ₯νλ€.
    ```
    

## Structures and Enumerations Are Value Types

A *value type* is a type whose value is *copied* when itβs assigned to a variable/constant, or when itβs passed to a function.

all of the basic types in Swiftβintegers, floating-point numbers, Booleans, strings, arrays and dictionariesβare value types, and are implemented as structures behind the scenes. 
(Swiftμ κΈ°λ³Έ data typeμ structureλ‘ κ΅¬νλμμΌλ―λ‘)

All structures and enumerations are value types in Swift. This means that any structure and enumeration instances you createβand any value types they have as propertiesβare always *copied* when theyβre passed around in your code.

Note: Collections defined by the standard library (like arrays, dictionaries), and strings use an optimization to reduce the performance cost of copying. Instead of making a copy immediately, these collections share the memory where the elements are stored between the original instance and any copies. If one of the copies of the collection is modified, the elements are copied just before the modification. (Collection λ° String typeμ κ²½μ°, λ³΅μ¬λ‘ μΈν μ±λ₯ μμ€μ μ΅μννκ³ μ μ΅μ ν κΈ°λ₯λ₯Ό μ¬μ©νλ€. μ²μλΆν° λ³΅μ¬λ³Έμ μν λ©λͺ¨λ¦¬λ₯Ό μλ‘ ν λΉνμ§ μκ³ , μΌλ¨ μλ³Έμ λ©λͺ¨λ¦¬λ₯Ό κ³΅μ νλ€. κ·Έλ¬λ€κ° λ³΅μ¬λ³Έμ΄ μμ λλ μν©μ΄ λ°μνλ©΄, μμ  μ§μ μ λ³΅μ¬λ³Έμ μλ‘μ΄ λ©λͺ¨λ¦¬λ₯Ό ν λΉνλ€!)

κ° typeμ μμ/λ³μμ ν λΉν  λ

```swift
let hd = Resolution(width: 1920, height: 1080) // declares a constant called hd, sets it to a Resolution instance initialized with the width/height of full HD video.
var cinema = hd // declares a variable called cinema, sets it to the current value of hd. (*Resolutionμ΄ κ° typeμ΄λ―λ‘ -> μμ hdμ νμ¬ κ°μ΄ ν λΉλλ€.)
// Because Resolution is a structure, a copy of the existing instance is made, and this new copy is assigned to cinema.
```

When cinema was given the current value of hd, the values stored in hd were copied into the new cinema instance.
Even though hd and cinema have the same width/height, theyβre two completely different instances.

![Untitled](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%203.png)

cinemaλ λ³μμ΄λ―λ‘ Resolution νλ‘νΌν°λ₯Ό λ³κ²½ κ°λ₯νλ€. cinemaλ κ°μ΄ λ³΅μ¬λλ©΄μ μμ±λ μλ‘μ΄ μΈμ€ν΄μ€λ₯Ό λ΄κ³  μμΌλ―λ‘ hdμ μΈμ€ν΄μ€μ μλ¬΄ μν₯μ λ―ΈμΉμ§ μλλ€.
(μ°Έκ³  - hdλ μμμ΄λ―λ‘ λ³κ²½ λΆκ°νλ€.)

```swift
cinema.width = 2048

print("hd is still \(hd.width) pixels wide") // Prints "hd is still 1920 pixels wide"
print("cinema is now \(cinema.width) pixels wide") // Prints "cinema is now 2048 pixels wide"
```

Enumλ λμΌνκ² μλνλ€.

```swift
enum CompassPoint {
    case north, south, east, west
    mutating func turnNorth() {
        self = .north
    }
}
var currentDirection = CompassPoint.west
let rememberedDirection = currentDirection // When rememberedDirection is assigned the value of currentDirection, itβs actually set to a copy of that value.
currentDirection.turnNorth() // currentDirectionμ caseκ° λ³κ²½λλ κ²μ λ³΅μ¬λ³Έμ μλ³ΈμΈ rememberedDirectionμ μλ¬΄λ° μν₯μ΄ μλ€.

print("The current direction is \(currentDirection)") // Prints "The current direction is north"
print("The remembered direction is \(rememberedDirection)") // Prints "The remembered direction is west"
```

## ***Classes Are Reference Types

*reference types* are *not copied* when theyβre assigned to a variable/constant, or when theyβre passed to a function. 
Rather than a copy, a reference to the same existing instance is used.

μ°Έμ‘° typeμ μμ/λ³μμ ν λΉν  λ

```swift
let tenEighty = VideoMode() // declares a constant called tenEighty, sets it to refer to a VideoMode instance. - ν΄λμ€ μΈμ€ν΄μ€μ μ°Έμ‘°κ° ν λΉλμλ€. (ν΄λμ€λ μ°Έμ‘° typeμ΄λ―λ‘)
tenEighty.resolution = hd  // Now itβs set to be interlaced (μ μ²΄ νλ‘νΌν°κ° λ³κ²½λ¨)
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0

// tenEighty is assigned to a new constant, called alsoTenEighty, and the frame rate of alsoTenEighty is modified:
let alsoTenEighty = tenEighty // *tenEightyμ λ€μ΄μλ ν΄λμ€ μΈμ€ν΄μ€μ μ°Έμ‘°κ° alsoTenEightyμ ν λΉλμλ€. λμΌν μ°Έμ‘°λ₯Ό κ³΅μ νκ³  μλ€. 
alsoTenEighty.frameRate = 30.0

// μ°Έμ‘°λ₯Ό κ³΅μ νλ―λ‘ ν μͺ½μμ λ³κ²½ν λ΄μ©μ λ€λ₯Έ μͺ½μμ νμΈν΄λ λ³κ²½λμ΄ μλ€.
print("\(alsoTenEighty.frameRate)") // 30.0
print("The frameRate property of tenEighty is now \(tenEighty.frameRate)") // Prints "The frameRate property of tenEighty is now 30.0"
```

```swift
// μ°Έκ³  - λΉκ΅μ©
let hd = Resolution() // declares a constant called hd, sets it to a Resolution instance. - κ΅¬μ‘°μ²΄ μΈμ€ν΄μ€μ κ°μ΄ ν λΉλμλ€. (κ΅¬μ‘°μ²΄λ κ° typeμ΄λ―λ‘)
```

Because classes are reference types, tenEighty and alsoTenEighty actually both refer to the same VideoMode instance. Effectively, theyβre just two different names for the same single instance.

![Untitled](Swift%20Language%20Guide%20&%20Reference%2032caab2bf40d4b56a0697807c398d9ae/Untitled%204.png)

This example also shows how reference types can be harder to reason about. Wherever you use tenEighty, you also have to think about the code that uses alsoTenEighty, and vice versa. 
(μ°Έμ‘° typeμ μΆλ‘ νκΈ° μ΄λ ΅λ€. Class instance tenEightyλ₯Ό μΈ λλ§λ€, (μ°Έμ‘°λ₯Ό κ³΅μ νκ³  μλ) alsoTenEightyλ₯Ό μ¬μ©νλ μ½λμ λν΄μλ μκ°ν΄μΌ νκ³ , κ·Έ λ°λλ λμΌνλ€.)

ν΄λμ€μ let μ μΈ μΈμ€ν΄μ€μ νλ‘νΌν° κ°μ λ³κ²½ κ°λ₯ν μ΄μ λ?

(μ°Έκ³  - Class μΈμ€ν΄μ€μ΄λ―λ‘ letμΌλ‘ μ μΈν΄λ 'κ°λ³ νλ‘νΌν°' κ°μ λ³κ²½ κ°λ₯νλ€. λ¨, μ°Έμ‘° μ λ³΄λ λ³κ²½ λΆκ°νλ€.)
Note that tenEighty and alsoTenEighty are declared as constants, rather than variables. However, you can still change `tenEighty.frameRate` and `alsoTenEighty.frameRate` because the values of the tenEighty and alsoTenEighty constants themselves donβt actually change. tenEighty and alsoTenEighty themselves donβt βstoreβ the VideoMode instanceβinstead, they both refer to a VideoMode instance behind the scenes. Itβs the frameRate property of the underlying VideoMode thatβs changed, not the values of the constant references to that VideoMode.
(μμ tenEighty/alsoTenEightyλ μΈμ€ν΄μ€μ νλ‘νΌν° κ°μ μ μ₯νκ³  μμ§ μλ€. μ°Έμ‘° (μ£Όμ)λ₯Ό κ°μ§κ³  μλ€.
λ°λΌμ `tenEighty.frameRate`λ₯Ό ν΄λ μ°Έμ‘° μ λ³΄λ λ³νμ§ μλλ€. λ³νλ κ²μ νλ‘νΌν° κ°μ΄λ€.)

- Identity Operators
    
    Because classes are reference types, itβs possible for multiple constants/variables to refer to the same single instance of a class. It can be useful to find out whether two constants/variables refer to exactly the same instance of a class.
    
    identity operators: Identical to `===`, Not identical to `!==`
    
    - [ ]  μ !===κ° μλμ§?
    
    ```swift
    if tenEighty === alsoTenEighty {
        print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
    } // Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
    ```
    
    === : *Identical* to means that two instances (two constants/variables) of class type refer to exactly the same class instance. 
    ==   : *Equal* to means that two instances are considered equal in value.
    
- Pointers
    
    If you have experience with C, C++, or Objective-C, you may know that these languages use pointers to refer to addresses in memory. 
    A Swift constant/variable that refers to an instance of some reference type is similar to a pointer in C, but isnβt a direct pointer to an address in memory, and doesnβt require you to write an asterisk (*) to indicate that you are creating a reference. Instead, these references are defined like any other constant/variable in Swift. 
    
    The standard library provides pointer and buffer types that you can use if you need to interact with pointers directlyβsee Manual Memory Management.
    
    - [ ]  Swiftμμ μ°Έμ‘° typeμ ν΄λμ€ λ°μ μλκ±° λ§λ? ν¨μ, ν΄λ‘μ λ μ°Έμ‘° type μ΄κ΅¬λ...
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

Swift uses *Automatic Reference Counting (ARC)* to track and manage your appβs memory usage. ARC automatically frees up the memory used by class instances when those instances are no longer needed.

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

# π¦ Language Reference

# 1. Lexical Structure

# 2. Types -

- Type κ΅¬λΆ - Named Type / Compound Type
    
    Swiftλ μ μ²΄ Typeμ λ κ°μ§λ‘ κ΅¬λΆνλ€. λν Type μλ€λ‘ ()λ₯Ό λΆμΌ μ μκ³ , μλ¬΄λ° ν¨κ³Όκ° μλ€.
    
    - Named Type (λͺλͺλ νμ) : μ μν  λ νΉμ ν μ΄λ¦μ λΆμ¬μ μ μΈνλ νμμ΄λ€. Swift standard libraryμμ μ μν, Structure κΈ°λ°μ κΈ°λ³Έ Data Type (Int, String λ±), μ¬μ©μ-μ μ-Type (Class, Structure, Enumeration, Protocol) κ·Έλ¦¬κ³  Swift standard libraryμμ μ μν Arrays, Dictionaries, and Optional λ±μ΄ μνλ€. 
    μλ₯Ό λ€μ΄ μ¬μ©μ μ μ Class typeμΈ MyClassμ μΈμ€ν΄μ€κ° μλ€λ©΄, ν΄λΉ μΈμ€ν΄μ€μ νμ μ΄λ¦μ MyClass νμμ΄λ€. 
    Extensionμ μ¬μ©νμ¬ κΈ°λ₯μ νμ₯ κ°λ₯νλ€.
    - Compound Type (λ³΅ν© νμ) : λ³λμ λͺλͺ μμ΄ Swift μΈμ΄λ‘ μ μλλ νμμ΄λ€. Function typeκ³Ό Tuple typeμ΄ μνλ€.
    Named Type λλ λ€λ₯Έ Compound Typeμ ν¬ν¨ν  μ μλ€. 
    μλ₯Ό λ€μ΄ Tuple typeμ (Int, (Int, Int))μ΄ μλ€. (μ²«λ²μ§Έ elementλ Int type, λλ²μ§Έ elementλ λ€λ₯Έ Compound TypeμΈ (Int, Int)μ΄λ€.)
- Type Annotation / Type Identifier / Type Inference
    - Type Annotation
        
        A `type annotation` explicitly specifies the type of a variable or expression.
        Write a type annotation by placing a colon(:) after the constant/variable name, followed by the type name.
        
        Type annotations can contain an optional list of type attributes before the type. `: attributes opt inoutopt type`
        
        - [x]  Type Attribute?
            - An attribute provides additional information about the 1) declaration or 2) type. (2κ°μ§ Attributeκ° μμ)
                
                1) Declaration - Specify an attribute by the attributeβs name (and any arguments that the attribute accepts). `@attribute name(attribute arguments)`
                
                2) Type Attribute - μλ₯Ό λ€μ΄ `@autoclosure`
                
    - Type Identifier
        
        A `type identifier` refers to either a named type or a type alias of a named or compound type.
        
        ```swift
        // ex-1. named typeμ referνλ κ²½μ°
        var exam1a: Int = 0
        var exam1b: Dictionary<String, Int> = ["key1":100]
        
        // ex-2. type aliasλ₯Ό referνλ κ²½μ° 
        typealias Point = (Int, Int)  // tuple typeμ type alias
        let origin: Point = (0, 0)
        
        // ex-3. named types declared in other modules or nested within other typesλ₯Ό referνλ κ²½μ° 
        var someValue: ExampleModule.MyType  // the named type MyType thatβs declared in the ExampleModule moduleμ referν¨
        ```
        
    - Type Inference
        - L/G
            
            `Type inference` enables a compiler to deduce the type of a particular expression automatically when it compiles your code, simply by examining the values you provide.
            Itβs rare that you need to write type annotations in practice. If you provide an initial value (*literal value or literal) for a constant or variable at the point that itβs defined, Swift can almost always infer the type to be used for that constant or variable, as described in Type Safety and Type Inference. 
            (λ³μ μ μΈ μ μ΄κΈ°κ°μ ν λΉνλ©΄, type annotationμ νμ§ μμλ Swiftκ° μμμ typeμ μΆλ‘ ν΄μ€λ€.) - λ¨, typeμ λͺμν΄μΌ λ²κ·Έ λμμ΄ μ½λ€.
            
            ```swift
            let meaningOfLife = 42  // meaningOfLife is inferred to be of type Int (μλνλ©΄ μμμ μ΄ μμΌλκΉ!)
            
            let pi = 3.14159  // pi is inferred to be of type Double
            If you donβt specify a type for a floating-point literal, Swift infers that you want to create a Double:
            Swift always chooses Double (rather than Float) when inferring the type of floating-point numbers.
            
            typealias MyNewInt = Int  // Type Alias (νμ λ³μΉ­)μ νμ© κ°λ₯ν¨ (Intμ λ³μΉ­μ μ§μ ν κ²κ³Ό κ°μ)
            var age: MyNewInt = 20    // Intλ‘ Type μ μ§μ ν κ²κ³Ό λμΌν¨
            var year: Int = 100 // κΈ°μ‘΄ Intλ μ¬μ© κ°λ₯
            age = year
            ```
            
        - L/R
            
            Swift uses `type inference` extensively, allowing you to omit the type or part of the type of many variables and expressions in your code.
            you can omit part of a type when the full type can be inferred from context.
            
            νμ μΆλ‘ μ μμμ λ£¨νΈ λ°©ν₯μΌλ‘ μ΄λ£¨μ΄μ§ μλ μκ³ , λ£¨νΈμμ μ λ°©ν₯μΌλ‘ μ΄λ£¨μ΄μ§ μλ μλ€.
            
            - μβλ£¨νΈ
                
                `var x: Int = 0` // the type of x is inferred by first checking the type of 0 and then passing this type information up to the root (the variable x).
                
            - λ£¨νΈβμ
                
                the explicit type annotation (: Float) on the constant eFloat causes the numeric literal 2.71828 to have an inferred type of Float instead of Double. ???
                
                - [ ]  Double.???
                
                ```swift
                let e = 2.71828 // The type of e is inferred to be Double.???
                let eFloat: Float = 2.71828 // The type of eFloat is Float. 
                ```
                
            
            Type inference operates at the level of a single expression or statement. (λ¨μΌ ννμ λλ λ¨μΌ κ΅¬λ¬Έ μμ€μμ μλνλ€.)
            This means that all of the information needed to infer an omitted type or part of a type in an expression must be accessible from type-checking the expression or one of its subexpressions. (νμ μ μΆμ νμν μ λ³΄λ ννμ, λλ νμ ννμ μ€ νλλ₯Ό type-checkingνμ¬ μ κ·Ό κ°λ₯ν΄μΌ νλ€.)
            
- Tuple Type
    - A tuple type is a comma-separated list of types, enclosed in parentheses.
    All tuple types contain two or more types, except for `Void` which is a type alias for the empty tuple type, (). β Voidλ Tuple typeμ΄λ€!
    - When an element of a tuple type has a name, that name is part of the type.
        
        ```swift
        var someTuple = (top: 10, bottom: 12)  // someTuple is of Tuple type (top: Int, bottom: Int)
        someTuple = (top: 4, bottom: 42) // OK: names match
        someTuple = (9, 99)              // OK: names are inferred
        someTuple = (left: 5, right: 5)  // Error: names don't match
        ```
        
- Function Type (λ°μΌλ‘ λ μΆκ° νμ)
    - A function type represents the type of a function/method/closure and consists of a parameter and return type separated by an arrow (->) `(parameter type) -> return type`
    - A parameter of the function type `() -> T` (where T is any type) can apply the autoclosure attribute to implicitly create a closure at its call sites.
        - [ ]  autoclosure attribute?
- Metatype Type (λ°μΌλ‘ λ μΆκ° νμ)
    
    AΒ *metatype type*Β refers to the type of any type, including class types, structure types, enumeration types, and protocol types.
    
    ... μ§κΈμ μ΄ν΄ μκ°
    
- Self Type (ν΄μ νμ)
    
    cf. self νλ‘νΌν°μ λ€λ¦
    
    - TheΒ `Self`Β type isnβt a specific type, but rather lets you conveniently refer to the current type without repeating or knowing that typeβs name.
        1. In a protocol declaration or a protocol member declaration, theΒ `Self`Β type refers to the eventual type that conforms to the protocol. (νλ‘ν μ½λ νμμ΄λ€.)
        2. In a structure/class/enumeration declaration, theΒ `Self`Β type refers to the type introduced by the declaration. 
    - Inside the declaration for a member of a type, theΒ `Self`Β type refers to that type. (member = ν΄λΉ νμμ νλ‘νΌν° λ° λ©μλ)
    In the members of a class declaration,Β `Self`Β can appear only as follows:
    - As the return type of a method 
    - As the return type of a read-only subscript
    - As the type of a read-only computed property
    - In the body of a method
        - [ ]  type(of:) ?
        
        ```swift
        class Superclass {
            func f() -> Self { return self } // f λ©μλμ return typeμ Selfμ΄λ€. self (Class μΈμ€ν΄μ€ μκΈ° μμ )λ₯Ό return νλ€.
        }
        
        let x = Superclass()  // μΈμ€ν΄μ€ μμ±
        print(type(of: x.f())) // Prints "Superclass"
        
        class Subclass: Superclass { }  // Superclassμ μμν΄λμ€ Subclass μ μ
        
        let y = Subclass()  // μΈμ€ν΄μ€ μμ±
        print(type(of: y.f())) // Prints "Subclass"
        
        let z: Superclass = Subclass()  // μΈμ€ν΄μ€ μμ±
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

ν¨ν΄μ κ°μ κ΅¬μ‘°μ΄λ€.

ν¨ν΄μ λ¨μΌ κ° (single value) λλ λ³΅ν© κ° (composite value)μ κ΅¬μ‘°λ₯Ό λνλΈλ€. μλ₯Ό λ€μ΄ tupleΒ `(1,Β 2)` μ κ΅¬μ‘°λ a comma-separated list of two elements μ΄λ€. 
ν¨ν΄μ 'νΉμ  κ°'μ΄ μλλΌ 'κ°μ κ΅¬μ‘°'λ₯Ό λνλ΄λ―λ‘ λ€μν κ°λ€κ³Ό match ν  μ μλ€. ν¨ν΄ `(x,Β y)` μ tupleΒ `(1,Β 2)` λλ 2κ°μ elementλ‘ κ΅¬μ±λ tupleκ³Ό match λλ€. 
ν¨ν΄μ κ°κ³Ό λ§€μΉνλ κ² μΈμλ, λ³΅ν© κ°μ μΌλΆ λλ μ μ²΄λ₯Ό μΆμΆνμ¬ κ° λΆλΆμ μμ/λ³μμ΄λ¦μ bind ν  μ μλ€.

*Patter matching : The process of checking whether a specific sequence of characters/tokens/data exists among the given data.
Pattern matching is used to determine whether source files of high-level languages are syntactically correct. It is also used to find and replace a matching pattern in a code with another code.

Swiftμλ 2κ°μ§ κΈ°λ³Έ ν¨ν΄μ΄ μλ€. 

1. λͺ¨λ  μ’λ₯μ κ°κ³Ό match λλ ν¨ν΄ 
    
    - λ¨μ λ³μ/μμ, optional bindingμμ κ°μ λΆν΄ (destructuring values) νλλ° μ¬μ©νλ€. 
    - wildcard patterns, identifier patterns κ·Έλ¦¬κ³  μ΄ ν¨ν΄μ ν¬ν¨νλ λͺ¨λ  value-binding patterns λλ tuple patternsμ΄ μ΄μ μνλ€. 
    - μ΄λ¬ν ν¨ν΄μ type annotationμ νλ©΄, νΉμ  typeμ κ°λ§ match λλλ‘ μ νν  μ μλ€.
    
2. λ°νμμμ νΉμ  κ°μ match λμ§ μμ μλ μλ (may fail to match) ν¨ν΄
    
    - full pattern matchingμ μ¬μ©νλ©°, match νλ €λ κ°μ΄ λ°νμ μ μμ μ μλ€. ???? 
    - enumeration case patterns, optional patterns, expression patterns, and type-casting patternsμ΄ μ΄μ μνλ€. 
    - a case label of aΒ `switch`Β statement (switchλ¬Έμ case), aΒ `catch`Β clause of aΒ `do`Β statement (doλ¬Έμ catchμ ), or in the case condition of anΒ `if`,Β `while`,Β `guard`,Β `for`-`in`Β statement (if/while/guard/for-inλ¬Έμ μ‘°κ±΄λΆ)μ μ΄ ν¨ν΄μ μ¬μ©νλ€.
    

- Wildcard Pattern
    
    A wildcard pattern matches and ignores any value. (λͺ¨λ  κ°μ match νκ³ , λ¬΄μνλ€.) 
    match ν  κ°μ μ κ²½μ°μ§ μμλ λ  λ μ¬μ©νλ€.
    
    ```swift
    for _ in 1...3 {
        // Do something three times. - ignoring 'the current value of the range (1...3)' on each iteration of the loop
    }
    ```
    
- Identifier Pattern
    
    An identifier pattern matches any value and binds the matched value to a variable/constant name. (λͺ¨λ  κ°μ match νκ³ , matchλ κ°μ μμ/λ³μμ΄λ¦μ bind νλ€.)
    
    `let someValue = 42` μμ μ μΈμμ someValueλ 'Int typeμ κ° 42'μ match λλ identifier patternμ΄λ€. matchμ μ±κ³΅ν  κ²½μ°, κ° 42λ μμμ΄λ¦ someValueμ ν λΉ (assigned, bound) λλ€.
    
    λ³μ/μμ μ μΈμ μΌμͺ½ ν¨ν΄μ΄ identifier patternμΈ κ²½μ°, identifier patternμ μμμ μΌλ‘ value-binding patternμ subpattern μ΄λ€.
    
- Value-Binding Pattern
    
    A value-binding pattern binds matched values to variable/constant names. (matchλ κ°μ μμ/λ³μμ΄λ¦μ bind νλ€.) 
    match λ κ°μ μμ/λ³μμ΄λ¦μ bindνλ Value-binding patternμ `let/var` ν€μλλ‘ μμνλ€.
    
    value-binding pattern λ΄λΆμ identifiers patternμ μλ‘μ΄ μμ/λ³μμ΄λ¦μ matchνλ κ°μ bind νλ€. 
    μλ₯Ό λ€μ΄ tupleμ elementλ₯Ό λΆν΄νκ³ , κ° elementμ κ°μ ν΄λΉ identifier patternμ bind νλ€. (You can decompose the elements of a tuple and bind the value of each element to a corresponding identifier pattern.)
    
    ```swift
    let point = (3, 2)
    
    switch point {
    case let (x, y): // Bind x and y to the elements of point. <- value-binding pattern letμ΄ λ΄λΆμ identifiers pattern x, yμ λΆν΄λ elementμ κ° 3, 2λ₯Ό κ°κ° μ λ¬/λΆλ°°νλ€.
        print("The point is at (\(x), \(y)).")
    } // Prints "The point is at (3, 2)."
    ```
    
    In the example above,`let` distributes to each identifier pattern in the tuple pattern (x, y). (`let`μ κ°κ°μ identifier pattern x, yμ λΆν΄λ elementμ κ° 3, 2λ₯Ό μ λ¬/λΆλ°°νλ€.) λ°λΌμ switchλ¬Έμ `caselet(x, y):` λ° `case (letx,lety):` μ λμΌνκ² λμνλ€.
    
- Tuple Pattern
    
    A tuple pattern is a comma-separated list of zero or more patterns (0κ° μ΄μμ ν¨ν΄μ λͺ©λ‘), enclosed in parentheses. 
    Tuple patterns match values of corresponding tuple types. (ν΄λΉ tuple typeμ κ°κ³Ό match νλ€.)
    
    Type annotionμ ν΅ν΄ νΉμ ν tuple typeμ match νλλ‘ tuple patternμ μ νν  μ μλ€. μλ₯Ό λ€μ΄, μμ μ μΈ `let (x, y): (Int, Int) = (1, 2)`μμ tuple pattern `(x, y): (Int, Int)`μ 2κ° elementκ° Int typeμΈ tuple typeμλ§ match νλ€.
    
    for-inλ¬Έ λλ μμ/λ³μ μ μΈμμ tuple patternμ΄ μ¬μ©λ  κ²½μ°, ν΄λΉ tuple patternμ wildcard patterns, identifier patterns, optional patterns λλ μ΄λ₯Ό ν¬ν¨νλ λ€λ₯Έ tuple patternλ§ κ°μ§ μ μλ€. μλ₯Ό λ€μ΄ μλ μ½λμ λ³΄λ©΄, tuple pattern (x, 0)μ element 0μ΄ expression pattern μ΄λ―λ‘ μ ν¨νμ§ μλ€. 
    
    ```swift
    let points = [(0, 0), (1, 0), (1, 1), (2, 0), (2, 1)]
    
    for (x, 0) in points { // This code isn't valid.
        /* ... */
    }
    ```
    
    tuple patternμ ()μ single elementκ° λ€μ΄μλ κ²½μ°, μλ¬΄λ° ν¨κ³Όκ° μλ€. tuple patternμ ν΄λΉ single element typeμ κ°μ match νλ€.
    
    ```swift
    let a = 2        // a: Int = 2  <- 3κ° ννμ λμΌνλ€.
    let (a) = 2      // a: Int = 2
    let (a): Int = 2 // a: Int = 2
    ```
    
- Enumeration Case Pattern
    
    An enumeration case pattern matches a case of an existing enumeration type. (κΈ°μ‘΄μ Enum typeμ caseμ match νλ€.) 
    switch λ¬Έμ case label, if/while/guard/for-in λ¬Έμ case conditionμμ μ¬μ©νλ€.
    
    match νλ €λ enumeration caseμ μ°κ΄κ°μ΄ μλ κ²½μ°, ν΄λΉ enumeration case patternμ κ°λ³ μ°κ΄κ°μ λν elementλ₯Ό κ°μ§λ tuple pattern λͺμν΄μΌ νλ€.
    
    ```swift
    enum Barcode {
        case upc(Int, Int, Int, Int)
        case qrCode(String)
    }
    ```
    
    λν enumeration case patternμ optionalμ warppedλ caseμ κ°μ match ν  μ μλ€. μ΄λ¬ν μΆμ½ λ¬Έλ²μΌλ‘ optional patternμ μλ΅ν  μ μλ€. 
    optionalμ enumμΌλ‘ κ΅¬νλλ€. λ°λΌμ `.none` λ° `.some`μ λμΌν swtich λ¬Έ λ΄λΆμ 'enum typeμ case'λ‘ λνλ  μ μλ€. 
    
    - [x]  What's the difference between optional none (.none for short) and nil?
        - There is no difference, as `.none` (basically it's `Optional.none` ) and `nil` are equivalent to each other. The use of nil is more common and is the recommended convention.
        - `.none` is an enum representation of the `nil` (absence of value) implemented by the Optional<T> enum.
    
    ```swift
    enum SomeEnum { case left, right }
    
    let x: SomeEnum? = .left  // optionalμ warppedλ case
    
    switch x {
    case .left:
        print("Turn left")
    case .right:
        print("Turn right")
    case nil:  // case .none: μ λμΌν¨
        print("Keep going straight")
    } // Prints "Turn left"
    ```
    
- Optional Pattern
    
    An optional pattern matches values wrapped in a `some(Wrapped)` case of an `Optional<Wrapped>` enumeration. (optional enum typeμ .some caseμ wrappedλ κ°μ match νλ€.) 
    
    identifier pattern λ€μ `?`λ₯Ό λΆμΈ ννμ΄λ©°, enumeration case patternκ³Ό λμΌν μμΉμμ μ¬μ©λλ€. (switch λ¬Έμ case label, if/while/guard/for-in λ¬Έμ case condition)
    
    *optional patternμ optional enumeration case patternμ λν syntactic sugarμ΄λ€. μλ μ½λλ λμΌνλ€.
    
    - [ ]  if case?
    - [ ]  // μ΄κ±΄ λ¬΄μ¨ ν¨ν΄μ΄μ§?
    if let x = someOptional {
        print(x)
    }
    
    ```swift
    let someOptional: Int? = 42
    
    // Match using an enumeration case pattern.
    if case .some(let x) = someOptional {  // optionalμ enumμΌλ‘ κ΅¬νλλ―λ‘ -> enumμ wrapped value (.some)μ λν΄ case ν€μλλ₯Ό μ¬μ© κ°λ₯νλ€.
        print(x)
    } // 42
    
    // Match using an optional pattern.
    if case let x? = someOptional {  
        print(x)
    } // 42
    
    // μ΄κ±΄ λ¬΄μ¨ ν¨ν΄μ΄μ§?
    if let x = someOptional {
        print(x)
    } // 42
    ```
    
    optional patternμ for-inλ¬Έμμ optional valueκ° λ€μ΄μλ arrayλ₯Ό interate ν  λ μ¬μ©νλ©΄ νΈλ¦¬νλ€. non-nil elementμ λν΄μλ§ loopμ λ΄μ©μ μ€ννκΈ° λλ¬Έμ΄λ€.
    
    - [ ]  for case in?
    
    ```swift
    let arrayOfOptionalInts: [Int?] = [nil, 2, 3, nil, 5]
    
    for case let number? in arrayOfOptionalInts {  // Match only non-nil values.
        print("Found a \(number)")
    } // Found a 2, Found a 3, Found a 5
    ```
    
- Type-Casting Patterns
    
    type-casting patternsμ `is` pattern λ° `as` pattern 2κ°μ§κ° μλ€. 
    is patternμ switchλ¬Έμ case labelμλ§ μ¬μ©νλ€. is λ° as patternμ ννλ μλμ κ°λ€.
    
    ```swift
    is type
    pattern as type
    ```
    
    The is pattern matches a value if the type of that value at runtime is the same as the type specified in the right-hand side of the is patternβor a subclass of that type. ??? (is patternμ λ°νμμμ ν΄λΉ κ°μ typeμ΄ 'is patternμ μ€λ₯ΈνΈμ λͺμλ type'κ³Ό λμΌν κ²½μ°, κ°μ match νλ€. (λλ ν΄λΉ typeμ subclassκ³Ό λμΌν κ²½μ° ???))
    
    is pattern behaves like the is operator in that they both perform a type cast but discard the returned type. (is ν¨ν΄μ type castλ₯Ό μννμ§λ§, return typeμ λ²λ¦°λ€λ μ μμ is operatorμ²λΌ λμνλ€.)
    
    as patternμ λ°νμμμ ν΄λΉ κ°μ typeμ΄ 'as patternμ μ€λ₯ΈνΈμ λͺμλ type'κ³Ό λμΌν κ²½μ°, κ°μ match νλ€. (λλ ν΄λΉ typeμ subclassκ³Ό λμΌν κ²½μ° ???)
    
    matchκ° μ±κ³΅νλ©΄, matchλ κ°μ typeμ pattern (as patternμ μ€λ₯ΈνΈμ λͺμλ pattern)μ cast λλ€.
    
    - an example that uses a switch statement to match values with is and as patterns.
        
        ```swift
        // Type Casting for Any and AnyObject
        class Movie {
            var name: String
            var director: String
            init(name: String, director: String) { // μμν΄λμ€ λ³ΈμΈμ initμ μ μνμΌλ―λ‘ λΆλͺ¨ initμ΄ μλ μμλμ§ μμλ€.
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
        things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman")) // Movie Classμ μΈμ€ν΄μ€
        things.append({ (name: String) -> String in "Hello, \(name)" }) // ν΄λ‘μ  - νΈμΆνμ§ μμ μνμ ν΄λ‘μ ???
        
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
                print(stringConverter("Michael")) // Hello, Michael - μ¬κΈ°μ ν΄λ‘μ λ₯Ό νΈμΆ
            default:
                print("something else")
            }
        }
        ```
        
    
- Expression Pattern
    
    An expression pattern represents the value of an expression. (μμμ κ°μ λνλΈλ€.) switchλ¬Έμ case labelμμλ§ μ¬μ©νλ€.
    *expression (μμ, evaluateλ₯Ό ν΅ν΄ κ°μΌλ‘ νμλ¨)
    *statement (κ΅¬λ¬Έ, μ€νκ°λ₯ν λλ¦½μ μΈ μ½λ μ‘°κ°, statementμ expressionμ ν¬ν¨ν  μ μμ)
    
    expression patternμ ν΅ν΄ λνλΈ expressionμ Swift νμ€ λΌμ΄λΈλ¬λ¦¬μ `~=` operatorλ₯Ό μ¬μ©νμ¬ input expressionμ κ°κ³Ό λΉκ΅λλ€. ??? 
    ~= operatorκ° trueλ₯Ό λ°ννλ©΄, matchμ μ±κ³΅νλ€. Defaultλ‘ ~= operatorλ == operatorλ₯Ό μ¬μ©νμ¬ λμΌν typeμ 2κ° κ°μ λΉκ΅νλ€. 
    
    λν, μλ μμμ κ°μ΄ νΉμ  κ°μ΄ range λ΄μ ν¬ν¨λμλμ§ νμΈνμ¬, ν΄λΉ κ°μ rangeμ match ν  μ μλ€.
    
    ```swift
    let point = (1, 2)
    
    switch point { // μ¬κΈ°μ expressionμ point μ΄λ€ ??? λ§λ?
    case (0, 0):
        print("(0, 0) is at the origin.") 
    case (-2...2, -2...2):
        print("(\(point.0), \(point.1)) is near the origin.")  // Prints "(1, 2) is near the origin."
    default:
        print("The point is at (\(point.0), \(point.1)).")
    } 
    ```
    
    You can overload the ~= operator to provide custom expression matching behavior. ??? For example, you can rewrite the above example to compare the point expression with a string representations of points. pointκ° μλλΌ points???
    
    - [ ]  ??? intλ stringμ λΉκ΅νκ±΄κ° μ§κΈ???
    
    ```swift
    // Overload the ~= operator to match a string with an integer.
    func ~= (pattern: String, value: Int) -> Bool { // ~= operatorκ° trueλ₯Ό λ°ννλ©΄, matchμ μ±κ³΅ν¨...........???
        return pattern == "\(value)"
    }
    
    switch point {
    case ("0", "0"):  // (0, 0) μ΄ μλλΌ ("0", "0")
        print("(0, 0) is at the origin.")
    default:
        print("The point is at (\(point.0), \(point.1)).")  // Prints "The point is at (1, 2)."
    }
    ```
    

# 8. Generic Parameters and Arguments

# 9. Summary Of the Grammar

---

# π¦ API Design Guidelines

μΆμ²: [https://swift.org/documentation/api-design-guidelines](https://swift.org/documentation/api-design-guidelines/)

*API : Application Programming Interface

## Fundamentals

1. μ¬μ© μμ μ κΈ°μ€μΌλ‘ λͺννκ² μμ±νλ κ²μ΄ κ°μ₯ μ€μν λͺ©νμ΄λ€. (Clarity at the point of useΒ is your most important goal.) λ©μλ λ° νλ‘νΌν°μ κ°μ Entitiesλ ν λ² μ μΈνλ©΄ λ°λ³΅μ μΌλ‘ μ¬μ©νλ€. μ΄λ¬ν Entitiesλ₯Ό λͺννκ³  κ°κ²°νκ² μ¬μ©νλλ‘ APIλ₯Ό μ€κ³ν΄μΌ νλ€. λμμΈμ κ²ν ν  λ μ μΈλΆλ₯Ό μ½λ κ²λ§μΌλ‘λ λΆμ‘±νλ€. ν­μ μ¬μ© μμ μ μν©μ κ²ν νμ¬ λ¬Έλ§₯μ λͺννμ§ νμΈν΄μΌ νλ€.
Entities such as methods and properties are declared only once butΒ usedΒ repeatedly. Design APIs to make those uses clear and concise. When evaluating a design, reading a declaration is seldom sufficient; always examine a use case to make sure it looks clear in context. 
    
    *Entity : μ μ₯λκ³ , κ΄λ¦¬λμ΄μΌ νλ λ°μ΄ν°μ μ§ν©μ΄λ€.
    An entity is represented by an instance of the NSEntityDescription class. This class provides access to a wide range of properties, such as its name, the data model it is defined in, and the name of the class the entity is represented by. ???
    

1. λͺνμ±μ΄ κ°κ²°μ±λ³΄λ€ μ€μνλ€. (Clarity is more important than brevity.) λ¨μν κΈμμλ₯Ό μ€μ¬μ κ°μ₯ μ§§μ μ½λλ₯Ό λ§λλ κ²μ΄ λͺ©νκ° μλλ€. Swift μ½λμ κ°κ²°μ±μ κ°λ ₯ν type μμ€νκ³Ό μμ°μ€λ½κ² λ°λ³΅μ μΌλ‘ μ¬μ¬μ©νλ μ½λλ₯Ό μ€μ΄λ κΈ°λ₯μΌλ‘ μΈν΄ λνλ λΆμμ μΈ ν¨κ³ΌμΌ λΏμ΄λ€. 
Although Swift code can be compact, it is aΒ *non-goal*Β to enable the smallest possible code with the fewest characters. Brevity in Swift code, where it occurs, is a side-effect of the strong type system and features that naturally reduce boilerplate. 
    
    *boilerplate : νμ€ νμ. boilerplateλ λ°λ³΅μ μΌλ‘ μ¬μ¬μ©νλ μ½λλ₯Ό μλ―Ένλ€.
    

1. λͺ¨λ  μ μΈμ λν΄ λ¬Έμν μ£Όμμ μμ±νλ€. λ¬Έμ μμ±μ ν΅ν΄ μ»λ ν΅μ°°λ ₯μ λμμΈμ ν° μν₯μ λ―ΈμΉ  μ μλ€. 
Write a documentation commentΒ for every declaration. Insights gained by writing documentation can have a profound impact on your design, so donβt put it off. 
    
    Note: APIμ κΈ°λ₯μ λν΄ κ°λ¨ν μ€λͺν  μ μμΌλ©΄, APIλ₯Ό μλͺ» μ€κ³νμ κ°λ₯μ±μ΄ μλ€.
    
    - Xcode μλμμ±μμ νμΈ κ°λ₯νλλ‘ Swift Markdownμ μ¬μ©νλ€.
        - [ ]  [https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497-CH2-SW1](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497-CH2-SW1)
    - λ¨Όμ , μμ½μ ν΅ν΄ μ μΈν Entityμ λν΄ μ€λͺνλ€. API μμ²΄λ μ μΈ λΆλΆκ³Ό μμ½λ§μΌλ‘λ μλ²½ν μ΄ν΄ν  μ μλ€.
        
        ```swift
        /// Returns a "view" of `self` containing the same elements in reverse order.
        func reversed() -> ReverseCollection
        ```
        
        - κ°λ₯νλ©΄ 1κ°μ λ¬Έκ΅¬ (a single sentence fragment)λ‘ μμ±νκ³ , λ§μΉ¨νλ‘ λλΈλ€. μμ ν λ¬Έμ₯ (complete sentence)μ μ¬μ©νμ§ μλλ€.
        - ν¨μκ° μννλ μμκ³Ό λ°ννλ λ΄μ©μ μ€λͺνλ€. null ν¨κ³Ό λ° Void λ°νμ μ€λͺμ μλ΅νλ€.
            
            ```swift
            /// Inserts `newHead` at the beginning of `self`.
            mutating func prepend(_ newHead: Int)
            
            /// Returns a `List` containing `head` followed by the elements of `self`.
            func prepending(_ head: Element) -> List
            
            /// Removes and returns the first element of `self` if non-empty; returns `nil` otherwise.
            mutating func popFirst() -> Element?
            // Note: λλ¬Όκ² popFirst ν¨μμ κ°μ΄ μμ½λ¬Έκ΅¬κ° μ¬λ¬ λ¬Έμ₯μΈ κ²½μ°, λ¬Έμ₯μ ;μΌλ‘ κ΅¬λΆνλ€.
            ```
            
        - Subscriptκ° μ κ·Όνλ λμμ μ€λͺνλ€.
        - μ΄λμλΌμ΄μ κ° μμ±νλ λμμ μ€λͺνλ€.
        - κ·Έ μΈμ κ²½μ°, μ μΈλ Entity(νλ‘νΌν° λ° λ©μλ λ±)κ° λ¬΄μμΈμ§ μ€λͺνλ€.
            
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
            
        
    - νμ μ 1κ° μ΄μμ λ¬Έλ¨ (paragraphs)κ³Ό κΈλ¨Έλ¦¬ κΈ°νΈ (bullet items)λ₯Ό μΆκ°νλ€. λ¬Έλ¨μ ν μ€μ λμ΄μ°κ³  (separated by blank), μμ ν λ¬Έμ₯ (complete sentences)μ μ¬μ©νλ€.
        
        ```swift
        /// Writes the textual representation of each    β Summary
        /// element of `items` to the standard output. 
        /// (κ° 'items' elementμ νμ€νΈ ννμ νμ€ μΆλ ₯μλ€ μ λλ€.) 
        ///                                              β Blank line
        /// The textual representation for each item `x` β Additional discussion (μΆκ° μ€λͺ)
        /// is generated by the expression `String(x)`.
        ///
        /// - Parameter separator: text to be printed    β«
        ///   between items.                             β
        /// - Parameter terminator: text to be printed   β¬ Parameters section
        ///   at the end.                                β
        ///                                              β­
        /// - Note: To print without a trailing          β«
        ///   newline, pass `terminator: ""`             β
        ///                                              β¬ Symbol commands (κΈ°ν μ°Έκ³ μ¬ν­)
        /// - SeeAlso: `CustomDebugStringConvertible`,   β
        ///   `CustomStringConvertible`, `debugPrint`.   β­
        public func print(
          _ items: Any..., separator: String = " ", terminator: String = "\n") // κ°λ³ λ§€κ°λ³μλκΉ argument itemsλ array ννλ‘ μ λ¬λλ€.
        ```
        
        - μμ½ μΈμ μΆκ° μ λ³΄λ symbol documentation markupμ μ¬μ©νμ¬ μ κ³΅νλ€.
            - [ ]  [https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW1](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW1)
        - symbol command syntaxλ₯Ό μ¬μ©νλ€. Xcodeλ μλ ν€μλλ‘ μμνλ κΈλ¨Έλ¦¬ κΈ°νΈ (bullet items)λ₯Ό μ·¨κΈνλ μ½μλ λ°©μμ΄ μλ€.
        Attention	Author Authors Bug Complexity Copyright Date Experiment Important Invariant Note Parameter Parameters	Postcondition	Precondition Remark Requires Returns	SeeAlso Since Throws ToDo Version Warning

## Naming (μ΄λ¦ μ§κΈ°)

### Promote Clear Usage (μ΄λ¦μΌλ‘ λͺνν μ¬μ©λ²μ μ μνκΈ°)

- μ½λλ₯Ό μ½λ μ¬λμ΄ λͺνν μ΄ν΄νλλ‘ νμν λͺ¨λ  λ¨μ΄λ₯Ό μ¬μ©νλ€.
    
    ex. Collection λ΄λΆμ μ£Όμ΄μ§ μμΉμ μλ elementλ₯Ό μ κ±°νλ λ©μλλ₯Ό κ³ λ €νλ€λ©΄,
    
    ```swift
    extension List {
      public mutating func remove(at position: Index) -> Element
    }
    employees.remove(at: x) - μ’μ μμ. xκ° μ­μ ν  elementμ μμΉλ₯Ό λνλ΄λ κ²μ΄ λͺννλ€.
    employees.remove(x) - λμ μμ. xλΌλ elementλ₯Ό μ κ±°νλ€λ μλ―Έλ‘ μ€μΈν  μ μλ€.
    ```
    
- λΆνμν λ¨μ΄λ μλ΅νλ€. μ΄λ¦μ λͺ¨λ  λ¨μ΄λ μ¬μ© μμ μμ ν΅μ¬μ μΈ μ λ³΄λ₯Ό μ λ¬ν΄μΌ νλ€. νΉν λ¨μν type μ λ³΄λ₯Ό μ€λ³΅μΌλ‘ μ μνλ λ¨μ΄λ μλ΅νλ€.
    
    ```swift
    public mutating func remove(member: Element) -> Element? 
    allViews.remove(cancelButton) - μ’μ μμ. λͺννλ€.
    
    public mutating func removeElement(member: Element) -> Element?
    allViews.removeElement(cancelButton) - λμ μμ. ν¨μ νΈμΆ μ ElementλΌλ λ¨μ΄λ ν΅μ¬ μ λ³΄κ° μλλ€.
    ```
    
- λ³μ/λ§€κ°λ³μ/μ°κ΄ typeμ μ΄λ¦μ type μ μ½ μ‘°κ±΄μ΄ μλλΌ "μ­ν "μ λ°λΌ μ§μ νλ€.
    
    ```swift
    var greeting = "Hello" // μ’μ μμ. Entityμ μ­ν μ νννλ μ΄λ¦μ μ¬μ©νλ κ²μ΄ μ’λ€.
    protocol ViewController {
    	  associatedtype ContentView : View
    }
    class ProductionLine {
    	  func restock(from supplier: WidgetFactory)
    }
    
    var string = "Hello" // λμ μμ. μ΄μ²λΌ type μ΄λ¦μ λ°λ³΅ν΄μ μ¬μ© (type nameμ μ©λ λ³κ²½ ???) νλ κ²μ λͺνμ±κ³Ό ννλ ₯μ λ¨μ΄νΈλ¦°λ€.
    protocol ViewController {
    	  associatedtype ViewType : View
    }
    class ProductionLine {
    	  func restock(from widgetFactory: WidgetFactory)
    }
    ```
    
    If an associated type is so tightly bound to its protocol constraint that the protocol name is the role, avoid collision by appending `Protocol` to the protocol name:
    (μ°κ΄ typeμ΄ protocol μ μ½μ¬ν­κ³Ό λ°μ ν μ°κ΄μ΄ μμ΄ protocol μ΄λ¦μ΄ ν΄λΉ μ­ν  μμ²΄μΈ κ²½μ°, protocol μ΄λ¦μ `Protocol`μ λΆμ¬μ μΆ©λμ λ°©μ§νλ€.)
    
    - [ ]  protocol constraint ???
    
    ```swift
    protocol Sequence {
      associatedtype Iterator : IteratorProtocol
    }
    protocol IteratorProtocol { ... }
    ```
    
- Compensate for weak type information to clarify a parameterβs role. (parameterμ μ­ν μ λͺμνκΈ° μν΄ λΆμ‘±ν type μ λ³΄λ₯Ό λ³΄μνλ€.) weak type ???
parameter typeμ΄ NSObject, Any, AnyObject, or a fundamental type (Int, String λ±)μΈ κ²½μ°, μ¬μ© μμΉμμ type μ λ³΄κ° λΆλͺννμ¬ ν΄λΉ λ¬Έλ§₯μ΄ μλλ₯Ό μΆ©λΆν λνλ΄κΈ° μ΄λ €μΈ μ μλ€.
    
    ```swift
    func addObserver(_ observer: NSObject, forKeyPath path: String) // μ’μ μμ. weak type parameterμΈ pathμ μ­ν μ μ€λͺνλ λͺμ¬(forKeyPath)λ₯Ό λΆμ¬μ μ λ³΄λ₯Ό λ³΄μνλ€.
    grid.addObserver(self, forKeyPath: graphics) // clear
    
    func add(_ observer: NSObject, for keyPath: String) // λμ μμ. μ μΈμ λͺννμ§λ§, μ¬μ© μμ  (use site)μμ λͺ¨νΈνλ€.
    grid.add(self, for: graphics) // vague
    ```
    

### *Strive for Fluent Usage (μ½κ³  λͺννκ² μ½νλλ‘ μμ±νκΈ°)

- ν¨μλ₯Ό μ¬μ©νλ μμΉμμ ν¨μ μ΄λ¦μ΄ 'μμ΄ λ¬Έμ₯μ νν'κ° λλλ‘ νλ€. Prefer function names that make use sites form grammatical English phrases.
    
    ```swift
    x.insert(y, at: z)          βx, insert y at zβ (xμ yλ₯Ό zμμΉμ μ½μ)  // μ’μ μμ
    x.subViews(havingColor: y)  βx's subviews having color yβ (xμ subviewsλ μμ yμ κ°μ§)
    x.capitalizingNouns()       βx, capitalizing nounsβ (xμ λͺμ¬λ₯Ό λλ¬Έμν)
    
    x.insert(y, position: z)  // λμ μμ
    x.subViews(color: y)
    x.nounCapitalize()
    ```
    
    μ²«λ²μ§Έ λλ λλ²μ§Έ argument λ€μμ μ λ¬νλ κ°λ€μ΄ μ€μνμ§ μμΌλ©΄, κ°λμ±μ μν΄  ν΄λΉ κ°λ€ μμ μ€ λ°κΏνλ κ²μ νμ©νλ€.
    
    ```swift
    AudioUnit.instantiate(
      with: description, 
      options: [.inProcess], completionHandler: stopProgressBar)  // μ€μλκ° λ?μ argumentsμ΄λ―λ‘ μλμ μΌλ‘ μ€λ°κΏ νλ€.
    ```
    
- factory methodμ μ΄λ¦μ `make`λ‘ μμνλ€. ex. `x.makeIterator()`
    - [ ]  factory method??? μ€λͺ μ΄ν΄ μλ¨
- The first argument to initializer and factory methods calls should not form a phrase starting with the base name. ex. `x.makeWidget(cogCount: 47)`
(μμ ν¨μλ₯Ό μ¬μ©νλ μμ μμ ν¨μ μ΄λ¦μ΄ 'μμ΄ λ¬Έμ₯μ νν'κ° λμ΄μΌ νλ€κ³  νλ€. νμ§λ§ κ·Έλ λ€κ³ ν΄μ (μ΄λμλΌμ΄μ  λ° factory method νΈμΆ μ) μ²«λ²μ§Έ argumentλ₯Ό ν¬ν¨νμ¬ μμ΄ λ¬Έμ₯μ κ΅¬μ±ν΄μλ μλλ€.)
    
    ```swift
    let foreground = Color(red: 32, green: 64, blue: 128) // μ’μ μμ. μ΄λμλΌμ΄μ  λ° factory method νΈμΆ μ, μ²«λ²μ§Έ argumentλ₯Ό λ¬Έμ₯μ μΌλΆλ‘ λ§λ€μ§ μμλ€. 
    let newPart = factory.makeWidget(gears: 42, spindles: 14)
    
    let foreground = Color(havingRGBValuesRed: 32, green: 64, andBlue: 128) // λμ μμ. μ²«λ²μ§Έ argumentμ μΆκ° μ€λͺμ λ§λΆμ¬ λ¬Έμ₯μ λ§λ€μλ€.
    let newPart = factory.makeWidget(havingGearCount: 42, andSpindleCount: 14)
    ```
    
    In practice, this guideline along with those for argument labels means the first argument will have a label unless the call is performing a value preserving type conversion. ???
    
    μ€μ λ‘ μ΄ κ°μ΄λλΌμΈκ³Ό argument labelμ κ΄ν κ°μ΄λλΌμΈμ??? μ²«λ²μ§Έ argumentκ° argument labelμ κ°μ§λ€λ κ²μ μλ―Ένλ€. ν¨μ νΈμΆμ ν΅ν΄ κ°μ μ μ§νλ type λ³νμ μ€ννμ§ μλλ€λ©΄.
    
    ```swift
    let rgbForeground = RGBColor(cmykForeground)
    ```
    
- ν¨μ μ΄λ¦μ Side-effects (λΆμμ© ???) μ¬λΆμ λ°λΌ μ§μ νλ€.
    - [ ]  λΆμμ©?
    - side-effectsκ° μλ κ²½μ° λͺμ¬κ΅¬λ‘ μ§μ νλ€. ex. `x.distance(to: y)`, `i.successor()`
    - side-effectsκ° μλ κ²½μ° λͺλ Ήν λμ¬κ΅¬ (imperative verb phrases)λ‘ μ§μ νλ€. ex. `print(x)`, `x.sort()`, `x.append(y)`
    - κ°λ³ (mutating) λ° λΆλ³ (nonmutating) λ©μλ pairμ μ΄λ¦μ μΌκ΄μ± μκ² μ§μ νλ€. 
    A mutating method will often have a nonmutating variant with similar semantics, but that returns a new value rather than updating an instance in-place. ????
        
        ```swift
        // μ°Έκ³  - mutating ν€μλ : κ΅¬μ‘°μ²΄μ λ©μλκ° κ΅¬μ‘°μ²΄ λ΄λΆμμ λ°μ΄ν° μμ  ν λλ Mutating ν€μλλ₯Ό μ μΈ ν΄μ£Όμ΄μΌν¨
        struct Point {
        var x = 0 
        var y = 0 
        
        mutating func moveTo(x: Int, y: Int) {
        	self.x = x  // mutating ν€μλκ° μμΌλ©΄ μ»΄νμΌ μλ¬ λ°μ
        	self.y = y 
          }
        }
        ```
        
        - λ©μλμ λμμ΄ λμ¬μ μν΄ μμ°μ€λ½κ² μ€λͺλ  λ, *κ°λ³ λ©μλμ μ΄λ¦μΌλ‘ λͺλ Ήν λμ¬λ₯Ό μ¬μ©νλ€. *λΆλ³ λ©μλμ μ΄λ¦μ "ed" λλ "ing" suffixλ₯Ό λΆμΈλ€.
            
            - κ°λ³ λ©μλ : Β `x.sort()`,Β `x.append(y)`
            
            - λΆλ³ λ©μλ : Β `z = x.sorted()`,Β `z = x.appending(y)`
            
            - λμ¬μ κ³Όκ±°λΆμ¬ (λ³΄ν΅ "ed"λ₯Ό λΆμ)λ₯Ό μ¬μ©νμ¬ λΆλ³ λ©μλμ μ΄λ¦μ μ§μ νλ€.
                
                ```swift
                /// Reverses `self` in-place. (λ€μ§μ) <- λΉκ΅μ©
                mutating func reverse()
                
                /// Returns a reversed copy of `self`. (λ€μ§ν μνμ λ³΅μ¬λ³Έμ λ°νν¨)
                func reversed() -> Self
                ...
                x.reverse()
                let y = x.reversed()
                ```
                
            - λμ¬κ° λͺ©μ μ΄λ₯Ό κ°μ§λ―λ‘ "ed"λ₯Ό λΆμ΄λ κ²μ΄ λ¬Έλ²μ λ§μ§ μμ κ²½μ°, λμ¬μ νμ¬λΆμ¬ (λ³΄ν΅ "ing"λ₯Ό λΆμ)λ₯Ό μ¬μ©νμ¬ λΆλ³ λ©μλμ μ΄λ¦μ μ§μ νλ€.
                
                ```swift
                /// Strips all the newlines from `self` (μ€λ°κΏμ μ κ±°ν¨) <- λΉκ΅μ©
                mutating func stripNewlines()
                
                /// Returns a copy of `self` with all the newlines stripped. (μ€λ°κΏμ μ κ±°ν μνμ λ³΅μ¬λ³Έμ λ°νν¨)
                func strippingNewlines() -> String
                ...
                s.stripNewlines()
                let oneLine = t.strippingNewlines()
                ```
                
        - λ©μλμ λμμ΄ λͺμ¬μ μν΄ μμ°μ€λ½κ² μ€λͺλ  λ, *λΆλ³ λ©μλμ μ΄λ¦μΌλ‘ λͺμ¬λ₯Ό μ¬μ©νκ³ , *κ°λ³ λ©μλμ μ΄λ¦μ "from" prefixλ₯Ό λΆμΈλ€.
            
            - λΆλ³ λ©μλ :Β `x = y.union(z)`,Β `j = c.successor(i)`
            
            - κ°λ³ λ©μλ : `y.formUnion(z)`,Β `c.formSuccessor(&i)`
            
        - λΆλ³ λ©μλ/νλ‘νΌν°μΌ λ, Boolean λ©μλ λ° νλ‘νΌν°λ receiver (λ°νκ°)μ λν μ£Όμ₯ (Assertion) ννλ‘ μ΄λ¦μ μ§μ νλ€. ex. `x.isEmpty`, `line1.intersects(line2)`
        - λ¬΄μΈκ°λ₯Ό μ€λͺνλ Protocolμ λͺμ¬λ‘ μ΄λ¦μ μ§μ νλ€. ex. `Collection`
        - κΈ°λ₯ (capability)μ μ€λͺνλ Protocolμ "able, ible, ing" suffixλ₯Ό λΆμΈλ€. ex. `Equatable`, `ProgressReporting`
        - μ΄μΈ type, νλ‘νΌν°, λ³μ/μμλ λͺμ¬λ‘ μ΄λ¦μ μ§μ νλ€.

### Use Terminology Well (μ©μ΄λ₯Ό μ λλ‘ μ¬μ©νκΈ°)

*Term of Art : νΉμ  λΆμΌμμ ν΅μ©λλ μ λ¬Έμ μΈ μ©μ΄λ₯Ό μλ―Ένλ€.

- μΌλ°μ μΈ λ¨μ΄κ° μλ―Έλ₯Ό μ μ λ¬ν  μ μλ€λ©΄ λͺ¨νΈν μ©μ΄λ νΌνλ€. βνΌλΆ(skin)βκ° λͺ©μ μ λ§λ μ©μ΄λΌλ©΄ βννΌ(epidermis)"λ₯Ό μ¬μ©νμ§ μλλ€. Terms of artλ μ€μν μ»€λ?€λμΌμ΄μ μλ¨μ΄μ§λ§, μλ―Έλ₯Ό μλ°ν κ΅¬λΆν  νμκ° μμ λλ§ μ¬μ©νλ€.
- Term of Artλ₯Ό μ¬μ©νλ€λ©΄ κΈ°μ‘΄μ μλ―Έμ μΆ©μ€ν΄μΌ νλ€.
    - APIλ λ°μλ€μ¬μ§λ μλ―Έμ λ°λΌ μκ²©νκ² μ©μ΄λ₯Ό μ¬μ©ν΄μΌ νλ€.
        
        - μ λ¬Έκ°λ₯Ό λλΌκ² νμ§ μλλ€: κΈ°μ‘΄ μ©μ΄μ μλ‘μ΄ μλ―Έλ₯Ό μ§μ΄λ΄λ©΄, μ΄λ―Έ ν΄λΉ μ©μ΄μ μ΅μν μ¬λμ΄ λΉν©ν  μ μλ€.
        
        - μ΄λ³΄μλ₯Ό νΌλμ€λ½κ² νμ§ μλλ€: μ΄λ³΄μλ μΉ κ²μμ ν΅ν΄ κΈ°μ‘΄μ ν΅μ©λλ μλ―Έλ₯Ό λ°μλ€μΈλ€.
        
- μ½μ΄ (abbreviations)λ₯Ό νΌνλ€. Abbreviations, especially non-standard ones, are effectively terms-of-art, because understanding depends on correctly translating them into their non-abbreviated forms. ??? (νΉν νμ€μ΄ μλ μ½μ΄λ νμ΄μ°λ κ³Όμ μμ μ€μΈλ  μ μλ€.) μ½μ΄λ₯Ό μ¬μ©νλ€λ©΄ μΉ κ²μμ ν΅ν΄ μ½κ² μ°Ύμ μ μμ΄μΌ νλ€.
- μ λ‘ (precedent)λ₯Ό λ°λ₯Έλ€.  μ΄λ³΄μλ₯Ό μν΄ μ©μ΄λ₯Ό μ΅μ ννμ§ μλλ€.
    
    - μμ-1. μ΄λ³΄μλΒ Listλ₯Ό λ μ½κ² μ΄ν΄ν  μ μμμ§λΌλΒ Arrayλ₯Ό μ¬μ©νλ κ² μ’λ€. Arrayλ λͺ¨λ  νλ‘κ·Έλλ¨Έκ° μ΅μν κΈ°μ΄ μ©μ΄μ΄κΈ° λλ¬Έμ΄λ€.
    
    - μμ-2. μν λ± νΉμ  νλ‘κ·Έλλ° Domainμμλ νλ‘κ·Έλλ¨Έ/μνμμκ² μ΅μν μ©μ΄μΈ "sin(x)"μ μ¬μ©νλ κ² μ’λ€. μμΉμ μΌλ‘λ "sine"μ΄ complete word μμλ λΆκ΅¬νκ³ , μ½μ΄κ° λ λ§μ΄ ν΅μ©λκΈ° λλ¬Έμ΄λ€. (In this case, precedent outweighs the guideline to avoid abbreviations.)
    

## Conventions (κ·μΉ)

### General Conventions (μΌλ°μ μΈ κ·μΉ)

- complexityκ° O(1)μ΄ μλ μ°μ° νλ‘νΌν°λ λͺ¨λ λ¬Έμννμ¬ μ€λͺνλ€. (Document the complexity of any computed property that is not O(1).) μ’μ’ νλ‘νΌν° μ κ·Όμ΄ μ€μν μ°μ°μ κ±°μΉμ§ μλλ€κ³  μκ°νλ€. μ΄λ μ¬λλ€μ΄ μ μ₯ νλ‘νΌν°λ₯Ό mental modelλ‘ μ¬κΈ°κΈ° λλ¬Έμ΄λ€. νμ§λ§ κ·Έλ μ§ μμ λκ° μλ€.
    
    *mental model : μ΄λ€ μ¬λ¬Όμ κΈ°μ΅νκΈ° μν΄ μ€μν νΉμ§μ΄λΌκ³  μΈμνλ κ²μ΄λ€. μ¬κΈ°μλ νλ‘νΌν°λ₯Ό κΈ°μ΅ν  λ λ³΄ν΅ μ μ₯ νλ‘νΌν°λ₯Ό λ¨Όμ  λ μ¬λ¦°λ€λ μλ―Έμ΄λ€.
    
- free function (μμ  ν¨μ???) λ³΄λ€ λ©μλ λ° νλ‘νΌν°κ° μ νΈλλ€. free functionμ νΉμν μν©μμλ§ μ¬μ©νλ€.
    1. λͺνν selfκ° μμ λ
    2. ν¨μκ° μ μ½ μλ μ λ€λ¦­ (unconstrained generic)μΌ λ ???
    3. ν¨μ λ¬Έλ²μ΄ κΈ°μ‘΄ λλ©μΈ νκΈ°λ²μ λ°λ₯Ό λ
        
        ```swift
        min(x, y, z) // 1
        print(x)     // 2
        sin(x)       // 3
        ```
        
- λμλ¬Έμ νκΈ°λ² (Case conventions)μ λ°λ₯Έλ€. Type λ° Protocolμ μ΄λ¦μ UpperCamelCaseλ₯Ό μ¬μ©νκ³ , κ·ΈμΈλ λͺ¨λ lowerCamelCaseλ₯Ό μ¬μ©νλ€.
    
    λ―Έκ΅­μ μμ΄μμ λλ¬Έμλ‘ νννλ Acronyms (λλ¬Έμμ΄, λ¨Έλ¦¬κΈμλ§ λ°μ λ§λ  λ¨μ΄) λ° Initialisms (μ΄λμ)μ μΌκ΄λκ² λλ¬Έμ λλ μλ¬Έμλ‘ μ¬μ©νλ€.
    
    ```swift
    var utf8Bytes: [UTF8.CodeUnit]
    var isRepresentableAsASCII = true
    var userSMTPServer: SecureSMTPServer
    ```
    
    κ·ΈμΈ Acronymsμ μΌλ°μ μΈ λ¨μ΄λ‘ μ·¨κΈνλ€.
    
    ```swift
    var radarDetector: RadarScanner
    var enjoysScubaDiving = true
    ```
    
- λμΌν κΈ°λ³Έ μλ―Έλ₯Ό κ°κ³  μκ±°λ, νΉμ  domainμμλ§ λμν  λ λ©μλλ base nameμ κ³΅μ ν  μ μλ€. 
*iff : (μν μ©μ΄) if and only if (νμμΆ©λΆμ‘°κ±΄μ λνλ)
    
    ```swift
    // λ©μλκ° λ³Έμ§μ μΌλ‘ λμΌνκ² λμνλ―λ‘ base nameμ κ³΅μ νλ€. (parameter typeμ λ€λ₯΄μ§λ§)
    extension Shape {  
      /// Returns `true` iff `other` is within the area of `self`.
      func contains(_ other: Point) -> Bool { ... }
    
      /// Returns `true` iff `other` is entirely within the area of `self`.
      func contains(_ other: Shape) -> Bool { ... }
    
      /// Returns `true` iff `other` is within the area of `self`.
      func contains(_ other: LineSegment) -> Bool { ... }
    }
    ```
    
    geometric types ??? λ° collectionsμ λ³λμ domain μ΄λ―λ‘ λμΌν νλ‘κ·Έλ¨ λ΄μμ λ©μλ μ΄λ¦μ΄ λμΌν΄λ λλ€.
    
    ```swift
    extension Collection where Element : Equatable {
      /// Returns `true` iff `self` contains an element equal to `sought`.
      func contains(_ sought: Element) -> Bool { ... }
    }
    ```
    
    index λ©μλλ λ€λ₯Έ μλ―Έλ₯Ό κ°μ§λ―λ‘ μλ‘ λ€λ₯Έ μ΄λ¦μΌλ‘ μ§μ ν΄μΌ νλ€.
    
    ```swift
    extension Database {  // λμ μμ. λ©μλμ μλ―Έ λ° λμμ΄ λ€λ₯΄λ―λ‘ λ€λ₯Έ μ΄λ¦μ μ§μ ν΄μΌ νλ€.
      /// Rebuilds the database's search index
      func index() { ... }
    
      /// Returns the `n`th row in the given table.
      func index(_ n: Int, inTable: TableID) -> TableRow { ... }
    }
    ```
    
    type inference κΈ°λ₯μΌλ‘ μΈν λͺ¨νΈμ±μ΄ λ°μνλ―λ‘ βoverloading on return typeβμ νΌνλ€. (return typeμ΄ λ€λ₯Έ κ²½μ° overloadingμ΄ κ°λ₯νμ§λ§, ν¨μμ΄λ¦μ λ€λ₯΄κ² μ§μ νλ κ²μ΄ λ«λ€.)
    
    ```swift
    extension Box {  // λμ μμ. return typeμ΄ λ€λ₯΄λ―λ‘ λ€λ₯Έ μ΄λ¦μ μ§μ νλκ² μ’λ€.
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

- λ¬Έμμ κ°λμ±μ λμ΄λ parameter μ΄λ¦μ μ§μ νλ€. ν¨μ μ¬μ© μμ μ parameter μ΄λ¦μ΄ λ³΄μ΄μ§ μμ μ μμ§λ§, ν¨μλ₯Ό μ€λͺνλ μ€μν μ­ν μ νλ€.
    
    ```swift
    /// Return an `Array` containing the elements of `self` that satisfy `predicate`.  // μ’μ μμ. μμ°μ€λ½κ² μ½νλ€.
    func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]
    
    /// Replace the given `subRange` of elements with `newElements`.
    mutating func replaceRange(_ subRange: Range, with newElements: [E])
    
    /// Return an `Array` containing the elements of `self` that satisfy `includedInResult`.  // λμ μμ. μ΄μνκ±°λ λ¬Έλ²μ λ§μ§ μλλ€.
    func filter(_ includedInResult: (Element) -> Bool) -> [Generator.Element]
    
    /// Replace the range of elements indicated by `r` with the contents of `with`.
    mutating func replaceRange(_ r: Range, with: [E])
    ```
    
- λ§€κ°λ³μ κΈ°λ³Έκ° (defaulted parameter)μ μ¬μ©νμ¬ ν¨μ μ¬μ©μ λ¨μννλ€. νν μ¬μ©νλ κ° 1κ°λ₯Ό μ λ¬λ°λ parameterλ λ§€κ°λ³μ κΈ°λ³Έκ°μ κ°μ§ νλ₯ μ΄ λλ€.
    
    ```swift
    let order = lastName.compare(royalFamilyName)  // μ’μ μμ. ν¨μ μ¬μ©μ΄ κ°λ¨νλ€.
    
    let order = lastName.compare(  // λμ μμ. κ΄λ ¨ μλ μ λ³΄λ₯Ό μ¨κΈ°μ§ μμμ κ°λμ±μ΄ λμλ€.
      royalFamilyName, options: [], range: nil, locale: nil)
    ```
    
    λ§€κ°λ³μ κΈ°λ³Έκ°μ λ³΄ν΅ method family (λ©μλ μ§ν©) λ³΄λ€ μ νΈλλλ°, APIλ₯Ό μ½μ λ λΆλ΄μ΄ μ κΈ° λλ¬Έμ΄λ€.
    
    ```swift
    extension String {  // μ’μ μμ. μ½μ λ λΆλ΄μ΄ μ λ€.
      /// ...description...
      public func compare(
         _ other: String, options: CompareOptions = [],
         range: Range? = nil, locale: Locale? = nil
      ) -> Ordering
    }
    
    extension String {  // λμ μμ. method familyλ μ½κΈ° λ³΅μ‘νλ€. (λ€λ₯Έ κ°μμ parameterλ‘ overload)
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
    
    μ¬μ©μ μμ₯μμλ method familyμ λͺ¨λ  memberμ λν κ°λ³μ μΈ μ€λͺμ μ½κ³  μ΄ν΄ν΄μΌλ§ λ©μλλ₯Ό μ¬μ©ν  μ μλ€. method family μ€μμ μ¬μ©ν  λ©μλλ₯Ό κ²°μ νλ €λ©΄, λͺ¨λ  λ©μλλ₯Ό μ΄ν΄ν΄μΌ νκΈ° λλ¬Έμ΄λ€. 
    λν κ°λ μμμΉ λͺ»ν μν©μμ (μλ₯Ό λ€μ΄ `foo(bar: nil)` λ° `foo()`λ ν­μ λμμ΄κ° μλ ???) λ―ΈμΈν μ°¨μ΄λ₯Ό μ°ΎκΈ° μν΄ μ μ²΄ λ¬Έμλ₯Ό νμΈν΄μΌ νλ λΆνΈν¨μ μ΄λνλ€. λ°λΌμ λ§€κ°λ³μ κΈ°λ³Έκ°μ μ¬μ©νλ λ¨μΌ λ©μλλ₯Ό μ¬μ©νλ©΄ λ³΄λ€ λμ κ°λ°μ΄ κ°λ₯νλ€.
    
- λ§€κ°λ³μ κΈ°λ³Έκ°μ΄ μλ parameterλ parameter listμ λλΆλΆμ λ°°μΉνλ€. λ§€κ°λ³μ κΈ°λ³Έκ°μ΄ μλ parameterκ° λ©μλμ μλ―Έμ λ μ€μνκ³ , λ©μλ νΈμΆ μμΉμμ μμ μ μΈ μ΄κΈ°ν ννλ₯Ό μ κ³΅νκΈ° λλ¬Έμ΄λ€.

### Argument Labels

```swift
func move(from start: Point, to end: Point)
x.move(from: x, to: y)
```

- argument labelμ΄ argumentλ₯Ό κ΅¬λΆνλλ° μ μ©νμ§ μμΌλ©΄, argument labelμ μλ΅νλ€. ex. `min(number1, number2)`, `zip(sequence1, sequence2)`
- μ΄λμλΌμ΄μ κ° value λ³΄μ‘΄ type λ³ν (value preserving type conversions ???)μ μνν  λ, μ²«λ²μ§Έ argument labelμ μλ΅νλ€. ex. `Int64(someUInt32)`
    
    ```swift
    extension String {  // μ’μ μμ. type λ³ν μ, μ²«λ²μ§Έ argumentλ ν­μ λ³ν λμ (source)μ΄λ€.
      // Convert `x` into its textual representation in the given radix. (*radix : μ§λ²)
      init(_ x: BigInt, radix: Int = 10)   β Note the initial underscore 
    }
    
    text = "The value is: "
    text += String(veryLargeNumber)
    text += " and in hexadecimal, it's"
    text += String(veryLargeNumber, radix: 16)
    ```
    
    λ¨, βnarrowingβ type λ³ν μ, narrowingμ μ€λͺνλ argument labelμ μ¬μ©νλ€. 
    *μ λ°ν (narrowing) type λ³νμ λ μ΄λΈμ μ λ°λλ₯Ό νννλ€. (μλ μ½λμμ 64λΉνΈμμ 32λΉνΈλ‘ κ·Έλ₯ μ€μ΄λ€ λλ truncatingλ₯Ό, 64λΉνΈμμ 32λΉνΈ κ·Όμ¬κ°μ μ²λ¦¬ν  λλ saturatingλ‘ νκΈ°νκ³  μλ€.)
    
    ```swift
    extension UInt32 {
      /// Creates an instance having the specified `value`.
      init(_ value: Int16)            β Widening, so no label
      /// Creates an instance having the lowest 32 bits of `source`.
      init(truncating source: UInt64)
      /// Creates an instance having the nearest representable approximation of `valueToApproximate`. (νν κ°λ₯ν κ°μ₯ κ°κΉμ΄ κ·Όμ¬μΉ) *approximation : κ·Όμ¬μΉ
      init(saturating valueToApproximate: UInt64)
    }
    ```
    
    κ° λ³΄μ‘΄ type λ³ν (a value preserving type conversion)μ *monomorphism (λ¨νμ±)μ΄λ€. μ¦, λ³ν λμ (source)μΈ κ°μ μ°¨μ΄λ κ²°κ³Όκ°μ μ°¨μ΄λ₯Ό μ΄λνλ€. ???
    μλ₯Ό λ€μ΄, Int8μμ Int64λ‘μ type λ³νμ κ°μ λ³΄μ‘΄νλ€. λ°λ©΄, λ°λ λ°©ν₯μΌλ‘μ type λ³νμ κ° λ³΄μ‘΄μ΄ λΆκ°νλ€. Int64μ κ°μ Int8λ‘ νν κ°λ₯ν κ°λ³΄λ€ λ λ§κΈ° λλ¬Έμ΄λ€.
    
    Note: μλ κ°μ μ»λ μ μλμ§ (the ability to retrieve the original value)λ ν΄λΉ type λ³νμ΄ κ°μ λ³΄μ‘΄νλμ§ μ¬λΆμ κ΄λ ¨μ΄ μλ€. ????? (κ°μ λ³΄μ‘΄νμ§ μλλΌλ μλ κ°μ κ·Έλλ‘ κ°μ Έμ¬ μ μλ κ²½μ°λ μκ³  κ°μ λ°λΌ λ€λ₯Ό μ μλ€.)
    
    *(OOP μ©μ΄) λ€νμ± (polymorphism)μ΄λ νλ‘κ·Έλ¨ μΈμ΄μ κ° μμλ€ (μμ, λ³μ, κ°μ²΄, ν¨μ λ±)μ΄ λ€μν data typeμ μνλ κ²μ΄ νμ©λλ μ±μ§μ΄λ€. 
    - λ¨νμ± : ν¨μλ κ³ μ μ μ΄λ¦μΌλ‘ μλ³λλ©°, ν κ°μ§ μλ―Έλ₯Ό κ°μ§λ€. λ°λΌμ λ€λ₯Έ κΈ°λ₯μ κ΅¬ννλ €λ©΄ λ€λ₯Έ μ΄λ¦μ μ¬μ©ν΄μΌ νλ€. 
    - λ€νμ± : λ€νμ± μ²΄κ³μμλ λ²μ© λ©μλ μ΄λ¦μ μ μνμ¬ ννμ λ°λΌ κ°κ° μ μ ν λ³ν λ°©μμ μ μν΄λμ΄μ κ°μ²΄μ μ’λ₯μ μκ΄μμ΄ μΆμνλ₯Ό λμΌ μ μλ€. (νλ©΄μ λΏλ¦°λ€ (render)λ κ°λκ³Ό μΌλ§₯μν΅νμ¬ λΆλͺ¨ν΄λμ€κ° μΌκ΄μ μΌλ‘ κ°μ§λ κ²μ΄ ν©λ¦¬μ μ΄λ€.)
    
    ```java
    // λ¨νμ±
    string = StringFromNumber(number);
    string = stringFromDate(date);
    
    // λ€νμ± 
    string = number.stringValue();
    string = date.StringValue();
    ```
    
- μ²«λ²μ§Έ argumentκ° μ μΉμ¬κ΅¬(prepositional phrase)μ μΌλΆμΈ κ²½μ°, argument labelμ μ¬μ©νλ€. μ΄λ argument labelμ λ³΄ν΅ μ μΉμ¬λ‘ μμνλ€. ex. `x.removeBoxes(havingLength: 12)`
    
    λ¨, μ²«λ²μ§Έ λ° λλ²μ§Έ argumentκ° λμΌν μΆμν μμ€μΈ κ²½μ° (when the first two arguments represent parts of a single abstraction. ???), μμΈμ μΌλ‘ argument label μμ μ μΉμ¬λ₯Ό μ λλ€.
    
    ```swift
    a.moveTo(x: b, y: c)  // μ’μ μμ. μμΈ μν©μ ν΄λΉνλ―λ‘ argument label μμ μ μΉμ¬λ₯Ό μ λλ€.
    a.fadeFrom(red: b, green: c, blue: d)
    
    a.move(toX: b, y: c)  // λμ μμ. 
    a.fade(fromRed: b, green: c, blue: d)
    ```
    
- κ·Έμ λ¬λ¦¬, μ²«λ²μ§Έ argumentκ° λ¬Έλ²μ λ§λ λ¬Έμ₯μ μΌλΆμΈ κ²½μ°, argument labelμ μλ΅νκ³ , base name (κΈ°λ³Έ ν¨μ μ΄λ¦)μ μ ν λ¨μ΄ (preceding word)λ₯Ό λΆμΈλ€. ex. `x.addSubview(y)`
    
    μ΄λ λ€λ₯Έ λ§λ‘ νλ©΄, μ²«λ²μ§Έ argumentκ° λ¬Έλ²μ λ§λ λ¬Έμ₯μ μΌλΆκ° μλ κ²½μ°μλ argument labelμ μ¬μ©ν΄μΌ νλ€λ λ»μ΄λ€. 
    
    ```swift
    view.dismiss(animated: false)  // μ’μ μμ. argument labelμ μ¬μ©νμ¬ λ¬Έλ²μ λ§λ λ¬Έμ₯μ λ§λ€μλ€.
    let text = words.split(maxSplits: 12)
    let studentsByName = students.sorted(isOrderedBefore: Student.namePrecedes)
    x.addSubView(y)  // arugument label μμ΄λ λ¬Έλ²μ λ§λ λ¬Έμ₯μ΄λ©°, μ ν λ¨μ΄ (SubView)λ₯Ό λΆμ¬μ μλ―Έλ₯Ό λͺνν νλ€.
    
    view.dismiss(false)   Don't dismiss? Dismiss a Bool?  // λμ μμ. λ¬Έλ²μλ λ§μ§λ§, μλͺ»λ μλ―Έλ‘ μ€μΈλ  μ μλ€. *μ νν μλ―Έλ₯Ό μ λ¬ν  μ μλ λ¬Έμ₯μ λ§λλ κ²μ΄ μ€μνλ€. 
    words.split(12)       Split the number 12?
    ```
    
    Note: λ§€κ°λ³μ κΈ°λ³Έκ°μ΄ μλ parameterλ μλ΅ κ°λ₯νλ©°, μ΄ κ²½μ° ν΄λΉ parameterλ₯Ό λ¬Έμ₯μ ν¬ν¨μν€μ§ μλλ€. λ§μ½ ν¬ν¨μν€λ©΄ ν΄λΉ argument labelμ ν­μ μ¬μ©ν΄μΌ νκΈ° λλ¬Έμ΄λ€.
    
- κ·ΈμΈμ κ²½μ°, λͺ¨λ  argumentμ argument labelμ μ§μ νλ€.

## Special Instructions

- APIμμ λνλλ μμΉμ Tuple memberμ argument labelμ μ§μ νκ³ , ν΄λ‘μ  parameterμ μ΄λ¦μ μ§μ νλ€. 
μ΄λ κ² μ§μ λ μ΄λ¦μ μ€λͺ κΈ°λ₯μ κ°μ§λ©°, λ¬Έμν μ£Όμμμ μ°Έκ³ ν  μ μκ³ , tuple memberμ λν μ κ·Όμ±μ λμΈλ€.
    
    ```swift
    /// Ensure that we hold uniquely-referenced storage for at least `requestedCapacity` elements. (μ μ΄λ `requestedCapacity` elementμ λν΄ uniquely-referenced μ μ₯κ³΅κ°μ νλ³΄ν¨μ λ³΄μ₯νλ€.)
    ///
    /// If more storage is needed, `allocate` is called with `byteCount` equal to the number of maximally-aligned bytes to allocate. (μΆκ° μ μ₯κ³΅κ°μ΄ νμνλ©΄, maximally-aligned λ°μ΄νΈ κ°μμ λμΌν `byteCount`λ₯Ό μ¬μ©νμ¬ `allocate`λ₯Ό νΈμΆνλ€.)
    ///
    /// - Returns:
    ///   - reallocated: `true` iff a new block of memory was allocated.
    ///   - capacityChanged: `true` iff `capacity` was updated.
    mutating func ensureUniqueStorage(
      minimumCapacity requestedCapacity: Int, 
      allocate: (_ byteCount: Int) -> UnsafePointer<Void>
    ) -> (reallocated: Bool, capacityChanged: Bool)
    ```
    
    ν΄λ‘μ  parameterμ μ΄λ¦μ μμ ν¨μμ parameter μ΄λ¦μ²λΌ μ§μ νλ€. νΈμΆ μμΉμμ ν΄λ‘μ μ argument labelμ μ§μλμ§ μλλ€. ???
    
- unconstrained polymorphism (μ μ½λμ§ μμ λ€νμ±)μ overload νλ©΄ λͺ¨νΈνκ² ννλ  μ μμΌλ―λ‘ νΉν μ£Όμνλ€.
unconstrained polymorphismμ μλ Any, AnyObject, and unconstrained generic parameters λ±μ΄λ€.
    
    ```swift
    struct Array {  // μ’μ μμ. overload μ argument labelμ μ§μ νμ¬ μλ―Έ λ° typeμ λͺνν νλ€.
      /// Inserts `newElement` at `self.endIndex`.
      public mutating func append(_ newElement: Element)
    
      /// Inserts the contents of `newElements`, in order, at `self.endIndex`.  <- μ£Όμκ³Ό argument labelμ΄ μΌμΉνλ€. (μ΄μ²λΌ μ£Όμμ μμ±νλ κ²μ API μμ±μμκ² μν₯μ λ―ΈμΉλ€.)
      public mutating func append(contentsOf newElements: S)
        where S.Generator.Element == Element
    }
    ```
    
    ```swift
    	struct Array {  // overload setμ λμ μμ. 
      /// Inserts `newElement` at `self.endIndex`.
      public mutating func append(_ newElement: Element)
    
      /// Inserts the contents of `newElements`, in order, at `self.endIndex`.
      public mutating func append(_ newElements: S)
        where S.Generator.Element == Element
    }
    
    // μ΄ λ©μλλ semantic family (μλ―Έ μ§ν©)μ νμ±νλ©°, μ²μμλ parameter typeμ΄ λλ ·ν κ΅¬λΆλλ κ²μ²λΌ λ³΄μΈλ€. 
    // νμ§λ§ Elementκ° Any typeμΈ κ²½μ°, '1κ° elementμ type' λ° 'a sequence of elementsμ type'μ΄ λμΌν  μλ μλ€. ???
    
    var values: [Any] = [1, "a"]
    values.append([2, 3, 4]) // [1, "a", [2, 3, 4]] or [1, "a", 2, 3, 4] ?  <- elementμ typeμ΄ λͺ¨νΈνλ€.
    ```
    
- μ°Έκ³  - [https://gist.github.com/godrm/d07ae33973bf71c5324058406dfe42dd](https://www.notion.so/d07ae33973bf71c5324058406dfe42dd)

# π¦ Coding Guidelines for Cocoa - Naming Methods -

- [ ]  [https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html)

General Rules 

- Start the name with a lowercase letter. Donβt use prefixes.
- two specific exceptions: You may begin a method name with a well-known acronym in uppercase (such as TIFF or PDF)), 
and you may use prefixes to group and identify private methods (see Private Methods).
- For methods that represent actions an object takes, start the name with a verb:
    
    Do not use βdoβ or βdoesβ as part of the name because these auxiliary verbs rarely add meaning. Also, never use adverbs or adjectives before the verb.
    
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
