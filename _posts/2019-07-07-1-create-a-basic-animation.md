---
layout: default
title: 12.1 为视图添加动画

category: swiftui-animation
---

SwiftUI 支持动画功能，实现起来也很简单，只需要为视图添加一个 `animation()` 修改器，并且设置动画参数。

举个例子，创建一个按钮，点击一次把按钮本身放大一倍，如下：

{% highlight swift %}
struct ContentView: View {
    @State var scale: Length = 1

    var body: some View {
        Button(action: {
            self.scale += 1
        }) {
            Text("Tap here")
                .scaleEffect(scale)
                .animation(.basic())
        }
    }
}
{% endhighlight %}

`.basic()` 是基础类型的动画。动画有默认的转场时间，如果需要设定转场时间，可以添加 `duration` 参数，单位是秒，如下：

{% highlight swift %}
Text("Tap here")
    .scaleEffect(scale)
    .animation(.basic(duration: 3))
{% endhighlight %}

除了转场时间，还可以设定动画曲线（curve），在 `.easeIn`、`.easeOut`、`.easeInOut` 和 `.custom` 之间进行选择，`.custom`可以自定义曲线。

比如我们要让整个动画的速度，先慢后变快，如下：

{% highlight swift %}
Text("Tap here")
    .scaleEffect(scale)
    .animation(.basic(curve: .easeIn))
{% endhighlight %}

除了缩放修改器，还可以为其它修改器添加动画，比如 2D、3D 旋转，透明度，边框等等。例如，下面这段代码在每次点击的时候会旋转并且边框增大：

{% highlight swift %}
struct ContentView: View {
    @State var angle: Double = 0
    @State var borderThickness: Length = 1

    var body: some View {
        Button(action: {
            self.angle += 45
            self.borderThickness += 1
        }) {
            Text("Tap here")
                .padding()
                .border(Color.red, width: borderThickness)
                .rotationEffect(.degrees(angle))
                .animation(.basic())
        }
    }
}
{% endhighlight %}

