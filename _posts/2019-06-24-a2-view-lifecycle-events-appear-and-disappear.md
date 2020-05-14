---
layout: default
title: 4.12 视图生命周期出现与消失事件
category: swiftui-event
---

UIKit 有 `viewDidAppear()` 和 `viewDidDisappear()` 两个周期事件，SwiftUI 也有对应的两个事件 `onAppear()` 和 `onDisappear()`。我们可以把代码写在这两个事件里，SwiftUI 会在事件发生的时候执行它们。

举个例子，在导航视图里添加 `onAppear()` 和 `onDisappear()` 两个事件，触发的时候则打印信息。代码如下：

{% highlight swift %}
struct ContentView : View {
    var body: some View {
        NavigationView {
            NavigationButton(destination: DetailView()) {
                Text("Hello World")
            }
        }.onAppear {
            print("ContentView appeared!")
        }.onDisappear {
            print("ContentView disappeared!")
        }
    }
}

struct DetailView : View {
    var body: some View {
        VStack {
            Text("Second View")
        }.onAppear {
                print("DetailView appeared!")
        }.onDisappear {
                print("DetailView disappeared!")
        }
    }
}
{% endhighlight %}

在模拟器中运行该代码，然后可以在 Xcode 控制台中看到对应打印信息。

*目前的测试版 `onAppear()` 工作得很好，但是 `onDisappear()` 貌似不太灵。*
