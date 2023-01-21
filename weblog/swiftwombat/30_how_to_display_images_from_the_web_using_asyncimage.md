---
Date: 2021-06-12 16:54
Location: /swiftwombat/how-to-display-images-from-the-web-using-asyncimage-in-swiftui
Tags: Swift Wombat, SwiftUI, Image
---

# How to display images from the web using AsyncImage

![How to display images from the web using AsyncImage](/weblog/swiftwombat/covers/how_to_display_images_from_the_web_using_asyncimage.png)

Each WWDC brings lots of new goods for all Apple ecosystem developers. The 2021 edition wasn't different. AsyncImage view is a new SwiftUI addition that expects an URL instead of an asset to display images. Read further to learn how to use it.

Displaying images from the web is a widespread task in mobile applications. You can see this in all social media applications, web-based knowledge bases, or apps that stream files from servers. It requires more work than displaying images locally. After all, we have to wait until the image is loaded (usually displaying placeholder) and handling situations where the network connection is not reliable (or the file doesn't exist anymore). `AsyncView` is creating to make those operations easier. When it comes to displaying images, it uses an `Image` view underneath.

The simplest form of AsyncView requires only the `url` parameter with the URL object. This `url` parameter is optional, so you don't have to check its existence when creating it using the `URL(string:_)` constructor.

```swift
let url = URL(string: "http://placekitten.com/200/200")
AsyncImage(url: url)
```

But often, you may want to do more with a result. One of the most common scenarios was to resize the image to fit its frame.  The `AsyncImage` [first initializer](https://developer.apple.com/documentation/swiftui/asyncimage/init(url:scale:content:placeholder:)) can handle `scale` parameter and two convenient closures - `content` and `placeholder`. The first one performs operations on the `Image` view when it is finally loaded, and the second one returns a view displayed when an image is loading.

```swift
AsyncImage(url: url) { image in
    image.resizable()
} placeholder: {
    Text("Loading...")
}
```

With content closure, you can resize the image, fit it to content or apply rounded corners.

But there is a [second initializer](https://developer.apple.com/documentation/swiftui/asyncimage/init(url:scale:transaction:content:)) that allows for more advanced control. You will probably be using that version more often. It also supports `url` and `scale` properties, but there is a new `transaction` parameter, and content closure gets [AsyncImagePhase enum](https://developer.apple.com/documentation/swiftui/asyncimagephase). You can use the data provided in that enum to return a view for different phases of image loading. When the network file is loaded (`success` phase), you will get an `Image` view. For `failure` error is returned. There is a third empty phase which means that the file is loading. The `transaction` parameter is used to provide an animation applied when phases are changing.

```swift
AsyncImage(
    url: url,
    transaction: Transaction(
        animation: .easeInOut
    )
) { phase in
     if let image = phase.image {
         image.resizable()
    } else if phase.error != nil {
        Text("Error!")
    } else {
        Text("Loading...")
    }
}
```

![AsyncImage SwiftUI Playground](/weblog/swiftwombat/images/30/asyncimage_swiftui_playground.png)

Want to test it yourself? Download this [Xcode Playground](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/AsyncImage).
