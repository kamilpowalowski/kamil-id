---
Date: 2021-03-28 11:56
Location: /swiftwombat/how-to-disable-button-in-swiftui
Tags: Swift Wombat, SwiftUI, Button, ViewModifier
---

# How to disable button in SwiftUI

![How to disable button in SwiftUI](/weblog/swiftwombat/covers/how_to_disable_button_in_swiftui.png)

In your SwiftUI applications, you will be using Button view very often. Simple user inputs are build using this interactive control. But in this article, you will learn how to make buttons disabled.

Think about a situation as follow - you have three different options, but some of them are only active when the toggle is true. You can hide inactive options using the `if` statement, but this will be counterintuitive for users. The "Don't make me think" principle assumes that users should know what's going on and not be surprised by UI. You can achieve it by just disabling that options.

To `disable` a button, you should use the disabled view modifier. It takes bool parameter, which for example, can be taken from the state.

```swift
@State private var disabled = true

var body: some View {
    Button("Press me", action: {})
        .disabled(disabled)
}
```

You can use the `disabled` modifier on a view above buttons (`VStack`, for example). That way, you will apply it to all buttons (or other controls below).

```swift
VStack {
    Button("Press me 1", action: {})
    Button("Press me 2", action: {})
    Button("Press me 3", action: {})
}
.disabled(true)
```

**Note**: According to [the documentation](https://developer.apple.com/documentation/swiftui/list/disabled(_:)), `disabled` from view higher in scope takes precedence on disabled from lower views. But my experiments (you can find a link to the project below) show that priority has the one that its parameter is set to `true` and order is not important.

![Disable button Xcode Example](/weblog/swiftwombat/images/22/disable_button_xcode_example.png)

Want to test it yourself? Download this [Xcode Project](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/DisableButton/).
