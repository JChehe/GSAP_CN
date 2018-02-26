## TweenMax

TweenMax 对 TweenLite 进行了扩展，添加了很多有用（但非必要）的特性，如 `repeat()`、`repeatDelay()`、`yoyo()` 等。它同时默认引入了很多额外的插件，以保证自身的全面性。当然，这些插件也适用于 TweenLite。只不过 TweenMax 帮你省了加载通用插件这一步骤，这些插件有：CSSPlugin、RoundPropsPlugin、BezierPlugin、AttrPlugin、DirectionalRotationPlugin、EasePack、TimelineLite 和 TimelineMax。因为 TweenMax 是 TweenLite 的扩展，所以能在其基础上做更多事情。两者的语法一致，可在项目中混合使用。但若需要考虑文件大小，则最好坚持使用 TweenLite，除非是 TweenMax 的专属特性。

> 译者注：对于与 TweenLite 重复的文档，在 TweenMax 中就尽量省去。避免冗余，保证文档精简。

### 入门

视频地址：[https://youtu.be/tMP1PCErrmE](https://youtu.be/tMP1PCErrmE)

### 用法

补间动画最常用的方式是 TweenMax.to\(\)，它允许你定义目标值：

```js
var photo = document.getElementById("photo");
TweenMax.to(photo, 2, {width:"200px", height:"150px"});
```

### 特殊属性、回调函数与缓动

除了处理补间动画的目标值，vars 对象还提供了更多的选项配置以配置补间动画：

```js
TweenMax.to(element, 1, {opacity:0, onComplete:completeHandler, ease:Back.easeOut, useFrames:true});
```

以下是 TweenMax 属性的描述：

* repeat（Number）：动画完成其第一次播放后应该重复的次数。若 `repeat` 为 1，则动画的总播放次数为 2（初始播放一次和重复播放一次）。-1 代表重复无数次。`repeat` 应总是为整数。
* repeatDelay（Number）：重复播放之间的间隔秒数（或帧，基于帧的补间动画）。若 `repeat` 为 2、`repeatDelay` 为 1，则播放首次后，需等待 1 秒再进行第二次播放，然后再等 1 秒后进行第三次播放。
* yoyo（Boolean）：若为 `true`，则所有 `repeat` 循环均反向播放。所以补间动画就呈现“往返”状态，但这并不会影响/更改 "`reversed`" 属性。因此，当 `repeat` 为 2、`yoyo` 为 `false` 时，将呈现：start - 1 - 2 - 3 - 1 - 2 - 3 - 1 - 2 - 3 - end。但若 `yoyo` 为 `true` 时，将呈现：start - 1 - 2 - 3 - 3 - 2 - 1 - 1 - 2 - 3 - end。
* yoyoEase（Ease \| Boolean）：可将 `yoyoEase` 设置 `Power2.easeOut` 或通过 `yoyoEase: true` 简单地对目前的缓动函数取反。注意：TweenMax 足够聪明，当有定义 yoyoEase 时，会自动设置 yoyo: true。这是为了让你编写更少的代码（GSAP 1.20.0 起）。
* onRepeat（Function）：动画每次重复播放时的回调函数。
* onRepeatParams（Array）：传入`onRepeat` 函数的参数数组。例如：`TweenMax.to(mc, 1, {x:100, onRepeat:myFunction, onRepeatParams:[mc, "param2"]});`若需要在参数列表中引用补间动画实例自身，可使用`"{self}"`，如：`onRepeatParams:["{self}", "param2"]`
* onRepeatScope（Object）：指定 `onRepeat` 函数的作用域（即函数内`"this"`的引用）。
* startAt（Object）：

### 插件

以下插件均被 TweenMax 自动激活：

* css（Object）：
* roundProps（String）：
* bezier（Object）：

### 构造函数

TweenMax\( target: object, duration: Number, vars: Object \);

### 属性

与 TweenLite 相同。



### 方法

getAllTweens\( includeTimelines: Boolean \): Array

globalTimeScale\( value: Number \): 

isTweening\( target: Object \): Boolean

killAll\( complete: Boolean, tweens: Boolean, delayedCalls: Boolean, timelines: Boolean \):

killChildTweensOf\( parent: Object, complete: Boolean \):

pauseAll\( tweens: Boolean, delayedCalls: Boolean, timelines: Boolean \):

repeat\( value: Number \): \*

repeatDelay\( value: Number \): \*

resumeAll\( tweens: Boolean, delayedCalls: Boolean, timelines: Boolean \): 

staggerFrom\( targets: Array, duration: Number, vars: Object, stagger: Number, onCompleteAll: Function, onCompleteAllParams: Array, onCompleteAllScope: \* \): Array

staggerFromTo\( targets:Array, duration:Number, fromVars:Object, toVars:Object, stagger:Number, onCompleteAll:Function, onCompleteAllParams:Array, onCompleteAllScope:\* \) : Array

staggerTo\( targets:Array, duration:Number, vars:Object, stagger:Number, onCompleteAll:Function, onCompleteAllParams:Array, onCompleteAllScope:\* \) : Array

updateTo\( vars:object, resetDuration:Boolean \) : \*

yoyo\( value:Boolean \) : \*

