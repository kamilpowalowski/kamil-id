---
Date: 2021-06-27 14:59
Location: /swiftwombat/how-to-add-pull-to-refresh-to-the-swiftui-view
Tags: Swift Wombat, SwiftUI, List, ViewModifer, async/await
---

# How to add Pull to Refresh to the SwiftUI view

![How to add Pull to Refresh to the SwiftUI view](/weblog/swiftwombat/covers/how_to_add_pull_to_refresh_to_the_swiftui_view.png)

Pull to Refresh is a widespread UX pattern on mobile. Drag a list down and load new items. You can find that in all social media applications and almost all other apps that display data download from servers.

To achieve this in previous versions of SwiftUI, you will have to use `UIKit` code, create your view, or third-party library. But SWiftUI 3 brings a native `ViewModifier` that changes adding Pull to Refresh into child's play. Let's dive into code.

To display Pull to Refresh, you have to add a `refreshable` modifier to scrollable view, most commonly `List` or `Grid`. The `refreshable` view modifier uses async/await syntax and handles dismissing of refresh indicator automatically.

```swift
struct ContentView: View {
    @State private var rows = 0

    var body: some View {
        List(0..<rows, id: \.self) { number in
            Text("row \(number)")
        }
        .refreshable {
            await Task.sleep(5_000_000_000) // wait 5s
            rows += 10
        }
    }
}
```

For this example, I'm using a `Task.sleep` that simply waits for a given number of nanoseconds to pass. Typically, you will perform time-consuming asynchronous (like downloading data from the network) operations here.

And that's it. This code sample will add full Pull to Refresh handling. But there is one more thing that is worth mentioning. SwiftUI 3 added a new modifier to download initial data for a given view. This modifier is called `task` and also expects the async/await syntax.

```swift
struct ContentView: View {
    @State private var rows = 0

    var body: some View {
        List(0..<rows, id: \.self) { number in
            Text("row \(number)")
        }
        .task {
            await Task.sleep(3_000_000_000) // wait 3s
            rows = 10
        }
        .refreshable {
            await Task.sleep(5_000_000_000) // wait 5s
            rows += 10
        }
    }
}
```

![Xcode refreshable experiment](/weblog/swiftwombat/images/31/xcode_refreshable_experiment.png)

Want to test it yourself? Download this [Xcode Project](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/Refreshable/).
