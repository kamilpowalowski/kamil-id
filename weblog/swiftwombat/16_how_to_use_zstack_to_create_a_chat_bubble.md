---
Date: 2021-01-31 13:58
Location: /swiftwombat/how-to-use-zstack-to-create-a-chat-bubble-in-swiftui
Tags: Swift Wombat, SwiftUI, ZStack, Image, Text, ViewModifier
---

# How to use ZStack to create a chat bubble

![How to use ZStack to create a chat bubble](/weblog/swiftwombat/covers/how_to_use_zstack_to_create_a_chat_bubble.png)

`ZStack` is one of the basics building blocks of SwiftUI applications. Along with `HStack` and `VStack`, you will be using them a lot. This article will learn what `ZStack` is and how to use it to build a crucial part of every chat UI - a chat bubble.

![Chat bubbles example](/weblog/swiftwombat/images/16/chat_bubble_image_capinsets_example.png)

So what is this view with the strange name `ZStack`? It's a view that arranges its children one into another/overlays them. In 3D space, we usually mark the third direction `z` along `x` and `y`.

`ZStack` syntax is very similar to `HStack` and `VStack`.

```swift
ZStack {
    Circle()
    	.foregroundColor(.green)
    Text("Swift Wombat")
}
```

Additionally, you can provide an alignment parameter that controls views position on a horizontal and vertical axis.

```swift
ZStack(
    alignment: Alignment(horizontal: .leading, vertical: .top)
) {
    Circle()
        .foregroundColor(.green)
    Text("Swift Wombat")
 }
```

There is one view modifier that you need to know to complete this task. It's called a `layoutPriority`, and by providing a `Double` value for it, you can control the importance of views when the stack will layout them. By default, each has a priority set to zero. Set it to a higher value, and it will be authoritative when the stack calculates its frame.

You may wonder why this is important. To make a chat bubble, you have to use `Image` view as a background with `resizable` modifier.

> Learn more about [resizable and how to control Image resize zones with it](https://swiftwombat.com/how-to-control-swiftui-image-resize-zones-with-capinsets/)

Resizable means that it will try to take all available space, but you want the background to fit the given text. It is a point where you should use `layoutPriority`.

```swift
ZStack {
    Image("bubble")
        .resizable(capInsets: EdgeInsets(top: 39, leading: 49, bottom: 39, trailing: 49))
        .renderingMode(.template)
        .foregroundColor(.blue)
     Text("Did you hear about the Swift Wombat website?")
        .foregroundColor(.white)
        .padding(.horizontal, 16)
        .padding(.vertical)
        .layoutPriority(1)
}
```

That way, the `Text` view will have a layout priority, and the `Image` will adjust to its size.

In the Xcode Playground example linked at the bottom of this article, you will find full code that will adjust the color and orientation of bubbles to distinguish messages that we send and receive.

![Chat bubble Xcode Playground](/weblog/swiftwombat/images/16/chat_bubble_xcode_playground.png)

Want to test it yourself? Download this [Xcode Playground](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/ChatBubble/).
