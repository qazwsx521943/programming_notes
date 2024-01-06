---
id: kdwsnesajgvc4js20f5pb75
title: Concurrency
desc: ''
updated: 1704447724983
created: 1704421852609
---

## What is concurrency?

在一定的時間內，有多個任務(Task)需要同時進行。

在ios開發中，我們可能同一個時段會需要處理不同的任務，那我們就會透過多線程的方式，讓每個線程去處理不同的任務。

比方說，跟 UI 有關的任務包含，建立 / 更新視圖，就一定要在主線程上執行。

而如果要等待資料回傳的任務（發送網路請求、下載文件等等），我們就會讓他在其他線程上去執行。

如果今天我們把需要花時間的任務請主線程處理的話，主線程會乾等在那導致UI無法進行任何處理，使用者就會覺得我們的應用程式壞掉。

## 高併發跟多線程的關係？

## iOS中有什麼處理多線程的技術？

1. NSThread
   > 屬於Objective-C中的一個類，可以用來建立 / 管理 線程
2. [[GCD|programming.language.swift.framework.grand-central-dispatch]]
   > Apple 開發的框架，讓我們可以有效針對不同裝置的硬體配備進行多線程的處理。

   - 解決了 NSThread 什麼問題？
     - 簡單易用
     - 高效：可以更有效利用設備的多核心處理能力
     - 安全可靠

3. async / await (swift 5.5)
   >

## 衡量標準

1. QPS(Query per second)
2. RT (Response Time)

## 通常在解決高併發有兩種擴展方式

1. 垂直擴展 -> 升級硬體
   1. 提高硬體核心數
2. 水平擴展
   1. 負載均衡 ex: F5 + nginx 去做 load balance
   2. 分散式緩存： redis, memcached. 搭配CDN
