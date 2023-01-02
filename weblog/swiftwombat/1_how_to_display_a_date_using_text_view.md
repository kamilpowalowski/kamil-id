---
Date: 2020-12-06 16:20
Location: /swiftwombat/how-to-display-a-date-using-text-view
Tags: Swift Wombat, Text, SwiftUI, String, Date, WidgetKit
---

# How to display a date using Text view

![How to display a date using Text view](/weblog/swiftwombat/covers/how_to_display_a_date_using_text_view.png)

SwiftUI has a very convenient way to display dates. It provides a special [Text initializer](https://developer.apple.com/documentation/swiftui/text/init%28_:style:%29) that receives date and style. You can pick from a list of five available [DateStyles](https://developer.apple.com/documentation/swiftui/text/datestyle): `time`, `date`, `relative`, `offset`, and `timer`. Each one displays time in a different format.

```swift
let now = Date() // Dec 6, 2020 at 2:51 PM
let twoDays: TimeInterval = 2 * 24 * 60 * 60
let date = now.advanced(by: twoDays) // Dec 8, 2020 at 2:51 PM

struct ContentView: View {
    var body: some View {
        VStack {
            Text(date, style: .time) // 2:51 PM
            Text(date, style: .date) // December 8, 2020
            Text(date, style: .relative) // 1 day, 23 hr
            Text(date, style: .offset) // -1 day
            Text(date, style: .timer) // 47:59:50
        }
    }
}
```

Three date styles: `relative`, `offset`, and `timer`, are dynamic styles and can display values that change over time. There are very useful if you want to create a widget using WidgetKit that counts down a time. Currently, this is the only way to display non-static elements in system widgets.

---

Two additional initializers handle ranges of date. They support [DateIntervals](https://developer.apple.com/documentation/swiftui/text/init(_:)-56n81) and [ClosedRange of dates](https://developer.apple.com/documentation/swiftui/text/init(_:)-4k7ab). These two works very similar and you can use the one that's suits you best. As you can see in the example below, they modify its looks based on the given dates.

```swift
let now = Date() // Dec 6, 2020 at 2:51 PM
let hundredMinutes: TimeInterval = 100 * 60
let twoDays: TimeInterval = 2 * 24 * 60 * 60
let date = now.advanced(by: twoDays) // Dec 8, 2020 at 2:51 PM

struct ContentView: View {
    var body: some View {
        VStack {
            Text(DateInterval(start: now, duration: twoDays)) // Dec 6 - Dec 8
            Text(DateInterval(start: now, duration: hundredMinutes)) // 2:51-4:31 PM
            Text(now...date) // Dec 6 - Dec 8
        }
    }
}
```

---

You can use Swift string interpolation to display DateStyle text inside other strings. You will find that useful when you want to put your date in the middle of another sentence.

```swift
let now = Date() // Dec 6, 2020 at 2:51 PM
let twoDays: TimeInterval = 2 * 24 * 60 * 60
let date = now.advanced(by: twoDays) // Dec 8, 2020 at 2:51 PM

struct ContentView: View {
    var body: some View {
        Text("Styles can be used inside other strings with string interpolation: \(date, style: .relative)")
        // Output: Styles can be used inside other strings with string interpolation: 1 day, 23 hr
    }
}
```

---

Unfortunately, not all DateStyles are localized. Offset and relative always displays its value in English, no matter what language your project supports.

---

![Text and Date example taken in Xcode Playground](/weblog/swiftwombat/images/1/text_view_and_date_example.png)

Want to test it yourself? Download this [Xcode Playground](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/TextWithDate/).
