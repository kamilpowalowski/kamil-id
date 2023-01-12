---
Date: 2021-01-28 20:14
Location: /swiftwombat/how-to-apply-text-modifiers-based-on-the-swiftui-view-state
Tags: Swift Wombat, SwiftUI, Text, ViewModifier
---

# How to apply text modifiers based on a view state

![How to apply text modifiers based on a view state](/weblog/swiftwombat/covers/how_to_apply_text_modifiers_based_on_a_view_state.png)

I saw the "How to optionally make a Text italic?" question on Reddit, and I thought it would be good material for a short article. From this tutorial, you will learn how to prepare a conditional italic modifier. Additionally, you will find out how to make more generic modifiers to control every Text's properties.

So, let's start with italic. Let's assume that you have a @State property wrapper that looks like this one:

```swift
@State private var isItalic = true
```

You can use the `if ... else ...` statement to control which version of Text is displayed.

```swift
if isItalic {
    Text("Swift Wombat")
        .italic()
} else {
    Text("Swift Wombat")
}
```

This way, you have to repeat a code, which you should try to avoid.
Some text modifiers ([strikethrough for example](https://developer.apple.com/documentation/swiftui/text/strikethrough(_:color:))) have an `active` parameter, but not `italic`. You can easily create that with this extension:

```swift
import SwiftUI

extension Text {
    func italic(_ active: Bool) -> Text {
        guard active else { return self }
        return italic()
    }
}
```

And this alone will be an answer to the question from the first paragraph. But what if you want to handle also bold or font modifiers?
You can iterate at the solution above and add an additional modifier to the Text extension.

```swift
import SwiftUI

extension Text {
    func active(
        _ active: Bool,
        _ modifier: (Text) -> Text
    ) -> Text {
        guard active else { return self }
        return modifier(self)
    }
}
```

It is a bit harder to read. This function takes two arguments. One is the active state that controls if modifier is applied on not, and the second one is a function - which gets a Text struct and returns its modified version. You can use it like this:

```swift
Text("Swift Wombat")
    .active(isTitle, { $0.font(.title) })
```

For functions without arguments, you may prepare another extension:

```swift
import SwiftUI

extension Text {
    func active(
        _ active: Bool,
        _ modifier: (Text) -> () -> Text
    ) -> Text {
        guard active else { return self }
        return modifier(self)()
    }
}
```

With it, you will be able to apply italic or bold even quicker:

```swift
Text("Swift Wombat")
    .active(isItalic, Text.italic)
    .active(isBold, Text.bold)
```

Note capital case of struct name `Text` in that version.

![Active text modifier example Xcode Playground](/weblog/swiftwombat/images/15/active_text_modifier_example_xcode_playground.png)

Want to test it yourself? Download this [Xcode Playground](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/ActiveTextModifier/).
