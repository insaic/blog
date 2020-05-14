---
layout: default
title: 不使用 jQuery 进行事件处理
category: not-jquery
---
在 [上一篇文章](https://appblur.com/not-jquery/2016/08/17/ajax-requests-by-pure-javascript.html) 中，讲解了使用纯 JavaScript 进行 Ajax 请求。本集将讲一些关于事件处理的。同样地，会把 jQuery 方式和原生的做个对比。再次强调一点，这个网站并没有唱衰 jQuery 的意思。

### 触发原生（DOM）事件

由于事件是可以自定义的，但是 DOM 自己也有内置一部分，比如 `click`，`focus`，`blur`，`change` … 假设要触发一个点击事件，如果使用 jQuery，写法如下：

{% highlight javascript %}
$(element).click();
{% endhighlight %}

如使用 Web API，如下：

{% highlight javascript %}
element.click()
{% endhighlight %}

二者的写法都差不多，差别在于如何选择元素，这个写法可以在 IE 6+ 以上的浏览器工作。于此，jQuery 并没有什么优势，也可以同样的方式触发其它内置事件，比如 `focus()`，`blur()`，或者触发 `submit()` 提交一个表单。

### 触发自定义事件

假设我们有一个自定义事件 my-custom-event ，并且要触发它，可以随默认情况是否冒泡，同时也可以撤销这个事件。使用 jQuery 触发：

{% highlight javascript %}
$('element').trigger('my-custom-event');
{% endhighlight %}

上述这段代码，会触发 `element` 的 `my-custom-event` 事件，并且这个事件默认情况下会跟随 DOM 的结构向上冒泡。但是这个事件只会在 jQuery 范围内生效，也就是使用 jQuery 触发，也必须使用 jQuery 监听才行。

如使用 Web API，如下：

{% highlight javascript %}
var event = document.createEvent('Event');
event.initEvent('my-custom-event', true, true); //第二个参数，是否冒泡，第三个参数，是否可取消（阻止）
element.dispatchEvent(event);
{% endhighlight %}

事件是否可取消是啥意思？如可以取消，则表示用 `preventDefault()` 方法可以取消与事件关联的默认动作。上述这段代码，可以在所有的浏览器工作，但是 `document.createEvent()` 这个方法，已经被弃用了。即使如此，按所知，它还是可以在所有浏览器工作，但是，但是不建议使用。对于高级浏览器，取而代之的是 `CustomEvent` 这个构造函数。如下：

{% highlight javascript %}
var event = new CustomEvent('my-custom-event', {bubbles: true, cancelable: true});
element.dispatchEvent(event);
{% endhighlight %}

不过这也不是完美的，因为在 IE 浏览器中，并不支持 `CustomEvent` 的构造函数，虽然其它高级浏览器都支持。考虑到需要兼容 IE 浏览器，如果是桌面端，几乎是肯定的，我们只能继续使用 `createEvent` 这种方式，如果以后新版的浏览器将 `createEvent` 彻底抛弃，只能增加判断，然后权衡是否需要用 `CustomEvent` 代替之。

### 监听事件

在事件监听上，不管是原生事件还是自定义事件，使用 jQuery 和原生的方法，在高级浏览器里写法已经十分相似了，我们举个例子，监听一个元素的点击 `click` 事件，如果使用 jQuery，如下：

{% highlight javascript %}
$(element).on('click', function() {
    // 处理逻辑
});
{% endhighlight %}

或者也可以使用 `.click()` 方法，使用此方法，只需把处理逻辑的函数当作第一个参数传入即可，如下：

{% highlight javascript %}
$(element).click(function() {
    // 处理逻辑
});
{% endhighlight %}

如使用 Web API，如下：

{% highlight javascript %}
element.addEventListener('click', function() {
    // 处理逻辑
});
{% endhighlight %}

原生和 jQuery 确实十分相似，但是这个范围要基于现代浏览器（IE 9+），因为低级浏览器并不支持 `addEventListener` 这个方法，而是使用 `attachEvent` 这个方法，但还是有些差别。如下：

{% highlight javascript %}
if (element.addEventListener) {
    element.addEventListener('click', function() {
        // 处理逻辑
    }, false);
}
else {
    element.attachEvent('onclick', function() {
        // 处理逻辑
    });
}
{% endhighlight %}

注意 `attachEvent` 这个方法，第一个参数是 'onclick' 不是 'click'。

### 移除事件的处理逻辑

可以理解为解绑一个事件，同样地，虽然 jQuery 提供了一个 off() 的方法可以移除事件，但是也只能移除通过 jQuery 绑定/监听的事件。而不会移除 `addEventListener` 监听的事件。 假设我们有一个事件处理逻辑的函数：

{% highlight javascript %}
var myEventHandler = function(event) {
    // 处理逻辑
}
{% endhighlight %}

现在我们要移除它，如使用 jQuery ，如下：

{% highlight javascript %}
$(element).off('click', myEventHandler);
{% endhighlight %}

或者使用 Web API，如下：

{% highlight javascript %}
element.removeEventListener('click', myEventHandler);
{% endhighlight %}

### 修改事件

1，阻止在 DOM 结构上向父级冒泡，比如子元素被触发了一个点击事件，那么它的父元素也会获得一个点击事件，一级传一级，直到根元素，简称冒泡。很多时候我们并不希望事件如此传播，需要将其阻止。使用 jQuery 如下：

{% highlight javascript %}
$(element).on('some-event', function(event) {
    event.stopPropagation();
});
{% endhighlight %}

或者使用 Web API，如下：

{% highlight javascript %}
element.addEventListener('some-event', function(event) {
    event.stopPropagation();
});
{% endhighlight %}

二者写法几乎相似，使用的都是 <a href="https://developer.mozilla.org/zh-CN/docs/Web/API/Event/stopPropagation" target="_blank">stopPropagation()</a>。

2，除了阻止冒泡行为，可能我们还希望阻止当前事件所在元素上的所有相同类型事件的事件处理函数的继续执行.比如一个元素的某个事件被添加两次监听，常规情况下，事件一旦触发，这两次监听的逻辑处理函数将会依次被执行。但有时候我们可能就希望，第一次的逻辑函数处理完之后，不希望列队之后的第二第三次...被执行，如下：

{% highlight javascript %}
$(element).on('some-event', function(event) {
    event.stopImmediatePropagation();
});
{% endhighlight %}

或者使用 Web API，如下：

{% highlight javascript %}
$(element).addEventListener('some-event', function(event) {
    event.stopImmediatePropagation();
});
{% endhighlight %}

注意一点，使用原生的方式，只有在 IE 9+ 的浏览器才会生效，详情可参考 <a href="https://developer.mozilla.org/zh-CN/docs/Web/API/Event/stopImmediatePropagation" target="_blank">stopImmediatePropagation()</a>。


3，阻止浏览器事件的默认行为，比如点击 `a` 标签，浏览器会默认跳转到标签 `href` 属性上的页面，某些矫情的时候，我们并不希望这样的事情发生。如果事件对象的 `cancelable` 属性为 `true`，则该方法可以取消事件的默认动作，但并不取消事件的冒泡行为.使用 jQuery 如下：

{% highlight javascript %}
$(someAnchor).on('click', function(event) {
    event.preventDefault();
});
{% endhighlight %}

或者使用 Web API，如下：

{% highlight javascript %}
someAnchor.addEventListener('click', function(event) {
    event.preventDefault();
});
{% endhighlight %}

二者的写法还是挺像的。

### 事件委托

首先要先了解一下什么是事件委托。假设有一个列表，每个列表都有个标题，每个标题是一个 `button` 标签，都可以点击，这样的标题有百八十个，总不能写百八十个监听事件吧，虽然可以在 `button` 标签上加入 `onclick=“…”` 属性以避免重复写监听，但是最合理优雅的做法是使用事件委托，在根标签绑定一个点击事件，由于事件是会冒泡传播的，所以点击列表标题的 `button` 标签，就相当于点击了根标签 `document` ，而再根据 `event` 参数来判断是哪个 `button` 标签被点击了，这就是事件委托的原理。

假设这个列表是这样的：

{% highlight html %}
<ul id="my-list">
    <li>foo <button>x</button></li>
    <li>bar <button>x</button></li>
    <li>abc <button>x</button></li>
    <li>123 <button>x</button></li>
</ul>
{% endhighlight %}

需要点击 button 的时候，把这个列表移除。如使用 jQuery 如下：

{% highlight javascript %}
$('#my-list').on('click', 'button', function() {
    $(this).parent().remove();
});
{% endhighlight %}

如使用 Web API，如下：

{% highlight javascript %}
document.getElementById('my-list').addEventListener('click', function(event) {
    var clickedEl = event.target;
    if(clickedEl.tagName === 'button') {
       var listItem = clickedEl.parentNode;
       listItem.parentNode.removeChild(listItem);
    }
});
{% endhighlight %}

以上的原生方式，很粗糙，只是为了阐述事件委托的实现原理。

### 键盘事件

首先，我们要先了解一下普通键盘事件的三种类型的差别。

* 1，keydown

* 2，keypress

* 3，keyup

keydown 事件发生在键盘的键被按下的时候，接下来触发 keypress 事件。 keyup 事件在按键被释放的时候触发。

有几点要注意：

>keydown 触发后，不一定触发 keyup，当 keydown 按下后，拖动鼠标，那么将不会触发 keyup 事件。
>
>keypress 主要用来捕获数字(注意：包括Shift+数字的符号)、字母（注意：包括大小写）
>
>keypress 只能捕获单个字符
>
>keydown 和 keyup 可以捕获组合键。
>
>keypress 可以捕获单个字符的大小写
>
>keydown 和 keyup 对于单个字符捕获的 KeyValue 都是一个值，也就是不能判断单个字符的大小写。
>
>keypress 不区分小键盘和主键盘的数字字符。
>
>keydown 和 keyup 区分小键盘和主键盘的数字字符。
>
>其中 PrScrn 按键 keypress, keydown 和 keyup 都不能捕获。

假设说，我们开发的 Web 应用里，需要附带有键盘操作功能，比如按下 `Ctrl + H` 组合键，即可弹出帮助框，用 jQuery 实现如下：

{% highlight javascript %}
$(document).keydown(function(event) {
    if (event.ctrlKey && event.which === 72) {
        // 弹出帮助框
    }
});
{% endhighlight %}

如使用 Web API，如下：

{% highlight javascript %}
document.addEventListener('keydown', function(event) {
    if (event.ctrlKey && event.which === 72) {
        // 弹出帮助框
    }
});
{% endhighlight %}

其它事件也差不多，只需把 `keydown` 改成 `keypress` 或 `keyup`。更详细的信息，可以参考 <a href="https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent" target="_blank">键盘事件-MDN</a>。

### 浏览器加载事件

在谈到加载问题，有必要区分一下两个加载事件，全部加载完成和文档加载完成。

全部加载完成指的是：静态文档加载完成，文档内的图片，样式，iFrame，脚本也加载完成。

静态文档加载完成指的是：整个 DOM 已经加载到 <body> 结束了，由于文档中的图片样式等加载并不会阻塞加载。

如果是判断全部加载完成，使用 jQuery 方式：

{% highlight javascript %}
$(window).load(function() {
    // 全部加载并且页面渲染完成
})
{% endhighlight %}

如使用 Web API，如下：

{% highlight javascript %}
window.addEventListener('load', function() {
    // 全部加载并且页面渲染完成
});
{% endhighlight %}

如果是为了判断 DOM 是否加载完成，则：

{% highlight javascript %}
$(document).ready(function() {
    // DOM 加载完成
});
{% endhighlight %}

和原生版本的：

{% highlight javascript %}
document.addEventListener('DOMContentLoaded', function() {
    // DOM 加载完成
});
{% endhighlight %}

按常规的做法，一般都是把样式放在文档的 head 里面，把脚本放到最后。防止加载阻塞影响脚本执行。

除了文档与 `window`，其它的个别元素也有自己的加载事件。如 `img`， `link`，`script`。比如要判断一张图片是否加载完成，可以添加 `load` 事件，使用 jQuery 判断如下：

{% highlight javascript %}
$('img').load(function() {
    // 图片成功加载完成
});
{% endhighlight %}

也可加个加载失败的判断，如下：

{% highlight javascript %}
$('img').error(function() {
    // 图片加载失败或错误
});
{% endhighlight %}

换做 Web API，实现如下：

{% highlight javascript %}
img.addEventListener('load', function() {
    // 图片成功加载完成
});
{% endhighlight %}

失败的判断方法如下：

{% highlight javascript %}
img.addEventListener('error', function() {
    // 图片加载失败或错误
});
{% endhighlight %}

正如我们之前说过多次，jQuery 和现代浏览器的 Web API 语法是惊人地相似。

### 关于低级浏览器

当你的项目需要用在低级浏览器的时候，比如 IE 6/7，那么我觉得 jQuery 还是很大程度上不能抛弃的。因为低级浏览器的 Web/DOM API 混乱，特别是在事件处理上，更是如此。虽然有很多补救措施（不然 jQuery 也没法实现全部兼容），但基于个人情感，还是不讨论这些老东西了，2001年美国碰到 911 事件，XP 也在同年发布，到现在都过去 15年了，依然还残存在市场上。

如果需要兼容低级浏览器，还是建议使用 jQuery ，可以节约很多时间。
