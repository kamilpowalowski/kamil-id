---
Date: 2021-05-23 15:44
Location: /swiftwombat/how-to-use-personnamecomponentsformatter-in-swift
Tags: Swift Wombat, Swift
---

# How to use PersonNameComponentsFormatter

![How to use PersonNameComponentsFormatter](/weblog/swiftwombat/covers/how_to_use_personname_componentsformatter.png)

Foundation framework available for iOS, macOS, iPadOS, and other Apple platforms hides many gems that you can use to simplify operations required in nearly every application. One of the very often used sets of features are formatters. You probably are familiar with `DateFormatters`, but in today's article, I'll describe how to use `PersonNameComponentsFormatter`, which you can use to parse and format a person's name.

The structure of personal names varies between different locales. On the other hand, there are significant and meaningful for persons that use them. That's a reason you should treat them with respect and very carefully. `PersonNameComponentsFormatter` can help you with that. This formatter can parse string data provided and separated them into `PersonNameComponents`, and get that structure to display name using the correct locale.

First, you have to create an instance of the `PersonNameComponentsFormatter`:

```swift
let formatter = PersonNameComponentsFormatter()
```

To separate name string for components use `personNameComponents(from:)` function:

```swift
if let components = formatter.personNameComponents(
    from: "Sir David Frederick Attenborough"
) {
    print(components)
    /* Prints:
       namePrefix: Sir
       givenName: David
       middleName: Frederick
       familyName: Attenborough
    */
}
```

If you already have components (for example, created from the form data), you can use them with the same class to build different data representations. You can use one of 4 styles `short`, `medium(default)`, `long`, `abbreviated`.

```swift
formatter.style = .default
print(formatter.string(from: components))
// David Attenborough

formatter.style = .abbreviated
print(formatter.string(from: components))
// DA

formatter.style = .long
print(formatter.string(from: components))
// Sir David Frederick Attenborough

formatter.style = .medium
print(formatter.string(from: components))
// David Attenborough

formatter.style = .short
print(formatter.string(from: components))
// David
```

Additionally `PersonNameComponentsFormatter` can prepare Attributed String.

As you can see, `PersonNameComponentsFormatter` is very easy to use. With correctly collected or parsed data, you will create correct abbreviations or long-style formatted person details.

![PersonNameComponentsFormatter Xcode Playground](/weblog/swiftwombat/images/28/personnamecomponentsformatter_xcode_playground.png)

Want to test it yourself? Download this [Xcode Playground](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/PersonNameComponentsFormatter).
