
## What can we do with Extension?

Add functionality for class, struct, enum.

- 透過extension，我們可以擴展非open source的程式碼。

## Why can't we add stored properties into Extension?

- 影響程式碼的可維護性、可讀性
- 記憶體存放考量，stored properties是需要額外分配記憶體空間去儲存的。
- Swift是安全、強型別語言，如果允許在Extension中聲明stored properties，會需要在compile的時候引入type check, type inference，增加複雜度。

如果有想要在某Type的Extension中宣告stored properties的衝動，你或許該考慮繼承該Type。

## Why can we add static properties inside a type's Extension?

當我們在類別的宣告中聲明一個static property，這個static property就是一個放在該類別namespace底下的global property。

```swift
class Person {
    let name: String

    let age: Int

}

extension Person {
    static let john = Person(name: "John", age: 18)
}

// 想在程式碼任何地方取用時
Person.john
```