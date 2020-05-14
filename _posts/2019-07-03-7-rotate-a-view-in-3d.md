---
layout: default
title: 11.7 3D 旋转视图
category: swiftui-transform
---

之前讲的偏移，旋转都是在 2D 平面环境下。现在介绍使用 SwiftUI 的 `rotation3DEffect()` 修改器在 3D 的环境下进行视图操作。该修改器包含两个参数，一个是旋转的角度（单位接受度和弧度），另一个是三轴坐标，围绕该轴执行旋转。

![3D](/files/swiftUI/3d.png)

因此，如果您想围绕X轴旋转一些文本45度（这会导致文本的顶部看起来比底部更远），可以这样写：

{% highlight swift %}
Text("EPISODE LLVM")
    .font(.largeTitle)
    .foregroundColor(.yellow)
    .rotation3DEffect(.degrees(45), axis: (x: 1, y: 0, z: 0))
{% endhighlight %}

是的，跟《星球大战》的字幕效果一样。

如果需要修改旋转中心，则可以再加个锚点参数：

{% highlight swift %}
.rotation3DEffect(.degrees(45), axis: (x: 1, y: 0, z: 100), anchor: UnitPoint(x: 0, y: 0))
{% endhighlight %}

相关文档：<a href="https://developer.apple.com/documentation/swiftui/scrollview/3287538-rotation3deffect" target="_blank">rotation3DEffect(_:axis:anchor:anchorZ:perspective:)
</a>