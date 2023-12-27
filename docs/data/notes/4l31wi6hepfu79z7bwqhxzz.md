
## `ObservableObject` Protocol

當我們宣告一個class遵守`ObservableObject` protocol時，我們是在告訴SwiftUI這個class可以被View所觀測。

這個class中如果有property使用`@Published`標記，代表這當這個property變動時，所有引用到這個property的View都要重繪。

## When to use it?

> whenever an object with a property marked `@Published` is changed, all views using that object will be reloaded to reflect those changes.

## Behind the scenes

> the `@Published` property wrapper effectively adds a willSet property observer to items, so that any changes are automatically sent out to observers.