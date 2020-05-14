---
layout: default
title: 3.6 根据屏幕尺寸创建不同的视图
category: swiftui-view-layout
---

SwiftUI 支持 Size Classes ，它可以把屏幕的宽高分成紧凑（Compact）和正常（Regular），如下图所示：

![Size Classes](/files/swiftUI/size.jpg)

如何使用？首先创建一个 `@Environment` 对象，然后根据返回的值，显示相应的视图。如下：

{% highlight swift %}
struct ContentView : View {
    @Environment(\.horizontalSizeClass) var horizontalSizeClass: UserInterfaceSizeClass?

    var body: some View {
        if horizontalSizeClass == .compact {
            return Text("Compact")
        } else {
            return Text("Regular")
        }
    }
}
{% endhighlight %}

这段代码，虽然指定的是 `some View` 和随机返回，但是注意返回的都是 `Text` 视图。上述的 `.horizontalSizeClass` 是横向的，如果是想判断纵向的则可以使用 `.verticalSizeClass`。
