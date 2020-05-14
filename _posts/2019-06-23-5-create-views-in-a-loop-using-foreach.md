---
layout: default
title: 3.5 使用 ForEach 循环遍历创建子视图列表
category: swiftui-view-layout
---

循环遍历是一种很常见的操作，在 SwiftUI 里用 `ForEach` 来完成，这里要强调一下，这个 `ForEach` 跟 Swift 的 `forEach` 虽然看起来差不多，但事实并非如此。

在 SwiftUI 里 `ForEach` 是一个视图结构（view struct），这意味着如果有需要，可以直接从视图中返回它。给它一个数组，但是还需要告诉 SwiftUI 如果去区分每个子元素，因为这涉及到数组变化的时候子元素的值要及时更新。

举个例子，从 10 倒数到 1 ，然后结束的时候添加一个文本：

{% highlight swift %}
VStack(alignment: .leading) {
    ForEach((1...10).reversed()) {
        Text("\($0)…")
    }

    Text("Ready or not, here I come!")
}
{% endhighlight %}

对于简单类型数组（例如颜色，字符串，整数等）的循环，我们可以在数组上使用 `.identified(by: \.self)`  来告诉 SwiftUI 如何地标识区分每个子元素。所以，如果你的数组是一个类似于 `["cat", "dog", "monkey”]` 这样的，那么 SwiftUI 会使用子元素（字符串）本身来区分子元素。

下列代码，有一个数组，里面有三种颜色，然后循坏它们，并且输出子视图，并且在每个子视图显示对应的文本和渲染对应的背景颜色，如下：

{% highlight swift %}
struct ContentView : View {
    let colors: [Color] = [.red, .green, .blue]

    var body: some View {
        VStack {
            ForEach(colors.identified(by: \.self)) { color in
                Text(color.description.capitalized)
                    .padding()
                    .background(color)
            }
        }
    }
}
{% endhighlight %}

如果你的数组是一个自定义类型的数组，那么你必须使用 `identified(by:)` 和传入一个属性来定义每个数组的 id。

比如有这样的一个数据结构：

{% highlight swift %}
struct Result {
    var id = UUID()
    var score: Int
}
{% endhighlight %}

它有一个 `id` 属性，值是 `UUID()`，这意味着它们的 id 都是唯一的，值都是不同的，如果想循环这个数组，在一个 `VStack` 中显示多个文本视图，如下：

{% highlight swift %}
struct ContentView : View {
    let results = [Result(score: 8), Result(score: 5), Result(score: 10)]

    var body: some View {
        VStack {
            ForEach(results.identified(by: \.id)) { result in
                Text("Result: \(result.score)")
            }
        }
    }
}
{% endhighlight %}

这相当于告诉 SwiftUI 它可以通过它们的 `id` 属性来区分 `ForEach` 中出现的子视图。如果把 `Result ` 设成符合 `Identifiable` 协议，那么只需要写 `ForEach(results)` 即可。符合这个协议，这意味着自带了一个 id 属性。如下：

{% highlight swift %}
struct Result: Identifiable {
    var id = UUID()
    var score: Int
}
{% endhighlight %}