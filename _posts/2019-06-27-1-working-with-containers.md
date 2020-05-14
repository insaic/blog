---
layout: default
title: 8.1 使用视图容器
category: swiftui-containers
---

SwiftUI 的设计属于开箱即用，这意味着你可以根据需要将一个视图放在另一个视图中。

这在我们经常使用的主要容器视图中尤其有用，例如导航控制器和标签栏控制器。 我们可以将我们想要的任何视图放到另一个容器视图中，SwiftUI 将自动调整其布局。

在这方面，SwiftUI 自己的视图容器有 `NavigationView`，`TabbedView`，`Group` 等等。这跟我们自己设定的视图没什么两样。