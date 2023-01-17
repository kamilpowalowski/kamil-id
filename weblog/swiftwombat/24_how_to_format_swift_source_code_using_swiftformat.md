---
Date: 2021-04-11 14:04
Location: /swiftwombat/how-to-format-swift-source-code-using-swiftformat
Tags: Swift Wombat, Swift
---

# How to format Swift source code using SwiftFormat

![How to format Swift source code using SwiftFormat](/weblog/swiftwombat/covers/how_to_format_swift_source_code_using_swiftformat.png)

Command + I is a very commonly used Xcode shortcut. It is used to fix the indentation of the selected code part. But it does no more than that. What's worst, it adds four spaces (or one tab if you use it for indentation) to empty lines. This article will learn how to install, configure and use a third-party command-line application called SwiftFormat for better Swift source code formatting.

[SwiftFormat](https://github.com/nicklockwood/SwiftFormat) is an open-source project created and maintained by Nick Lockwood - creator of a few famous and commonly used iOS libraries. It formats your code using more than 50 rules which you can turn off and on individually. If you follow just the basic code formatting principles, you can be sure that SwiftFormat will handle the rest, giving you well-formatted code.

You can install SwiftFormat using one of the provided [methods](https://github.com/nicklockwood/SwiftFormat#how-do-i-install-it). I'll describe only one that is easiest for me - by using Homebrew.

You have to first [install Homebrew](https://brew.sh/) and then call

```bash
brew install swiftformat
```

And that's it. SwiftFormat is installed and ready to use.

To run SwiftFormat over your project, go in the console to project catalog and call:

```bash
swiftformat .
```

The presented command will format all `.swift` files into this and all subdirectories. For the first run, **I advise you to commit or stash your current code changes**.

If you go to [documentation](https://github.com/nicklockwood/SwiftFormat#options), you will find that you can control different properties. You can specify them using the command line argument of the `.swiftformat` file placed in the same director.

I like default settings, so the only thing I do is turn off formatting on some generated directories (like CoreData or Pods) and directly specify the Swift version. I made the `swiftformat.sh` file for this that looks similar to this one:

```bash
#!/bin/bash
# Install swiftformat via brew first with `brew install swiftformat`

swiftformat . --swiftversion 5.3 --exclude Shared/Resources,Shared/Models/CoreData --disable wrapMultilineStatementBraces
```
