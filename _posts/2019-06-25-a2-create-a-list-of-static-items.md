---
layout: default
title: 6.2 如何创建一个静态的列表视图
category: swiftui-lists
---

在创建一个静态列表视图之前，首先需要定义每个单元格要显示什么视图，以下是一个简单的视图：

{% highlight swift %}
struct RestaurantRow: View {
    var name: String

    var body: some View {
        Text("Restaurant: \(name)")
    }
}
{% endhighlight %}

单元格视图已经定好了，接着就创建一个 `List` 视图，然后把单元视图放进去即可，如下：

{% highlight swift %}
struct ContentView: View {
    var body: some View {
        List {
            RestaurantRow(name: "Joe's Original")
            RestaurantRow(name: "The Real Joe's Original")
            RestaurantRow(name: "Original Joe's")
        }
    }
}
{% endhighlight %}

运行的时候，可以在一个列表里看到三个单元格，这和 UIKit 的 `UITableView` 几乎一模一样。

不需要每个单元格都用相同的视图，可以与其它视图混合使用，比如加入 `Text` 与 `Image`，如下：

{% highlight swift %}
struct ContentView: View {
    
    var body: some View {
        List {
            RestaurantRow(name: "Joe's Original")
            Text("Hello")
            Image("example-image")
        }
    }
}
{% endhighlight %}