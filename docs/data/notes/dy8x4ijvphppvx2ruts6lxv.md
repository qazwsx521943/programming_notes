
## Download image

### Old way (escaping closure)

URLSession + escaping completionHandler closure

```swift
func responseHandler(_ data: Data?,_ response: URLResponse?) -> UIImage? {
    guard
        let data,
        let image = UIImage(data: data),
        let response = response as? HTTPURLResponse,
        response.statusCode >= 200 && response.statusCode < 300 else {
          return nil
        }

        return image
}

// old school
func downloadWithEscaping(completionHandler: @escaping (_ image: UIImage?, _ error: Error?) -> ()) {
    URLSession.shared.dataTask(with: URL(string: "xxx")) { [weak self] (data, response, error) in
        let image = responseHandler(data, response)
        completionHandler(image, error)
    }
}

// with combine
func downloadWithCombine() -> AnyPublisher<UIImage?, Error> {
    URLSession.shared.dataTaskPublisher(for: url)
        .map(responseHandler)
        .mapError { $0 }
        .eraseToAnyPublisher()
}
```

### New way (async await)

```swift
func downloadWithAsync() async throws -> UIImage {
    let (imageData, response) = try await URLSession.shared.data(from: URL(string: "xxx")!)

    guard let image = responseHandler(imageData, response) else {
        throw URLError(.badServerResponse)
    }

    return image
}
```

## How to load multiple images concurrently?

### Problem: loading images in a Task context loads by serial

```swift
struct AsyncLetView: View {
    @State private var images: [UIImage] = []

    @StateObject private var loadImageViewModel = LoadImageViewModel()

    let columns = [GridItem(.flexible()), GridItem(.flexible())]

    var body: some View {
        LazyVGrid(columns: columns) {
            ForEach(images, id: \.self) { image in
                Image(uiImage: image)
                .resizable()
                .scaledToFit()
                .frame(height: 150)
            }
        }
        .onAppear {
            Task {
                let image1 = try await loadImageViewModel.fetchImage()
                self.images.append(image1)
                let image2 = try await loadImageViewModel.fetchImage()
                self.images.append(image2)
                let image3 = try await loadImageViewModel.fetchImage()
                self.images.append(image3)
                let image4 = try await loadImageViewModel.fetchImage()
                self.images.append(image4)
            }
        }
    }
}
```

### Solution 1: Split in different Task

> Cons: bad code

```swift
   Task {
    let image1 = try await loadImageViewModel.fetchImage()
    self.images.append(image1)

   }

   Task {
    let image2 = try await loadImageViewModel.fetchImage()
    self.images.append(image2)
   }

   Task {
    let image3 = try await loadImageViewModel.fetchImage()
    self.images.append(image3)
   }

   Task {
    let image4 = try await loadImageViewModel.fetchImage()
    self.images.append(image4)
   }
```

### Solution 2: `async let`

> Cons: Better than solution 1, but still not scalable

```swift
Task {
    async let fetchImage1 = try await loadImageViewModel.fetchImage()
    async let fetchImage2 = try await loadImageViewModel.fetchImage()
    async let fetchImage3 = try await loadImageViewModel.fetchImage()
    async let fetchImage4 = try await loadImageViewModel.fetchImage()

    let (image1, image2, image3, image4) = try await (fetchImage1, fetchImage2, fetchImage3, fetchImage4)

    images.append(contentsOf: [image1, image2, image3, image4])
}
```

### Solution 3: TaskGroup

> Pros: Better than above solution, clean on handling multiple tasks
> Cons: Need to be careful of the error handling

```swift
struct User: Codable {
    let id: String

    let name: String
}

func fetchUsersWithTaskGroup(ids: [String]) async throws -> [User] {
    var users: [User] = []

    return try await withThrowingTaskGroup(of: User?.self) { group in
        for id in ids {
            group.addTask {
                try? await fetchUser(userId: id)
            }
        }

        for try await user in group {
            if let user {
                users.append(user)
            }
        }

      return users
    }
}
```

## SDK 不支援 async await （使用completionHandler）

### Solution: 使用`withCheckedThrowingContinuation`包

```swift
func getData(url: URL) async throws -> Data {
    return try await withCheckedThrowingContinuation { continuation in
        URLSession.shared.dataTask(with: url) { data, response, error in
            if let data {
                continuation.resume(returning: data)
            } else if let error {
                continuation.resume(throwing: error)
            } else {
                continuation.resume(throwing: URLError(.badURL))
            }
        }
        .resume()
    }
}
```

## 問題： 在還沒有actor之前，要如何解決不同物件之間對同一個class的存取/修改？

A: 在有可能被同時存取的class中宣告特定的Queue，規範某些需要排隊處理的操作。

## What is global Actors?

## What is `@MainActor`?

當某個資料會影響到UI時，會需要標記`@MainActor`，告訴compiler說這個資料需要在main thread做更新才可以確保thread safety。

通常在有標記`@Published`的variable都會需要標記`@MainActor`，或是直接在viewModel標記也可以。

## What is Sendable protocol?

當某個type遵守`Sendable`協議時，並不需要特別定義properties或methods，只是要確保在併發環境下，某個property可以保證thread safe就是符合`Sendable`協議。

> 定義struct時，通常就默認遵守Sendable (value types)

## Manage strong & weak reference with Async / Await?

通常不需要在Task呼叫的closure中做weak self的記憶體管理，而是在Task層面。

如果在ViewModel中做了很多async await的操作，可以將Task的Reference紀錄起來，當View disappear的時候去一一cancel還正在執行的Task。

## MVVM 中使用Async / Await 的 Pattern

相較於在View中使用Task包住ViewModel的function，如果情況允許的話，在ViewModel的function中用Task包住concurrent的code會比較好管理這些Task。