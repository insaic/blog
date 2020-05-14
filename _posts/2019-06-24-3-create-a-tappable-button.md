---
layout: default
title: 4.3 创建一个点击按钮
category: swiftui-event
---

SwiftUI 的按钮类似于 `UIButton`，它不但可以在显示内容方面更灵活，而且它使用闭包来进行点击回调的相应操作。

先从以下代码创建一个按钮：

{% highlight swift %}
Button(action: {
    // your action here
}) {
    Text("Button title")
}
{% endhighlight %}

比如我们要点击查看更多内容，那么完整代码如下：

{% highlight swift %}
struct ContentView : View {
    @State var showDetails = false

    var body: some View {
        VStack {
            Button(action: {
                self.showDetails.toggle()
            }) {
                Text("Show details")
            }

            if showDetails {
                Text("You should follow me on Twitter: @twostraws")
                    .font(.largeTitle)
                    .lineLimit(nil)
            }
        }
    }
}
{% endhighlight %}

按钮里的文本视图，可以换做其它类型的视图，比如图片：

{% highlight swift %}
Button(action: {
    self.showDetails.toggle()
}) {
    Image("example-image")
}
{% endhighlight %}






