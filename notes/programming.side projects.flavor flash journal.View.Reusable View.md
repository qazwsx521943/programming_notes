---
id: nkp55fqc7bcwlb00sxlh3dw
title: Reusable View
desc: ''
updated: 1701344135971
created: 1701307744073
---

## 管理要渲染哪個Loading View的Container View

```swift
public struct ActivityIndicatorView: View {

    public enum IndicatorType {
        case `default`(count: Int = 8)
        case arcs(count: Int = 3, lineWidth: CGFloat = 2)
        case rotatingDots(count: Int = 5)
        case flickeringDots(count: Int = 8)
        case scalingDots(count: Int = 3, inset: Int = 2)
        case opacityDots(count: Int = 3, inset: Int = 4)
        case equalizer(count: Int = 5)
        case growingArc(Color = .black, lineWidth: CGFloat = 4)
        case growingCircle
        case gradient(_ colors: [Color], CGLineCap = .butt, lineWidth: CGFloat = 4)
    }

    @Binding var isVisible: Bool
    var type: IndicatorType

    public init(isVisible: Binding<Bool>, type: IndicatorType) {
        _isVisible = isVisible
        self.type = type
    }

    public var body: some View {
        if isVisible {
            indicator
        } else {
            EmptyView()
        }
    }

    // MARK: - Private

    private var indicator: some View {
        ZStack {
            switch type {
            case .default(let count):
                DefaultIndicatorView(count: count)
            case .arcs(let count, let lineWidth):
                ArcsIndicatorView(count: count, lineWidth: lineWidth)
            case .rotatingDots(let count):
                RotatingDotsIndicatorView(count: count)
            case .flickeringDots(let count):
                FlickeringDotsIndicatorView(count: count)
            case .scalingDots(let count, let inset):
                ScalingDotsIndicatorView(count: count, inset: inset)
            case .opacityDots(let count, let inset):
                OpacityDotsIndicatorView(count: count, inset: inset)
            case .equalizer(let count):
                EqualizerIndicatorView(count: count)
            case .growingArc(let color, let lineWidth):
                GrowingArcIndicatorView(color: color, lineWidth: lineWidth)
            case .growingCircle:
                GrowingCircleIndicatorView()
            case .gradient(let colors, let lineCap, let lineWidth):
                GradientIndicatorView(colors: colors, lineCap: lineCap, lineWidth: lineWidth)
            }
        }
    }
}
```

1. 先定義Enum，代表這個Container View可以渲染的Loading View有哪些

2. 定義`init`方式 (傳入一個`isVisible`的Binding variable、一個上面定義好的Loading Type)

3. 透過`computed value`決定這個view要渲染哪個Loading View

## Loading View怎麼設計的

先決定Loading想要的變化(Ex: 透明度、長度、變形)

## How to extract Subviews

1. Simple view => 拆成其他computed value就可以
2. Need reusability(stateful view) => extract into subview