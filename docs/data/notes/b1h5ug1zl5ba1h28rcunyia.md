
## `#available(iOS 17, *)`

檢查OS版本來決定要不要運行code

```swift
if #available(iOS 17, *) {
    print("this code only runs on iOS 17 and up")
} else {
    print("this code only runs on iOS 16 and lower")
}
```

## `@available(iOS 17, *)`

定義class/method時標註可被調用的OS版本

```swift
@available(iOS 17, *)
final class Camera {
    // ..
}
```