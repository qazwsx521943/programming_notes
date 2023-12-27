
## `LazyVGrid` Instagram layout

```swift
struct LazyVGridView: View {
    var body: some View {
    ScrollView {
    Rectangle()
      .fill(AngularGradient(colors: [.blue, .purple, .red], center: .bottomLeading))
      .frame(width: .infinity, height: 500)

    LazyVGrid(
        columns: [
            GridItem(.flexible(), spacing: 10),
            GridItem(.flexible(), spacing: 10),
            GridItem(.flexible(), spacing: 10),
        ],
        alignment: .leading,
        spacing: 10,
        pinnedViews: [.sectionHeaders],
        content: {
            Section {
              ForEach(0..<20) { index in
                  Rectangle()
                    .frame(height: 150)
              }
            } header: {
              Text("This is Section 1")
                  .font(.subheadline)
                  .foregroundStyle(.brown)
            }
        })
    }
    }
}

```