
## Dependency Injection

Common used cases:

- When initializing a provider, we might pass in a dataService for the provider to init, and then the provdier can use that dataService instance to fetch Data.

- When using `#Preview` in swiftUI, some views depends on the environment values when rendering the view, so we might use an environment modifier to inject the values that we want to see on the preview canvas.