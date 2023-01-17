---
Date: 2021-04-05 15:19
Location: /swiftwombat/how-to-create-own-property-wrapper-in-swift
Tags: Swift Wombat, SwiftUI, Property wrapper
---

# How to create own property wrapper

![How to create own property wrapper](/weblog/swiftwombat/covers/how_to_create_own_property_wrapper.png)

Swift (SwiftUI in particular) commonly uses property wrappers to make repeatable operations easier. For example, the @State is used for observing and storing view state, and @AppStorage to store app parameters using local persistence.

> Learn [how to use AppStorage to access UserDefaults in SwiftUI](/swiftwombat/how-to-access-userdefaults-using-swiftui-appstorage-property-wrapper/).

But you are not limited to build in property wrappers. You can make your property wrappers and add additional functionalities to struct and class properties (property wrappers are not supported at the top level).

This article will learn how to build a super simple property wrapper that will print the current value when you read or write it. It can help debug, but the goal is to show you how custom property wrappers work.

Property wrapper is a struct marked with `@propertyWrapper` attribute. It requires a `wrappedValue` property.

```swift
@propertyWrapper
struct CustomWrapper<Value> {
    var wrappedValue: Value
}
```

That's it. You can now use your property wrapper on any property.

```swift
@CustomWrapper var value = 0
```

But in the current state, it doesn't do anything. Let's add some logic when the wrapped value is set or get.

```swift
@propertyWrapper
struct Logged<Value> {
    private var value: Value

    init(wrappedValue: Value) {
        self.value = wrappedValue
    }

    var wrappedValue: Value {
        get {
            print("Value get: \(value)")
            return value
        }
        set {
            print("Value set: \(newValue)")
            value = newValue
        }
    }
}
```

When you try to read or change a given property, you will also see information about a current state on the console.

```swift
// Property wrappers are not supported on top level
struct Object {
    @Logged var value = 0
}

var object = Object()

print("Value is equal to: \(object.value)")
print("---")
object.value = 100
print("---")
print("Value is equal to: \(object.value)")

/*
Prints:
Value get: 0
Value is equal to: 0
---
Value set: 100
---
Value get: 100
Value is equal to: 100
*/
```

Of course, the provided example is elementary, but property wrappers can be used in various cases. Some libraries help with [data validation](https://github.com/SvenTiigi/ValidatedPropertyKit) and simplify usage of [codable](https://github.com/marksands/BetterCodable).

![Simple property wrapper example - Xcode Playground](/weblog/swiftwombat/images/23/simple_property_wrapper_xcode_playground.png)

Want to test it yourself? Download this [Xcode Project](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/CustomPropertyWrapper).
