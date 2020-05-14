---
layout: default
title: 10.1 如何向导航视图 push 一个新视图
category: swiftui-presenting
---

场景如下，现在拥有一个导航视图（NavigationView），如果想让用户点击某个视图就跳转到另一个导航视图，那么应该使用 `NavigationLink`，在 Xcode 11 beta 1/2  中已用 `NavigationButton` 的方式支持。`NavigationLink` 的第一个参数为 destination，意思是跳转的目标视图，第二个参数是一个尾随闭包，即是可以触发跳转的视图，代码如下：

{% highlight swift %}
struct DetailView: View {
    var body: some View {
        Text("Detail")
    }
}
{% endhighlight %}


{% highlight swift %}
struct DetailView : View {
    var body: some View {
        Text("Detail View")
    }
}

struct ContentView : View {
    var body: some View {
        NavigationView {
            NavigationButton(destination: DetailView()) {
                Text("Click")
            }.navigationBarTitle(Text("Navigation"))
        }
    }
}
{% endhighlight %}