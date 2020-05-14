---
layout: default
title: 12.5 添加或移除视图的时候使用动画

category: swiftui-animation
---

添加或删除视图，是一种常见的操作。例如下面一个例子，点击一个按钮时，会添加或删除文本详情视图：

{% highlight swift %}
struct ContentView: View {
    @State var showDetails = false

    var body: some View {
        VStack {
            Button(action: {
                withAnimation {
                    self.showDetails.toggle()
                }
            }) {
                Text("Tap to show details")
            }

            if showDetails {
                Text("Details go here.")
            }
        }
    }
}
{% endhighlight %}

默认情况下，SwiftUI 会使用淡入淡出的动画来添加或删除视图。但是如果需要定制化，可以使用 `transition()` 修改器来实现。

比如对于这个文本详情的动画我们不想使用淡入淡出，要使用从底部滑入滑出的方式，代码如下：

{% highlight swift %}
Text("Details go here.")
    .transition(.move(edge: .bottom))
{% endhighlight %}

也有一个 `.slide` 动画转场，视图动画从左侧滑入，右侧滑出，如下：

{% highlight swift %}
Text("Details go here.")
    .transition(.slide)
{% endhighlight %}

另一个叫做 `.scale` 动画转场，视图动画从最小放大到正常大小，再从正常缩小到无，如下：

{% highlight swift %}
Text("Details go here.")
    .transition(.scale())
{% endhighlight %}