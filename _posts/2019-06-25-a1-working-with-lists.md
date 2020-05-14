---
layout: default
title: 6.1 使用列表视图（List）
category: swiftui-lists
---

SwiftUI 的 `List` 类似于 UIKit 的 `UITableView`，主要用来创建静态或动态的单元格列表视图。但是它用起来非常简单，我们不需要在 storyboard 创建单元格原型，也不需要注册它们，也不需要告诉 `UITableView` 有多少个单元格，也不需要手动去配置单元格，等等。

相反的，SwiftUI的列表视图是一种可组合的设计，可以用小东西来构建大东西。不是一个需要手动来配置的大型视图控制器。而是我们先定义一些小的视图，然后放在一个列表（List）里。

在代码方面，差距巨大，相比 UIKit 几乎可以删除所有的表格视图代码，并且外观仍然相同。