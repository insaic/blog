---
layout: default
title: 使用纯 JavaScript 操作 DOM
category: not-jquery
---
在 [上一篇文章](https://appblur.com/not-jquery/2016/08/14/selecting-elements-without-jquery.html) 中，讲到如何使用浏览器的原生选择器。那么选完元素之后呢，当然是要操作！论操作方式，jQuery 有 `after()`，`append()`，`prepend()`，`before()` 等等这些方法，虽然原生的 API 没有提供这些方法，但是这些方法都是基于原生实现的，所以下面将介绍使用原生的 JavaScript 操作 DOM 的几个主要方法。

### 创建新元素

使用 jQuery 的方式：

{% highlight javascript %}
$('<div></div>');
{% endhighlight %}

使用 DOM API 操作：

{% highlight javascript %}
document.createElement('div');
// IE 5.5+
{% endhighlight %}

差别不大，倒是 jQuery 的方式可以节约一点字符。

### 在前/后插入元素

有一个 `id` 为 `element1` 的元素，我们要在其后插入一个新的元素。

比如原本的节点为：

{% highlight html %}
<div id="element1"></div>
<div id="element2"></div>
{% endhighlight %}

想要的 DOM 结构是：

{% highlight html %}
<div id="element1"></div>
	<div id="element1_1"></div> <!--想插入的-->
<div id="element2"></div>
{% endhighlight %}

使用 jQuery 的方式：

{% highlight javascript %}
$('#element1').after('<div id="element1_1"></div>');
{% endhighlight %}

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('#element1').insertAdjacentHTML('afterend', '<div id="element1_1"></div>');
// IE 4+
{% endhighlight %}

那如果我们想要的 DOM 结构是这样的：

{% highlight html %}
	<div id="element0_9"></div> <!--想插入的-->
<div id="element1"></div>
<div id="element2"></div>
{% endhighlight %}

使用 jQuery 的方式：

{% highlight javascript %}
$('#element1').before('<div id="element0_9"></div>');
{% endhighlight %}

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('#element1').insertAdjacentHTML('beforebegin', '<div id="element0_9"></div>');
// IE 4+
{% endhighlight %}

前后插入的方式就几个关键的差别，jQuery 的 `before()` 和 `after()`，而原生的 `insertAdjacentHTML()` 方法是，第一个参数不同，`afterend` 和 `beforebegin`，注意这两个参数是字符串。

### 作为子节点插入元素

原本的 DOM 结构为：

{% highlight html %}
<div id="parent">
    <div id="oldChild"></div>
</div>
{% endhighlight %}

那我们想要的是，创建一个新元素，并且作为子元素插入到 `#parent` 这个 `div` 里面，并且排在前面，期望如下：

{% highlight html %}
<div id="parent">
    <div id="newChild"></div> <!--想插入的-->
    <div id="oldChild"></div>
</div>
{% endhighlight %}

使用 jQuery 的方式：

{% highlight javascript %}
$('#parent').prepend('<div id="newChild"></div>');
{% endhighlight %}

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('parent').insertAdjacentHTML('afterbegin', '<div id="newChild"></div>');
// IE 4+
{% endhighlight %}

那如果想插入的时候排在后面呢，如下：

{% highlight html %}
<div id="parent">
  <div id="oldChild"></div>
  <div id="newChild"></div> <!--想插入的-->
</div>
{% endhighlight %}

使用 jQuery 的方式：

{% highlight javascript %}
$('#parent').append('<div id="newChild"></div>');
{% endhighlight %}

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('parent').insertAdjacentHTML('beforeend', '<div id="newChild"></div>');
// IE 4+
{% endhighlight %}

和上面的前后插入差不多，jQuery 的方法是 `prepend()` 和 `append()`，而原生的差别也是在第一个参数，`afterbegin` 和 `beforeend`。并且兼容性特别强，IE 4+ ....

### 移动元素

比如原本的 DOM 结构是这样的：

{% highlight html %}
<div id="parent">
  <div id="c1"></div>
  <div id="c2"></div>
  <div id="c3"></div>
</div>
<div id="orphan"></div> <!--移动前-->
{% endhighlight %}

那现在我们想把 `#orphan` 移到 `#parent` 里面，并排在最后，效果如下：

{% highlight html %}
<div id="parent">
    <div id="c1"></div>
    <div id="c2"></div>
    <div id="c3"></div>
    <div id="orphan"></div> <!--移动后-->
</div>
{% endhighlight %}

使用 jQuery 的方式：

{% highlight javascript %}
$('#parent').append($('#orphan'));
{% endhighlight %}

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('parent').appendChild(document.getElementById('orphan'));
// IE 5.5+
{% endhighlight %}

同样是 so easy，二者的差别是 `.append()` 和 `appendChild()`。那如果我们把元素移进去，想排在最前面呢？

{% highlight html %}
<div id="parent">
		<div id="orphan"></div> <!--移动后-->
    <div id="c1"></div>
    <div id="c2"></div>
    <div id="c3"></div>
</div>
{% endhighlight %}

使用 jQuery 的方式：

{% highlight javascript %}
$('#parent').prepend($('#orphan'));
{% endhighlight %}

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('parent').insertBefore(document.getElementById('orphan'), document.getElementById('c1'));
// IE 5.5+
{% endhighlight %}

虽然使用原生的看起来有点长，重点在于 `insertBefore()`，但是同样可以一行完成，而且不用引入一个庞大的库呀！

### 删除元素

如何在一个 DOM 结构里把某个元素删除掉？ 大家熟悉的 jQuery 方式：

{% highlight javascript %}
$('#element').remove();
{% endhighlight %}

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('element').parentNode.removeChild(document.getElementById('element'));
// IE 5.5+
{% endhighlight %}

在此，不得不说原生的方法确实有点丑陋，实现方式是先用属性 `parentNode` 找到父节点，然后在用 `removeChild()` 把自己删除掉，略感曲折悲壮！

### 添加/删除 CSS 类样式

我们有一个 `id` 为 `element` 的 `div` :

{% highlight html %}
<div id="element"></div>
{% endhighlight %}

现在要为该节点添加一个 `bold` 的 CSS 类，蓝图如下：

{% highlight html %}
<div id="element" class="bold"></div>
{% endhighlight %}

使用 jQuery 的方式：

{% highlight javascript %}
$('#element').addClass('bold');
{% endhighlight %}

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('element').classList.add('bold');
// 只支持高级浏览器 IE 10+

document.getElementById('element').className += 'bold';
// 支持所有浏览器
{% endhighlight %}

好，现在把 `bold` 移除。

使用 jQuery 的方式：

{% highlight javascript %}
$('#element').removeClass('bold');
{% endhighlight %}

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('foo').classList.remove('bold');
// 只支持高级浏览器 IE 10+

document.getElementById('foo').className = document.getElementById('foo').className.replace(/^bold$/, '');
// 支持所有浏览器
{% endhighlight %}

如果使用原生的方式，在高级浏览器是没啥问题，但是如果需要兼容低级浏览器，那么可能实现起来有点麻烦，而且挺丑陋的。

### 添加/删除/修改属性

还是原来那个 div。

{% highlight html %}
<div id="element"></div>
{% endhighlight %}

现在我们需要为其添加一个 `role = "button"` 属性。

使用 jQuery 的方式：

{% highlight javascript %}
$('#element').attr('role', 'button');
{% endhighlight %}

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('element').setAttribute('role', 'button');
// IE 5.5+
{% endhighlight %}

于此，如果原属性不存在，则会创建，如果原属性已经存在，则更新。如果已存在，又要移除呢？

使用 jQuery 的方式：

{% highlight javascript %}
$('#element').removeAttr('role');
{% endhighlight %}

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('element').removeAttribute('role');
// IE 5.5+
{% endhighlight %}

关于原生的，重点在于 `setAttribute()` 和 `removeAttribute()` 这两个方法。

### 添加/修改元素内的文本内容

我们现在有一个元素，如下：

{% highlight html %}
<div id="element">Hi there!</div>
{% endhighlight %}

但是现在我们要把里面的内容，改成 "Goodbye!"。

使用 jQuery 的方式：

{% highlight javascript %}
$('#element').text('Goodbye!');
{% endhighlight %}

如果要清空，只需使用 `.text('')` 即可。

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('element').innerHTML = 'Goodbye!';
// IE 5.5+

document.getElementById('element').innerText = 'GoodBye!';
// IE 5.5+ 但是 firefox 不支持

document.getElementById('element').textContent = 'Goodbye!';
// IE 9+
{% endhighlight %}

如果没有为上面的属性赋值，则返回的是该节点的文本内容：

{% highlight javascript %}
document.getElementById('element').innerHTML;
// 返回原内容 "Hi there!"
{% endhighlight %}

在此要注意 `innerHTML` 和 `innerText` 的区别，使用 `innerHTML` 赋入的文本是会被转义成 HTML 的，如果文本内容是由用户提供，请谨慎使用 `innerHTML`，防止被注入 srcipt 脚本。

### 添加/修改 style 样式

为元素修改样式是一个常见的需求，可以使用添加/删除 CSS 类这个方式，但是略麻烦，更简单是直接修改其 style 属性。以下是一个简单的元素：

{% highlight html %}
<span id="note">Attention!</span>
{% endhighlight %}

现在我们需要加粗里面的字体。

使用 jQuery 的方式：

{% highlight javascript %}
$('#note').css('fontWeight', 'bold');
{% endhighlight %}

如果要清空，只需使用 `.text('')` 即可。

使用 DOM API 操作：

{% highlight javascript %}
document.getElementById('note').style.fontWeight = 'bold';
// IE 5.5+
{% endhighlight %}

我觉得原生的方式会更好一点，虽然看起来长了一点，但是直观。

### 相关的其它库

上面说了那么多，用你心中的那杆称，也许知道自己该怎么选择了。通常为了项目的快速开发，会选择一个库，省时省力。但缺点是，门槛太低了，如果是多人协作的项目，估计很有可能会大大降低代码的质量与美感。如果你需要繁琐地操作 DOM ，而又不想使用 jQuery ，可以考虑使用 <a href="http://jbone.js.org/" target="_blank">jbone</a>，Backbone 御用（之前用的是 jQuery），gzip 之后，只有 2.5K 大小。
