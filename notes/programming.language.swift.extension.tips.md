---
id: vngl32fzlfilw9u46sgx02x
title: Tips
desc: ''
updated: 1703686299503
created: 1703684868712
---

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