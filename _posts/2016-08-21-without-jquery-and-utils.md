---
layout: default
title: 抛弃 jQuery 之后的其它补充
category: not-jquery
---

这是[本系列](https://appblur.com/not-jquery.html)的最后一篇文章了，之前讲的选择元素，DOM 操作，Ajax 处理，事件处理已涵盖了大部分，但是 jQuery 并非如此简单，它还内置了很多其它方法。同样本篇文章讲的大多还是要基于高级浏览器（IE 9+），可以视作 HTML5 环境吧。

### 判断是否是对象，数组，函数

虽然这个需求并不太常用，但还是会有碰到的时候，比如在函数里检测传入的参数。如果是使用 jQuery 实现，则方面很多，如下：

{% highlight javascript %}
$.isFunction(value);
//是否是函数

$.isPlainObject(value);
//是否是对象

$.isArray(value);
//是否是数组
{% endhighlight %}

上面这三个是 jQuery 内置的方法，返回的是个布尔值。注意第二个，是 isPlainObject 而不是 isObject 。中间多了个 Plain，纯的意思，因为在 JavaScript 里，函数和数组的类型都是 Object。

如使用纯 JavaScript 可以使用 `typeof` 来判断一个值是不是函数：

{% highlight javascript %}
typeof value === 'function';
{% endhighlight %}

如果判断一个对象，单单使用 typeof 是不够的，因为：

{% highlight javascript %}
typeof null
//返回 object

typeof []
//返回 object
{% endhighlight %}

于此为了精确判断，必须使用 `Object.toString()` 加持一下。如下：

{% highlight javascript %}
value != null && Object.prototype.toString.call(value) === '[object Object]';
{% endhighlight %}

如果要判断一个值是否是一个数组，原理一样，但高级浏览器有内置了一个方法，如下：

{% highlight javascript %}
Array.isArray(value);
//只对高级浏览器

Object.prototype.toString.call(value) === '[object Array]';
//全部浏览器
{% endhighlight %}

### 合并与拷贝对象

假设有两个对象，o1 和 o2。

{% highlight javascript %}
var o1 = {
        a: 'a',
        b: {
            b1: 'b1'
        }
    },
    o2 = {
        b: {
            b2: 'b2'
        },
        c: 'c'
    };
{% endhighlight %}

如果要把 o2 合并到 o1 ，那么结果应该是这样的：

{% highlight javascript %}
{
    a: 'a',
    b: {
        b1: 'b1',
        b2: 'b2'
    },
    c: 'c'
}
{% endhighlight %}

如果使用 jQuery 操作，使用 `$.extend()` 方法，如下：

{% highlight javascript %}
$.extend(true, o1, o2);
//把 o2 合并到 o1

var newObj = $.extend(true, {}, o1, o2);
// o2 合到 o1 再合到一个空对象，返回一个新对象
{% endhighlight %}

上述的第一个参数 `true`，意指是否深度拷贝。深拷贝和浅拷贝的区别在于内存地址指向，如果只是 `var newObj = o2`, 那么 `newObj` 只是把指向指到 `o2` 的内存地址，如果 `o2` 改变了，`newObj` 也会跟着改变，这是浅拷贝，显然大多时候不能满足需求。而深拷贝是新建一个内存地址，并把原来的值拷贝过来，`o2` 改变了，`newObj` 不会跟着变。于此，使用 jQuery 拷贝一个对象，写法如下：

{% highlight javascript %}
var copyOfO1 = $.extend(true, {}, o1);
{% endhighlight %}

如果使用纯 javaScript 合并对象，我们得先写个 extend 函数，中心思想是遍历对象和递归调用，如下：

{% highlight javascript %}
function extend(first, second) {
    for (var secondProp in second) {
        var secondVal = second[secondProp];
        // Is this value an object?  If so, iterate over its properties, copying them over
        if (secondVal && Object.prototype.toString.call(secondVal) === "[object Object]") {
            first[secondProp] = first[secondProp] || {};
            extend(first[secondProp], secondVal);
        }
        else {
            first[secondProp] = secondVal;
        }
    }
    return first;
};

extend(o1, o2);
//把 o2 合并到 o1

var newObj = extend(extend({}, o1), o2);
// o1 先与空对象合并，返回一个新对象，o2 再与这个新对象合并，最后返回一个新对象
{% endhighlight %}

如果只是为了拷贝一个对象，其实只是少了个合并而已，拷贝 o1 如下：

{% highlight javascript %}
var copyOfO1 = extend({}, o1);
{% endhighlight %}


### 遍历对象属性

假设我们有一个对象，称之为 `parentObject`，如下：

{% highlight javascript %}
var parentObject = {
    a: 'a',
    b: 'b'
};
{% endhighlight %}

然后我们在创建一个对象 `myObject`，并且继承 `parentObject`：

{% highlight javascript %}
var myObject = Object.create(parentObject);
myObject.c = "c";
myObject.d = "d";
{% endhighlight %}

那么这个时候，`myObject` 就有 4 个属性了，但是只有 c 和 d 是自己的，a 和 b 是继承自 `parentObject`。因为它们在同一条原型链上。那么此时我们要遍历 `myObject` 的属性，但是呢，我们只需要属于 `myObject` 的那部分属性，而不是所有属性。使用 jQuery 的话，可以用 `$.each()` 实现：

{% highlight javascript %}
$.each(myObject, function(propName, propValue) {
    // 处理逻辑
});
{% endhighlight %}

上述代码，会遍历 `myObject` 的所有属性，但是会忽略不直属于 `myObject` 的属性，也就是遍历出来的只有 c 和 d。

如果使用纯 javaScript 遍历一个对象，我们可以用 `for...in` 循环，如下：

{% highlight javascript %}
for (var myObjectProp in myObject) {
    // 处理逻辑
}
{% endhighlight %}

但是，但是如果只使用 `for...in` 会把继承的属性也循环出来，也就是遍历出来的属性有四个，a、b、c、d。显然不合适，此时必须使用 `Object.hasOwnProperty()` 加持，如下：

{% highlight javascript %}
for (var prop in myObject) {
    if (myObject.hasOwnProperty(prop)) {
        // 逻辑处理，只有 c 和 d 会进入到这
    }
}
{% endhighlight %}

以上代码兼容所有浏览器，但看起来略复杂，如果使用高级浏览器，则有更简便的方法，因为 ECMAScript 5 定义了一个新方法：`Object.keys(someObj)`。会把传入的对象的属性以数组的形式返回，同样不包含继承的属性，如下：

{% highlight javascript %}
Object.keys(myObject).forEach(function(prop) {
    // 逻辑处理，只有 c 和 d 会进入到这
});
{% endhighlight %}

以上代码只适用于高级浏览器。

### 遍历数组

我们有一个数组如下：

{% highlight javascript %}
var myArray = ['a', 'b', 'c'];
{% endhighlight %}

如使用 jQuery 遍历，同样使用 `$.each()`，如下：

{% highlight javascript %}
$.each(myArray, function(index, value) {
    // 逻辑处理
});
{% endhighlight %}

如使用纯 JavaScript 处理，可以使用 `for` 循环，如下：

{% highlight javascript %}
for (var i = 0; i < myArray.length; i++) {
    var arrayVal = myrray[i];
    // 逻辑处理
}
{% endhighlight %}

以上代码兼容所有浏览器，但是和 jQuery 的比起来，显然不够优雅，如果是高级浏览器，则可以使用 `Array.prototype.forEach` 这个方法，如下：

{% highlight javascript %}
myArray.forEach(function(value, index) {
    // 逻辑处理
}
{% endhighlight %}

以上代码，注意一点，传入的函数的参数，`value` 在前，`index` 在后，跟 jQuery 的正好相反，个人喜欢这个顺序。jQuery 把 `index` 放在前面，太反人类了。


### 查找数组中的元素

先设定一个简单的数组，如下：

{% highlight javascript %}
var theArray = ['a', 'b', 'c'];
{% endhighlight %}

如果要查找某个元素在数组中的索引值，使用 jQuery 如下：

{% highlight javascript %}
var indexOfValue = $.inArray('c', theArray);
{% endhighlight %}

以上的 `indexOfValue` 为 2，因为索引值是从 0 开始算的。如果说查找的元素不在数组里，则索引值会返回 -1 ，这个也是判断某个数组是否含有某个元素的做法。

如果要把数组中的某些元素过滤出来，比如在上述数组中，要把值不等于 b 的元素全部提取出来，可以使用 jQuery 的 `grep` 方法，如下：

{% highlight javascript %}
var allElementsThatMatch = $.grep(theArray, function(theArrayValue) {
    return theArrayValue !== 'b';
});
{% endhighlight %}

以上代码 `allElementsThatMatch` 会等于 ['a', 'c'] 。

如果使用纯 JavaScript 来查找数组中的某个元素，在高级浏览器可以使用 `Array.prototype.indexOf` 这个方法，如下：

{% highlight javascript %}
var indexOfValue = theArray.indexOf('c');
{% endhighlight %}

比较遗憾的是，这个方法在低级浏览器并不含有，比如 IE 8，那只能使用 `for` 循环了。如下：

{% highlight javascript %}
function indexOf(array, valToFind) {
    var foundIndex = -1;
    for (var index = 0; index < array.length; index++) {
        if (array[index] === valToFind) {
            foundIndex = index;
            break;
        }
    }
    return foundIndex;
}

indexOf(theArray, 'c');
// 返回 2
{% endhighlight %}

如果是要过滤元素，在高级浏览器可以使用 `Array.prototype.filter` 这个方法，如下：

{% highlight javascript %}
var allElementsThatMatch = theArray.filter(function(theArrayValue) {
    return theArrayValue !== 'b';
});
{% endhighlight %}

这个写法跟 jQuery 的 `grep` 很像了，如果是为了照顾低级浏览器，还是要使用 `for` 循环，如下：

{% highlight javascript %}
function filter(array, conditionFunction) {
var validValues = [];
    for (var index = 0; index < array.length; i++) {
        if (conditionFunction(theArray[index])) {
            validValues.push(theArray[index]);
        }
    }
}

var allElementsThatMatch = filter(theArray, function(arrayVal) {
    return arrayVal !== 'b';
})
// 返回 ['a', 'c']
{% endhighlight %}

### 把伪数组变成真数组

在 JavaScript 里，真数组是这样的：

{% highlight javascript %}
var realArray = ['a', 'b', 'c'];
{% endhighlight %}

那么啥是伪数组？如下：

{% highlight javascript %}
var pseudoArray1 = {
    1: 'a',
    2: 'b',
    3: 'c'
    length: 3
};
{% endhighlight %}

这个伪数组 `key` 和真数组一样，并且还有一个 `length` 属性。这是一个人造的伪数组，如果不人造呢，内置的有 <a href="https://developer.mozilla.org/zh-CN/docs/Web/API/NodeList" target="_blank">NodeList</a> ，<a href="https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCollection" target="_blank">HTMLCollection</a> 还有 <a href="https://developer.mozilla.org/zh-CN/docs/Web/API/FileList" target="_blank">FileList</a>。它们和数组很像，但它们又不是数组，以至于没办法用数组的方法进行操作。实际上它们是一个对象，但是又具备了 `length` 属性，所以我们可以把它们转化成数组，再用数组的方法进行操作。

如果是使用低级浏览器，到没有转换成真数组的必要，因为操作都要用 `for` 循环啊！如果是高级浏览器，希望使用数组的 `forEach`，`map`，`filter` 这些方法，那么就先要转化成真的数组了，使用 jQuery，可以用其之 `$.makeArray()` 实现，如下：

{% highlight javascript %}
var realArray = $.makeArray(pseudoArray);
{% endhighlight %}

如果使用纯 JavaScript 实现如下：

{% highlight javascript %}
var realArray = [].slice.call(pseudoArray);
{% endhighlight %}

如果只是为了把某个数组方法用在伪数组上，可以更简单，实现如下：

{% highlight javascript %}
[].forEach.call(pseudoArray, function(arrayValue) {
    // 逻辑处理
});
{% endhighlight %}

### 改变函数的上下文

函数的上下文，主要指该函数执行时，函数内部 `this` 的指向。比如有如下函数：

{% highlight javascript %}
function Outer() {
    var eventHandler = function() {
        this.foo = 'buzz';    
    };

    this.foo = 'bar';
    //定义 foo

    eventHandler();
    //重新定义 foo
}

var outer = new Outer();
{% endhighlight %}

在这个函数中，`outer` 等于 `{foo: "bar"}`，很明显 `eventHandler()` 并没有把 `this.foo` 的值重新定义，而我们需要的是 `outer.foo === 'buzz'`。为什么？通过 `console.log()` 打印 `this` 可以发现 `eventHandler()` 里面的 `this` 指向是全局 `window`，而外面的 `this` 指向的是 `Outer()` 本身。导致执行重新定义的时候，相当于给 window 增加了一个 `foo` 的属性，值为 `buzz`。此时我们就需要给 eventHandler() 指定一个上下文了。可以使用 jQuery 的 `$.proxy()` 实现，如下：

{% highlight javascript %}
function Outer() {
    var eventHandler = $.proxy(function() {
        this.foo = 'buzz';
    }, this);

    this.foo = 'bar';
    //定义 foo

    eventHandler();
    //重新定义 foo

var outer = new Outer();
{% endhighlight %}

注意 `$.proxy()` 的第二个参数是 `this`，而这个 `this` 因为是在外面，所以指到了 `Outer()` 本身，所以最后 `outer` 等于 `{foo: "buzz"}`，符合我们的需求。那么使用原生的 JavaScript 要如何实现？如果是高级浏览器，有内置了一个 <a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind" target="_blank">Function.prototype.bind()</a> 方法，实现起来和 jQuery 很像，如下：

{% highlight javascript %}
function Outer() {
    var eventHandler = function() {
        this.foo = 'buzz';
    }.bind(this);

    this.foo = 'bar';
    //定义 foo

    eventHandler();
    //重新定义 foo

var outer = new Outer();
{% endhighlight %}

注意 `eventHandler()` 最后的 `.bind(this)`，由此加持，最后 `outer` 等于 `{foo: "buzz"}` 符合需求。如果是 IE 8 以及以下浏览器，实现的方式其实也可以说挺方便的，如下：

{% highlight javascript %}
function Outer() {
    var self = this,
        eventHandler = function() {
            self.foo = 'buzz';
        };

    this.foo = 'bar';
    //定义 foo

    eventHandler();
    //重新定义 foo

var outer = new Outer();
{% endhighlight %}

重点在于 `var self = this` 这句，把外层函数的 `this` 定义成一个变量，然后内嵌函数以此操作。

### 清除字符串前后空格

价格有一个字符串，如下

{% highlight html %}
"  hi there!   "
{% endhighlight %}

现在我们要把前后的空格删除掉，结果需要如下：

{% highlight html %}
"hi there!"
{% endhighlight %}

如果使用 jQuery 可以用其内置的 `$.trim()` 方法，如下：

{% highlight javascript %}
$.trim('  hi there!   ');
{% endhighlight %}

如果是高级浏览器，其在 `String` 原型上也有内置的 `trim()` 方法，如下：

{% highlight javascript %}
'  hi there!   '.trim();
{% endhighlight %}

如果是低级浏览器，就没啥方法，不过可以使用正则表达式替换成空，如下：

{% highlight javascript %}
'  hi there!   '.replace(/^\s+|\s+$/g, '');
{% endhighlight %}

当然我们可以把二者结合起来，自己封装一个 trim 函数，以达到兼容效果，需要的话：

{% highlight javascript %}
function trim(string) {
    if (string.trim) {
        return string.trim();
    }
    return string.replace(/^\s+|\s+$/g, '');
}
{% endhighlight %}

### 把数据关联到 HTML 元素

把数据“关联”到元素的方法，最为直接的方法，作为属性添加到其标签上，但显然这样操作起来非常有限，完全应付不了复杂的数据，并且不安全，所以必须附属在内存上，而非标签本身。这样可以附属任何类型的数据，避免了循环引用而导致内存泄漏。

比方说，我们有两个元素：

{% highlight html %}
<div id="one">one</div>
<div id="two">two</div>
{% endhighlight %}

当我们点击其中一个，另一个元素在隐藏与显示之间切换，我们用 jQuery 的 `$.data()` 实现，如下：

{% highlight javascript %}
$('#one').data('partnerElement', $('#two'));
$('#two').data('partnerElement', $('#one'));

$('#one, #two').click(function() {
    $(this).data('partnerElement').toggle();
});
{% endhighlight %}

运行效果可以看这里 <a href="https://appblur.com/demos/not-jquery/html-data-1.html" target="_blank">https://appblur.com/demos/not-jquery/html-data-1.html</a>。jQuery 的 `$.data()` 会为每个需要关联数据的元素创建一个 ID，接着把这些 ID 作为属性集合到该元素的 JavaScript 对象上，然后使用这个 ID 去中心对象（jQuery 创建，数据统一储存在这）操作对应的值。用图表示：

![jquery.data](/files/notjQuery/jquery.data.jpg)

如果使用原生 JavaScript 实现这个功能呢？代码有点多：

{% highlight javascript %}
var data = (function() {
    var lastId = 0,
        store = {};

    return {
        set: function(element, info) {
            var id;
            if (element.myCustomDataTag === undefined) {
                id = lastId++;
                element.myCustomDataTag = id;
            }
            store[id] = info;
        },

        get: function(element) {
            return store[element.myCustomDataTag];
        }
    };
}());

var one = document.getElementById('one'),
    two = document.getElementById('two'),
    toggle = function(element) {
        if (element.style.display !== 'none') {
            element.style.display = 'none';
        }
        else {
            element.style.display = 'block';
        }
    };

data.set(one, {partnerElement: two});
data.set(two, {partnerElement: one});

one.addEventListener('click', function() {
    toggle(data.get(one).partnerElement);
});
two.addEventListener('click', function() {
    toggle(data.get(two).partnerElement);
});
{% endhighlight %}

运行效果可以看这里 <a href="https://appblur.com/demos/not-jquery/html-data-2.html" target="_blank">https://appblur.com/demos/not-jquery/html-data-2.html</a>。这段代码可以在所有的浏览器上运行，思路是，先创建两个函数，一个用于把 ID 关联到元素和关联到存储数据的中心对象，另一个函数负责检索数据。讲真，这一段和 jQuery 比起来实在太糟糕了，像个地狱一样。但是记住，这是运行在所有浏览器上的，所以略显复杂。但是浏览器和 JavaScript 都一直在更新，很多方式实现起来会越来越简单，ECMAScript 6 内置了一个全新的对象 <a href=“https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakMap” target=“_blank”>WeakMap”</a>，WeakMap 就是简单的键/值映射，但键只能是对象值，不可以是原始值。关于支持情况，WeakMap 支持的偏更高端：IE 11+，Chrome 36+， Safari 7.1 和 Firefox 6+。如果使用该对象实现上述功能，如下：

{% highlight javascript %}
var weakMap = new WeakMap(),
    one = document.getElementById('one'),
    two = document.getElementById('two'),
    toggle = function(element) {
        if (element.style.display !== 'none') {
            element.style.display = 'none';
        }
        else {
            element.style.display = 'block';
        }
    };

weakMap.set(one, {partnerElement: two});
weakMap.set(two, {partnerElement: one});

one.addEventListener('click', function() {
    toggle(weakMap.get(one).partnerElement);
});
two.addEventListener('click', function() {
    toggle(weakMap.get(two).partnerElement);
});
{% endhighlight %}

比较尴尬的是 `weakMap` 对 IE 的支持实在太差了，不过没关系，我们可以自己搞个 Polyfill，如下：

{% highlight javascript %}
var data = window.WeakMap ? new WeakMap() : (function() {
    var lastId = 0,
        store = {};

    return {
        set: function(element, info) {
            var id;
            if (element.myCustomDataTag === undefined) {
                id = lastId++;
                element.myCustomDataTag = id;
            }
            store[id] = info;
        },
        get: function(element) {
            return store[element.myCustomDataTag];
        }
    };
}());
{% endhighlight %}

这段代码可以在所有的浏览器工作，请放心使用。

### One More Thing

[《或许你不再需要使用 jQuery 了》](https://appblur.com/not-jquery.html) 这个系列，到此结束。希望阁下能够多少受益，天空多灰，我们亦放亮...
