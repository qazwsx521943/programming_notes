
## iOS 16.0 (interfacing with UIKit)

當某個struct conform `UIViewRepresentable`
需要實作 `makeUIView`, `updateUIView` protocol function

### Callback Order

1. makeCoordinator
   1. Creates the custom instance that you use to communicate changes(by delegates) from your view to other parts of your SwiftUI interface.

    ```swift
    associatedtype Coordinator = Void

    @MainActor func makeCoordinator() -> Self.Coordinator
    ```

2. makeUIView
   1. Creates the view object and configures its initial state.

    ```swift
    /// You must implement this method and use it to create your view object.
    /// Configure the view using your app's current data and contents of the
    /// `context` parameter. The system calls this method only once, when it
    /// creates your view for the first time. For all subsequent updates, the
    /// system calls the ``UIViewRepresentable/updateUIView(_:context:)``
    /// method.

    associatedtype UIViewType : UIView

    func makeUIView(context: Self.Context) -> Self.UIViewType
    ```

3. updateUIView
   1. Updates the state of the specified view with new information from SwiftUI.
   2. When properties like `@Published` changed, calls this method to update portions of your interface affected by those changes.

    ```swift
    /// When the state of your app changes, SwiftUI updates the portions of your
    /// interface affected by those changes. SwiftUI calls this method for any
    /// changes affecting the corresponding UIKit view. Use this method to
    /// update the configuration of your view to match the new state information
    /// provided in the `context` parameter.
    func updateUIView(_ uiView: Self.UIViewType, context: Self.Context)
    ```
