---
layout: default
title: 6.3 如何创建一个动态的列表视图
category: swiftui-lists
---

在处理动态单元格时，首先必须告诉 SwiftUI 如何去识别这些单元格，这个可以使用 `Identifiable ` 协议来完成，这个协议要求只有一个，每个单元格元素设置一个独有的 id ，以便让 SwiftUI 识别。

例如，可以创建一个 `Restaurant` 结构，有 id 和 name 两个属性，id 使用了随机标识符，如下：

{% highlight swift %}
struct Restaurant: Identifiable {
    var id = UUID()
    var name: String
}
{% endhighlight %}

接下来就定一个单元格的视图 `RestaurantRow`，显示餐馆的名称，如下：

{% highlight swift %}
struct RestaurantRow: View {
    var restaurant: Restaurant

    var body: some View {
        Text("Come and eat at \(restaurant.name)")
    }
}
{% endhighlight %}

最后，我们创建列表视图，既然是动态，那就得创建一些测试的数据，然后把这些放到一个数组里，然后传到列表里，如下：

{% highlight swift %}
struct ContentView: View {
    var body: some View {
        let first = Restaurant(name: "Joe's Original")
        let second = Restaurant(name: "The Real Joe's Original")
        let third = Restaurant(name: "Original Joe's")
        let restaurants = [first, second, third]

        return List(restaurants) { restaurant in
            RestaurantRow(restaurant: restaurant)
        }
    }
}
{% endhighlight %}

上面的代码大部分是用在创建数据，只有最后一部分才是真正有 “作用” 的：

{% highlight swift %}
return List(restaurants) { restaurant in
    RestaurantRow(restaurant: restaurant)
}
{% endhighlight %}

这会从 `restaurants` 创建一个别表，为数组中的每个元素执行一次闭包函数，每次把数据传进去，最后创建了返回了 `RestaurantRow`。

其实，针对这样琐碎的例子，还有更短的代码可以使用：

{% highlight swift %}
return List(restaurants, rowContent: RestaurantRow.init)
{% endhighlight %}