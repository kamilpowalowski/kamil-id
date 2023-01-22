---
Date: 2021-08-08 15:02
Location: /swiftwombat/how-to-expand-and-collapse-cells-in-swiftui
Tags: Swift Wombat, SwiftUI, ForEach, Animation
---

# How to expand and collapse cells in SwiftUI

![How to expand and collapse cells in SwiftUI](/weblog/swiftwombat/covers/how_to_expand_and_collapse_cells_in_swiftui.png)

Animations are a crucial but often neglected part of good UI. Correctly used can guide used in the app. In this article, you will learn how to make a list cell that expands on tap and apply animation to it.

We will create a simple Todo List with subtasks.

<div style="padding:216.41% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/584498693?h=895b305edf&amp;badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen style="position:absolute;top:0;left:0;width:100%;height:100%;" title="How to expand and collapse cells in SwiftUI"></iframe></div><script src="https://player.vimeo.com/api/player.js"></script>

## Models

First of all, let's make a models for this data.

```swift
struct Task: Identifiable {
    let id: String = UUID().uuidString
    let title: String
    let subtask: [Subtask]
}

struct Subtask: Identifiable {
    let id: String = UUID().uuidString
    let title: String
}
```

Of course, it won't be a fully working example. In the final application, you will probably need additional fields for selected state, deadline, etc.

## Cells

Then let's focus on cells. I decided to go with three separate cells designs. One for `Subtask` model:

```swift
struct SubtaskCell: View {
    let task: Subtask
    
    var body: some View {
        HStack {
            Image(systemName: "circle")
                .foregroundColor(Color.primary.opacity(0.2))
            Text(task.title)
        }
    }
}
```

one when a new/empty subtask:

```swift
struct EmptySubtaskCell: View {
    @State private var text: String = ""
    
    var body: some View {
        HStack {
            Image(systemName: "circle")
                .foregroundColor(Color.primary.opacity(0.2))
            TextField("new task", text: $text)
        }
    }
}
```

and one for a `Task` model:

```swift
struct TaskCell: View {
    @State private var isExpanded: Bool = false
    
    let task: Task
    
    var body: some View {
        content
            .padding(.leading)
            .frame(maxWidth: .infinity)
    }
    
    private var content: some View {
        VStack(alignment: .leading, spacing: 8) {
            header
            if isExpanded {
                Group {
                    ForEach(task.subtask) { subtask in
                        SubtaskCell(task: subtask)
                    }
                    EmptySubtaskCell()
                }
                .padding(.leading)
            }
            Divider()
        }
    }
    
    private var header: some View {
        HStack {
            Image(systemName: "square")
                .foregroundColor(Color.primary.opacity(0.2))
            Text(task.title)
        }
        .padding(.vertical, 4)
        .onTapGesture {
            withAnimation { isExpanded.toggle() }
        }
    }
}
```

That last one will support expanding and collapsing, so it's a bit bigger. Let's make a small dissection.

In the body, we load content and ensure that the cell takes the entire width of a given view.

```swift
var body: some View {
    content
        .padding(.leading)
        .frame(maxWidth: .infinity)
}
```

The `TaskCell` will store information about its state. You can find the `isExpanded` `@State` property wrapper on the top of it.

```swift
@State private var isExpanded: Bool = false
```

The cell's content displays a header where the title and checkbox are displayed and subtasks if the cell is expanded. At the bottom, it also shows the `Divider` to separate one task from another.

```swift
private var content: some View {
    VStack(alignment: .leading, spacing: 8) {
        header
        if isExpanded {
            Group {
                ForEach(task.subtask) { subtask in
                    SubtaskCell(task: subtask)
                }
                EmptySubtaskCell()
            }
            .padding(.leading)
        }
        Divider()
    }
}
```

The cell's header is similar to the `SubtaskCell`, but it displays a square image instead of a circle. It also has the `onTapGesture` method, which toggles the `isExpanded` state.

```swift
 private var header: some View {
    HStack {
        Image(systemName: "square")
            .foregroundColor(Color.primary.opacity(0.2))
        Text(task.title)
    }
    .padding(.vertical, 4)
    .onTapGesture { isExpanded.toggle() }
}
```

## ToDo List

With all these cells, you are ready to create a final view.

```swift
struct TasksView: View {
    private let tasks: [Task] = [
        Task(title: "Create playground", subtask: []),
        Task(title: "Write article", subtask: []),
        Task(
            title: "Prepare assets",
            subtask: [
                Subtask(title: "Cover image"),
                Subtask(title: "Screenshots")
            ]
        ),
        Task(title: "Publish article", subtask: [])
    ]
    
    var body: some View {
        NavigationView {
            ScrollView {
                ForEach(tasks) { task in
                    TaskCell(task: task)
                        .animation(.default)
                }
                .navigationTitle("Todo List")
            }
        }
    }
}
```

At the top are some static `Tasks` hardcoded (for the production-ready application, you should store them in application state.)

Body list all tasks using the `ForEach` view. And how I achieved this animation effect, you may ask. It was as simple as adding the `.animation(.default)` view modifier to the `TaskCell` view.

## Summary

By following this tutorial, you will achieve an effect presented at the top of the article. With the help of SwiftUI, adding animations that delights the user experience can be straightforward.

You can find the full code of this project below:

```swift
struct Task: Identifiable {
    let id: String = UUID().uuidString
    let title: String
    let subtask: [Subtask]
}

struct Subtask: Identifiable {
    let id: String = UUID().uuidString
    let title: String
}

struct TasksView: View {
    private let tasks: [Task] = [
        Task(title: "Create playground", subtask: []),
        Task(title: "Write article", subtask: []),
        Task(
            title: "Prepare assets",
            subtask: [
                Subtask(title: "Cover image"),
                Subtask(title: "Screenshots")
            ]
        ),
        Task(title: "Publish article", subtask: [])
    ]
    
    var body: some View {
        NavigationView {
            ScrollView {
                ForEach(tasks) { task in
                    TaskCell(task: task)
                        .animation(.default)
                }
                .navigationTitle("Todo List")
            }
        }
    }
}

struct TaskCell: View {
    @State private var isExpanded: Bool = false
    
    let task: Task
    
    var body: some View {
        content
            .padding(.leading)
            .frame(maxWidth: .infinity)
    }
    
    private var content: some View {
        VStack(alignment: .leading, spacing: 8) {
            header
            if isExpanded {
                Group {
                    ForEach(task.subtask) { subtask in
                        SubtaskCell(task: subtask)
                    }
                    EmptySubtaskCell()
                }
                .padding(.leading)
            }
            Divider()
        }
    }
    
    private var header: some View {
        HStack {
            Image(systemName: "square")
                .foregroundColor(Color.primary.opacity(0.2))
            Text(task.title)
        }
        .padding(.vertical, 4)
        .onTapGesture { isExpanded.toggle() }
    }
}

struct SubtaskCell: View {
    let task: Subtask
    
    var body: some View {
        HStack {
            Image(systemName: "circle")
                .foregroundColor(Color.primary.opacity(0.2))
            Text(task.title)
        }
    }
}

struct EmptySubtaskCell: View {
    @State private var text: String = ""
    
    var body: some View {
        HStack {
            Image(systemName: "circle")
                .foregroundColor(Color.primary.opacity(0.2))
            TextField("new task", text: $text)
        }
    }
}
```

Want to test it yourself? Download this [Xcode Project](https://github.com/kamilpowalowski/swiftwombat-projects/tree/main/ExpandableCell/).
