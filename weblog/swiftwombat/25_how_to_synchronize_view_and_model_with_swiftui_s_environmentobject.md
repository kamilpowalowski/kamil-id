---
Date: 2021-04-25 13:42
Location: /swiftwombat/how-to-synchronize-view-and-model-with-swiftuis-environmentobject
Tags: Swift Wombat, SwiftUI, Property wrapper, ViewModifier
---

# How to synchronize View and Model with SwiftUI's EnvironmentObject

![How to synchronize View and Model with SwiftUI's EnvironmentObject](/weblog/swiftwombat/covers/how_to_synchronize_view_and_model_with_swiftui_s_environmentobject.png)

SwiftUI offers an easy way to inject your model classes into a view hierarchy. Additionally, it handles model changes pretty automatically, and only a tiny amount of additional code is needed from your side.In this article, I'll explain how to provide and retrieve `@EnvironmentObject` anywhere within your views hierarchy.

Why you need an environment object, you may ask? It simplifies your code. Of course, you can create a @State or @StateObject in your top view and provide it as Binding down below, but this will make your code less modular. Think of environment objects like a dependency injection mechanism. You provide your model in one place, and then in other, you can read it or perform actions. My favorite Redux-like state management library [SwiftDux](https://github.com/StevenLambion/SwiftDux) uses environment objects to provide store and action dispatcher to any view down the graph.

The first puzzle you need is a class (model) that you will pass as an environment object. It needs to conform to the `ObservableObject` protocol. In that class, you can use the `@Published` property wrapper to expose a variable. When you change the value of the `@Published` variable, it will inform `ObservableObject`, and SwiftUI will handle the rest. Of course, a given object can contain standard variables and methods as well.

```swift
class Counter: ObservableObject {
    @Published var value = 0
}
```

Now, when you have your model ready, you must find a place to create an instance of it and find a place to inject it into the SwiftUI view structure. Usually, this is the first view of the view's hierarchy. But it doesn't have to be. Sometimes you want to provide some data to just a part of a structure. When you pick your place, create an instance of your model class using the `@StateObject` property wrapper, and then inject it using the `environmentObject` view modifier

```swift
struct ContentView: View {
    @StateObject private var counter = Counter()
    
    var body: some View {
        CounterView()
            .environmentObject(counter)
    }
}
```

You created your model, provided it for SwiftUI views, so it's time to learn how to use it. To retrieve your model, you should use the `@EnvironmentObject` property wrapper. You can operate on the provided object, and SwiftUI will refresh it in other places of the UI where the given environment object is used.

```swift
struct CounterView: View {
    @EnvironmentObject private var counter: Counter

    var body: some View {
        VStack {
            Text("\(counter.value)")
            Divider()
            IncrementView()
        }
    }
}
```

```swift
struct IncrementView: View {
    @EnvironmentObject private var counter: Counter

    var body: some View {
        Button("Increment", action: { counter.value += 1 })
    }
}
```

![Xcode Playground with EnvironmentObject example](/weblog/swiftwombat/images/25/enviroment_object_xcode_playground.png)

Want to test it yourself? Download this [Xcode Playground](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/EnvironmentObject).
