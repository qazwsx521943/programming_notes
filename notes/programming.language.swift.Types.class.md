---
id: td8qrahpv2jd2hkgjpre031
title: Class
desc: ''
updated: 1704124882112
created: 1704087722909
---

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

## **Value Type** vs **Reference Type**

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

![value type](/assets/images/programming.language.swift.Types.value-type.png)

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

![reference type](/assets/images/programming.language.swift.Types.reference-type.png)