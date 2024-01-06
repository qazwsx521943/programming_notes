
## What's the benefits of using this new concurrency model?

- No more nested completionHandlers -> Big improvement in Readability
- Reduce the risk of thread explosion
- Introduces `Actor`, reducing the race conditions risks.

以之前Stylish登入然後拿使用者資料為例，我們需要依序做三件事，因為每個Task都依賴於前一個Task拿到的資訊：

1. 拿fbToken
2. 拿stylishToken
3. 拿User資料

```swift
// 以下的code都是示意而已，無法在 playground 測試，且不考量error handling
typealias Token = String

struct User {
    let name: String
    let id: String
}

func login() -> User {
    getFBToken { fbToken in
        loginWithFB(fbToken: fbToken) { stylishToken in
            getUserData(token: stylishToken) { user in
                return user
            }
        }
    }
}

func getFBToken(completionHandler: (Token) -> Void) {
    var tokenString: Token
    // get token by facebook sdk
    completionHandler(tokenString)
}

func loginWithFB(fbToken: Token, completionHandler: (Token) -> Void) {
    var stylishToken: Token
    // login stylish by fb token
    completionHandler(stylishToken)
}

func getUserData(token: Token, completionHandler: (User) -> Void) {
    // get user data by stylish token
    completionHandler(User(name: "John", id: "123"))
}
```

如果用async await

```swift
func login() async -> User {
  let fbToken = await getFBToken()
  let stylishToken = await loginWithFB(fbToken: fbToken)
  let user = await getUserData(token: stylishToken)

  return user
}

func getFBToken() async -> Token {
    var tokenString: Token
    // get token by facebook sdk
    return tokenString
}

func loginWithFB(fbToken: Token) -> Token {
    var stylishToken: Token
    // login stylish by fb token
    return stylishToken
}

func getUserData(token: Token) -> User {
    // get user data by stylish token
    return User(name: "John", id: "123")
}
```

很明顯的感受到一樣的功能，使用async await語法可以讓可讀性增加不少，就連沒學過程式的人可能都看得懂。

## Basic syntax

```swift
func fetchData() async throws {
    try await Task.sleep(for: .seconds(2))
    print("successed")
}
```

★ 當程式碼執行到`await`會發生什麼事？

當程式碼執行遇到`await`時，是告訴系統說接下來的任務是異步的(會需要時間執行)，為了不要讓他卡在當前的thread上導致其他任務無法執行，他會暫停當前這個異步任務在當前線程的執行，先讓其他任務在該線程上執行。

等到`await`後面的異步任務執行完後Swift runtime會確保回到原本的thread在適當的時候繼續執行接下來的code。

- 遇到`await`，暫定的是方法，不是thread
- 暫停的前後有可能會切換到不同的 thread 執行

所以這個5.5出來的concurrency model做了什麼？

- 他弱化了開發者對"**Thread**"的概念，讓線程的創建完全由concurrency model做判斷

## What is a `Sendable` Protocol?

★ `Sendable` protocol 是一個`Marker protocol`，他不要求任何實作，不能做conditional cast

```swift
let isSendable = Car.self is Sendable  // error: Marker protocol 'Sendable' cannot be used in a conditional cast.
```

> A type that can be shared from one concurrency domain to another is known as a sendable type.
> You mark a type as being sendable by declaring conformance to the Sendable protocol.

可以在多線程環境下共享的類別，我們就稱他是一個Sendable的類別。

從

## AsyncSequence protocol

