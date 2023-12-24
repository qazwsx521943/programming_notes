---
id: itzjg71gb2k0xaj8p8a0a9b
title: Extensions
desc: ''
updated: 1701336450003
created: 1701336373819
---

## `Color`

```swift
extension Color {
  static func generateRandomColor() -> Self {
  enum RColor: Int {
    case red = 0, green = 1, blue = 2, yellow = 3

    var color: Color {
      switch self {
      case .blue: return Color.blue
      case .green: return Color.green
      case .yellow: return Color.yellow
      case .red: return Color.red
      }
    }
  }

  let random = Int.random(in: 0...3)

  return RColor(rawValue: random)!.color
  }
}
```
