---
id: bbsq3i60b9ihroumbe0928d
title: ARC
desc: ''
updated: 1703917016257
created: 1703772605114
---

## What is ARC (Automatic Reference Counting)?

> Swift uses Automatic Reference Counting (ARC) to track and manage your app’s memory usage. In most cases, this means that memory management “just works” in Swift, and you don’t need to think about memory management yourself. ARC automatically frees up the memory used by class instances when those instances are no longer needed.

從官方文件可以得知，他是一個跟物件(class)生命週期有關的自動化機制。

## How ARC Works in Example

```swift
class Person {
    let name: String

    init(name: String) {
        self.name = name
        print("\(name) is initialized")
    }

    deinit {
        print("\(self.name) is deinitialized")
    }
}

var reference1: Person?
var reference2: Person?
var reference3: Person?

// Jason person object reference count = 0
reference1 = Person(name: "Jason") // count = 1
reference2 = reference1 // count = 2
reference3 = reference1 // count = 3

reference2 = nil // count = 2
reference3 = nil // count = 1
//reference1 = nil
// comment out this line of code so that count = 0.
// ARC deallocates this Jason person object's memory,
// since there are no more strong reference
```

## Strong reference Example

![strong ref establish](/../assets/images/programming.language.swift.ARC_strong-ref_1.png)

![remove variable references](/../assets/images/programming.language.swift.ARC_strong-ref_2.png)

可以發現儘管`john`跟`ApartmentA`都沒有再引用到Person、Apartment實例，但是因為這兩個實例本身互相引用了，導致Reference Count沒有辦法歸零並釋放記憶體，進一步造成memory leak。

## How to prevent reference cycle?

> Swift provides two ways to solve reference cycles when working with a class type: weak references, and unowned references .

評估物件的生命週期適合用`weak` 還是 `unowned`

```swift

// 使用weak原因：StorageManager實例調用saveImage時，
// 我們不確定delegate的物件是否已經deallocated, 所以使用weak。
// 如果很確定delegate的物件不會被deallocated，我們可以使用unowned
class StorageManager {
    weak var delegate: StorageManagerDelegate?

    public func saveImage() {
        var imageData: Data
        // fetch image...


        self.delegate?.didReceiveImage(self, imageData)
    }
}

```

## Closure也是Reference Type，所以也會有 reference cycle

> 透過定義 capture list 來明確指定外部變數的捕獲方式。