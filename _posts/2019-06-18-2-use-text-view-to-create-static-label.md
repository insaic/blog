---
layout: default
title: 2.2 如何使用 Text 创建一个静态文本标签
category: swiftui-text-images
---

上一章节讲的，初始化模版就是一个 Hello World 文本标签，如下：

{% highlight swift %}
Text("Hello World")
{% endhighlight %}

`Text` 视图相当于 UIKit 的 `UILabel`，默认情况下 `Text` 视图只能显示一行，如果空间不足，超出字符将会被裁剪，替换为 “...”。如果你想让文本完整显示，可以设置显示的行数：

{% highlight swift %}
Text("Hello World")
    .lineLimit(3)
{% endhighlight %}

这个 `.lineLimit(3)` 直接放在 `Text` 后面，有点类似于前端 jQuery 的链式写法，上述代码为了方便阅读，所以做了换行处理。参数是 `3` 是因为最多只能显示 3 行吗？不是的，最多只能显示两行，如果需要显示 3 行则参数应为 `4`，那如果无法预知行数，又需全部显示呢？使用 `nil` 参数：

{% highlight swift %}
Text("Hello World")
    .lineLimit(nil)
{% endhighlight %}

除了调整文本的行数，还可以调整 SwiftUI 的文本截断方式。默认情况下是从文本末尾删除并替换成省略号，但是也可以根据需要把省略号放在文本开头或者中间。举个例子，把省略号放中间：

{% highlight swift %}
var body: some View {
    Text("This is an extremely long string that will never fit even the widest of Phones")
        .truncationMode(.middle)
}
{% endhighlight %}

`.truncationMode()` 对应的三个参数分别是：开头 `.head`，中间 `.middle`，结尾 `.tail`。默认结尾。

无论如果设置文本的截断方式，你看到的文本视图都是整齐地位于主视图中间，这是 SwiftUI 的默认行为，除非视图有特别定位，否则都是在屏幕中间。



