---
id: xjoeuc8wuqj1r0fwoesk2k1
title: Core Data
desc: ''
updated: 1703253952500
created: 1702460092546
---

## SwiftUI 中導入Core Data

1. 建立Data Model
2. 建立DataController / DataManagar Helper class，負責初始化data model persistent store
3. 在App入口初始化DataController，並透過`@environmnet(\.managedObjectContext, dataController.container.viewContext)`讓整個view hiearchy 都可以存取到

## 如何使用 `@FetchRequest` property wrapper?

> Always declare properties that have a fetch request wrapper as private.
