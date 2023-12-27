
## Why use it?

有時候我們會需要取得一些裝置、系統設定的資訊如：

- colorScheme 當前系統 深 / 淺 色
- managedObjectContext CoreData的viewContext

透過`@Environment`我們可以很輕鬆取得這些資訊

## What is it?

他是一個可以用來取得一些SwiftUI預設宣告好的物件。

## Difference between `@EnvironmentObject`

- `@EnvironmentObject` allows us to inject arbitrary values into the environment.

- `@Environment` is specifically there to work with SwiftUI’s own pre-defined keys.

## Behind the scenes