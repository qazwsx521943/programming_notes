---
id: 9mr2s7gpmdzbi4penck2gg0
title: Dependency Injection
desc: ''
updated: 1703948754034
created: 1703253972301
---

## Dependency Injection

> Dependency injection aims to separate the concerns of constructing objects and using them, leading to loosely coupled programs.

Common used cases:

- When initializing a provider, we might pass in a dataService for the provider to init, and then the provdier can use that dataService instance to fetch Data.

- When using `#Preview` in swiftUI, some views depends on the environment values when rendering the view, so we might use an environment modifier to inject the values that we want to see on the preview canvas.