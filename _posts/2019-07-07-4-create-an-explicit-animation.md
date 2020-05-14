---
layout: default
title: 12.4 如何创建显式动画

category: swiftui-animation
---

如果将动画修改器附加到视图，最终会是一个隐式动画。隐式动画就是我们并没有指定任何动画的类型。我们仅仅改变了一些属性，然后系统来决定如何并且何时去实现动画。

而显式动画，它能够对一些属性做指定的自定义动画，或者创建非线性动画，比如沿着任意一条曲线移动。我们可以使用 `withAnimation()` 修改器来让 SwiftUI 进行精确的动画设定。

例如，使用显式动画来让按钮逐渐消失：

{% highlight swift %}
struct ContentView: View {
    @State var opacity: Double = 1

    var body: some View {
        Button(action: {
            withAnimation {
                self.opacity -= 0.2
            }
        }) {
            Text("Tap here")
                .padding()
                .opacity(opacity)
        }
    }
}
{% endhighlight %}

`withAnimation()` 接受一个所需动画类型的参数，因此可以创建一个 3 秒钟的基本动画，如下所示：

{% highlight swift %}
withAnimation(.basic(duration: 3)) {
    self.opacity -= 0.2
}
{% endhighlight %}

显式动画通常很有用，他们它会使每个受影响的视图都出现动画，而不仅仅是哪些附加了隐式动画的动画。例如，如果视图 A 必须为视图 B 的动画腾出空间，但是只有视图 B 有动画效果，那么就必须使用显示动画，不然视图 A 会直接无动画地跳到新的位置。