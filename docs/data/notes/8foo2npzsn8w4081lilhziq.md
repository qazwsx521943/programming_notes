
## What is `@State`?

> A property wrapper type that can read and write a value managed by SwiftUI.

## When to use it?

> Use state as the single source of truth for a given value type that you store in a view hierarchy. Create a state value in an ``App``, ``Scene``, or ``View`` by applying the `@State` attribute to a property declaration and providing an initial value

## Behind the scene

> SwiftUI manages the property's storage. When the value changes, SwiftUI updates the parts of the view hierarchy that depend on the value. To access a state's underlying value, you use its `wrappedValue` property. However, as a shortcut Swift enables you to access the wrapped value by referring directly to the state instance.

SwiftUI 中每個View都是以struct定義的，由於struct中的property是immutable，如果我們有一個variable是會改變的，而每次改變時我們希望View會重繪，那這時候我們就可以用`@State`這個SwiftUI定義好的property wrapper。當我們更改這個以`@State`定義的variable，SwiftUI就會幫我們更新View。

在struct中宣告變數加上`@State`前綴，SwiftUI會將這個變數管理在一個shared storage(Redux like)，讓我們在View的重繪時不會遺失這個變數。

從SwiftUI documentation可以看到`@State`定義的一些public property：

```swift
@propertyWrapper public struct State<Value>: DynamicProperty {
    public init(wrappedValue value: Value)

    public init(initialValue value: Value)

    public var wrappedValue: Value { get nonmutating set }

    public var projectedValue: Binding<Value> { get }
}

public protocol DynamicProperty {

    /// Updates the underlying value of the stored value.
    ///
    /// SwiftUI calls this function before rendering a view's
    /// ``View/body-swift.property`` to ensure the view has the most recent
    /// value.
    mutating func update()
}
```