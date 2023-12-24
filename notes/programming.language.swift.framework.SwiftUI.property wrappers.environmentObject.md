---
id: 0xf3nhksxkl8k16che9xfwg
title: EnvironmentObject
desc: ''
updated: 1703408013456
created: 1703406825357
---

## Why use it?

當我們想要在View Hiearchy中共享某個物件，不想要一層一層傳遞該物件時，我們可以在View的最父層透過`environmentObject()`modifier，讓要使用的子層在View中以`@EnvironmentObject`去取得這個共享的物件。

## What is it?

他跟`@ObservedObject`有點像，都是參照符合`ObservableObject`protocol 的物件，差別在於:

- `@ObservedObject`需要父層傳遞該Observable物件
- `@EnvironmentObject`則是找到在到View階層環境中的Observable物件。
  > SwiftUI會找尋該View階層中與宣告型別一樣的物件。

## Behind the scenes

```mermaid

```

## Example
