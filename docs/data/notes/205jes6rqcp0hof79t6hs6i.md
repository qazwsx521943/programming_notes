
## 為什麼我們要好好的處理concurrency?

設計良好的concurrency code可以讓我們充分使用到CPU的資源

## 如何使用DispatchQueue?

**DispatchQueue**就是幫我們管理Tasks的一個物件。

### Step 1: 取得或建立一個DispatchQueue

```swift
let mainQueue = DispatchQueue.main  // Serial

let globalQueue = DispatchQueue.global() // Concurrent

let customQueue = DispatchQueue(label: "example.com.domain") // default Serial
```

### Step 2: 我們可以透過以下方式提交Task到一個DispatchQueue物件上，並指定DispatchQueue要如何執行這個Task

```swift
globalQueue.sync {
  // Task1...
}

globatQueue.async {
  // Task2...
}
```

## 什麼是DispatchItem？為何要使用他？

如果我們想要控制我們的Task狀態呢？ ex: 在三秒內沒有回傳結果時我想要取消這個Task

  1. 情境1:  使用者輸入UISearchBar時，不希望每次text change都打一次API，造成後端的負擔。

      我們可以將Task透過DispatchItem封裝，再透過DispatchQueue設定asyncAfter延後執行這個DispatchItem，我們就可以達到延遲fetch資料，減少對後端的負載


```swift
let workItem = DispatchWorkItem {
  // Task1...
}

globalQueue.async(execute: workItem)
```

## 什麼是DispatchGroup? 為何要使用他？

  1. 情境1: 我們有一個陣列的api要打，我們希望他可以全部加載完再一次更新到畫面上

```swift
let globalQueue = DispatchQueue.global()
let loadImageGroup = DispatchGroup()

loadImageGroup.enter()
globalQueue.async {
    task1() { response in
        ...
        loadImageGroup.leave()
    }
}

loadImageGroup.enter()
globalQueue.async {
    task2() { response in
        ...
        loadImageGroup.leave()
    }
}

// 1. wait synchronously for all tasks to finish
loadImageGroup.wait()
// ↓ Tasks that need to be done after loadImageGroup's task are all done
print("all done")


// 2. adding a completion handler for loadImageGroup
loadImageGroup.notify(queue: .main) {
  // ui update code
}

```

## 什麼是DispatchSemaphore？為何要使用他？

## 什麼是Barrier？為何要使用他？

當把Task加到Concurrent Queue中，有時候不同Thread的Task會需要共同存取+修改到某個Value，造成一些Bug。

透過在submit一個task時設定這個Task被執行時不能有其他task存取到同一個共同資源，可以確保這個task執行時不會有其他存取到共同資源的Task在其他Thread中存取。

![](/assets/images/language.swift.grand-central-dispatch.concurrency_barrierConcept.png)