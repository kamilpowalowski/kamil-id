---
Date: 2021-02-15 18:27
Location: /swiftwombat/how-to-style-text-view-in-swiftui
Tags: Swift Wombat, SwiftUI, Text, ViewModifier
---

# How to style Text view in SwiftUI

![How to style Text view in SwiftUI](/weblog/swiftwombat/covers/how_to_style_text_view_in_swiftui.png)

You can style SwiftUI Text views with a bunch of dedicated ViewModifiers. Learn about them in this article.

![Text styles covered in this article](/weblog/swiftwombat/images/18/text_styles.png)

## How to apply italic text style?

You can switch the font to cursive using the `italic` modifier.

```swift
Text("italic")
    .italic()
```

## How to apply bold text style?

The `bold` view modifier will make the displayed text bolder.

```swift
Text("bold")
    .bold()
```

Of course, you can stack these two modifiers to create an italic and bold typeface.

```swift
Text("bold & italic")
    .bold()
    .italic()
```

## How to change font weight?

If you want to set your font to something different than bold, you can use the `weight` modifier.

```swift
Text("fontWeight heavy")
    .fontWeight(.heavy)
```

## How to change the font?

The `font` modifier is applicable if you want to change more text parameters. There are predefined fonts that you can use for various elements of the app. In the example below `title` font is set.

```swift
Text("font .title")
    .font(.title)
```

You can create own Font struct to pick different font family, size or weight.

## How to strikethrough a text?

There are two versions of the `strikethrough` modifier. One without argument that displays a line in the foreground color:

```swift
Text("strikethrough")
    .strikethrough()
```

The second one with an `active` parameter and color option where you can control a strikethrough color.

```swift
Text("strikethrough color")
    .strikethrough(true, color: .red)
```

## How to underline a text?

Instead of strikethrough, you may want to underline a text. API for this is very similar. More straightforward underline modifier takes foreground color value:

```swift
Text("underline")
    .underline()
```

And more advance has active and color parameters:

```swift
Text("underline color")
    .underline(true, color: .red)
```

## How to change the text color?

Text view content color can be changed using the `foregroundColor` modifier.

```swift
Text("foregroundColor")
    .foregroundColor(.red)
```

> Learn [how to colorize text using a gradient](https://swiftwombat.com/how-to-create-custom-viewmodifier/).

## How to change the Text view background color?

The background of any view can be applied using the `background` modifier. Since SwiftUI Color is also a view, you can use it to apply a color background below the text.

```swift
Text("background")
    .background(Color.red)
```

## How to change the baseline offset of text?

The baseline can be used to display your text above or below the normal text baseline. The `baselineOffset` view modifier accept positive and negative numbers. You will often use it with Text view concatenation with `+` sign.

```swift
Text("baseline ")
   + Text("Off")
       .baselineOffset(10)
   + Text("set")
       .baselineOffset(-10)
```

## How to adjust kerning?

You can adjust kerning (a distance between pair of letters) with the kerning modifier.

```swift
Text("kerning kerning kerning")
    .kerning(1.2)
```

## How to adjust tracking?

Tracking delivers a similar effect to kerning, but it adds distance between each character, which means that ligatures will also be separated.

```swift
Text("tracking tracking tracking")
    .tracking(1.2)
```

![Text styles examples in Xcode Playground](/weblog/swiftwombat/images/18/text_view_styles_xcode_example.png)

Want to test it yourself? Download this [Xcode Playground](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/TextStyles/).
