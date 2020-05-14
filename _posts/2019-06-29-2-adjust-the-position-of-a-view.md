---
layout: default
title: 11.2 使用 offset() 给视图定位
category: swiftui-transform
---

在一般情况下，视图在它的视图层次结构中都有一个正常的自然位置，`offset()` 修改器可以更改视图的位置。但是使用 `offset()` 修改位置只会导致跟原本的自然位置发生偏移，并且不会影响到其它视图的位置，这意味着说，这种偏移方式，会导致与旁边的视图发生重叠，这很可能不是大家想要的。

举个例子，在 `VStack` 堆栈视图里使用 `offset()`把第二个视图向下移动 15 个点，看下会不会和第三个视图发生重叠。

{% highlight swift %}
VStack {
    Text("Home")
    Text("Options")
        .offset(y: 15)
    Text("Help")
}
{% endhighlight %}

那要怎么才能实现顺利偏移呢，`offset()` 不会引起其它视图跟着偏移，但是可以走一点旁白左道，通过 `padding()` 来增加自己的宽高，二者结合起来就会引起其它视图的自然偏移，二者结合如下：

{% highlight swift %}
VStack {
    Text("Home")
    Text("Options")
        .offset(y: 15)
        .padding(.bottom, 15)
    Text("Help")
}
{% endhighlight %}

当然这个例子这样的做法也很弱智，直接 `padding()` 往上顶就可以了。

{% highlight swift %}
struct ContentView: View {
    var body: some View {
        VStack {
            Text("Home")
            Text("Options")
            .padding(.top, 15)
            Text("Help")
        }
    }
}
{% endhighlight %}
