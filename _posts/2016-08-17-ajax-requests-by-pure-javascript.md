---
layout: default
title: 使用纯 JavaScript 进行 Ajax 请求
category: not-jquery
---
在 [上一篇文章](https://appblur.com/not-jquery/2016/08/16/dom-manipulation-by-pure-javascript.html) 中，讲到如何使用原生 JavaScript 进行 DOM 操作。 除了 DOM 操作，jQuery 还封装了一个优秀功能，Ajax 请求操作。现在的大多项目中，几乎所有的数据全部基于 Ajax 请求实现，实行前后端分离了嘛，不再像之前 HTML 与动态语言（PHP，Java...）混在一起。那么到后来，Ajax 也有很多高级一点的进化，比如使用 <a href="https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API" target="_blank">fetch API</a>，<a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise" target="_blank">Promise对象</a>。这暂不在本文范畴内，本文讲原始一点的 `XMLHttpRequest` 请求，包括跨域之类的，因为兼容性强嘛。

### GET 请求

最简单最常见的请求，比如我们需要用向服务器请求用户名，唯一的参数是 ID，ID 不重复，那 URL 即是 `myservice/username?id=some-unique-id` 如果请求错误或者失败需要有报错或错误提示。

使用 jQuery 有方法 `$.ajax()` 还有简写的 `$.get()`。如果使用第一种：

{% highlight javascript %}
$.ajax('myservice/username', {
    data: {
        id: 'some-unique-id'
    }
})
.then(
    function success(name) {
        alert('用户名：' + name);
    },
    function fail(data, status) {
        alert('请求失败，返回状态：' + status);
    }
);
{% endhighlight %}

如果使用原生的 `XMLHttpRequest` 进行请求：

{% highlight javascript %}
var xhr = new XMLHttpRequest();
xhr.open('GET', 'myservice/username?id=some-unique-id');
xhr.onload = function() {
    if (xhr.status === 200) {
        alert('用户名：' + xhr.responseText);
    }
    else {
        alert('请求失败，返回状态：' + xhr.status);
    }
};
xhr.send();
{% endhighlight %}

上述的原生方法，只能在 IE 7+ 上运行，如果是 IE 6 则需要把 `new XMLHttpRequest()` 替换成 `new ActiveXObject("MSXML2.XMLHTTP.3.0")`。使用原生方法，虽然看起来不够复杂，但是在直观上美观上难免差强人意，毕竟没经过包装嘛！

### POST 请求

POST 也是很常见的请求方式，至于和 GET 有和差别，在此不表，也不是三两句可以说得完的。把上述 GET 的场景升级一下，还是原来的接口，还是一个参数 ID，但是这次我们不是要取得用户名，而是修改用户名，这意味着需要多一个参数 name ，把参数放在请求报文内，并且 URL 参数需要先进行 URL 编码，服务器会返回是否更新。为什么要进行编码，因为在某些浏览器，可能会报错。

使用 jQuery 和 `GET` 方式差不多，但 `method` 要改成 POST，如下：

{% highlight javascript %}
var newName = 'John Smith';

$.ajax('myservice/username?' + $.param({id: 'some-unique-id'}), {
    method: 'POST',
    data: {
        name: newName
    }
})
.then(
    function success(name) {
        if (name !== newName) {
            alert('发生错误.  当前用户名是：' + name);
        }
    },
    function fail(data, status) {
        alert('请求失败，返回状态：' + status);
    }
);
{% endhighlight %}

如果使用原生的 `XMLHttpRequest` 进行请求：

{% highlight javascript %}
var newName = 'John Smith',
    xhr = new XMLHttpRequest();

xhr.open('POST', 'myservice/username?id=some-unique-id');
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.onload = function() {
    if (xhr.status === 200 && xhr.responseText !== newName) {
        alert('发生错误.  当前用户名是：' + xhr.responseText);
    }
    else if (xhr.status !== 200) {
        alert('请求失败，返回状态：' + xhr.status);
    }
};
xhr.send(encodeURI('name=' + newName));
{% endhighlight %}

再看一下，还是感觉 jQuery 的方法会更优雅一些。但考虑到需要引入整个 jQuery 文件，那么如果是几个简单的请求，显然是不需要引入 jQuery 的，或者自己封装一下 `XMLHttpRequest` ，也会看起来很优雅！

### URL 编码

jQuery 内置了一个方法，`$.param()` 可以把一个对象编码成符合 URL 规范的参数，比如：

{% highlight javascript %}
$.param({
    key1: 'some value',
    'key 2': 'another value'
});
{% endhighlight %}

转换之后

	key1=some+value&key+2=another+value

如果是参数里含有中文

{% highlight javascript %}
$.param({
    key1: '金正恩',
    'key 2': 'another value'
});
{% endhighlight %}

转换之后

	key1=%E9%87%91%E6%AD%A3%E6%81%A9&key+2=another+value

原生的有提供 <a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI" target="_blank">encodeURI</a> 和 <a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent" target="_blank">encodeURIComponent</a> 两个方法，二者差别，自行去研究。但是实现起来并没有如同 jQuery 一样来得优雅。 不过没关系，可以自己写一个类似的函数，如下：

{% highlight javascript %}
function param(object) {
    var encodedString = '';
    for (var prop in object) {
        if (object.hasOwnProperty(prop)) {
            if (encodedString.length > 0) {
                encodedString += '&';
            }
            encodedString += encodeURIComponent(prop + '=' + object[prop]);
        }
    }
    return encodedString;
}
{% endhighlight %}

### 发送和接收 JSON

JSON 数据格式，已经是前后端通讯 API 的标配了，通常也是请求的参数和返回的值都是 JSON 格式，比如我们要更新服务器数据库某个表的值，更新成功后，会把最新的值返回回来，在此我们使用 PUT 请求，jQuery 方式：

{% highlight javascript %}
$.ajax('myservice/user/1234', {
    method: 'PUT',
    contentType: 'application/json',
    processData: false,
    data: JSON.stringify({
        name: 'John Smith',
        age: 34
    })
})
.then(
    function success(userInfo) {
        // userInfo 是一个 JavaScript 对象
    }
);
{% endhighlight %}

如果使用原生方式：

{% highlight javascript %}
var xhr = new XMLHttpRequest();
xhr.open('PUT', 'myservice/user/1234');
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.onload = function() {
    if (xhr.status === 200) {
        var userInfo = JSON.parse(xhr.responseText);
    }
};
xhr.send(JSON.stringify({
    name: 'John Smith',
    age: 34
}));
{% endhighlight %}

上面的原生方式只能在 IE 8+ 上起作用，因为更低级的浏览器是不支持原生 JSON 对象的。修复方案是是引入 <a href="https://github.com/douglascrockford/JSON-js" target="_blank">json.js</a>。如果是使用 POST 请求，修改 method 即可。

### 上传文件

在包括 IE 9 在内及以下的浏览器，上传文件需要具备两个条件，`<form>` 标签和标签内的字段 `<input type="file">` ，这两个是必须的。但是高级浏览器是可以通过 File API 上传文件，也就是说用 Ajax 的方式上传。

首先需要一个 HTML 标签

{% highlight html %}
<input type="file" id="test-input">
{% endhighlight %}

使用 jQuery 上传

{% highlight javascript %}
var file = $('#test-input')[0].files[0],
    formData = new FormData();

formData.append('file', file);

$.ajax('myserver/uploads', {
    method: 'POST',
    contentType: false,
    processData: false,
    data: formData
});
{% endhighlight %}

注意 `contentType: false` 和 `processData: false` 这两个参数，这是什么意思？这是必须必须设置的，主要是为了防止 jQuery 不会在请求头部插入自己的 Content-Type，因为浏览器必须指明内容类型，以至于服务器才可以正确解析。当然也可以换一种写法，不要把数据 append 到 formData 对象里，直接作为一个数据载体：

{% highlight javascript %}
var file = $('#test-input')[0].files[0];

$.ajax('myserver/uploads', {
    method: 'POST',
    contentType: file.type,
    processData: false,
    data: file
});
{% endhighlight %}

这样看起来会更好一点点，但是仍然必须指明 `processData` 为 false，也是用来防止 jQuery 对数据进行 URL 编码。

使用原生的 XMLHttpRequest 进行上传：

{% highlight javascript %}
var formData = new FormData(),
    file = document.getElementById('test-input').files[0],
    xhr = new XMLHttpRequest();

formData.append('file', file);
xhr.open('POST', 'myserver/uploads');
xhr.send(formData);
{% endhighlight %}

如果不使用 `append()` 则可以写成如下：

{% highlight javascript %}
var file = document.getElementById('test-input').files[0],
    xhr = new XMLHttpRequest();

xhr.open('POST', 'myserver/uploads');
xhr.setRequestHeader('Content-Type', file.type);
xhr.send(file);
{% endhighlight %}

在这点上，原生的比 jQuery 看起来会更加好看一点。

### CORS 跨域

CORS 意思是 Cross Origin Resource Sharing，字面理解是，跨资源共享，即发送跨域Ajax请求。这也是相当复杂的话题，网上各种文章，各种分析也很多了。如果不清楚 CORS 和同源策略，请参考这个 <a href="https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS" target="_blank">HTTP访问控制(CORS)</a> 。

在高级浏览器，发送一个跨域 Ajax 请求难度并不大。但是在 IE8，IE9 会略显麻烦一点。主要重点是，设置自身的请求头 `Origin` 属性，在被请求域需要设置响应头 `Access-Control-Allow-Origin` 属性，并且注意 https 不用发起 http 请求。还有一点要注意的，在默认的跨域请求中，是不会携带凭证信息（如：cookie）的，如果有此需求，需要在 `XMLHttpRequest` 对象设置属性 `withCredentials = true`。

下面是使用 jQuery 发起一个跨域请求：

{% highlight javascript %}
$.ajax('http://someotherdomain.com', {
    method: 'POST',
    contentType: 'text/plain',
    data: 'sometext',
    beforeSend: function(xmlHttpRequest) {
        xmlHttpRequest.withCredentials = true;
    }
});
{% endhighlight %}

如果用原生的则如下：

{% highlight javascript %}
var xhr = new XMLHttpRequest();
xhr.open('POST', 'http://someotherdomain.com');
xhr.withCredentials = true;
xhr.setRequestHeader('Content-Type', 'text/plain');
xhr.send('sometext');
{% endhighlight %}

在这一点上，jQuery 也没啥优势。如果需要在 IE 8/9 上进行跨域请求，则 jQuery 不但没有任何优势，反而会让人有点头疼。那为什么 jQuery 在 IE 8/9 上进行跨域请求的时候会如此无力，首选需要先学习/复习几个知识点：

>1，首先在 IE 8/9 上进行跨域请求只能使用 IE 专有的 <a href="https://developer.mozilla.org/zh-CN/docs/Web/API/XDomainRequest" target="_blank">XDomainRequest</a> 对象，这是一个非标准特性，显然得到的社区支持也是很有限的。
>
>2，恰恰比较致命的是，jQuery 的 `$.ajax()` 是在 `XMLHttpRequest` 基础上封装的，也就是所有请求都基于 XMLHttpRequest 实现。那么如何实现在 IE 8/9 实现跨域请求？首先，可以使用插件，毕竟它有强大的插件支持。还一招，最直接的，你可以不使用 jQuery，直接使用原生的 JavaScript，重点代码如下：

{% highlight javascript %}
if (new XMLHttpRequest().withCredentials === undefined) {
    var xdr = new XDomainRequest();
    xdr.open('POST', 'http://someotherdomain.com');
    xdr.send('sometext');
}
{% endhighlight %}

要注意一点，在使用 `XDomainRequest` 是没办法设置任何请求报头的。

### JSONP 请求

不大建议使用 JSONP ，涉及到安全问题，如果你的项目属于 HTML 5，强烈建议使用 CORS。如果你不了解，先来理解一下什么是 JSONP，不要被它的名字所欺骗，它跟 JSON 没有任何关系，甚至跟 `XMLHttpRequest` 都没有关系。因为它返回的是一串像调用函数一样的文本，然后 把它当作 JavaScript 脚本来执行，以达到跨域的目的。简单思路如下：

* 1，定义函数 function Fn(data) { ……. }
* 2，发送 JSONP 请求
* 3，创建一个 script 标签，src 设置为 JSONP 请求路径
* 4，JSONP 返回的是 Fn({a:1, b:2})，相当于带参数执行了函数 Fn() ，以此达到跨域目的

之所以说跟 `XMLHttpRequest` 没关系，是因为 JSONP 是一个脚本，而脚本是不受同源策略限制的，也可以理解成是一个注入脚本攻击。用 jQuery 实现 JSONP 请求：

{% highlight javascript %}
$.ajax('http://jsonp-aware-endpoint.com/user', {
    jsonp: 'callback',
    dataType: 'jsonp',
    data: {
        id: 123
    }
}).then(function(response) {
});
{% endhighlight %}

注意两个参数 `jsonp: 'callback'` 和 `和 dataType: 'jsonp'`。如果使用原生方式，如下：

{% highlight javascript %}
function jsonp(url, callback) {
		var callbackName = 'jsonp_callback_' + Math.round(100000 * Math.random());
		window[callbackName] = function(data) {
			delete window[callbackName];
			document.body.removeChild(script);
			callback(data);
		};
		var script = document.createElement('script');
		script.src = url + (url.indexOf('?') >= 0 ? '&' : '?') + 'callback=' + callbackName;
		document.body.appendChild(script);
}

//调用方式
jsonp('http://jsonp-aware-endpoint.com/user?id=1',function(data){
	alert(data)
	})
{% endhighlight %}


### 扩展阅读

<a href="https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API" target="_blank">Fetch API</a>，相比于 XMLHttpRequest 更强大更灵活。

<a href="https://github.com/jpillora/xdomain" target="_blank">xdomain</a>，一个迷你的 CORS 库，支持 IE 8 9 10。

<a href="https://github.com/erikarenhill/Lightweight-JSONP
" target="_blank">Lightweight-JSONP</a>，如名称所示，一个迷你的 JSONP 请求库。
