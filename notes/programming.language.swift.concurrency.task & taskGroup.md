---
id: 4momin9k0ledbifglklmqyv
title: Task & TaskGroup
desc: ''
updated: 1704531237979
created: 1704531164027
---

## Task

> A unit of **asynchronous** work.

## Task Groups

> A task itself does only one thing at a time, but when you create **multiple tasks**, Swift can schedule them to **run simultaneously**.

### 情境(一)：想要把某個相簿中的所有照片下載下來。

TaskHiearchy分析：

步驟：

1. 透過`withTaskGroup`建立一個task group，並傳入一個closure

    ```swift
    @available(macOS 10.15, iOS 13.0, watchOS 6.0, tvOS 13.0, *)
    @inlinable public func withTaskGroup<ChildTaskResult, GroupResult>(of childTaskResultType: ChildTaskResult.Type, returning returnType: GroupResult.Type = GroupResult.self, body: (inout TaskGroup<ChildTaskResult>) async -> GroupResult) async -> GroupResult where ChildTaskResult : Sendable
    ```

    這個closure提供一個新建立好的`taskGroup`，我們可以把想要加到group中的task以`addTask`加入

    ★ 如果今天加入到group的task是可能會throw的，可以使用`withThrowingTaskGroup`

    ```swift
    @available(macOS 10.15, iOS 13.0, watchOS 6.0, tvOS 13.0, *)
    @inlinable public func withThrowingTaskGroup<ChildTaskResult, GroupResult>(of childTaskResultType: ChildTaskResult.Type, returning returnType: GroupResult.Type = GroupResult.self, body: (inout ThrowingTaskGroup<ChildTaskResult, Error>) async throws -> GroupResult) async rethrows -> GroupResult where ChildTaskResult : Sendable
    ```

2. 先抓到相簿中所有照片的name
3. 把每張要下載的照片加入task group
4. 透過`for await`將已回傳的photo加到result陣列中。

```swift
let photos = await withTaskGroup(of: Data.self) { group in

    let photoNames = await listPhotos(inGallery: "Summer Vacation")

    for name in photoNames {
        group.addTask {
            return await downloadPhoto(named: name)
        }
    }

    var results: [Data] = []
    for await photo in group {
        results.append(photo)
    }

    return results
}
```