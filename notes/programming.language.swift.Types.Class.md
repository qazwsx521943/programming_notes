---
id: e967gt61ay4vhqbij2qmofv
title: Class
desc: ''
updated: 1704421591299
created: 1704195508926
nav_order: 1
---

## Declare a class

```swift
class Car {
    var brand: String
    var model: String

    init(brand: String, model: String) {
        self.brand = brand
        self.model = model
    }
}
```

## Initialize a class instance

- stored property with `default value`

  ```swift
  class Car {
      var brand: String = "Tesla"
      var model: String = "Model Y"
  }

  var tesla = Car()
  ```

- set up stored property values in `init` function

  ```swift
  class Car {
      var brand: String
      var model: String

      init(brand: String, model: String) {
          self.brand = brand
          self.model = model
      }
  }

  var testla = Car(brand: "Tesla", model: "Model Y")
  ```

- stored property with `optional value`

    ```swift
    class Car {
        var brand: String?
        var model: String?
    }

    var tesla = Car()
    tesla.brand = "Tesla"
    tesla.model = "Model Y"
    ```

    ```swift
    class Car {
        var brand: String?
        var model: String?

        init(brand: String?, model: String?) {
            self.brand = brand
            self.model = model
        }
    }

    var tesla = Car()
    tesla.brand = "Tesla"
    tesla.model = "Model Y"
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

3. `lazy` property

   使用前要思考一下為什麼要使用`lazy`:

    1. Performance
    2. Prevent retain cycle (lazy property closure default weak captures self)

    ```swift
    class ElectricCar {
        lazy var heavyComputation = {
            var num = 0
            for i in 0..<200 {
                num += 2
            }

            return num
        }()
    }

    let tesla = ElectricCar()

    let testla = tesla.heavyComputation
    ```

    ★ `computed property`, `lazy`不要搞混，`computed property` getter每次會計算後回傳，`lazy`只會在第一次被access時執行。

    ★ `lazy` property is not thread safe!!!

    ★ `lazy`, `computed property` can only be declared using `var`!!!

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

★ 想要繼承父類別的`convenience initializer`，我們需要`override`父類別的所有`designated initializer`

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