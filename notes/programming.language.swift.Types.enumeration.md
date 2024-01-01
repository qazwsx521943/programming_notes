---
id: m30ay8bv1fkxsl30m2r9frl
title: Enumeration
desc: ''
updated: 1703996576253
created: 1703951656006
---

## enum or struct?

### 可枚舉但不需封裝:

```swift
enum LoginType: String {
    case nativeLogin = "native login"
    case fbLogin = "facebook login"
    case appleLogin = "apple login"
    case gitHubLogin = "gitHub login"
}

struct LoginTypeStruct {
    static let appleLogin = "apple login"
    static let fbLogin = "facebook login"
    static let githubLogin = "gitHub login"
    static let nativeLogin = "native login"
}
```

in this case, it's better to use `enum`.

- type safety
- readibility

### 可枚舉且需封裝物件資料:

```swift
struct LoginTypeStruct {
    let name: String
    let description: String
    var isEnabled: Bool // Example of additional data

    static let appleLogin = LoginTypeStruct(name: "Apple Login", description: "Login using Apple ID", isEnabled: true)
    static let fbLogin = LoginTypeStruct(name: "Facebook Login", description: "Login using Facebook", isEnabled: false)
    static let githubLogin = LoginTypeStruct(name: "GitHub Login", description: "Login using GitHub", isEnabled: true)
    static let nativeLogin = LoginTypeStruct(name: "Native Login", description: "Login using our native authentication", isEnabled: true)
}

enum LoginTypeEnum {
    case appleLogin(isEnabled: Bool)
    case fbLogin(isEnabled: Bool)
    case githubLogin(isEnabled: Bool)
    case nativeLogin(isEnabled: Bool)

    var name: String {
        switch self {
            case .appleLogin: return "Apple Login"
            case .fbLogin: return "Facebook Login"
            case .githubLogin: return "GitHub Login"
            case .nativeLogin: return "Native Login"
        }
    }

    var description: String {
        switch self {
            case .appleLogin: return "Login using Apple ID"
            case .fbLogin: return "Login using Facebook"
            case .githubLogin: return "Login using GitHub"
            case .nativeLogin: return "Login using our native authentication"
        }
    }

    var isEnabled: Bool {
        switch self {
        case let .appleLogin(isEnabled),
            let .fbLogin(isEnabled),
            let .githubLogin(isEnabled),
            let .nativeLogin(isEnabled):
            return isEnabled
        }
    }
}
```

## Use Cases

1. Error Handling
    ```swift
    ```
2.