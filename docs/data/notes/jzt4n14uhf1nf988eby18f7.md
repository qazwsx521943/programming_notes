
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