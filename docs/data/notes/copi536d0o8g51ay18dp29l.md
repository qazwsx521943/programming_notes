
適當的使用泛型可以確保我們的程式碼可讀性、重用性。

```swift
let intArray = [1, 2, 2, 3, 3, 3]
let stringArray = ["one", "two", "two", "three", "three", "three"]

func count<T: Hashable>(of element: T, in array: [T]) -> Int? {
    let mappedItems = array.map { ($0, 1) }
    let counts = Dictionary(mappedItems, uniquingKeysWith: +)
    return counts[element]
}

count(of: 2, in: intArray)
count(of: "two", in: stringArray)
```