
## What is Protocol?

> A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. The protocol can then be adopted by a class, structure, or enumeration to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to conform to that protocol.

Protocol是功能的藍圖，定義好可以交由Type實現功能。

舉例來說，我們是老闆，我們再請工廠生產我們的產品之前，先給工廠一個產品的requirement，讓我們兩種不同產品，都提供一些相同功能。

ex: 可以旋轉、可以變色等等

這些功能是我們的最低要求，我們請工廠一定在建立產品之前，要符合這些功能給我們產品的藍圖，再去實際生產

## Why do we need Protocols?

## Protocol Requirements

### properties

```swift

```

### functions

- 如果用**mutating**標記function的草稿，在實作class method時不必再寫一次在method前，但struct, enum時需要。

> 為什麼需要mutating標記呢？
>
> protocol 本身並不會定義function，只定義**名字**、**參數**、**回傳值**。 但是有時候還是很難透過這三個東西去表示這個function實作時需要注意的事，像是會不會去更改到該Type的stored properties，但如果用mutating前綴作為標記的話，就可以一目瞭然，知道這個function實作時會更改到stored property。

```swift
enum OnOffSwitch: Togglable {
    case off, on
    // class實作時不需要再標記為mutating
    mutating func toggle() {
        switch self {
        case .off:
            self = .on
        case .on:
            self = .off
        }
    }
}
var lightSwitch = OnOffSwitch.off
lightSwitch.toggle()
```

### initializers

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}
// 實作時要加上`required`，但如果標記為`final`的class，則可以不必
class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // initializer implementation goes here
    }
}
```

## Protocol vs Inheritance

Struct, Enum 是無法被繼承的Type，但我們可以透過讓這些Type遵循某個共同的Protocol去讓Type之間有一些很相似的特性。

## Note：

- Protocol 也是一個類別

> 有時候instance之間並沒有共同superclass，但是有共同的功能，這時候就可以透過protocol的type讓他們有以功能分類的概念。

```swift
let objects: [AnyObject] = [Circle(radius: 10), Sun(radius: 100), Frisbee(radius: 50), Square(width: 10)]

for object in objects {
  if let roundObject = object as? Round {
    print("round object")
  } else {
    print("not round")
  }
}
```

- Protocol 可以其他 Protocol 繼承
- Protocol 可以互相組合
- -@objc protocols can be adopted only by classes, not by structures or enumerations
- Protocol 搭配 extension 做