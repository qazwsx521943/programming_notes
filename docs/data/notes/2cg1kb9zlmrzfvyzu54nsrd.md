
在swift語言中，用struct跟class宣告的custom type能達到幾乎一樣的功能。

## Definition Syntax

```swift
struct SomeStructure {

}

class SomeClass {

}
```

## Examples

```swift
struct LocationStruct {
    var latitude: CGFloat
    var longitude: CGFloat
}

class LocationClass {
    var latitude: CGFloat
    var longitude: CGFloat

    init(latitude: CGFloat, longitude: CGFloat) {
       self.latitude = latitude
       self.longitude = longitude
    }
}

let locationStruct = LocationStruct(latitude: 120, longitude: 25)

let locationClass = LocationClass(latitude: 120, longitude: 25)

```

## **Pass by value** vs **Pass by reference**

### CoA

```swift
struct PersonStruct {
    var name: String
    var age: Int
}

var firstPerson = PersonStruct(name: "StructPerson", age: 18)
var secondPerson = firstPerson

firstPerson.name = "StructPersonModified"

print(firstPerson.name)
print(secondPerson.name)

getStackAddress(target: &firstPerson)
getStackAddress(target: &secondPerson)
```

### CoW

```swift
var personStructArray = [
    PersonStruct(name: "TOYX", age: 18),
    PersonStruct(name: "TOYY", age: 18),
    PersonStruct(name: "TOYZ", age: 18),
]
var personStructArrayCopy = personStructArray

getStackAddress(target: &personStructArray) == getStackAddress(target: &personStructArrayCopy)

personStructArray[0].name = "TOYA"

getStackAddress(target: &personStructArray) == getStackAddress(target: &personStructArrayCopy)
```

![value type](/../assets/images/programming.language.swift.Types.value-type.png)

Reference

```swift
class PersonClass {
    var name: String
    var age: Int

    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}

var thirdPerson = PersonClass(name: "ClassPerson", age: 20)
var fourthPerson = thirdPerson

thirdPerson.name = "ClassPersonModified"

print(thirdPerson.name)
print(fourthPerson.name)
print(thirdPerson === fourthPersonec)
```

![reference type](/../assets/images/programming.language.swift.Types.reference-type.png)

## Both can:

- 定義store properties, computed properties
- 定義methods
- 用extension來擴展他們的功能
- 定義subscripts，讓我們可以透過`[]`的方式取得值

## Class 特性:

- 繼承
- 生命週期由ARC管理
- Dynamic dispatch(type casting)

    ```swift
    class Animal {
        func eat() {
            print("animals can eat")
        }
    }

    class Dog: Animal {
        override func eat() {
            print("dogs can eat")
        }

        func onlyDog() {
            print("only dog can call")
        }
    }

    let animal: Animal = Dog()

    animal.eat()
    if let dog = animal as? Dog {
      dog.onlyDog()
    }
    ```

## What apple says

> The additional capabilities that classes support come at the cost of increased complexity. As a general guideline, prefer structures because they’re easier to reason about, and use classes when they’re appropriate or necessary. In practice, this means most of the custom types you define will be structures and enumerations.

如果我們沒有要使用到class比struct多擁有的特性，我們在開發的時候建議都以struct、enum建立Type，因為他們比較好local reason，可以確保程式碼的可讀性、維護性。

### Heap Summary

- More dynamic but less efficient than stack.
- Goes through 3 steps:
  1. allocation
  2. tracking reference count
  3. deallocation
- Heap memory allocation is done for objects whose size can not be calculated at compile time, plus all reference types(because reference types life time is not based on their defined scope).
- heap memory is somehow a global host for objects and all threads can have access to it, so the objects stored on it are not thread-safe

### Stack Summary

- value types are all stored on the stack memory. Note that the value types should not have any reference types associated with them(i.e. they are either not contained by or contain a reference type) otherwise they wouldn’t be stored on the stack.
- The amount of memory needed is normally calculated at compile time since they are not dynamic and do not need reference count semantics to decide how long they have to live(They live in a scope and when they are used the memory will dump them)

## Situations when choosing different data types

1. Structs:
   1. Data Models
   2. SwiftUI Views

2. Classes:
   1. ViewModels

3. Actors:
   1. Shared 'Manager' / 'Data Store'

## Comparison

types | Struct | Class | Actor | Enum
---------|:----------:|:---------:|:--------:|:------:|
 Stored in | Stack | Heap | Heap | Stack
 copy by | Value | Reference | Reference | Value
 Thread safe | V | X | V | V
 Inheritance | X | V | X | X

References:
- https://www.backblaze.com/blog/whats-the-diff-programs-processes-and-threads/