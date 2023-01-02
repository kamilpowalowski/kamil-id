---
Date: 2020-12-19 21:05
Location: /swiftwombat/how-to-display-scale-and-resize-an-image
Tags: Swift Wombat, SwiftUI, Image, ViewModifier
---

# How to display, scale, and resize an image

![How to display, scale, and resize an image](/weblog/swiftwombat/covers/how_to_display_scale_and_resize_an_image.png)

Images, along with shapes, colors, and texts, are essential ingredients of all applications UIs. Displaying images in SwiftUI is easy. You have to add your image file to the assets catalog and then use Image view to present it on screen.

```swift
Image("logo")
```

## Resize

You will quickly notice that the image doesn't fit your view. It works that way because raw Image view will display assets in real size. To fix this, you have to apply a resizable modifier. You usually will be using the default parameters, but you can control which part of the image is resizable and decide how an image should arrange the space (`stretch` or `tile`).

```swift
Image("logo")
    .resizable()
```

There is a high chance that an effect still won't fit your needs. Your image file may have a different aspect ratio than the space that the Image view has. You can use one of the three modifiers below to fix that issue.

## Scale to Fit

A `scaledToFit` modifier will make your image to fit fully into a space given by Image view. It will preserve an aspect ratio of an original file. It may result in empty spaces on the bottom and top (or left and right) of that view.

```swift
Image("logo")
    .resizable()
    .scaledToFit()
```

## Scale to Fill

If you don't want empty spaces and it is not that important to you to be sure that the image is visible entirely, you can use the `scaledToFill` modifier. With it, your file will fill a full view, its aspect ratio also will be preserved, but it may be cropped on edges.

```swift
Image("logo")
    .resizable()
    .scaledToFill()
```

> Resizable and scaled modifiers often came together. Learn how to refactor a code and [create a custom modifier](/swiftwombat/how-to-create-custom-viewmodifier/).

## Custom Aspect Ratio

With the aspectRatio modifier, you can get more control over how an image is displayed. You can provide two parameters to its ratio of width to height (unnamed parameter represented `aspectRatio`) and `contentMode` (it can be set to `fit` or `fill`).

```swift
Image("logo")
    .resizable()
    .aspectRatio(6, contentMode: .fit)
```

The `contentMode` itself allows you can achieve the same effect that `scaledToFill` and `scaledToFit` offers. But since those two are more readable, use them first and `aspectRatio` modifier only when necessary.

![Example of various resize options](/weblog/swiftwombat/images/4/image_resize_example.png)

Want to test it yourself? Download this [Xcode Playground](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/ImageResize/).
