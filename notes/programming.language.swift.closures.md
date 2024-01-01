---
id: 41n9yjlpdhmbls1eigctl1v
title: Closures
desc: ''
updated: 1703863190534
created: 1703772642942
---

## Capture list 情境

### 捕獲的變數為 value type

```swift
/// closure capture value at define
var captureValue = 20

let captureFunction = { [captureValue] in
    print("captured value: \(captureValue)")
}

captureValue = 100
captureFunction() // 20
```

⇧ 的code跟 ↓ 是等價的，只是用capture list更簡潔。

```swift
var captureValue = 20
var capturedClosureValue = captureValue

let captureFunction = {
    print("captured value: \(capturedClosureValue)")
}

captureValue = 100
captureFunction() // 20
```

```swift
/// closure capture value at execution
var captureValue = 20

let captureFunction = {
    print("captured value: \(captureValue)")
}

captureValue = 100
captureFunction() // 20
```