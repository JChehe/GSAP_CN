## TimelineMax

TimelineMax 对 TimelineLite 进行了扩展，在 TimelineLite 的基础上增加了很多有用（但非必要）的特性，如 repeat、repeatDelay、yoyo、currentLabel\(\)、tweenTo\(\)、tweenFromTo\(\)、getLabelAfter\(\)、getLabelBefore\(\) 和 getActive\(\) 等（将来会更多）。TimelineLite 是队列工具终极版，它充当了补间动画实例和其它时间轴实例的**容器，**使得整体控制和时间管理变得简单和精确。如果没有 TimelineLite，构建复杂的序列将会变得非常麻烦。

> 译者注：对于与 TimelineLite 重复的文档，在 TimelineMax 中就尽量省去。避免冗余，保证文档精简。

**下面是 TimelineMax 的其他一些好处和特点：**

* 对 time\(\)、totalTime\(\)、progress\(\) 或 totalProgress\(\) 的参数进行补间动画，以实现时间轴的快进/后退。甚至可以将两者中的一个绑定到滑块（slider）上，让用户能通过拖拽实现快进/后退时间轴。

* 可向构造函数的 `vars` 参数对象添加 `onComplete、onStart、onUpdate、onRepeat、onReverseComplete` 回调函数，如 `var tl = new TimelineMax({onComplete:myFunction});`

* 可对时间轴设置数次/无限次循环播放，也可以设置每次循环之间的延迟时间或设置为 yoyo 状态，以呈现来回循环播放的效果。

* 时间轴实例的 getActive\(\) 方法可获取当前活跃补间动画。

* 通过 `currentLabel()` 获取当前标记，或通过 getLabelAfter\(\) 和 getLabelBefore\(\) 获取时间轴上各个位置上的标记

**特殊属性和回调函数**

使用构建函数的 `vars` 参数去配置 TimelineMax 的各种选项。

```js
new TimelineMax({onComplete:myFunction, repeat:2, repeatDelay:1, yoyo:true});
```

TimelineMax 构建函数 vars 参数的所有属性如下（译者注：尽可能去掉与 TimelineLite 相同的属性，避免冗余）：

* onRepeat（Function）：动画每次重复播放时的回调函数。
* onRepeatScope（Object）：指定 `onRepeat` 函数的作用域（即函数内`"this"`的引用）。
* repeat（Number）：动画完成其第一次播放后应该重复的次数。若 `repeat` 为 1，则动画的总播放次数为 2（初始播放一次和重复播放一次）。-1 代表重复无数次。`repeat` 应总是为整数。
* repeatDelay（Number）：重复播放之间的间隔秒数（或帧，基于帧的补间动画）。若 `repeat` 为 2、`repeatDelay` 为 1，则播放首次后，需等待 1 秒再进行第二次播放，然后再等 1 秒后进行第三次播放。
* yoyo（Boolean）：yoyo（Boolean）：若为 `true`，则所有 `repeat` 循环均反向播放（译者注：对上一次循环的运动方向取反）。所以补间动画就呈现“往返”状态，但这并不会影响/更改 "`reversed`" 属性。因此，当 `repeat` 为 2、`yoyo` 为 `false` 时，将呈现：start - 1 - 2 - 3 - 1 - 2 - 3 - 1 - 2 - 3 - end。但若 `yoyo` 为 `true` 时，将呈现：start - 1 - 2 - 3 - 3 - 2 - 1 - 1 - 2 - 3 - end。
* onRepeatParams（Array）：传入`onRepeat`函数的参数数组。例如：TimelineMax`.to(mc, 1, {x:100, onRepeat:myFunction, onRepeatParams:["param1", "param2"]});`若需要在参数列表中引用补间动画实例自身，可使用`"{self}"`，如：`onRepeatParams:["{self}", "param2"]`

### 构建函数

TimelineMax\( vars: Object \);

### 属性

与 TimelineLite 相同，缺省

### 方法

[**currentLabel**](https://greensock.com/docs/TimelineMax/currentLabel%28%29)**\( value:String\) :\***

获取当前时间上或最近（当前时间之前）的一个标记，或快进至指定的标记（具体行为取决于是否向该方法传入参数）。

[**getActive**](https://greensock.com/docs/TimelineMax/getActive%28%29)**\( nested:Boolean, tweens:Boolean, timelines:Boolean\) :Array**

返回时间轴当前活跃的子元素实例（tweens/timelines），即时间轴当前进度所在的子元素（tweens/timelines），前提是该子元素未处于暂停状态。

[**getLabelAfter**](https://greensock.com/docs/TimelineMax/getLabelAfter%28%29)**\( time:Number\) :String**

返回指定时间参数后的一个标记（译者注：不包括等于的情况）

[**getLabelBefore**](https://greensock.com/docs/TimelineMax/getLabelBefore%28%29)**\( time:Number\) :String**

返回指定时间参数前的一个标记（译者注：不包括等于的情况）

[**getLabelsArray**](https://greensock.com/docs/TimelineMax/getLabelsArray%28%29)**\( \) :Array**

返回标记对象数组，标记对象拥有 "time" 和 "name" 属性，数组元素则按照"标记"在时间轴上出现的位置排序。

[**removeCallBack**](https://greensock.com/docs/TimelineMax/removeCallBack%28%29)**\( callback:Function, timeOrLabel:\*\) :TimelineMax**

移除特定位置上的一个回调函数。

[**removePause**](https://greensock.com/docs/TimelineMax/removePause%28%29)**\( position:\*\) :\***

移除通过 TimelineMax.addPause\(\) 向时间轴添加的暂停回调函数。

[**repeat**](https://greensock.com/docs/TimelineMax/repeat%28%29)**\( value:Number\) :\***

获取或设置补间动画首次完成后需要重复的次数。

[**repeatDelay**](https://greensock.com/docs/TimelineMax/repeatDelay%28%29)**\( value:Number\) :\***

获取或设置循环间的延迟秒数（或帧，基于帧的补间动画）。

[**tweenFromTo**](https://greensock.com/docs/TimelineMax/tweenFromTo%28%29)**\( fromPosition:\*, toPosition:\*, vars:Object\) :TweenLite**

创建一个线性补间动画，本质上是将进度从特定时间或标记处拖到另一特定时间或标记处。

[**tweenTo**](https://greensock.com/docs/TimelineMax/tweenTo%28%29)**\( position:\*, vars:Object\) :TweenLite**

创建一个线性补间动画，本质上是将进度拖到特定时间点或标记处，然后停止。

[**yoyo**](https://greensock.com/docs/TimelineMax/yoyo%28%29)**\( value:Boolean\) :\***

获取或设置时间轴的 yoyo 状态，true 代表每次循环都交替运动方向播放。

