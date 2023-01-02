---
Date: 2020-12-09 17:51
Location: /swiftwombat/how-to-preview-and-debug-widgetkit-views
Tags: Swift Wombat, SwiftUI, WidgetKit
---

# How to preview and debug WidgetKit views

![How to preview and debug WidgetKit views](/weblog/swiftwombat/covers/how_to_preview_and_debug_widgetkit_views.png)

SwiftUI offers an easy way to preview changes in your views without compiling and running the application. Introduced in iOS 14 and macOS 11, WidgetKit widgets are SwiftUI (static) views so that you can use the same functionality for them.You can display iOS and macOS widgets in three sizes called [WidgetFamiles](https://developer.apple.com/documentation/widgetkit/widgetfamily): `systemSmall`, `systemMedium`, and `systemLarge`. That information has to be provided to preview, and you can do that using [WidgetPreviewContext](https://developer.apple.com/documentation/widgetkit/widgetpreviewcontext).

```swift
struct Widget_Previews: PreviewProvider {
    static var previews: some View {
        Group {
            MyWidgetView()
                .previewContext(WidgetPreviewContext(family: .systemSmall))
            MyWidgetView()
                .previewContext(WidgetPreviewContext(family: .systemMedium))
            MyWidgetView()
                .previewContext(WidgetPreviewContext(family: .systemLarge))
        }
    }
}
```

In real-life examples, you will probably need to provide an Entry configuration to MyWidgetView.

---

WidgetKit Simulator App is a superior solution that will display placeholders, timelines, snapshots for all widget states. Unfortunately, you can use it only to test macOS widgets. For iOS and iPad widgets, you will have to use a simulator or a real device. With WidgetExtension target Environment Variables, you can control kind `_XCWidgetKind` (if you have more than one widget in your app) and family `_XCWidgetFamily` (with `small`, `medium`, or `large` values) of a displayed widget.

To set those values, select your WidgetExtension scheme and choose `Edit scheme....` Next, go to the Arguments section, where you can find `_XCWidgetFamily`, `_XCWidgetKind`, and `_XCWidgetDefaultView` in the Environment Variables section.

![_XCWidgetFamily, _XCWidgetKind, and _XCWidgetDefaultView  in the Environment Variables section](/weblog/swiftwombat/images/2/xcwidgetfamily_set_example.png)

For more information about that topic, visit [this official article](https://developer.apple.com/documentation/widgetkit/debugging-widgets).
