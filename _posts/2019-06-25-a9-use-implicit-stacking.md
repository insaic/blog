---
layout: default
title: 6.9 如何为单元格创建多个视图
category: swiftui-lists
---

之前的例子，单元格都是有一行文本，单个视图的，那如何放置多个视图呢？ 比如一个单元格里又有图片又有文字。SwiftUI 的解决方案也同样很简单，在默认情况下，它会有一个隐式的 `HStack` 排版行为，会自动进行水平布局。

举个例子，我们想在单元格的左边放一张小图片，然后剩余的空间放一段文本。先创建一个数据结构，包含两个属性，如下：

{% highlight swift %}
struct User: Identifiable {
    var id = UUID()
    var username = "Anonymous"
}
{% endhighlight %}

完成后，我们再创建一个包含三个用户的数组，并在列表中显示它们，如下：

{% highlight swift %}
struct ContentView : View {
    let users = [User(), User(), User()]

    var body: some View {
        List(users) { user in
            Image("paul-hudson")
                .resizable()
                .frame(width: 40, height: 40)
            Text(user.username)
        }
    }
}
{% endhighlight %}