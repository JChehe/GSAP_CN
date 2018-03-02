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
* yoyo（Boolean）：若为 `true`，则所有 `repeat` 循环均反向播放（译者注：对上一次循环的运动方向取反）。所以补间动画就呈现“往返”状态，但这并不会影响/更改 "`reversed`" 属性。因此，当 `repeat` 为 2、`yoyo` 为 `false` 时，将呈现：start - 1 - 2 - 3 - 1 - 2 - 3 - 1 - 2 - 3 - end。但若 `yoyo` 为 `true` 时，将呈现：start - 1 - 2 - 3 - 3 - 2 - 1 - 1 - 2 - 3 - end。
* yoyoEase（Ease \| Boolean）：可将 `yoyoEase` 设置 `Power2.easeOut` 或通过 `yoyoEase: true` 简单地对目前的缓动函数取反。注意：TweenMax 足够聪明，当有定义 yoyoEase 时，会自动设置 yoyo: true。这是为了让你编写更少的代码（GSAP 1.20.0 起）。
* onRepeat（Function）：动画每次重复播放时的回调函数。
* onRepeatParams（Array）：传入`onRepeat` 函数的参数数组。例如：`TweenMax.to(mc, 1, {x:100, onRepeat:myFunction, onRepeatParams:[mc, "param2"]});`若需要在参数列表中引用补间动画实例自身，可使用`"{self}"`，如：`onRepeatParams:["{self}", "param2"]`
* onRepeatScope（Object）：指定 `onRepeat` 函数的作用域（即函数内`"this"`的引用）。
* startAt（Object）：允许你定义补间动画属性的初始值。一般情况下，TweenMax 使用当前值作为初始值（无论何时启动补间动画），但 `startAt` 允许你覆盖这种行为。只需在动画开始前简单地传入对象（任何属性）即可。例如，当前 `mc.x` 当前值是 100，而你需要进行从 0 到 500 的补间动画，则 `TweenMax.to(mc, 2, {x:500, startAt:{x:0}});`

### 插件

以下插件均被 TweenMax 自动激活：

* css（Object）：能处理几乎所有 CSS 相关的值，如：backgroundColor、width、height、fontSize、color、top、left、marginTop 等。还能处理 transform，如 rotation、scaleX、scaleY、skewX、skewY、x（译者注：即 translateX）、y（译者注：即 translateY）、xPercent、yPercent、directionalRotation（译者注：顺/逆时针旋转）、autoAlpha（译者注：opacity 为 0 后自动 display: none）。它还能认清 transformOrigin 和 backgroundPosition（译者注：由于 CSS3 的 transform-origin 存在堆栈行为，即每次修改 transform-origin 都会基于上一个值进行叠加，次数多了就会容易出现错乱）。transform 兼容 IE7 及以上（尽管这些浏览器渲染 transform 时相当缓慢）。`TweenMax.to(myElement, 1, {width:"50%", height:"300px", backgroundColor:"#ff0000", delay:1});`
* roundProps（String）：在补间动画更新时，对于需要进行四舍五入的属性以逗号分隔，组成 `roundProps` 属性的值。例如，需要对 mc 目标对象进行 x、y 和 opacity 属性的补间动画，并且需要在渲染时对 x、y 的值进行四舍五入，则需要：`TweenMax.to(mc, 2, {x:300, y:200, opacity:0.5, roundProps:"x,y"});`
* bezier（Object）：允许目标对象沿贝塞尔曲线/路径进行运动。通过提供的一组值来绘制一条贝塞尔曲线，以确保其平滑。可以控制其“弯曲度”，可定义二次或三次贝塞尔曲线。该插件很灵活，它甚至可以按比例分配贝塞尔中的运动，从而避免大多数贝塞尔动画系统的常见问题，能在路径上的各个位置进行加速或减速运动。有关语法和示例代码的详细信息，请阅读 BezierPlugin 文档。

### 构造函数

TweenMax\( target: Object, duration: Number, vars: Object \);

### 属性

与 TweenLite 相同，缺省

### 方法

**getAllTweens\( includeTimelines: Boolean \): Array**

\[静态\] 返回一个包含所有补间动画的数组（可包含时间轴（timeline），根时间轴除外）。

**globalTimeScale\( value: Number \):**

\[静态\] 获取或设置全局 timeScale。它是一个乘数，可同等地影响**所有**动画。是一次性对所有动画进行整体加速/减速的好方法。

**isTweening\( target: Object \): Boolean**

\[静态\] 判断特定对象目前是否在进行补间动画。

**killAll\( complete: Boolean, tweens: Boolean, delayedCalls: Boolean, timelines: Boolean \):**

\[静态\] 取消掉所有补间动画/delayedCalls/callbacks/timelines，可选择将它们强制立刻完成。

**killChildTweensOf\( parent: Object, complete: Boolean \):**

\[静态\] 取消掉特定 DOM 元素下所有子元素的补间动画，可选择将它们强制立刻完成。

**pauseAll\( tweens: Boolean, delayedCalls: Boolean, timelines: Boolean \):**

\[静态\]\[弃用\] 暂停所有补间动画/delayedCalls/callbacks/timelines

**repeat\( value: Number \): \***

获取或设置补间动画首次完成后需要重复的次数。

**repeatDelay\( value: Number \): \***

获取或设置重复播放之间的间隔秒数（或帧，基于帧的补间动画）。

**resumeAll\( tweens: Boolean, delayedCalls: Boolean, timelines: Boolean \):**

\[静态\]\[弃用\] 恢复所有已暂停播放的补间动画的播放状态（方向不变，正向或反向），可选择快进至特定时间点。

**staggerFrom\( targets: Array, duration: Number, vars: Object, stagger: Number, onCompleteAll: Function, onCompleteAllParams: Array, onCompleteAllScope: \* \): Array**

\[静态\] 将目标对象数组进行初始值相同的补间动画（将当前值作为目标值），但将每个目标对象的启动时间按指定时间进行错开，从而以尽可能少的代码创建一个间隔均匀的序列。

**staggerFromTo\( targets:Array, duration:Number, fromVars:Object, toVars:Object, stagger:Number, onCompleteAll:Function, onCompleteAllParams:Array, onCompleteAllScope:\* \) : Array**

\[静态\] 对目标对象数组指定相同的初始值和目标值，但将每个目标对象的启动时间按指定时间进行错开，从而以尽可能少的代码创建一个间隔均匀的序列。

**staggerTo\( targets:Array, duration:Number, vars:Object, stagger:Number, onCompleteAll:Function, onCompleteAllParams:Array, onCompleteAllScope:\* \) : Array**

\[静态\] 将目标对象数组进行目标值相同的补间动画，但将每个目标对象的启动时间按指定时间进行错开，从而以尽可能少的代码创建一个间隔均匀的序列。

**updateTo\( vars:object, resetDuration:Boolean \) : \***

动态更新补间动画的值（译者注：目标值），以让动画看起来能够无缝地更改路线，即使补间动画正在渲染。

**yoyo\( value:Boolean \) : \***

获取或设置补间动画的 yoyo 状态。若为 true，则每次循环都交替运动方向播放。

