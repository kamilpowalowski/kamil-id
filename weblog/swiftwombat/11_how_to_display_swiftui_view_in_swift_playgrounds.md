---
Date: 2021-01-15 20:00
Location: /swiftwombat/how-to-display-swiftui-view-in-swift-playgrounds
Tags: Swift Wombat, SwiftUI
---

# How to display SwiftUI view in Swift Playgrounds

![How to display SwiftUI view in Swift Playgrounds](/weblog/swiftwombat/covers/how_to_display_swiftui_view_in_swift_playgrounds.png)

One of the most significant advantages of cross-platform frameworks like React Native or Flutter is hot reloading. During development, you can see recent code changes without recompiling and rebuilding. SwiftUI offers a similar but somehow limited functionality called live preview. It works only for single views, not a whole application, but it is handy when you have to create or fine-tune one of them.

The live preview is not the only option to quickly play with SwiftUI. You can use the Swift Playgrounds application to build and test SwiftUI views on Mac or iPad. In the form of Xcode Playgrounds, I'm using it in many of Swift Wombat's articles.

To display your SwiftUI view on Playgrounds, you have to add one additional line of code.

```swift
import PlaygroundSupport

PlaygroundPage.current.setLiveView(\*your view*\)
```

But please, be aware that not everything will work that way. For example, NavigationView doesn't work correctly.

> I have to create a regular iOS project to make [a Picker with a segmented control style](/swiftwombat/how-to-use-swiftui-picker-to-create-a-segmented-control/).

Nevertheless, there are many things that you can very quickly prototype using that functionality. And since it is effortless to set up on the go (on iPad), you may find that little code snippet very useful.

```swift
import SwiftUI
import PlaygroundSupport

struct ContentView: View {
    var body: some View {
        Text("Swift Wombat")
            .padding()
    }
}

PlaygroundPage.current.setLiveView(ContentView())
```
