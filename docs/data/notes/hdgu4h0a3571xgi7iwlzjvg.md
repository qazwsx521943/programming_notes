
## `.navigationTitle(_ title: String)` 是怎麼運作的？

通常在child view中要改變parent view的值我們會使用Binding的方式，但是像navigationTitle這種modifier我們並沒有做綁定，為什麼還是可以在任何child view中改變parent view的navigationTitle呢？

當某個View透過modifier設定navigationTitle時，實際上是透過PreferenceKey去改變最父層的navigationTitle

```swift
struct CustomPreferenceKey: PreferenceKey {
  static var defaultValue: String = ""

  static func reduce(value: inout String, nextValue: () -> String) {
    value = nextValue()
  }
}
```

- Steps:
  1. 建立一個遵從`PreferenceKey`的struct
  2. Parent View 用`onPreferenceChange`modifier監聽
  3. Child View用`preference(key: RectGeoPreferenceKey.self, value: size)`改變自定義的struct

## 使用情境

1. 監聽`scrollView`中某個View的offset