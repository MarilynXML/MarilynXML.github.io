---
layout: post
title: "Javascript任务"
date: 2016-04-28 10：10：23
category: JavaScript
---

* content
{:toc}

Javascript基础，结合做的任务，主要总结下JS事件、DOM、BOM和Ajax。

## 练习1：处理兴趣列表

### 任务要求

在`task0002`目录下创建一个`task0002_1.html`文件，以及一个`js`目录和`css`目录，在`js`目录中创建`task0002_1.js`，并将之前写的`util.js`也拷贝到`js`目录下。然后完成以下需求。

**第一阶段**

在页面中，有一个单行输入框，一个按钮，输入框中用来输入用户的兴趣爱好，允许用户用半角逗号来作为不同爱好的分隔。

当点击按钮时，把用户输入的兴趣爱好，按照上面所说的分隔符分开后保存到一个数组，过滤掉空的、重复的爱好，在按钮下方创建一个段落显示处理后的爱好。

**第二阶段**

单行变成多行输入框，一个按钮，输入框中用来输入用户的兴趣爱好，允许用户用换行、空格（全角/半角）、逗号（全角/半角）、顿号、分号来作为不同爱好的分隔。

当点击按钮时的行为同上

**第三阶段**

用户输入的爱好数量不能超过10个，也不能什么都不输入。当发生异常时，在按钮上方显示一段红色的错误提示文字，并且不继续执行后面的行为；当输入正确时，提示文字消失。

同时，当点击按钮时，不再是输出到一个段落，而是每一个爱好输出成为一个checkbox，爱好内容作为checkbox的label。

### 思路

主要就是对字符串的操作，`split()` 的使用，以及正则表达式的使用。

### 实现

* [代码](https://github.com/Gaohaoyang/ife/tree/master/task/task0002/work/Gaohaoyang)
* [在线demo](http://gaohaoyang.github.io/ife/task/task0002/work/Gaohaoyang/task0002_1.html)


## 练习2：倒计时

### 任务要求

在和上一任务同一目录下面创建一个`task0002_2.html`文件，在`js`目录中创建`task0002_2.js`，并在其中编码，实现一个倒计时功能。

- 界面首先有一个文本输入框，允许按照特定的格式`YYYY-MM-DD`输入年月日；
- 输入框旁有一个按钮，点击按钮后，计算当前距离输入的日期的00:00:00有多少时间差
- 在页面中显示，距离YYYY年MM月DD日还有XX天XX小时XX分XX秒
- 每一秒钟更新倒计时上显示的数
- 如果时差为0，则倒计时停止

### 思路

* `setInterval()` 方法可按照指定的周期（以毫秒计）来调用函数或计算表达式。

     `setInterval()` 方法会不停地调用函数，直到 `clearInterval()` 被调用或窗口被关闭。由 `setInterval()` 返回的 ID 值可用作 `clearInterval()` 方法的参数。
* `clearInterval()` 方法可取消由 `setInterval()` 设置的 timeout。

    `clearInterval()` 方法的参数必须是由 `setInterval()` 返回的 ID 值。
* `setTimeout()` 方法用于在指定的毫秒数后调用函数或计算表达式。

    setTimeout() 只执行 code 一次。如果要多次调用，请使用 setInterval() 或者让 code 自身再次调用 setTimeout()。   
* `clearTimeout()` 方法可取消由 setTimeout() 方法设置的 timeout。

### 实现

* [代码](https://github.com/Gaohaoyang/ife/tree/master/task/task0002/work/Gaohaoyang)
* [在线demo](http://gaohaoyang.github.io/ife/task/task0002/work/Gaohaoyang/task0002_2.html)


## 练习3：图片轮播

### 任务要求

在和上一任务同一目录下面创建一个`task0002_3.html`文件，在`js`目录中创建`task0002_3.js`，并在其中编码，实现一个轮播图的功能。

- 图片数量及URL均在HTML中写好
- 可以配置轮播的顺序（正序、逆序）、是否循环、间隔时长
- 图片切换的动画要流畅
- 在轮播图下方自动生成对应图片的小点，点击小点，轮播图自动动画切换到对应的图片

效果示例：[http://echarts.baidu.com/](http://echarts.baidu.com/) 上面的轮播图（不需要做左右两个箭头）

### 思路

将图片排列成一排，一起向左运动，每次运动的距离刚好是一张图片的宽度。

对于下面的小圆点，使用事件代理，将事件传递给每个 a 标签。

**参考：**

* [JS图片切换](http://www.itxueyuan.org/view/6323.html)

### 实现

* [代码](https://github.com/Gaohaoyang/ife/tree/master/task/task0002/work/Gaohaoyang)
* [在线demo](http://gaohaoyang.github.io/ife/task/task0002/work/Gaohaoyang/task0002_3.html)

### 关于变速运动

评论中有人问到运动部分为什么这样写，下面我讲一下吧。

```js
function startMove(target) {
    clearInterval(timerInner);
    timerInner = setInterval(function() {
        var speed = (target - imgListDiv.offsetLeft) / 6;
        speed = speed > 0 ? Math.ceil(speed) : Math.floor(speed);

        imgListDiv.style.left = imgListDiv.offsetLeft + speed + "px";
    }, 30);
}
```

上面是运动部分代码。

* 参数 `target` 是运动终点的位置。
* 首先停止计时器，为了避免上一次调用方法时，计时器没有关闭带来的干扰。

```js
clearInterval(timerInner);
```

* 下面开始开启计时器，每隔 30ms 执行一次内部的函数。

* 变速运动

```js
var speed = (target - imgListDiv.offsetLeft) / 6;
```

    逐渐变慢，最后停止，距离越远速度越大，速度由距离决定

    速度=(目标值-当前值)/缩放系数

    这样写的原因就是为了让它做缓冲运动，而不是匀速运动，这样给用户带来的交互感觉会更好。

* 速度取整

```js
speed = speed > 0 ? Math.ceil(speed) : Math.floor(speed);
```

    像素不能是小数，所以速度大于0的时候，向上取整。速度小于0时，向下取整

* 最后关于运动终止条件。

```js
imgListDiv.style.left = imgListDiv.offsetLeft + speed + "px";
```

    由这一行可以看出，`imgListDiv.style.left` 在不断增大，即 `imgListDiv.offsetLeft` 在不断增大。这两个是相同的属性，只不过一个是在赋值时使用，第二个是在取值时使用。

    再看这行代码，由于这部分是每个30ms执行一次的，所以继续执行到这里。

```js
var speed = (target - imgListDiv.offsetLeft) / 6;
```

    当不断增大的 `imgListDiv.offsetLeft` 等于 `target` 时，`speed` 为0。宏观表现为不再运动，这便是运动终止的状态，但是这里的方法还是不断在执行，每个30ms在执行。



## 练习4：输入框即时提示

### 任务要求

在和上一任务同一目录下面创建一个`task0002_4.html`文件，在`js`目录中创建`task0002_4.js`，并在其中编码，实现一个类似百度搜索框的输入提示的功能。

要求如下：

- 允许使用鼠标点击选中提示栏中的某个选项
- 允许使用键盘上下键来选中提示栏中的某个选项，回车确认选中
- 选中后，提示内容变更到输入框中

**初级班：**

- 不要求和后端交互，可以自己伪造一份提示数据例如：

```js
var suggestData = ['Simon', 'Erik', 'Kener'];
```

**中级班：**

- 自己搭建一个后端Server，使用Ajax来获取提示数据

### 思路

这里我使用了给 input 标签加 input 监听，即输入框内容发生改变时，触发事件。并兼容到 IE7。

关于 input 监听的代码如下：

```js
function addInputListener() {
    if (inputArea.addEventListener) { // all browsers except IE before version 9
        inputArea.addEventListener("input", OnInput);
    }
    if (inputArea.attachEvent) { // Internet Explorer and Opera
        inputArea.attachEvent("onpropertychange", OnPropChanged); // Internet Explorer
    }
}

// Firefox, Google Chrome, Opera, Safari from version 5, Internet Explorer from version 9
function OnInput(event) {
    var inputValue = event.target.value;
    handleInput(inputValue);
}
// Internet Explorer
function OnPropChanged(event) {
    var inputValue = "";
    if (event.propertyName.toLowerCase() == "value") {
        inputValue = event.srcElement.value;
        handleInput(inputValue);
    }
}
```

其中 handleInput() 为下一步要执行的方法。

其实后来想了想也可以使用 keyup 事件了做这个任务。

匹配的过程同样适用正则表达式，从开头开始匹配。遍历备选单词，如果匹配成功，则放入 li 标签中，准备展示。

然后分别添加点击事件，键盘的 keydown 事件，用来选中提示出的单词。

**参考：**

* [oninput 事件](http://help.dottoro.com/ljhxklln.php)

### 实现

* [代码](https://github.com/Gaohaoyang/ife/tree/master/task/task0002/work/Gaohaoyang)
* [在线demo](http://gaohaoyang.github.io/ife/task/task0002/work/Gaohaoyang/task0002_4.html)



## 练习5：拖拽交互

### 任务要求

- 实现一个可拖拽交互的界面
- 如示例图，左右两侧各有一个容器，里面的选项可以通过拖拽来左右移动
- 被选择拖拽的容器在拖拽过程后，在原容器中消失，跟随鼠标移动
- 注意拖拽释放后，要添加到准确的位置
- 拖拽到什么位置认为是可以添加到新容器的规则自己定
- 注意交互中良好的用户体验和使用引导

### 思路

1. 页面布局时，将要被拖拽的 div 设置为绝对定位，因为这样在后面拖拽的时候才方便更改坐标。
2. 初始化界面的时候，首先让 div 块按照相应的高度重新排列一下。
3. 拖拽方法的实现。由 mousedown mousemove mouseup 三部分组成。
4. 在 mousemove 中判断，不能让鼠标拖出浏览器窗口。
5. 在 mouseup 中判断，是否到达指定区域。完成拖拽。

我在这里没有使用 html5 中的拖拽 API，所以兼容性还是很好的。

### 实现

* [代码](https://github.com/Gaohaoyang/ife/tree/master/task/task0002/work/Gaohaoyang)
* [在线demo](http://gaohaoyang.github.io/ife/task/task0002/work/Gaohaoyang/task0002_5.html)



