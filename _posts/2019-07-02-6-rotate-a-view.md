---
layout: default
title: 11.6 如何旋转视图
category: swiftui-transform
---

SwiftUI 的 `rotationEffect()` 修改器可以旋转视图，接受的参数可以为度（degree）和弧度（radian）。

**1 度是圆周长的 1/360，一度表示为 1゜。1 弧度是圆周长的 1/2πr，1 弧度 = 360゜/(2π) **

举个例子，不如逆时针旋转 90 度，使用方式如下：

{% highlight swift %}
Text("Up we go")
    .rotationEffect(.degrees(-90))
{% endhighlight %}

如果是用弧度做单位，可以使用 `.radians()` 参数，如下：

{% highlight swift %}
Text("Up we go")
    .rotationEffect(.radians(.pi))
{% endhighlight %}

当然，可以把这个旋转值设成动态的，为此我们搭配一个滑块视图，用来调整旋转角度，代码如下：

{% highlight swift %}
struct ContentView: View {
    @State var rotation: Double = 0

    var body: some View {
        VStack {
            Slider(value: $rotation, from: 0.0, through: 360.0, by: 1.0)
            Text("Up we go")
                .rotationEffect(.degrees(rotation))
        }
    }
}
{% endhighlight %}

默认情况下，旋转都是围绕视图中心来转的，如果要从特定的坐标来作为旋转圆心，额外添加一个 `anchor` 锚点参数就可以，比如想让视图以左上角为中心旋转，代码如下：

{% highlight swift %}
struct ContentView: View {
    @State var rotation: Double = 0

    var body: some View {
        VStack {
            Slider(value: $rotation, from: 0.0, through: 360.0, by: 1.0)
            Text("Up we go")
                .rotationEffect(.degrees(rotation), anchor: UnitPoint(x: 0, y: 0))
        }
    }
}
{% endhighlight %}