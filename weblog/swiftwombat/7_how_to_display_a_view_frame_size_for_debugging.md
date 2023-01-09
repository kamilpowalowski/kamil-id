---
Date: 2020-12-31 13:03
Location: /swiftwombat/how-to-display-a-view-frame-size-for-debugging
Tags: Swift Wombat, SwiftUI, GeometryReader, ZStack, ViewModifier
---

# How to display a view frame size for debugging

![How to display a view frame size for debugging](/weblog/swiftwombat/covers/how_to_display_a_view_frame_size_for_debugging.png)

In this SwiftUI tutorial, you will learn what a `GeometryReader` is and how to use it to prepare a modifier that will display size and dashed frame around a view. It can be handy for debugging. The image above presents en effect of applying a `frameSize` modifier.

![Outcome of applying a custom frameSize ViewModifier](/weblog/swiftwombat/images/7/frame_size_example.png)

## What is GeometryReader?

According to Apple documentation, `GeometryReader` is a view in which content is represented as a function of its size and coordinate space. It doesn't sound easy, but in reality, it just means that `GeometryReader` is a container, and when we provide its content, we get its size. To be precise, not only size but struct called [GeometryProxy](https://developer.apple.com/documentation/swiftui/geometryproxy), which contains `size`, `safeAreaInsets` and can calculate container bounds in given coordinate space.

```swift
GeometryReader { geometry in 
    Text("\(Int(geometry.size.width))x\(Int(geometry.size.height))")
}
```

This code will display the size of a view you put it in. By default, GeometryReader will take all available space in stacks.

This view can be useful when you need to layout your SwiftUI views based on the given bound but can't use build layout solutions.

## How to draw a dashed border?

At first, you may think that the `border` modifier will do the trick. But It turns out this only supports a color and line width. But fear no more. You can achieve the same effect by overlay modifier.

```swift
Text("Swift Wombat")
    .overlay(
        Rectangle()
            .strokeBorder(style: StrokeStyle(lineWidth: 1, dash: [5]))
    )
```

In the code above, the `Rectangle` shape with `strokeBorder` is created and applied to `Text` view using the build-in overlay `ViewModifier`. Experiment with `StrokeStyle` parameters. Using a dash array, you can specify the length and distance between dashes.

## How to combine those two using ZStack?

Now, you should combine `GeometryReader` and dashed line together. To do this, you can use `ZStack`. It allows displaying one view on top of another. Take a look at this code:

```swift
Text("Swift Wombat")
    .overlay(
        GeometryReader { geometry in
            ZStack {
                Rectangle()
                    .strokeBorder(style: StrokeStyle(lineWidth: 1, dash: [5]))
                Text("\(Int(geometry.size.width))x\(Int(geometry.size.height))")
            }
        }
```

But using it in this form will not be fun. You can do better by abstracting that logic to a custom `ViewModifier`.
> Learn [the basics about modifiers and create gradient overlay above any view](/swiftwombat/how-to-create-custom-viewmodifier/).

Full, ready to use code of `frameSize` modifier styled a bit with color, `ZStack` alignment, and font size may look like the one below:

```swift
import SwiftUI

extension View {
    func frameSize() -> some View {
        modifier(FrameSize())
    }
}

private struct FrameSize: ViewModifier {
    static let color: Color = .blue
    
    func body(content: Content) -> some View {
        content
            .overlay(GeometryReader(content: overlay(for:)))
    }
    
    func overlay(for geometry: GeometryProxy) -> some View {
        ZStack(
            alignment: Alignment(horizontal: .trailing, vertical: .top)
        ) {
            Rectangle()
                .strokeBorder(
                    style: StrokeStyle(lineWidth: 1, dash: [5])
                )
                .foregroundColor(FrameSize.color)
            Text("\(Int(geometry.size.width))x\(Int(geometry.size.height))")
                .font(.caption2)
                .foregroundColor(FrameSize.color)
                .padding(2)
        }
    }
}
```

![Xcode project where you can test frameSize ViewModifier](/weblog/swiftwombat/images/7/frame_size_xcode_project_preview.png)

Want to test it yourself? Download this [Xcode project](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/FrameSize/).
