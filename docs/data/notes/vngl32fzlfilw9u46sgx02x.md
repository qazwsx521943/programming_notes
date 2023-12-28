
## computed property設計

```swift
extension String {
    var firstLetter: Character {
        get {
            return self.count > 0 ? self[self.startIndex] : " "
        }

        set {
            if self.count > 0 {
                self.replaceSubrange(self.startIndex...self.startIndex, with: String(newValue))
            } else {
                self.append(newValue)
            }
        }
    }
}
```