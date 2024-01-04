---
id: mr2culexmtziuklrq2gt22f
title: Types
desc: ''
updated: 1704388122468
created: 1703951617977
nav_order: 1
---

## Usage Overview

- class, struct

> Model custom types that encapsulate data.

- enum

> Model custom types that define a list of possible values.

## Main Difference between `Struct` & `Class`

&nbsp | Struct | Class
---------|:---------:-|:--------:-
 pass by | value | reference
 method dispatch | static | dynamic
 inheritance | X | V

### Value Type , Reference Type

在記憶體中的儲存和管理方式不同

#### value type:

- 通常較小，且輕量，因為儲存的是實際數據的`值`，可以直接存在`Stack`上
- pass by value（CoW）

![value type](/assets/images/programming.language.swift.Types.value-type.png)

#### reference type:

- 可能較大，因為儲存的是對實際數據的`引用`，可能會包含其他資訊(類型訊息、ARC相關訊息)
- pass by reference

![reference type](/assets/images/programming.language.swift.Types.reference-type.png)

### Static Dispatch vs Dynamic Dispatch

#### Method dispatch

&nbsp | initial Declaration | Extension
---------| :----------: |:---------:
 Value Type(struct, enum) | Static | Static
 Protocol | Table | Static
 Class | Table | Static
 NSObject Subclass | Table | Message

- Dynamic dispatch example

    ```swift
    protocol Dispatch {
        func testDispatch()
    }

    extension Dispatch {
        func shared() {
            print("\(#function) from protocol extension")
        }
    }

    class A: Dispatch {
      func testDispatch() {
        print("\(#function) from A")
      }
    }

    class B: A {
        override func testDispatch() {
            print("\(#function) from B")
        }

        func shared() {
            print("\(#function) from B")
        }
    }

    let a: A = B()

    a.testDispatch()
    a.shared()
    ```

```swift
// 1. Value Type (Struct): All Static Dispatch
struct Person {
    func isHungry() -> Bool { } // Static
}
extension Person {
    func sayHello() -> String { } // Static
}

// 2. Protocol: Table & Static
protocol Animal {
    func isCute() -> Bool { } // Table
}
extension Animal {
    func canGetAngry() -> Bool { } // Static
}

// 3. Class
class Dog: Animal {
    func isCute() -> Bool { } // Table
    // add @objc & dynamic keyword
    @objc dynamic func hoursSleep() -> Int { } // Table -> Message
}
extension Dog {
    func canBite() -> Bool { } // Static
    // add @objc keyword
    @objc func goWild() { } // Static -> Message
}
// add final keyword
final class Employee {
    func canCode() -> Bool { } // Table -> Static
}
```

## Which to choose, `Struct` or `Class`?

> The additional capabilities that classes support come at the cost of increased complexity. As a general guideline, prefer structures because they’re easier to reason about, and use classes when they’re appropriate or necessary. In practice, this means most of the custom types you define will be structures and enumerations.

- Use Classes When You Need Objective-C Interoperability
- Use Classes When You Need to Control Identity

Always keep [[programming.language.swift.performance]] principle in mind

## Situations when choosing different data types

1. Structs:
   1. Data Models
   2. SwiftUI Views

2. Classes:
   1. ViewModels
   2. KVO

3. Actors:
   1. Shared 'Manager' / 'Data Store'

## Comparison

&nbsp | Struct | Class | Actor | Enum
---------|:----------:|:---------:|:--------:|:------:|
 Stored in | Stack | Heap | Heap | Stack
 copy by | Value | Reference | Reference | Value
 Thread safe | 不一定 | X | V | V
 Inheritance | X | V | X | X

References:

- https://www.backblaze.com/blog/whats-the-diff-programs-processes-and-threads/
