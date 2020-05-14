---
layout: default
title: 1.2 关于 SwiftUI 的常见问题
category: swiftui-introduction
---

### 学习 Swift 还是 UIKit ？

目前大多数 iOS 应用程序都是使用 UIKit 编写的，有些则是使用了其它技术栈，比如 React Native， Flutter 等，但是他们有点犯了舍入误差（rounding error）的错误，因为就我们的认知而言，UIKit 和 iOS 应用程序应是齐头并进的。

所以如果你忽略了学习 SwiftUI 一年，两年，甚至更长时间，没有人会认为你会因此而丢掉工作，因为 iOS 开发社区也不太可能大规模用最快的速度全部迁移到 SwiftUI，因为大多数公司他们已经在 UIKit 应用程序中投入了大量的人力，时间，金钱，多少都达到了稳定的效果，如果转移到 SwiftUI 会涉及到迁移成本以及未知风险。

另一方面说，如果你对 iOS 开发很感兴趣，并且对 UIKit 不太擅长，或者根本就没学过，那么则可以专注于 SwiftUI，当然短时间内也别期望靠着 SwiftUI 找到工作，因为这玩意儿发布不久，距离大规模的商用，还需要很长一段时间。可以参考下，Swift 这门语言已经推出 5 年了，但是至今依然无法完全取代 Objective-C。


### SwiftUI 可以使用在哪些地方？

SwiftUI 的运行平台包括 iOS 13, macOS 10.15, tvOS 13 和 and watchOS 6 及它们的未来版本。这意味着，如果你想让你的应用向下兼容 1-2 个版本，则必须等待一两年的时间才能够迁移到 SwiftUI 上。

但是你不要认为，SwiftUI 是一个像 Java 的 Swing 或者 React Native 一样的跨平台（系统派系, win, mac, android ...）框架，至少从目前的官方描述来看，它不是一个传统的跨平台框架，它只能在 Apple 的平台上创建跨平台应用。

但是呢，这个跨平台的区别，也不只是平台的区别。有一个重要的区别就是，Apple 没有说你可以在不同平台用相同的 SwiftUI 代码，因为这需要考虑到平台环境，比如在 Mac 上就无法使用 Apple Watch 的表冠（Digital Crown），同样的也无法在 watchOS 上使用 tab bar。

### SwiftUI 会取代 UIKit 吗？

不会，许多的 SwiftUI 是在现有的 UIKit 上“封装”而来的，比如 `UITableView`。当然也有其它的很多不是，他们是 SwiftUI 独有的控件，而 UIKit 没有。但这不是重点，虽然 SwiftUI 或多或少都抢了 UIKit 的风头，但目前苹果并没有让 UIKit 完全消失的意思，而是将 UIKit 的用法与属性暴露给了 SwiftUI。


### SwiftUI 是使用自动布局（Auto Layout）吗？

针对 SwiftUI 而言不是（虽然某些场景幕后使用的还是自动布局），SwiftUI 使用的是弹性盒子布局（Flexible Box Layout），这对于前端人员来说不陌生。


### SwiftUI 快吗？

在我迄今为止的所有测试中，SwiftUI 可以说是非常的快，它似乎超过了 UIKit。在与 SwiftUI 的团队的交流中我也略知一二原因。首先，他们积极地压平了结构层次，因此系统只需做更少的绘图工作。再者，很多操作完全绕过了核心动画（Core Animation）。

所以在无多做额外工作的情况下，SwiftUI 的速度就非常快了。


### Xcode 看不到代码预览效果？

升级到 macOS 10.15。

### 代码跟预览的匹配程度如何？

当你修改代码的时候，预览也会同步更新，二者是保持同步的。

### 为什么颜色看起来有点偏差？

SwiftUI 为我们提供了标准的系统颜色，如红色，蓝色和绿色，但这些跟你平常习惯使用的 `UIColor` 不太一样。这是因为 SwiftUI 提供的颜色会自动适应明亮或黑暗风格，所以根据系统的外观风格设置，有时候看起来会明亮一些，有时候会暗淡一些。

### UIKit 退休了吗？

没有，因为在此次的 WWDC 上 Apple 还介绍了大量的 UIKit 新功能。只要是 Apple 官方还在讨论 UIKit，那么它就是还在岗，所以不需要担心。



