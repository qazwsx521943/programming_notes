---
id: rokc7843iqin5f5kfi9sew8
title: UIImage
desc: ''
updated: 1699581897116
created: 1699581784876
---

```swift
func resize(size: CGSize) -> UIImage? {
  UIGraphicsBeginImageContext(size)
  draw(in: CGRect(x: 0, y: 0, width: size.width, height: size.height))
  let image = UIGraphicGetImageFromCurrentImageContext()
  UIGraphicsEndImageContext()
  return image
}
```