---
layout: default
title: 12.2 使用弹簧动画

category: swiftui-animation
---

除了基本动画，SwiftUI 也支持弹簧动画（spring animation），弹簧动画有一定类似阻尼的效果，移动到目标点的时候，会稍微超过，然后反弹回来。

举个例子，创建一个文本视图，每次点击旋转 45 度，把 `.basic()` 换成 `.spring()`，如果没有额外参数，那么它会自动设定一个合理的默认值。

{% highlight swift %}
struct ContentView: View {
    @State var angle: Double = 0

    var body: some View {
        Button(action: {
            self.angle += 45
        }) {
            Text("Tap here")
                .padding()
                .rotationEffect(.degrees(angle))
                .animation(.spring())
        }
    }
}
{% endhighlight %}

如果要手动设定 `.spring()` 动画效果，可设定的参数有 mass、stiffness、damping 和 initialVelocity。

* mass 模拟的是质量，影响图层运动时的弹簧惯性，质量越大，弹簧拉伸和压缩的幅度越大 默认值：1 。取值应该为大于等于0的数。取值越大，动画持续时间越长。
* stiffness刚度系数(劲度系数/弹性系数)，刚度系数越大，形变产生的力就越大，运动越快。默认值： 100。
* damping阻尼系数，阻止弹簧伸缩的系数，阻尼系数越大，停止越快。默认值：10。
* initialVelocity初始速率，动画视图的初始速度大小。默认值：0 。速率为正数时，速度方向与运动方向一致，速率为负数时，速度方向与运动方向相反。

例如，我们创建一个弹簧阻尼非常低的按钮，它会在达到目标角度之前进行长时间的反弹，如下：

{% highlight swift %}
Button(action: {
    self.angle += 45
}) {
    Text("Tap here")
        .padding()
        .rotationEffect(.degrees(angle))
        .animation(.spring(mass: 1, stiffness: 1, damping: 0.1, initialVelocity: 10))
}
{% endhighlight %}