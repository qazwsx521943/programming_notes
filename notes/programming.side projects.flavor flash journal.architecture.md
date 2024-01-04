---
id: idmoihik9jm09tb79mbpk1q
title: Architecture
desc: ''
updated: 1704334133026
created: 1701041481511
---

## MVC, MVVM, MVP Decision

思路：

- 關注點分離
- 低耦合（物件之間的相依程度）
- 容易維護
- 容易測試

### MVC

- Flow:

  1. View會監聽使用者action然後告知controller
  2. controller針對使用者action去叫model做對應的事
  3. model更新然後回應給controller
  4. controller更新view

### 為何我不喜歡用MVC?

1. 他把view跟資料處理的邏輯寫在一起了

## Why use async / await

因為之前在剛開始學swift的時候，都是用completion handler這種closure去規範Task執行的順序，但當某個function的步驟很多，而且是需要按造順序去執行的，那這個時候就會有callback hell。所以我這次個人專案為了讓project更好維護、可讀性更高，所以有稍微去看一下swift中要如何去使用async await。

那我覺得我有做比較特別的部分是，像我有使用到一些sdk，像是firebase跟google places api，那因為那些api提供的function可能是設計成completionHandler的方式，所以我有再次把這些function包成async await。

### Why don't use GCD in your project?

GCD（Grand Central Dispatch）是一個基於隊列的框架，那它提供了四種不同的 DispatchQueue 物件，讓我們能夠根據需求將不同的任務分配到不同的隊列中。主要分為 serial（串行）和 concurrent（並行）兩種類型的隊列。

我們可以根據任務的性質，選擇使用 sync 或 async 的方式將任務添加到隊列中。使用 sync 會讓當前的執行緒等待任務完成，而 async 則允許執行緒繼續執行其他任務而不需等待當前任務完成。

--S--A--S--A--S

## Dependency Injection

Q: What did it solve?

A: Singleton design pattern cons

### How it works?

> Use protocols to make a blue print of DataServices

1. Initializing dataServices at a intro level (Can use a Master class to initialize all dataServices)
2. Passing these dataService instances into Views and use it to initialize view models (Injection)

## Singleton

- Pros:

  1. Very convenient
  2. Beginner friendly

- Cons:

  1. Singleton's are global (not thread safe)
  2. can't customize init (bad for testing)
  3. can't swap out service

## App LifeCycle

Prior to iOS 14, iOS apps had a class named "AppDelegate" that the creation of this class was the starting point of an app.

### `@Main` property wrapper

Indicates the entry execution of the app

### 'App' protocol

struct that conform to the App protocol, has a default implementation of the method `main()`, which manages the launch process of the app.