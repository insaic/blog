---
layout: default
title: 12.3 状态改变的时候附加动画效果

category: swiftui-animation
---

SwiftUI 的双向绑定可以让我们很好地管理程序的各种状态，例如在视图层面，我们可以使某些视图显示或消失，或调整视图的透明度。

但是状态的更改是没有动画的，不过我们可以通过添加 `animation()` 修改器来实现动画。例如，我们先创建一个切换按钮，然后根据按钮的切换来显示或隐藏某个文本视图：

{% highlight swift %}
struct ContentView : View {
    @State var showingWelcome = false

    var body: some View {
        VStack {
            Toggle(isOn: $showingWelcome) {
                Text("Toggle label")
            }

            if showingWelcome {
                Text("Hello World")
            }
        }
    }
}
{% endhighlight %}

运行这段代码就会发现，文本的显示或消失，是没有动画的。如需动画，则添加动画修改器 `$showingWelcome.animation()`，那这样子就有动画了。

{% highlight swift %}
Toggle(isOn: $showingWelcome.animation()) {
    Text("Toggle label")
}
{% endhighlight %}

当然这个 `animation()` 修改器也是支持参数的，比如加个弹簧效果。如下：

{% highlight swift %}
Toggle(isOn: $showingWelcome.animation(.spring())) {
    Text("Toggle label")
}
{% endhighlight %}