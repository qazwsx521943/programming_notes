---
id: 2cg1kb9zlmrzfvyzu54nsrd
title: Struct
desc: ''
updated: 1704387824824
created: 1703728237903
nav_order: 2
---

## Definition Syntax

```swift
struct Car {
    var brand: String
    var model: String

    // structures have an automatically generated memberwise initializer
}
```

## **Pass by value**

### CoA （Copy-on-Assign）

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

getStackAddress(target: &firstPerson) == getStackAddress(target: &secondPerson)
```

### CoW （Copy-on-Write）

```swift
var personStructArray = [
    PersonStruct(name: "TOYX", age: 18),
    PersonStruct(name: "TOYY", age: 18),
    PersonStruct(name: "TOYZ", age: 18),
]
var personStructArrayCopy = personStructArray

getStackAddress(target: &personStructArray) == getStackAddress(target: &personStructArrayCopy)

personStructArrayCopy[0].name = "TOYA"

getStackAddress(target: &personStructArray) == getStackAddress(target: &personStructArrayCopy)
```

![value type](/assets/images/programming.language.swift.Types.value-type.png)

