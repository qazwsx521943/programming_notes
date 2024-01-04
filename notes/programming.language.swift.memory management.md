---
id: hvogtqvodqmpqkl2y0zxkph
title: Memory Management
desc: ''
updated: 1704388196917
created: 1703918584124
nav_order: 2
---

https://developer.apple.com/documentation/swift/manual-memory-management

## Stack vs Heap

他們都是記憶體區域

### Stack 特性

- **具有區域性:**

  呼叫function時 function scope中的變數會被存在stack中，當執行完畢會釋放

- **堆疊結構:**

  stack 是一種堆疊結構，讀寫stack上的資料可以非常迅速

- **自動分配和釋放:**

  想想function，我們從來沒有主動去釋放function內宣告的變數，只要變數超過作用域，空間就會被自動釋放

### Heap 的特性

- **動態分配:**

  在heap上分配記憶體是動態的，它不像 stack 那樣有明確的作用範圍和生命週期

- **生命週期:**

  heap上的資料不受作用域限制，可以依據程式的需求進行調整

- **全局性:**

  heap上的資料可以在整個應用程式中共享

### Heap Summary

- More dynamic but less efficient than stack.
- Goes through 3 steps:
  1. allocation
  2. tracking reference count
  3. deallocation
- Heap memory allocation is done for objects whose size can not be calculated at compile time, plus all reference types(because reference types life time is not based on their defined scope).
- heap memory is somehow a global host for objects and all threads can have access to it, so the objects stored on it are not thread-safe

### Stack Summary

- value types are all stored on the stack memory. Note that the value types should not have any reference types associated with them(i.e. they are either not contained by or contain a reference type) otherwise they wouldn’t be stored on the stack.
- The amount of memory needed is normally calculated at compile time since they are not dynamic and do not need reference count semantics to decide how long they have to live(They live in a scope and when they are used the memory will dump them)