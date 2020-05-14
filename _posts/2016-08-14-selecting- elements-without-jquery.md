---
layout: default
title: 不使用 jQuery 框架选择 HTML 元素
category: not-jquery
thumbnail_image: '/files/notjQuery/notjQuery.png'
---

`$('#element')` 在 jQuery 代码中最常见的，估计你我都写了很多 :) 本章节主要讲一下在不使用 jQuery 的情况下，如何使用浏览器的 DOM API 来选择元素。选择方式主要有以下几种：

### 根据 ID 选择元素

使用	jQuery 选择器

{% highlight javascript %}
$('#element');
//返回一个 jQuery 对象，最多只有一个元素
{% endhighlight %}

使用 DOM API 选择器

{% highlight javascript %}
document.getElementById('element');
//IE 5.5+
{% endhighlight %}

或者

{% highlight javascript %}
document.querySelector('#element');
//IE 8+
{% endhighlight %}

这些方法都只会返回一个元素，而且  `getElementById()` 比 `querySelector()` 更高效。以上的选择方式，jQuery 看起来更有优势吗？我并不觉得。

### 根据 CSS 类选择元素

使用	jQuery 选择器

{% highlight javascript %}
$('.element');
//返回一个 jQuery 对象，可能会有多个匹配的元素
{% endhighlight %}

使用 DOM API 选择器

{% highlight javascript %}
document.getElementsByClassName('.element');
//IE 9+
{% endhighlight %}

或者

{% highlight javascript %}
document.querySelector('.element');
//IE 8+
{% endhighlight %}

`getElementsByClassName()` 返回的是 <a href="https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCollection" target="_blank">HTML 集合</a> ，相比起来，会比 `querySelector()` 更加的高效，而 `querySelector()` 返回的是一个 <a href="https://developer.mozilla.org/zh-CN/docs/Web/API/NodeList" target="_blank">节点集合</a>。同样的比起来，jQuery 选择器也体现不出什么优势。

### 根据标签选择元素

使用	jQuery 选择器

{% highlight javascript %}
$('div');
{% endhighlight %}

使用 DOM API 选择器

{% highlight javascript %}
document.getElementsByTagName('div');
//IE 5.5+
{% endhighlight %}

或者

{% highlight javascript %}
document.querySelectorAll('div');
//IE 8+
{% endhighlight %}

如同上面说的一样，`querySelectorAll()`(返回节点集合) 没有比 `getElementsByTagName()`(返回 HTML 集合)高效。

### 根据属性选择元素

如果需要选择属性 `data-foo = "val"` 的所有元素，使用 jQuery：

{% highlight javascript %}
$('[data-foo="val"]');
{% endhighlight %}

使用 DOM API 选择器

{% highlight javascript %}
document.querySelectorAll('[data-foo="val"]');
//IE 8+
{% endhighlight %}

这两个选择方法倒是看起来挺像的。

### 根据伪类选择元素

比如现在有一个 `id` 为 `form` 的表单，我们需要选择有效的输入框，如果使用 jQuery ：

{% highlight javascript %}
$('#form :invalid');
{% endhighlight %}

使用 DOM API 选择器

{% highlight javascript %}
document.querySelectorAll('#form :invalid');
//IE 10+
{% endhighlight %}

这一点如果用原生的对浏览器要求有点高，但是如果是开发 HTML5 应用就可以忽略这一点了。

### 选择子元素

假设有一个 id 为 `parent` 的 `div`, 根据需要我们要选择它的子元素。子元素指的是第二级的节点元素，而不包含子元素的子元素。孙元素 :) 如果使用 jQuery 选择：

{% highlight javascript %}
$('#parent').children();
{% endhighlight %}

使用 DOM API 选择器

{% highlight javascript %}
document.getElementById('parent').childNodes;
//IE 5.5+
//返回的会包括注释掉的节点和文本节点
{% endhighlight %}

或者使用

{% highlight javascript %}
document.getElementById('parent').children;
//IE 9+
//返回的不包括注释掉的节点和文本节点
{% endhighlight %}

如果说要选择属性有 `ng-click` 的子元素。使用 jQuery 选择：

{% highlight javascript %}
$('#parent').children('[ng-click]');
{% endhighlight %}

或者

{% highlight javascript %}
$('#myParent > [ng-click]');
{% endhighlight %}

使用 DOM API 选择器

{% highlight javascript %}
document.querySelector('#parent > [ng-click]');
//IE 8+
{% endhighlight %}

### 排除选择元素

比如需要选择所有 CSS 类不含 `ignore` 的 `div` 元素，使用 jQuery 选择：

{% highlight javascript %}
$('div').not('.ignore');
{% endhighlight %}

或者

{% highlight javascript %}
$('div:not(.ignore)');
{% endhighlight %}

使用 DOM API 选择器

{% highlight javascript %}
document.querySelectorAll('div:not(.ignore)');
//IE 9+
{% endhighlight %}

### 选择多个不同的元素

使用 jQuery 选择：

{% highlight javascript %}
$('div, a, script');
{% endhighlight %}


使用 DOM API 选择器

{% highlight javascript %}
document.querySelectorAll('div, a, script');
//IE 8+
{% endhighlight %}

### 封装一个选择器

如果说，项目里需要使用选择器的地方多，而不又不考虑低于 IE8 的低级浏览器，那么合理的做法是自己写一个选择器函数，参如下：

{% highlight javascript %}
window.$ = function(selector) {
    var selectorType = 'querySelectorAll';
    if (selector.indexOf('#') === 0) {
        selectorType = 'getElementById';
        selector = selector.substr(1, selector.length);
    }
    return document[selectorType](selector);
};
{% endhighlight %}

在大部分情况下，这已经可以满足大部分的项目需求，但是在某些极端情况下，比如你的项目只要支持 IE 7，那么合理的做法，还是需要一些第三方库，或者直接使用 jQuery。当然还有一些优秀的迷你库，比如 <a href="http://sizzlejs.com/" target="_blank">Sizzle</a>，使用方法和 jQuery 一样，不过只支持选择器功能。还有另外一个库 <a href="http://selectivizr.com/" target="_blank">Selectivizr</a>，可以在低级浏览器模拟 CSS3 伪类和属性选择。
