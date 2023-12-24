---
id: rc44em7kcfzkyprtzidrtjs
title: Data Structure
desc: ''
updated: 1702897389759
created: 1699313135493
---


## What does conforming to **Codable** do?

```swift
struct Landmark: Codable {
    var name: String
    var foundingYear: Int

    // Landmark now supports the Codable methods init(from:) and encode(to:),
    // even though they aren't written as part of its declaration.
}
```

遵循Codable協議就可以在編碼或解碼資料時，將資料轉成swift的資料格式

## 搭配 **CodingKeys** 做 encode, decode

如果api回傳的資料(encoded的格式)跟我們的struct定義不一致，我們可以提供自定義的encode, decode邏輯。

## JSONSerialization

> An object that converts between JSON and the equivalent Foundation objects.

## `Decodable` vs `Decoder` Protocol

`Decodable`是一個Protocol，並要求遵守的類必須提供一個`init()`，而這個`init`函式的唯一input是一個`Decoder`類

```swift
/// A type that can decode itself from an external representation.
public protocol Decodable {

    /// Creates a new instance by decoding from the given decoder.
    ///
    /// This initializer throws an error if reading from the decoder fails, or
    /// if the data read is corrupted or otherwise invalid.
    ///
    /// - Parameter decoder: The decoder to read data from.
    init(from decoder: Decoder) throws
}
```

`Decoder`也是一個Protocol，我們使用遵守他的類來解碼payload。

範例：

當我們自定義Decoder的初始化函式時，我們透過`Decoder`實體的`container()` func拿到一個`KeyedDecodingContainer`的實體，並利用這個`container`的`decode()`來解碼payload。

Ｑ： CodingKeys 扮演什麼角色？

Ａ： Decoder呼叫container時需要傳入CodingKeys這個類，之後就可以透過container的`decode`去取值。

```swift
struct Person: Decodable {
    let name: String
    let age: Int
    let gender: String

    enum CodingKeys: CodingKey {
        case name
        case age
        case gender
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        self.name = try container.decode(String.self, forKey: .name)
        self.age = try container.decode(Int.self, forKey: .age)
        self.gender = try container.decode(Gender.self, forKey: .gender).rawValue
    }
}

enum Gender: String, Decodable {
    case male
    case female
}
```