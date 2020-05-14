---
layout: default
title: 11.11 调整视图的强调色（accent color）

category: swiftui-transform
---

iOS 有一套默认的主题颜色，可操作部分的控件以蓝色为主。在 SwiftUI 环境里也有同样的功能，叫做强调色（ accent colors），使用 `accentColor()` 修改器。跟 UIKit 一样，当你在一个视图设置强调色的时候，它会影响到该视图的子视图，颜色都会跟着变。

举个例子，在 VStask 中创建一个按钮，然后设置一个橙色的强调色，如下：

{% highlight swift %}
VStack {
    Button(action: {}) {
        Text("Tap here")
    }
}.accentColor(Color.orange)
{% endhighlight %}