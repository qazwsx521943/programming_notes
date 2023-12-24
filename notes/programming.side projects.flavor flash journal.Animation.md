---
id: eprh315gok7nqhqxs1dyos3
title: Animation
desc: ''
updated: 1701398930357
created: 1701322800476
---

## 3 ways of implementing animation in swiftUI

1. By `.animation` modifier

    ```swift
    struct PersonMoveView: View {

    @State private var moveDistance: CGFloat = 0

      var body: some View {
      Button {
        moveDistance += 100
      } label: {
        Text("移動")
      }

      Image(.jason)
        .resizable()
        .scaledToFit()
        .offset(x: moveDistance)
        .animation(.bouncy, value: moveDistance)
      }
    }
    ```

2. `withAnimation`

    ```swift
        struct PersonMoveView: View {

      @State private var moveDistance: CGFloat = 0

        var body: some View {
        let animation = Animation.easeInOut(duration: 0.2).delay(0.2).repeatForever(autoreverses: true)

        Button {
          moveDistance += 100
        } label: {
          Text("移動")
        }

        Image(.jason)
          .resizable()
          .scaledToFit()
          .offset(x: moveDistance)
          .animation(.bouncy, value: moveDistance)
          .onAppear {
            withAnimation(animation) {
              moveDistance += moveDistance == 100 ? -100 : 100
            }
          }
        }
    }
    ```

3. calling `animation` on `Binding` values

## Transition

1. 類似sheet的酷東東

    ```swift
    struct TransitionView: View {

      @State private var showView: Bool = false

      var body: some View {
        ZStack(alignment: .bottom) {
          VStack {
            Button("Button test transition") {
              withAnimation(.easeInOut) {
                showView.toggle()
              }
            }

            Spacer()
          }

          if showView {
            RoundedRectangle(cornerRadius: 30.0)
              .frame(height: UIScreen.main.bounds.height * 0.5)
              .transition(.move(edge: .bottom))
          }
        }
        .ignoresSafeArea(edges: .bottom)
        }
    }
    ```

2. `asymmetric`可以做進出場的控制

    ```swift
    .transition(
        AsymmetricTransition(
            insertion: .move(edge: .bottom), removal: .move(edge: .trailing)))
    ```

### Transition vs Animation 的使用時機？

view原本就在螢幕上，且單純想做搖擺等動畫 => animation

要新增某個view到現有的view => Transition + conditional rendering

## Sheet vs Transition vs Animation offset