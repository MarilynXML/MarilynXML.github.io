---
layout: post
title: "Javascript基础2"
date: 2016-04-22 22：10：24
category: JavaScript
---

* content
{:toc}

Javascript基础，结合做的任务，主要总结下JS事件、DOM、BOM和Ajax。

## 事件

### 绑定注册事件与移除事件

**任务与实现：**

```js
// 给一个element绑定一个针对event事件的响应，响应函数为listener
function addEvent(element, event, listener) {
    if (element.addEventListener) {
        element.addEventListener(event,listener);
    } else if(element.attachEvent){
        element.attachEvent("on"+event,listener);
    }
}

// 移除element对象对于event事件发生时执行listener的响应
function removeEvent(element, event, listener) {
    if (element.removeEventListenr) {
        element.removeEventListenr(event,listener);
    } else if(element.detachEvent){
        element.detachEvent("on"+event,listener);
    }
}
```

**相关说明：**

IE8+ 支持 `addEventListener()`。IE8 以下的版本使用 `attachEvent()`。

* `attachEvent()` 不支持事件捕获。
* `attachEvent()` 第一个参数事件处理程序属性名使用前缀 on。
* `attachEvent()` 允许相同的事件处理程序函数注册多次。


### click 与 enter 键事件绑定

**任务与实现：**

```js
// 实现对click事件的绑定
function addClickEvent(element, listener) {
    addEvent(element, "click", listener);
}

// 实现对于按Enter键时的事件绑定
function addEnterEvent(element, listener) {
    addEvent(element, "keydown", function(event) {
        if (event.keyCode == 13) {
            listener();
        }
    });
}
```

**相关说明：**

这里我直接使用了上一个任务写好的 `addEvent()` 函数。这样可以简化代码，并有良好的兼容性。

enter 键的 keyCode 为 13。


### 事件代理

**参考：**

* [javascript事件代理（委托）](http://www.cnblogs.com/Aralic/p/4446030.html)
* [JS - 事件代理](http://www.cnblogs.com/leo388/p/4461579.html)

**任务与实现：**

```js
function delegateEvent(element,tag,eventName,listener){
    addEvent(element, eventName, function(event){
        var target = event.target || event.srcElement;
        if(target.tagName.toLowerCase() == tag.toLowerCase()) {
            listener.call(target, event);
        }
    });
}
```




## DOM



### 基本任务

**任务：**

先来一些简单的，在你的util.js中完成以下任务：

```js
// 为element增加一个样式名为newClassName的新样式
function addClass(element, newClassName) {
    // your implement
}

// 移除element中的样式oldClassName
function removeClass(element, oldClassName) {
    // your implement
}

// 判断siblingNode和element是否为同一个父元素下的同一级的元素，返回bool值
function isSiblingNode(element, siblingNode) {
    // your implement
}

// 获取element相对于浏览器窗口的位置，返回一个对象{x, y}
function getPosition(element) {
    // your implement
}
```

**思路：**

* `addClass()`

    对于element本身如果没有样式类，那么使用Element的className属性获取的是空字符串，则直接添加新的样式类字符串即可。对于已经有了样式类的元素，获取到原有的样式类后，在后面添加一个空格，再添加新的样式类即可。

* `removeClass()`

    获取原始的样式，然后用正则表达式去匹配这个要删掉的样式，由于是动态的正则表达式，所以要用正则的构造函数 `RegExp()` 来创建，并且使用 `\b` 来确定单词边界。匹配好后用空字符串替换被匹配的样式类即可。

* `isSiblingNode()`

    直接判断两个父节点是不是相同


*   使用 `getBoundingClientRect()` 方法获取当前元素相对于可视区域的位置，再加上滚动条的位置。

    关于滚动条的位置 `scrollTop`, `scrollLeft` 这两个属性的使用，各个浏览器还都不一样。

    * 详情见 [document.body.scrollTop or document.documentElement.scrollTop](http://www.cnblogs.com/zhenyu-whu/archive/2012/11/13/2768004.html)。

    简单的说就是：FF、Opera 和 IE 浏览器认为在客户端浏览器展示的页面的内容对应于整个 HTML，所以使用 `document.documentElement`来代表，相应的滚动距离则通过 `document.documentElement.scrollLeft` 和 `document.documentElement.scrollTop` 来获取，而 Safari 和 Chrome 浏览器则认为页面开始于 body 部分，从而相应的滚动距离用 `document.body.scrollLeft` 和 `document.body.scrollTop` 来获取。另外需要注意的是，FF 和 IE 的 quirks mode（兼容模式）下是用 `document.body` 来获取的。

    documentElement 对应的是 html 标签，而 body 对应的是 body 标签

    针对跨浏览器的解决方案则可简单的用如下代码获取：

```js
var scrollLeft = Math.max(document.documentElement.scrollLeft, document.body.scrollLeft);
var scrollTop = Math.max(document.documentElement.scrollTop, document.body.scrollTop);
```

**实现：**

```js
// 为element增加一个样式名为newClassName的新样式
function addClass(element, newClassName) {
    var oldClassName = element.className; //获取旧的样式类
    element.className = oldClassName === "" ? newClassName : oldClassName + " " + newClassName;
}

// 移除element中的样式oldClassName
function removeClass(element, oldClassName) {
    var originClassName = element.className; //获取原先的样式类
    var pattern = new RegExp("\\b" + oldClassName + "\\b"); //使用构造函数构造动态的正则表达式
    element.className = originClassName.replace(pattern, '');
}

function isSiblingNode(element, siblingNode) {
    return element.parentNode === siblingNode.parentNode;
}

function getPosition(element) {
    var pos={};
    pos.x = element.getBoundingClientRect().left + Math.max(document.documentElement.scrollLeft, document.body.scrollLeft);
    pos.y = element.getBoundingClientRect().top + Math.max(document.documentElement.scrollTop, document.body.scrollTop);
    return pos;
}
```





## BOM

**任务与实现：**

```js
// 判断是否为IE浏览器，返回-1或者版本号
function isIE() {
    var s = navigator.userAgent.toLowerCase();
    console.log(s);
    //ie10的信息：
    //mozilla/5.0 (compatible; msie 10.0; windows nt 6.2; trident/6.0)
    //ie11的信息：
    //mozilla/5.0 (windows nt 6.1; trident/7.0; slcc2; .net clr 2.0.50727; .net clr 3.5.30729; .net clr 3.0.30729; media center pc 6.0; .net4.0c; .net4.0e; infopath.2; rv:11.0) like gecko
    var ie = s.match(/rv:([\d.]+)/) || s.match(/msie ([\d.]+)/);
    if(ie) {
        return ie[1];
    } else {
        return -1;
    }
}

// 设置cookie
function setCookie(cookieName, cookieValue, expiredays) {
    var cookie = cookieName + "=" + encodeURIComponent(cookieValue);
    if (typeof expiredays === "number") {
        cookie += ";max-age=" + (expiredays * 60 * 60 * 24);
    }
    document.cookie = cookie;
}

// 获取cookie值
function getCookie(cookieName) {
    var cookie = {};
    var all = document.cookie;
    if (all==="") {
        return cookie;
    }
    var list = all.split("; ");
    for (var i = 0; i < list.length; i++) {
        var p = list[i].indexOf("=");
        var name = list[i].substr(0, p);
        var value = list[i].substr(p + 1);
        value = decodeURIComponent(value);
        cookie[name] = value;
    }
    return cookie;
}
```

* 参考自：JavaScript权威指南


### sessionStorage、localStorage 和 cookie 之间的区别

* **共同点**

    都是保存在浏览器端，且同源的。都是键值对存储。

* **区别**

    特性 | Cookie | localStorage | sessionStorage
    数据的声明周期 | 一般由服务器生成，可设置失效时间。如果在浏览器端生成Cookie，默认是关闭浏览器后失效 | 除非被清除，否则永久保存 | 仅在当前会话下有效，关闭页面或浏览器后被清除
    存放数据大小 | 4K左右 | 一般为5MB | 同左
    与服务器端通信 | 每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题 | 仅在客户端（即浏览器）中保存，不参与和服务器的通信 | 同左
    易用性 | 需要程序员自己封装，源生的Cookie接口不友好 | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 | 同左

* **应用场景**

    每个 HTTP 请求都会带着 Cookie 信息，所以 Cookie 应当简单，比如判断用户是否登陆。

    localStorage 接替 Cookie 管理购物车，同时也可以存储 HTML5 游戏的一些本地数据。

    sessionStorage 在表单内容较多的时候，为了优化用户体验，按步骤分页引导填写，这时使用sessionStorage 就发挥了作用。

* **安全性**

    cookie 中最好不要放置任何明文的东西。两个 storage的数据提交后在服务端一定要校验

**参考：**

* [详说 Cookie, LocalStorage 与 SessionStorage](http://jerryzou.com/posts/cookie-and-web-storage/)


## Ajax

**任务：**

```js
// 学习Ajax，并尝试自己封装一个Ajax方法。实现如下方法：
function ajax(url, options) {
    // your implement
}

// 使用示例：
ajax(
    'http://localhost:8080/server/ajaxtest',
    {
        data: {
            name: 'simon',
            password: '123456'
        },
        onsuccess: function (responseText, xhr) {
            console.log(responseText);
        }
    }
);　
```

**实现：**

```js
function ajax(url, options) {

    var dataResult; //结果data

    // 处理data
    if (typeof(options.data) === 'object') {
        var str = '';
        for (var c in options.data) {
            str = str + c + '=' + options.data[c] + '&';
        }
        dataResult = str.substring(0, str.length - 1);
    }

    // 处理type
    options.type = options.type || 'GET';

    //获取XMLHttpRequest对象
    var xhr = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject('Microsoft.XMLHTTP');

    // 发送请求
    oXhr.open(options.type, url, true);
    if (options.type == 'GET') {
        oXhr.send(null);
    } else {
        oXhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
        oXhr.send(dataResult);
    }

    // readyState
    xhr.onreadystatechange = function() {
        if (xhr.readyState === 4) {
            if (xhr.status === 200) {
                if (options.onsuccess) {
                    options.onsuccess(xhr.responseText, xhr.responseXML);
                }
            } else {
                if (options.onfail) {
                    options.onfail();
                }
            }
        }
    };
}
```

**说明：**

1. 首先是处理 data，因为测试用例中的 data 是对象，所以把它遍历出来，把键和值中间用 = 连接，和下一组数据用 & 连接。
2. 处理 type，默认是 GET 请求。
3. 使用 `open()` 指明请求方法和 url。方法一般为 GET 或 POST。
4. 调用 `send()` 方法，GET 请求没有主体，所以应该传递 null 或省略这个参数。POST 请求有主体，同时使用 `setRequestHeaders()` 来指定 "Content-type" 头。这样便成功发送了请求。
5. 取的响应。一个完整的 HTTP 响应是由状态码、响应头集合、响应主体组成。
    * `readyState` 是一个整数，它指定了 HTTP 请求的状态。其值和含义如下表：

    值 | 含义
    0 | open() 尚未调用
    1 | open() 已调用
    2 | 接收到响应头信息
    3 | 接收到响应主体
    4 | 响应完成

    * `status` 和 `statusText` 属性以数字和文本的形式返回 HTTP 状态码。这些属性保存标准的 HTTP 值。如，200和 "OK" 表示成功请求，404和 "Not Found" 表示 URL 不能匹配服务器上的任何资源。
    * `getResponseHeader()` 和 `getAllResponseHeaders()` 能查询响应头。
    * 响应主体可以从 `responseText` 属性中得到文本形式的，从 `responseXML` 属性中得到 Document 形式的。

6. 补充一点 `onreadystatechange` 事件会在 `readyState` 改变时被触发。

**参考：**

* [Ajax W3C](http://www.w3school.com.cn/ajax/index.asp)
* [Comet：基于 HTTP 长连接的“服务器推”技术](http://www.ibm.com/developerworks/cn/web/wa-lo-comet/)
