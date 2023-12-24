---
id: 2v31mamk1fjl6km26amg70u
title: property wrappers
desc: ''
updated: 1703406353296
created: 1703321142883
---

> Property Wrapper跟SwiftUI是同一年推出的，導致很多人會誤以為property wrapper也是SwiftUI框架中的一部份，但其實只是一個swift語言 5.1 的新功能。

## What is Property Wrappers?

The `@propertyWrapper` is a Swift attribute used on structs, enums or classes

> Property wrappers 是一個語法糖，告訴compiler說這個類別建立出來的實體有wrapped value這層setter / getter的包裝。

REF: https://www.hackingwithswift.com/quick-start/swiftui/all-swiftui-property-wrappers-explained-and-compared

1. `@AppStorage`: (Owns its data)
   1. A easier way to manage userDefaults in swiftUI.
2. `@Binding`: (Doesn't own its data)
3. `@Environment`:
   1. Reads data from the system. (ex: Color scheme, scenePhase)
4. `@EnvironmentObject`: (Doesn't own its data)
   1. Reads a shared Object that we placed into the environment.
5. `@State`: (Owns its data)
   1. Lets us manipulate small amounts of value type data locally to a view.
6. `@StateObject`: (Owns its data)
   1. Used to store new instances of reference type data that conforms to the `ObservableObject` protocol.
7. `@ObservedObject`: (Doesn't own its data)
   1. Refers to an instance of an external class that conforms to the `ObservableObject` protocol.
8. `@Published`: (Owns its data)
   1. Is attached to properties inside an 1, and tells SwiftUI that it should refresh any views that use this property when it is changed.