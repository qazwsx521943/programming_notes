---
id: jzt4n14uhf1nf988eby18f7
title: Divider
desc: ''
updated: 1701413806275
created: 1701413716811
---

## ------ Text ------ Effect

```swift
Divider()
  .frame(height: 2)
  .overlay(.gray)
  .padding()
  .overlay {
    Text("Continue with Email")
      .font(.caption)
      .padding(.horizontal, 10)
      .background(.black)
  }
```