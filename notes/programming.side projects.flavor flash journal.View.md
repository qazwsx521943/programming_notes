---
id: pe6vepkpwtgibepjb75nfmg
title: View
desc: ''
updated: 1701400109005
created: 1701136515001
---

## Container Views

### VStack -> Vertical

### HStack -> Horizontal

### ZStack -> zIndex (back to front)

> VStack, HStack has default spacing.

## Common Modifiers

### `.frame()`

```swift
Text("Hello, World!")
  .background(.blue)
  .frame(width: 300,height: 200, alignment: .bottom)
  .background(.red)
```

view before the `.frame()` modifier will be the content of the frame.

### `.background()`是可以不斷疊加上去的

### `.overlay`會在background之上

> background, overlay 設定的alignment是對齊Self的frame.

## `background` vs `ZStack`

當堆疊很多物件時，使用`ZStack`，單純的情況就盡量使用`background`

## 善用`Spacer()`推 Stack 裡面的laout

## How to use `enum` and `init` in swiftUI reusable views

## ScrollView 小技巧

> 可以nest，如果想要做瀑布流可以考慮

```
ScrollView -> VStack -> **ForEach** -> ScrollView -> HStack -> **ForEach** -> RoundedRectangle
```

如果資料比較多，我們應該要使用LazyVStack, LazyHStack

## Conditional Rendering

1. if else statements ( `||`, `&&` )
2. ternary operators


## Action sheet, Alert view 都是可以封裝成viewBuilder的