---
id: idmoihik9jm09tb79mbpk1q
title: Architecture
desc: ''
updated: 1701074732617
created: 1701041481511
---

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