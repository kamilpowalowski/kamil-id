---
Date: 2021-09-11 14:06
Location: /swiftwombat/how-to-add-keyboard-toolbar-actions-in-swiftui
Tags: Swift Wombat, SwiftUI, Toolbar, ViewModifier, Button, Property wrapper
---

# How to add keyboard toolbar actions in SwiftUI

![How to add keyboard toolbar actions in SwiftUI](/weblog/swiftwombat/covers/how_to_add_keyboard_toolbar_actions_in_swiftui.png)

SwiftUI contains a clever mechanism for adding toolbar actions. With one view modifier - `toolbar`, we can control toolbar items in different places of the application. It doesn't matter that you want to add a toolbar to the macOS navigation bar, iOS navigation bar, or iOS bottom bar; you will always start with .`toolbar { }`.

SwiftUI 3 brings additional, longly awaited functionality - the possibility to displays toolbar actions above the system keyboard. You probably expect that already, but adding views to the keyboard toolbar is also very easy. Let's take a look at a provided example.

![Toolbar over keyboard in SwiftUI](/weblog/swiftwombat/images/35/keyboard_toolbar_screenshot.png)

To show this functionality, I created a small project with the TextEditor and three keyboard actions - two of them put ASCI emojis in TextEditor, and the third one hides the keyboard.

```swift
struct ContentView: View {
    @State private var text = ""
    @FocusState private var focus: Bool
    
    var body: some View {
        TextEditor(text: $text)
            .focused($focus)
            .frame(height: 150)
            .padding()
            .overlay(
                RoundedRectangle(cornerRadius: 8)
                    .stroke(Color.primary, lineWidth: 1)
            )
            .padding()
    }
}
```

Code above displays `TextEditor` with some basic styling for better visibility. I also added the `@FocusState` property wrapper, which helps programmatically displaying and hiding the keyboard. The `@FocusState` is also a new addition in SwiftUI 3, but I'll prepare a separate article about it.

```swift
.toolbar {
    ToolbarItemGroup(placement: .keyboard) {
        Button("Shrug") { text += "\n¯\\_(ツ)_/¯" }
        Button("Flip") { text += "\n(╯°□°）╯︵ ┻━┻" }
        Spacer()
        Button("Hide") { focus = false }
    }
}
```

I told you it would be simple. In the content of the `toolbar` view modifier, you have to put `ToolbarItemGroup` view with placement parameter set to keyboard. You can place any View in the content ViewBuilder of `ToolbarItemGroup`. Usually, you will want to use `Buttons` and `Spacers` to align them properly.

The first two buttons modify text property by adding correct ASCI characters; the third sets focus property to false. Calling this will hide the keyboard if it's visible.

Below you can find the complete code of this experiment and a link to the Xcode project.

```swift
import SwiftUI

struct ContentView: View {
    @State private var text = ""
    @FocusState private var focus: Bool
    
    var body: some View {
        TextEditor(text: $text)
            .focused($focus)
            .frame(height: 150)
            .padding()
            .overlay(
                RoundedRectangle(cornerRadius: 8)
                    .stroke(Color.primary, lineWidth: 1)
            )
            .padding()
            .toolbar {
                ToolbarItemGroup(placement: .keyboard) {
                    Button("Shrug") { text += "\n¯\\_(ツ)_/¯" }
                    Button("Flip") { text += "\n(╯°□°）╯︵ ┻━┻" }
                    Spacer()
                    Button("Hide") { focus = false }
                }
            }
    }
}
```

![Keyboard Toolbar Xcode Project](/weblog/swiftwombat/images/35/keyboard_toolbar_xcode_project.png)

Want to test it yourself? Download this [Xcode Project](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/KeyboardToolbar/).
