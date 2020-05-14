---
layout: default
title: 10.2 列表单元格点击的时候 push 一个新视图
category: swiftui-presenting
---

SwiftUI 没有像 `UITableView` 有一个 `didSelectRowAt` 类似的方法，但是我们可以在列表视图结合 `NavigationLink` 来达到此功能。

首先我们先创建一些简单的数据结构，如下：

{% highlight swift %}
struct Restaurant: Identifiable {
    var id = UUID()
    var name: String
}
{% endhighlight %}

然后创建列表的单元格视图：

{% highlight swift %}
struct RestaurantRow: View {
    var restaurant: Restaurant

    var body: some View {
        Text(restaurant.name)
    }
}
{% endhighlight %}

最后创建列表视图：

{% highlight swift %}
struct ContentView: View {
    var body: some View {
        let first = Restaurant(name: "Joe's Original")
        let restaurants = [first]

        return NavigationView {
            List(restaurants) { restaurant in
                RestaurantRow(restaurant: restaurant)
            }.navigationBarTitle(Text("Select a restaurant"))
        }
    }
}
{% endhighlight %}

现在视图上列表上已经有一个单元格了，但是它没有选中操作。

为了能够点击选中跳转到另外一个视图，我们又得创建一个子视图，算作详情页的，如下：

{% highlight swift %}
struct RestaurantView: View {
    var restaurant: Restaurant

    var body: some View {
        Text("Come and eat at \(restaurant.name)")
            .font(.largeTitle)
    }
}
{% endhighlight %}

这时我们重新回到 `ContentView` 列表里单元格 `RestaurantRow ` 视图上，使用 `NavigationLink ` 进行包装，如下：

{% highlight swift %}
return NavigationView {
    List(restaurants) { restaurant in
        NavigationLink(destination: RestaurantView(restaurant: restaurant)) {
            RestaurantRow(restaurant: restaurant)
        }
    }.navigationBarTitle(Text("Select a restaurant"))
}
{% endhighlight %}

如代码所示：使用了  `RestaurantView(restaurant: restaurant)` 作为目标视图，并且进行了传值操作。

** 注意 **: `NavigationLink` 至少需要在 Xcode 11 beta-3 (2019-07-02 release) 及以上版本以上才能实现。