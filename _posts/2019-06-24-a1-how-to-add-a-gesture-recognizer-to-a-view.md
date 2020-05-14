---
layout: default
title: 4.11 如何为视图添加手势识别
category: swiftui-event
---

任何 SwiftUI 视图都可以添加手势识别，并且这些手势识别可以依次附加闭包函数，在识别触发后依次调用。

首先，创建一个图片视图，接着添加  `TapGesture` 这个点击识别，然后添加一个 `onEnded` 闭包函数，顾名思义在点击结束的时候触发，函数美触发一次都会变大图像，代码如下

{% highlight swift %}
struct ContentView : View {
    @State private var scale: Length = 1.0

    var body: some View {
        Image("example-image")
            .scaleEffect(scale)

            .gesture(
                TapGesture()
                    .onEnded { _ in
                        self.scale += 0.1
                    }
            )
    }
}
{% endhighlight %}

接着我们换成一个长按动作识别 `LongPressGesture`，并添加 `minimumDuration` 参数，意思是长按图片两秒会触发回调函数，代码如下

{% highlight swift %}
Image("example-image")
    .gesture(
        LongPressGesture(minimumDuration: 2)
            .onEnded { _ in
                print("Pressed!")
        }
    )
{% endhighlight %}

最后说下 `DragGesture` 这个拖动手势，当用户按下视图并且拖动一定距离的时候，会触发回调函数。添加 `minimumDistance` 参数，代码如下：

{% highlight swift %}
Image("example-image")
    .gesture(
        DragGesture(minimumDistance: 50)
            .onEnded { _ in
                print("Dragged!")
            }
    )
{% endhighlight %}

更多手势，请查看 Apple 文档 <a href="https://developer.apple.com/documentation/swiftui/gestures" target="_blank">SwiftUI Gestures</a>。