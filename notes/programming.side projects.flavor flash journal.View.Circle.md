---
id: 65euco42r84gg5c3ch3fz7j
title: Shape
desc: ''
updated: 1701245131381
created: 1701238872908
---


## 畫邊框

```swift
Circle()
  .stroke(
    Color.green,
    style: StrokeStyle(lineWidth: 10, lineCap: .square, dash: [40])
  )
```

![Circle stroke](/assets/images/project%20brainstorming.flavor%20flash%20journal.View.CircleStroker.png)

```swift
struct PathView: View {
    var body: some View {
  ZStack {
   Path { path in
    path.move(to: CGPoint(x: 20, y: 100))
    path.addLine(to: CGPoint(x: 370, y: 100))
   }
   .stroke(style: StrokeStyle(lineWidth: 30, lineCap: .square))
  }
    }
}
```

![linecap](/assets/images/project%20brainstorming.flavor%20flash%20journal.View.stroke_linecap.png)

```swift
  GeometryReader { geometry in
   ZStack {
    Path { path in
     path.move(to: CGPoint(x: geometry.size.width / 2, y: 100))
     path.addLine(to: CGPoint(x: geometry.size.width / 2 - 50, y: 200))
     path.addLine(to: CGPoint(x: geometry.size.width / 2 + 50, y: 200))
    }
    .stroke(style: StrokeStyle(lineWidth: 30, lineCap: .square, lineJoin: .round))
   }
  }
```

![lineJoin](/assets/images/project%20brainstorming.flavor%20flash%20journal.View.lineJoin.png)

`strokeStyle` init parameters:

1. lineCap: 控制線條尾端的形狀
   1. round 圓尾
   2. square、butt 方形尾
      > 如果想要嚴格控制物件的大小 => butt，因為square的方形尾會超出起點＆終點

2. lineJoin: 控制線條的連接style
   1. round、milter、bevel
3. dash: 控制虛線樣式&間隔的長度

### 搭配`trim`做一個Loading的動畫

```swift
import SwiftUI
import Combine

struct CircleView: View {

 @State var progress: CGFloat = 0
 let timer = Timer.publish(every: 1, on: .main, in: .common).autoconnect()

    var body: some View {
      Circle()
        .trim(from: 0.0, to: progress)
        .stroke(Color.green, lineWidth: 20)
        .frame(width: 200, height: 200)
        .onReceive(timer, perform: { _ in
          addProgress()
   })
 }

 func addProgress() {
  if progress >= 1.0 {
   progress = 0.1
  } else {
   progress += 0.1
  }
 }
}
```

Without using Combine ↓

```swift
struct CircleView: View {

 @State var progress: CGFloat = 0

    var body: some View {
      Circle()
        .trim(from: 0.0, to: progress)
        .stroke(Color.green, lineWidth: 20)
        .frame(width: 200, height: 200)
        .onAppear {
          let setTimer = Timer.scheduledTimer(withTimeInterval: 1, repeats: true) { timer in
            addProgress()
          }
        }
 }

 func addProgress() {
  if progress >= 1.0 {
   progress = 0.1
  } else {
   progress += 0.1
  }
 }
}
```
