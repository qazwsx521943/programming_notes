
```swift
func resize(size: CGSize) -> UIImage? {
  UIGraphicsBeginImageContext(size)
  draw(in: CGRect(x: 0, y: 0, width: size.width, height: size.height))
  let image = UIGraphicGetImageFromCurrentImageContext()
  UIGraphicsEndImageContext()
  return image
}
```