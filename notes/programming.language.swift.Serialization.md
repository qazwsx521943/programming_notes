---
id: rxplme0keulo3fon6e4gxmc
title: Serialization
desc: ''
updated: 1703480016107
created: 1703433158198
---

**Foundation** library定義了`Encodable`, `Decodable`, 也提供`Encoder`, `Decoder` API讓我們可以很方便地進行資料處理。

當我們需要更深入的設定時，也可以使用`EncodableWithConfiguration`, `DecodableWithConfiguration`這兩個protocols。

## 為何我們需要將資料encode、decode呢？

當我們透過網路傳送資料、將資料存到硬碟上，通常都需要先將檔案編碼成特定的格式再做傳輸、儲存。

而我們在撰寫程式碼時，會定義出一些類(class、struct)，當我們需要將這些類別的實體拿來做傳輸、儲存時，我們必須要讓這個類`Codable`，才可以透過Decoder、Encoder處理我們的資料。

## `Codable` Behind the scenes

![codable behind the scenes](/../assets/images/programming.language.swift.Serialization_behind-the-scenes.png)

## Example

```swift
struct TestEncode {
    let id: String
    let intArr: [Int] = [1,2,3,4,5]
}

// Instance method 'encode' requires that 'TestEncode' conform to 'Encodable'
let encodedJSONData = try? JSONEncoder().encode(TestEncode(id: "encode&decode"))
let encodedPropertyListData = try? PropertyListEncoder().encode(TestEncode(id: "encode&Decode"))
```

如果自定義的類沒有遵守`Encodable` protocol，encoder的encode方法是沒有辦法接收自定義類的實例的。

這時候只需要讓TestEncode遵守`Encodable`，即可順利encode了

```swift
struct TestEncode: Encodable {
    let id: String
    let intArr: [Int] = [1,2,3,4,5]
}
```

## Foundation codable types

- String
- Int
- Double
- Date
- Data
- URL

## 自定義 `CodingKeys`

當自定義的類遵守`Codable`時，我們可以選擇在類別內宣告一個叫`CodingKeys`的enum，且這個enum需遵守`CodingKey`協議。

### Why?

有時候序列化後的資料格式(key-value pair)的key命名並不符合Swift的命名邏輯，且有時候這個key會頻繁被更改，與其隨著這些外部的資料key去做property的命名，不如就透過宣告`CodingKeys`拿來map這些外部的資料格式。

```swift
let mockJSONData = """
    {
      5"first_name": "Jason",
      "last_name": "Chung"
    }
"""

struct User: Codable {
    let firstName: String
    let lastName: String

    enum CodingKeys: String, CodingKey {
      case firstName = "first_name"
      case lastName = "last_name"
    }
}

let encodedJSONData = try! mockJSONData.data(using: .utf8)!
let decodedJSONData = try JSONDecoder().decode(User.self, from: encodedJSONData)
```

## `Codable`常用的情境：

1. JSON Serialization/Deserialization
2. PropertyList Serialization
3. UserDefaults
4. Unit Testing (透過自定義類建立假資料，轉換成JSON格式測試Decoding出錯情境)