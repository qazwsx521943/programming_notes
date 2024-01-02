---
id: m30ay8bv1fkxsl30m2r9frl
title: Enumeration
desc: ''
updated: 1704208895807
created: 1703951656006
---

## What is enum?

是一個列舉型別，可以用來建立**有限**且**相關**的值集合

## Declaration

```swift
enum Direction: String {
    case north = "N"
    case south = "S"
    case west = "W"
    case east = "E"
}
```

## 用enum的優勢

- Type Safety → compile-time check

- Readability & Maintainability: 減少其他人看code需要的通靈技能

    ```swift
    enum Animal {
        case mammal(name: String, legs: Int)
        case bird(name: String, wingspan: Double)
        case fish(name: String, color: String)
    }

    let lion = Animal.mammal(name: "Lion", legs: 4)
    let eagle = Animal.bird(name: "Eagle", wingspan: 2.5)
    let goldfish = Animal.fish(name: "Goldfish", color: "Gold")
    ```

- Extensibility：可以輕鬆擴展 / 修改 case value

  ```swift
  enum Animal {
        case mammal(name: String, legs: Int)
        case bird(name: String, wingspan: Double)
        case fish(name: String, color: String)
        case reptile(name: String, scales: Bool)  // Add reptile type
    }
  ```

## Use Cases

1. 表示狀態

    ```swift
    enum OrderStatus {
        case pending
        case processing
        case shipped
        case delivered
        case cancelled
    }
    ```

2. 搭配`associated value`當作容器：

    ```swift
    // 1. 定義 Message 容器
    enum Message: Codable {
        enum MediaType: String, Codable {
            case image
            case video
            case audio
        }

        case text(payload: String)
        case media(type: MediaType, payload: Data)
    }

    // 2. 建立 Message instance
    let message: Message = .text(payload: "Hello")
    let imageMessage: Message = .media(type: .image, payload: Data())

    // 3. 取出 Message 容器內的 data
    convertMessage(message)
    convertMessage(imageMessage)

    func convertMessage(_ message: Message) {
        switch message {
        case .text(let text):
            print("this is a text message: \(text)")
        case let .media(media, payload):
            print("this is \(media) message with Data: \(payload)")
        }
    }
    ```