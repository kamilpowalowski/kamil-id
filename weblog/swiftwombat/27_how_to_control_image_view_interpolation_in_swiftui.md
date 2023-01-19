---
Date: 2021-05-09 15:16
Location: /swiftwombat/how-to-control-image-view-interpolation-in-swiftui
Tags: Swift Wombat, SwiftUI, Image, ViewModifier
---

# How to control Image view interpolation in SwiftUI

![How to control Image view interpolation in SwiftUI](/weblog/swiftwombat/covers/how_to_control_image_view_interpolation_in_swiftui.png)

When you are scaling up (and down) a small image in SwiftUI, there is an operation called interpolation. The size of a current image pixel is calculated based on neighbor pixels. It gave a more natural and better-looking effect, but it also means that the image appears blurry (look at the first image in the example above). More advanced interpolations also increase computational complexity.

Sometimes you want to display the image in a more pixelated form (second image on the example above). There is a view modifier for that. Its name is an `interpolation(...)` (surprise, surprise).

![Top: Interpolation - default, Bottom: Interpolation - none](/weblog/swiftwombat/images/27/image_interpolation_difference.png)

Top: Interpolation - default, Bottom: Interpolation - none

Using this modifier is very easy. It expects one of four `Interpolation` enum values: `high`, `medium`, `low`, and `none`. The first three will control interpolation quality, and the last one goes with the most straightforward approach of duplicating current pixel values (and gives pixelated effect).

To change interpolation quality, call the given view modifier on Image view:

```swift
Image("wombat")
    .interpolation(.none)
```

You will probably don't need that function too often, but it's good to know its existence and play with that settings a bit.

![Image interpolation Xcode playground](/weblog/swiftwombat/images/27/image_interpolation_xcode_playground.png)

Want to test it yourself? Download this [Xcode Playground](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/ImageInterpolation).
