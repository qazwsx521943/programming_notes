
在swift語言中，用struct跟class宣告的Type能達到幾乎一樣的功能。

## Both can:

- 定義store properties, computed properties
- 定義methods
- 用extension來擴展他們的功能
- 定義subscripts，讓我們可以透過`[]`的方式取得值

## Classes additional capabilities:

- 繼承其他class
- Type casting （因為他有繼承的功能，所以可以將某個Instance在runtime的時候轉型成parent class type）

    ```swift

    class Person {
        let name: String

        init(name: String) {
            self.name = name
        }
    }

    class Employee: Person {
      let id: String

      init(id: String, name: String) {
        self.id = id
        super.init(name: name)
      }
    }

    let john = Employee(id: "123", name: "John") as! Person
    ```

- Reference counting
- 透過 Deinitializer 釋放內存空間

## What apple says

> The additional capabilities that classes support come at the cost of increased complexity. As a general guideline, prefer structures because they’re easier to reason about, and use classes when they’re appropriate or necessary. In practice, this means most of the custom types you define will be structures and enumerations.

如果我們沒有要使用到class比struct多擁有的特性，我們在開發的時候建議都以struct、enum建立Type，因為他們比較好local reason，可以確保程式碼的可讀性、維護性。

## Value vs Reference types

types | Struct | Class | Actor | Function
---------|:----------:|:---------:|:--------:|:------:|
 type | Value | Reference | Reference | Reference
 Stored in | Stack | Heap | Heap | Heap
 Speed | Faster | Slower | Slower
 Thread safe | V | X | V | X
 Inheritance | X | V | X | X

## Stack vs Heap

Value based data types are stored in stack

Reference based data types are stored in heap

## Automatic Reference Counting (ARC)

value types such as structures and enumerations are just copying the data to data. Therefore it's not affected by ARC.

ARC is used to track and manage the app's memory usage. When class instances are no longer needed, ARC automatically frees up the memory used by that class.

這也是為什麼esacaping closure通常會需要標註`weak self`的原因，以免當某個class不需要被使用時，卻因為某個closure中有引用到strong self而無法正常釋放記憶體。

## Situations when choosing different data types

1. Structs:
   1. Data Models
   2. SwiftUI Views

2. Classes:
   1. ViewModels

3. Actors:
   1. Shared 'Manager' / 'Data Store'

References:
- https://www.backblaze.com/blog/whats-the-diff-programs-processes-and-threads/