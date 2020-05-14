---
layout: default
title: 3.7 如何将内容放在安全区域（safe area）之外
category: swiftui-view-layout
---

开头之前先看下什么去安全区域（safe area）：

![安全区域](/files/swiftUI/safe-area.png)

这个问题主要出现在非矩形屏幕上，默认情况下，视图内容是无法覆盖到顶头的刘海的。在 SwiftUI 情况下，内容是贴到屏幕最底的，但是一样无法贴到顶头刘海。如果想改变，比如把图片覆盖到真正的全屏幕上，那么可以使用 `.edgesIgnoringSafeArea()` 修改器。代码如下：

{% highlight swift %}
Text("Hello World")
    .frame(minWidth: 0, maxWidth: .infinity, minHeight: 0, maxHeight: .infinity)
    .background(Color.red)
    .edgesIgnoringSafeArea(.all)
{% endhighlight %}