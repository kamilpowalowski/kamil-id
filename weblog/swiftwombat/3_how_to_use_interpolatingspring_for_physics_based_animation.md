---
Date: 2020-12-13 20:38
Location: /swiftwombat/how-to-use-interpolatingspring-for-physics-based-animation
Tags: Swift Wombat, SwiftUI, Animation, Image, ViewModifier
---

# How to use interpolatingSpring for physics-based Animation

![How to use interpolatingSpring for physics-based Animation](/weblog/swiftwombat/covers/how_to_use_interpolatingspring_for_physics_based_animation.png)

![Notification bell animation created with interpolatingSpring.](/weblog/swiftwombat/images/3/bell.gif)

SwiftUI offers a very convenient way to display simple value-based animation. By adding an animation modifier, you can tell that you want to animate other modifier value changes. Additionally, with SwiftUI, you get many Animations like `easeIn`, `easeOut`, or `linear`. There are also Animations that emulates physical spring behavior: `spring`, `interactiveSpring`, and `interpolatingSpring`. This article will show you how to use the last one to animate bell from SF Symbols like on GIF above.

```swift
struct BellView: View {
    @State private var animate: Bool = false
    
    var body: some View {
        Image(systemName: "bell.badge.fill")
            .resizable()
            .scaledToFit()
            .rotationEffect(
                animate ?  .zero : .degrees(30),
                anchor: UnitPoint(x: 0.5, y: 0.05)
            )
            .animation(.interpolatingSpring(stiffness: 200, damping: 2))
            .onAppear { animate = true }
    }
}
```

To achieve that animation, you have to use a [rotationEffect](https://developer.apple.com/documentation/swiftui/view/rotationeffect(_:anchor:)) modifier. This function expects two parameters. First is a rotation Angle. You can use provided static functions to create Angle from `degrees` or `radians`. The second parameter is optional, but it allows for modification of the rotation anchor. Values provided above will set your rotation point to half of the width (0.5 * width) and just below the top (0.05 * height) of the Image.

Next, you have to inform SwiftUI that you want to animate that view. You can do this by animation modifier, which expects an `Animation` object. To get that spring-like animation, you have to use [interpolatingSpring](https://developer.apple.com/documentation/swiftui/animation/interpolatingspring(mass:stiffness:damping:initialvelocity:)) animation. You can control the `mass`, `stiffness`, `damping`, and `initialVelocity` of an animated property. Fine-tuning these values is usually a long trial and error. Xcode Playground is very useful for that work.

To trigger the animation, you have to modify the value already seen in the SwiftUI views tree. To achieve this, you can use the State property and `onAppear` function. That way, you will be sure that the animation will start when a given view is visible. In the example above, I control `rotationEffect` angle by `animate` property: `animate ? .zero : .degrees(30)`. It starts from thirty degrees and goes to zero when a view appears.

> If you want to apply that animation to different views, you can [create a custom modifier](/swiftwombat/how-to-create-custom-viewmodifier/).

![Xcode Playground can be useful to fine-tune animation.](/weblog/swiftwombat/images/3/interpolatingSpring_animation_example.png)

Want to test it yourself? Download this [Xcode Playground](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/SpringAnimation/).
