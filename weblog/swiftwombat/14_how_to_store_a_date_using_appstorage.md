---
Date: 2021-01-25 11:29
Location: /swiftwombat/how-to-store-a-date-using-appstorage-in-swiftui
Tags: Swift Wombat, SwiftUI, Date, Property wrapper
---

# How to store a Date using AppStorage

![How to store a Date using AppStorage](/weblog/swiftwombat/covers/how_to_store_a_date_using_appstorage.png)

By default, `@AppStorage` property wrapper supports `Int`, `String`, `Double`, `Data`, or `URL` values. You can store other classes/structs/enums if you make them conform to the `RawRepresentable` protocol. This tutorial will learn how to keep Dates in `UserDefaults` handled by `@AppStorage`.

> Learn [basics of @AppStorage property wrapper](/swiftwombat/how-to-access-userdefaults-using-swiftui-appstorage-property-wrapper/).

If you write something like this:

```swift
@AppStorage("savedDate") var date: Date = Date()
```

the Swift compiler gives you an error because the `@AppStorage` property wrapper doesn't support Date type.

But it supports objects that conform to `RawRepresentable` protocol, where that raw value is a String or Int. A proposed implementation may look like this:

```swift
import Foundation

extension Date: RawRepresentable {
    private static let formatter = ISO8601DateFormatter()
    
    public var rawValue: String {
        Date.formatter.string(from: self)
    }
    
    public init?(rawValue: String) {
        self = Date.formatter.date(from: rawValue) ?? Date()
    }
}
```

This code uses `ISO8601DateFormatter` to format a date to String and map it back. That formatter is static because creating and removing `DateFormatters` is an expensive operation. If you add the code above to your project, you will be able to read and store dates in your SwiftUI app.

```swift
struct DateView: View {
    @AppStorage("savedDate") var date: Date = Date()
    
    var body: some View {
        VStack {
            Text(date, style: .date)
            Text(date, style: .time)
            Button("Save date") { date = Date() }
        }
    }
}
```

![AppStorage Date example in Xcode Project](/weblog/swiftwombat/images/14/appstorage_date_xcode_example_project.png)

Want to test it yourself? Download this [Xcode project](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/AppStorage/).
