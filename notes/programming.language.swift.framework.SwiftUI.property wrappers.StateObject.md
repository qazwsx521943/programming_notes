---
id: e3wts7ybn3i1jj1huiz9rid
title: StateObject
desc: ''
updated: 1703406747121
created: 1703397824582
---

## What is `@StateObject`

> A property wrapper type that instantiates an observable object.

## When to use it?

> Use a state object as the single source of truth for a reference type that you store in a view hierarchy.
>
> when you need to create a reference type inside one of your views and make sure it stays alive for use in that view and others you share it with.

## Behind the scenes

## Injecting `@StateObjects` into views

```swift
struct MyInitializableView: View {
    @StateObject private var model: DataModel
    init(name: String) {
        // SwiftUI ensures that the following initialization uses the
        // closure only once during the lifetime of the view, so
        // later changes to the view's name input have no effect.
        _model = StateObject(wrappedValue: DataModel(name: name))
    }
    var body: some View {
        VStack {
            Text("Name: \(model.name)")
        }
    }
}
```

## Force `@StateObject` property to reinitialize

如果希望View中的reference type在View重繪時重新初始化，可以透過給定id modifier的方式去強迫這個reference type重新初始化（一般來說重繪View時，如果View的id沒有變化，SwiftUI就不會重新初始化這個 reference type property）

## iOS 17.0 introduced `@Observable` macro

- 使用`@Observable` macro 帶來什麼好處？

  1. 寫更少扣：
     1. 只要class前面帶有`@Observable`，裡面宣告的變數都帶有`@Published`特性。
     2. 在View中初始化該class的實體不需要標記為`@StateObject`。
     3. 該class也不需要在定義時標記遵守 `ObservableObject`。