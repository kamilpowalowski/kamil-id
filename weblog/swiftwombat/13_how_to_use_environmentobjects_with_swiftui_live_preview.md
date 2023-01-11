---
Date: 2021-01-20 18:17
Location: /swiftwombat/how-to-use-environmentobjects-with-swiftui-live-preview
Tags: Swift Wombat, SwiftUI, Property wrapper, ViewModifier
---

# How to use EnvironmentObjects with SwiftUI Live Preview

![How to use EnvironmentObjects with SwiftUI Live Preview](/weblog/swiftwombat/covers/how_to_use_environmentobjects_with_swiftui_live_preview.png)

SwiftUI views in larger applications may use some configuration passed to them as environment objects. It is a very beneficial way when the config is needed somewhere deep into a view tree. If your view uses the `@EnvironmentObject` property wrapper, you can still debug and tweak it using live preview functionality.

Take a look at this example. In the [article about home screen Quick Actions in SwiftUI](/swiftwombat/how-to-add-home-screen-quick-actions-to-swiftui-app/), I've used `ObservableObject` called `QuickActionService`. You can use it to pass selected Quick Action to the given view.

```swift
import Foundation

enum QuickAction: String {
    case newMessage, search, inbox
}

final class QuickActionService: ObservableObject {
    @Published var action: QuickAction? = nil
}
```

When you provide that object using `environmentObject` modifier, you can listen to changes in it in any place down the view tree.

```swift
private let quickActionService = QuickActionService()

var body: some Scene {
    WindowGroup {
        ContentView()
            .environmentObject(quickActionService)
    }
}
```

Live previews, though, are rendered without that context of upper views. So you have to use the same `environmentObject` modifier in your preview code.

```swift
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
            .environmentObject(QuickActionService())
    }
}
```

With a slight modification of `QuickActionService`:

```swift
final class QuickActionService: ObservableObject {
    @Published var action: QuickAction?
    
    init(initialValue: QuickAction? = nil) {
        action = initialValue
    }
}
```

you will be able to test different versions of your screen based on different values for quick action parameters.

```swift
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        Group {
            ContentView()
                .environmentObject(QuickActionService())
            ContentView()
                .environmentObject(QuickActionService(initialValue: .newMessage))
        }
    }
}
```

---

Want to test it yourself? Download this [Xcode project](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/HomeScreenQuickActions/).
