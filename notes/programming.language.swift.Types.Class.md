---
id: f0rt95n87dvpuuf5ktobguw
title: Class
desc: ''
updated: 1704203683171
created: 1704195508926
---

## Declare a class

```swift
class Size {
    var width: Int = 10
    var height: Int = 10
}
Size()
```

## Initialize a class instance

- stored property with `default value`

  ```swift
  class Size {
      var width: Int = 10
      var height: Int = 10
  }

  var size = Size()
  ```

- set up stored property values in `init` function

  ```swift
  class Size {
      var width: Int
      var height: Int

      init() {
        width = 10
        height = 10
      }

      init(width: Int, height: Int) {
        self.width = width
        self.height = height
      }
  }

  var size1 = Size(width: 10, height: 10)
  var size2 = Size()
  ```

- stored property with `optional value`

    ```swift
  class Size {
      var width: Int?
      var height: Int?
  }

  var size = Size()
  size.width = 10
  size.height = 10
  ```

- failable initializer

  ```swift
  class Size {
      var width: Int
      var height: Int

      init() {
          width = 10
          height = 10
      }

      init?(width: Int, height: Int) {
          self.width = width
          self.height = height

          if width <= 0 || height <= 0 {
            return nil
          }
      }
  }

  var size1 = Size(width: -10, height: 10) // nil
  var size2 = Size(width: 10, height: 10) // { width: 10, height: 10 }
  ```

## Property

1. `stored property`

    ```swift
    class Size {
        var width: Int = 10
        var height: Int = 10
        var x: Int = 0
        var y: Int = 0
    }
    ```

2. `computed property`
   1. getter(must) / setter(optional)

    ```swift
    class Size {
        var width: Int = 10
        var height: Int = 10
        var x: Int = 0
        var y: Int = 0

        var centerX: Int {
            get {
                return x + width / 2
            }
            set {
                x = newValue - width / 2
            }
        }

        var centerY: Int {
            get {
                return y + height / 2
            }
            set {
                y = newValue - height / 2
            }
        }
    }
    ```

3. property observer

    ```swift
    class Size {
        var width: Int = 10 {
            willSet {
                newValue <= 0 {
                    // do something
                }
            }
            didSet {
                if width <= 0 {
                    width = oldValue
                }
            }
        }

        var height: Int = 10
        var x: Int = 0
        var y: Int = 0
    }
    ```

4. `lazy` delay initialization

## method

- 沒有`input`但有`return value` → 可以考慮用computed property 讓 code更簡潔

    ```swift
    class Size {
        var width: Int = 10
        var height: Int = 10
        var x: Int = 0
        var y: Int = 0

        func description() -> String {
            return "\(width) x \(height)"
        }
    }

    class Size {
        var width: Int = 10
        var height: Int = 10
        var x: Int = 0
        var y: Int = 0

        var description: String {
            "\(width) x \(height)"
        }
    }
    ```

    ```swift
    class Size {
        var width: Int = 10
        var height: Int = 10
        var x: Int = 0
        var y: Int = 0

        func adjustSize(newWidth: Int, newHeight: Int) {
            width = newWidth
            height = newHeight
            print("Size adjusted to \(width) x \(height)")
        }
    }

    let mySize = Size()

    mySize.adustSize(newWidth: 20, newHeight: 20)
    ```

## Inheritance

```swift
class Shape {
    var color: String

    init(color: String) {
        self.color = color
    }

    func draw() {
        print("Drawing a shape in \(color) color.")
    }
}

class Circle: Shape {
    var radius: Double

    init(color: String, radius: Double) {
        self.radius = radius
        super.init(color: color)
    }

    override func draw() {
        print("Drawing a circle with radius \(radius) in \(color) color.")
    }
}

class Square: Shape {
    var width: Double
    var height: Double

    init(color: String, width: Double, height: Double) {
        self.width = width
        self.height = height
        super.init(color: color)
    }

    override func draw() {
        print("Drawing a rectangle with width \(width) and height \(height) in \(color) color.")
    }
}

let redCircle = Circle(color: "Red", radius: 5.0)
let blueSquare = Square(color: "Blue", width: 8.0, height: 6.0)

redCircle.draw()
blueSquare.draw()

// dynamic dispatch
var circleShape: Shape = Circle(color: "Red", radius: 5.0)
circleShape.draw()
```

## designated / convenience initializer

Principle:

1. A designated initializer must call a designated initializer from its immediate superclass.
2. A convenience initializer must call another initializer from the same class.
3. A convenience initializer must ultimately call a designated initializer.

Remember:

- Designated initializers must always delegate up.

- Convenience initializers must always delegate across.

```swift
class Car {
    var brand: String
    var model: String
    // designated initializer
    init(brand: String, model: String) {
        self.brand = brand
        self.model = model
    }
}

class ElectricCar: Car {
    var batteryCapacity: Double

    // Designated initializer, must call parent designated initializer
    init(brand: String, model: String, batteryCapacity: Double) {
        self.batteryCapacity = batteryCapacity
        super.init(brand: brand, model: model)
    }

    // Convenience initializer, must call current class defined designated initializer
    convenience override init(brand: String, model: String) {
        // default battery capacity set to 60
        self.init(brand: brand, model: model, batteryCapacity: 60.0)
    }
}

let teslaModel3 = ElectricCar(brand: "Tesla", model: "Model 3", batteryCapacity: 75.0)
let nissanLeaf = ElectricCar(brand: "Nissan", model: "Leaf")
```

- concept:

1. 想要父類別的`convenience initializer`，我們需要`override`父類別的所有`designated initializer`