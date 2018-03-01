## TimelineLite

TimelineLite 是一个强大的队列工具，它充当了补间动画实例和其它时间轴实例的**容器**。将它们作为一个整体后，让整体控制和时间管理变得简单和精确。如果没有 TimelineLite，构建复杂的序列将会变得非常麻烦，因为你需要为每个补间动画使用 delay 属性，这使得将来的改动变得更加乏味。下面是一个没有使用 TimelineLite 的序列案例（繁琐的方法）：

```js
TweenLite.to(element, 1, {left:100});
TweenLite.to(element, 1, {top:50, delay:1});
TweenLite.to(element, 1, {opacity:0, delay:2});
```

上面代码将元素的 CSS 属性 "left" 过渡到 100，然后将 "top" 过渡到 50，最后将 "opacity" 过渡到 0（注意：除了第一个补间动画，其余均需要设置 `delay` 属性）。但想象以下情景：如果将第一个补间动画的持续时间增加到 1.5，那么就需要更改其后续所有的延迟时间。更不用说：对整个队列进行 pause\(\) 或 restart\(\) 或 reverse\(\) 或快进至特定时间点的操作。这无疑变得十分繁琐。TimelineLite 却让这一切变得非常简单：

```js
var tl = new TimelineLite();
tl.add( TweenLite.to(element, 1, {left:100}) );
tl.add( TweenLite.to(element, 1, {top:50}) );
tl.add( TweenLite.to(element, 1, {opacity:0}) );

//then later, control the whole thing...
tl.pause();
tl.resume();
tl.seek(1.5);
tl.reverse();
...
```

或者使用快捷方法`to()`和链式调用，使其更加精简：

```js
var tl = new TimelineLite();
tl.to(element, 1, {left:100}).to(element, 1, {top:50}).to(element, 1, {opacity:0});
```

现在你就可以随意地调整任意一个补间动画，而无须担心延时时间的涓滴效应。增加第一个补间动画的过渡时间后，其余将自动调整。

下面是 TimelineLite 的其他一些好处和特点：

* 只要你想，东西（译者注：如补间动画实例、时间轴实例等）可以在时间轴上重叠放置，并拥有这些补间动画和时间轴的所有控制权。其他大多数动画工具只能做一个接着一个的基本队列操作，而不能发生重叠。若想追加一个补间动画来移动一个对象，并且你希望它在该补间结束前0.5秒开始淡出？使用 TimelineLite 就很容易实现这个情景。
* 随时添加标签、play\(\)、stop\(\)、seek\(\)、restart\(\)、reverse\(\) 等。
* 可在时间轴上多层嵌套时间轴。这意味着你可以对代码进行模块化，并使其更高效。想象一下，使用常见的 animateIn\(\) 和 animateOut\(\) 方法构建你的应用，并且它们返回的是一个补间动画或时间轴实例，所以你可以将事情串连起来：`myTimeline.append( myObject.animateIn() ).append( myObject.animateOut(), "+=4").append( myObject2.animateIn(), "-=0.5")...`
* 通过 `timeScale()` 方法对整个时间轴进行加速/减速。甚至可以通过补间动画实现逐渐加速/减速，使动画更加流畅。
* 使用 `progress()` 方法获取或设置时间的进度。如快进至中点：`myTimeline.progress(0.5);`
* 对 `time` 或 `progress` 进行补间动画，以实现时间轴的快进/后退。甚至可以将两者其中一个属性绑定到滑块（slider）上，让用户能通过拖拽实现快进/后退时间轴。
* 可向构造函数的 `vars` 参数对象添加 `onComplete、onStart、onUpdate、onReverseComplete` 回调函数，如 `var tl = new TimelineLite({onComplete:myFunction});`
* 可通过 `kill(null, target)` 取消掉时间轴内特定对象的补间动画、可通过 `getTweensOf()` 获取对象的补间动画、可通过 `getChildren()` 获取时间轴内所有补间动画/时间轴。
* 向 `vars` 参数设置 `useFrames: true`，即可将原本基于秒的刻度改为帧。请注意，时间轴的时刻模式也决定了其子元素的时刻模式。
* 可随时通过 `TimelineLite.exportRoot()` 将根（主）时间轴的所有补间动画/时间轴导出到该 TimelineLite 实例中，以便可以对他们所有进行 `pause()`、`timeScale()` 等操作。但不影响后续创建的补间动画/时间轴。设想一个游戏，其所有动画均由 GSAP 驱动，现在需要在弹出窗口时进行暂停或减速操作。使用该方法是不是使得这一操作变得很简单？
* 若需要更多诸如 `repeat、repeatDelay、yoyo、currentLabel()、getLabelAfter()、getLabelBefore()、addCallback()、removeCallback()、getActive()` 等特性，请使用 TimelineLite 的扩展版——TweenMax。

### 特殊属性、缓动和回调函数

可以在构建函数的 `vars` 参数中定义下面的特殊属性（语法案例：`new TimelineLite({onComplete:myFunction, delay:2});`）

* delay（Number）：动画开始前需要延迟的秒数（或帧数，基于帧的补间动画）。

* 为 `true`  时，动画会在创建后立即停止。

* onComplete（Function）：动画完成时的回调函数。

* onCompleteScope（Object）：指定 onComplete 函数的作用域（即函数内 `"this"` 的引用）。

* useFrames（Boolean）：若 useFrames 为 true，则补间动画的时刻（译者注：即刻度）是基于帧，而不是秒。因为它被添加到基于帧的根时间轴。这使其 duration 和 delay 均基于帧。补间动画实例的时刻模式总是由其父时间轴决定。

* tweens（Array）：向 tweens 属性传递 TweenLite/TweenMax/TimelineLite/TimelineMax 实例，可实现向时间轴一次性插入多个补间动画的特性。若结合 align 和 stagger 属性可以最少代码实现复杂的队列。其实这些值只是传递给 add\(\) 方法。

* align（String）：仅适用于通过 tweens 属性一次性插入多个补间动画时使用。默认值是 "normal"，可选值：

  * "sequence"：将补间动画一个接一个排放

  * "start"：所有补间动画的启动时间相同（忽略子元素的延迟时间）

  * "normal"：所有补间动画的启动时间相同（受子元素的延迟时间影响）

  align 属性并不会强制所有子元素（补间动画/时间轴）去维持其相对位置。例如：在使用 "sequence" 的情况下，修改其中一个补间动画的持续时间，这并不会强制修改其后续所有时间轴的位置。align 属性仅对初始化时通过 vars 对象的 tweens 属性添加的补间动画有效。

* stagger（Number）：仅适用于通过 tweens 属性一次性插入多个补间动画时使用。根据设置的秒数（或帧，基于帧的动画）错开所有补间动画。如：stagger 为 0.5、"align" 属性为 "start"，那么第二个补间动画将比第一个迟 0.5 秒启动，而第三个补间动画则再迟 0.5 后启动，如此类推。若 align 属性为 "sequence"，那么这 0.5 秒将添加到每个补间动画之间。默认值为 0。

* onStart（Function）：动画启动时的回调函数（即其 time 从 0 变到其他值，可触发多次，即补间动画多次重新启动）。

* onStartScope（Object）：指定 `onStart` 函数的作用域（即函数内 `"this"` 的引用）。

* onReverseComplete（Function）：动画反向运动到初始开始位时所触发的回调函数。例如，当 `reverse()` 被调用时，补间动画将反向运动到初始位置，当其 `time` 为 0 时，`onReverseComplete` 函数会被触发。即使将补间动画实例放置在 TimelineLite 或 TimelineMax 实例中，若 Timeline 实例反向运动且到达初始位置时（译者注：补间动画实例的初始位置）也会触发该函数（译者注：补间动画实例的 onReverseComplete 函数）。

* onReverseCompleteScope（Object）：指定 onReverseComplete 函数的作用域（即函数内 `"this"` 的引用）。

* onUpdate（Function）：动画每次更新都会触发的回调函数（即活跃状态下动画的每一帧）

* onUpdateScope（Object）：指定 `onUpdate` 函数的作用域（即函数内 `"this"` 的引用）

* autoRemoveChildren（Boolean）：若 autoRemoveChildren 为 true，那么子元素（tweens/timelines）一旦完成后就会被移除。但这一般并需要，因为这阻止了后续回看的操作（如 reverse\(\) 或通过 progress\(\) 回看等）。该操作能提升速度和改善内存管理。

* smoothChildTiming（Boolean）：控制子元素（tweens/timelines）在更改属性时，是否需要自动重新定位（改变它们的 startTime），以维持播放的流畅性（译者注：即不会突变以赶上父时间轴的播放进度，而是通过更改它们的 startTime，以使启动时间与父时间轴的当前进度一致，案例：[https://codepen.io/JChehe/pen/mXvMwN](https://codepen.io/JChehe/pen/mXvMwN)）。例如：有一个将元素的 left 属性从 0 过渡到 100 的补间动画，其父时间轴的当前进度在该补间动画的 75% 进度处，此刻补间动画调用 reverse\(\) 方法。若 smoothChildTiming 是 false（默认值，根时间轴除外），那么补间动画将原地反转，但其 startTime 保持不变。因此，父时间轴的当前进度将处在补间动画的 25% 处，而不是 75%。记住：时间轴的进度和播放方向是不受其子元素（tweens/timinlines）改变的影响。元素的 left 属性将从 75 跳到 25，而补间动画在时间轴上的位置保持不变。若 smoothChildTiming 为 true，那么补间动画的 startTime 将进行调整，由于在 reverse\(\) 之前被调用，则时间轴的当前进度将与补间动画的当前进度相同。从而动画呈现出来是丝滑的（译者注：无跳动）。元素的 left 属性依然是 75，并随着进度移动。补间动画的 reverse\(\) 操作让 left 属性过渡到 0，而不是 100（译者注：两者的案例比较 [https://codepen.io/JChehe/pen/OQdEXp](https://codepen.io/JChehe/pen/OQdEXp)）。最终，这是优先考虑子元素（补间动画/时间轴）播放连续性还是位置一致性的决定。smoothChildTiming 为 true 时，子元素（tweens/timelines）以下属性的更改会导致 startTime 的更改：reversed、timeScale、progress、totalProgress、time、totalTime、delay、pause、resume、duration 和 totalDuration。

* onCompleteParams（Array）：传入 `onComplete` 函数的参数数组。例如：`TweenLite.to(element, 1, { left: "100px", onComplete: myFunction, onCompleteParams: [element, "param2"] });`若需要在参数列表中引用补间动画实例自身，可使用`"{self}"`，如：`onCompleteParams:["{self}", "param2"]`

* onStartParams（Array）：传入 `onStart` 函数的参数数组。例如 `TweenLite.to(element, 1, {left:"100px", delay:1, onStart:myFunction, onStartParams:[mc, "param2"]});` 若需要在参数列表中引用补间动画实例自身，可使用`"{self}"`，如：`onStartParams:["{self}", "param2"]`。

* onUpdateParams（Array）：传入 `onUpdate` 函数的参数数组。例如 `TweenLite.to(element, 1, {left:"100px", onUpdate:myFunction, onUpdateParams:[mc, "param2"]});` 若需要在参数列表中引用补间动画实例自身，可使用`"{self}"`，如：`onUpdateParams:["{self}", "param2"]`。

* onReverseCompleteParams（Array）：传入 `onReverseComplete` 函数的参数数组。例如 `TweenLite.to(element, 1, {left:"100px", onReverseComplete:myFunction, onReverseCompleteParams:[mc, "param2"]});`若需要在参数列表中引用补间动画实例自身，可使用`"{self}"`，如：`onReverseCompleteParams:["{self}", "param2"]`。

* callbackScope（Object）：为所有回调函数设置作用域（onStart、onUpdate、onComplete 等）。作用域即回调函数内的 "this" 引用。指定特定回调函数作用域的老旧属性（onStartScope、onUpdateScope、onCompleteScope、onReverseComplete 等）已被弃用，但仍然可用。

> 示例代码

```js
//create the timeline with an onComplete callback that calls myFunction() when the timeline completes
var tl = new TimelineLite({onComplete:myFunction});
//add a tween
tl.add( TweenLite.to(element, 1, {left:200, top:100}) );

//add another tween at the end of the timeline (makes sequencing easy)
tl.add( TweenLite.to(element, 0.5, {opacity:0}) );

//append a tween using the convenience method (shorter syntax) and offset it by 0.5 seconds
tl.to(element, 1, {rotation:30}, "+=0.5");

//reverse anytime
tl.reverse();
//Add a "spin" label 3-seconds into the timeline
tl.addLabel("spin", 3);
//insert a rotation tween at the "spin" label (you could also define the insertion point as the time instead of a label)
tl.add( TweenLite.to(element, 2, {rotation:"+=360"}), "spin");

//go to the "spin" label and play the timeline from there
tl.play("spin");
//nest another TimelineLite inside your timeline...
var nested = new TimelineLite();
nested.to(element, 1, {left:400}));
tl.append(nested);
```

### timeline 是如何运作的，其内部机制是？

每个动画（补间动画和时间轴）均放置在其父时间轴上（除了 2 个根时间轴——分别是普通补间动画（译者注：时刻为“秒”）和 "useFrames"）。从某种意义上说，它们（译者注：动画与时间轴）均有其各自的当前进度（即各自 "time" 的值，或没有 repeats 和 repeatDelays 时的 "totalTime"），但它们通常并不是相互独立的，因为它们均放置在一个时间轴上。当该父时间轴播放时，会同时更新其子元素。

当时间轴渲染某一时刻时，它会遍历其子元素，并告诉它说：“你应该渲染某时刻”。若子元素是时间轴，并带有子元素，那么就对该子元素执行同样的操作，如此循环。

唯一例外是补间动画/时间轴（译者注：子元素）是暂停的，其内部进度条就好像被“锁住”一样。所以在这种情况下，子元素的进度就不再和其父元素同步了。当取消子元素暂停状态时（`resume()`），其父元素就会将它“捡”起来，子元素内部的进度就会与父元素当前的进度同步起来。

所以基本上，当 `smoothChildTiming` 为 `true` 时，引擎会随时重新排列子元素，以保证进度是线性变化的，从而让动画看起来是丝滑的，即便是 `reverse()` 或 `timeScale`。若你更想严格控制时间轴上的队列，那就设置 `smoothChildTiming: false`。

> 译者注：通过案例来解释 smoothChildTiming 属性会更直观：[https://codepen.io/JChehe/pen/mXvMwN](https://codepen.io/JChehe/pen/mXvMwN)

再举一个案例：

### 构造函数

TimelineLite\( vars: Object \);

### 属性

autoRemoveChildren（Boolean）：true 代表子元素（补间动画/时间轴）完成自身动画后会自动被删除。

data（\*）：可存储任意数据的地方（若 vars.data 不为空，则在初始化时填入）。

smoothChildTiming（Boolean）：

timeline（SimpleTimeline）\[只读\]：父时间轴。

vars（Object）：构建函数的参数 vars 存放着配置变量，如 onComplete、onUpdate 等。

### 方法

**add\( value: \*, position: \*, align: String, stagger: Number \): \***

\[override\] 向时间轴添加补间动画、时间轴、回调函数或标记（均可为数组）。

**addLabel\( label: String, position: \* \)：\***

向时间轴添加标记，标记重要的位置/时间。

**addPause\( position: \*, callback: Function, params: Array, scope: \* \): \***

向特定时间点或标记处插入一个暂停时间轴播放的回调函数。

**call\( callback: Function, params: Array, scope: \*, position: \* \): \***

向时间轴末端添加一个回调函数（或通过指定 "position" 参数添加到别处）。其实这与 add\(TweenLite.delayedCall\(...\)\) 完全相同，只不过是更少代码的实现方式。

**clear\( labels: Boolean \): \***

清空时间轴内所有补间动画、时间轴和回调函数（可选标记）。

**delay\( value: Number \): \***

获取或设置动画的初始延时时间，单位为秒（或帧，基于帧的动画）。

**duration\( value: Number \): \***

\[override\] 获取时间轴的持续时间。当将其作为 setter 时，则通过调整时间轴的 timeScale 属性，以满足指定的持续时间。

**endTime\( includeRepeats: Boolean \): Number**

根据父时间轴的本地时间返回动画完成的时间。

**eventCallback\( type: String, callback: Function, params: Array, scope: \* \): \***

获取或设置一个回调函数，如 "onComplete"、"onUpdate"、"onStart"、"onReverseComplete" 和 "onRepeat"（onRepeat 仅在 TweenMax 和 TimelineMax 实例有效），可向回调函数传入任意参数。

**exportRoot\( vars: Object, omitDelayedCalls: Boolean \): TimelineLite**

\[静态\] 无缝地将根时间轴上的补间动画、时间轴和 delayedCall（可选）转移到一个新 TimelineLite 实例上。这样就可以将该 TimelineLite 实例看作是全局的，以便执行一些整体操作。当然，这并不影响后续创建的补间动画/时间轴。

[**from**](https://greensock.com/docs/TimelineLite/from%28%29)**\( target:Object, duration:Number, vars:Object, position:\* \) :\***

向时间轴末端添加一个 TweenLite.from\(\) 补间动画（或通过 "position" 参数添加到别处）。其实这与 add\( TweenLite.from\(...\) \) 完全相同，只不过是更少代码的实现方式。

[**fromTo**](https://greensock.com/docs/TimelineLite/fromTo%28%29)**\( target:Object, duration:Number, fromVars:Object, toVars:Object, position:\* \) :\***

向时间轴末端添加一个 TweenLite.fromTo\(\) 补间动画（或通过 "position" 参数添加到别处）。其实这与 add\( TweenLite.fromTo\(...\) \) 完全相同，只不过是更少代码的实现方式。

[**getChildren**](https://greensock.com/docs/TimelineLite/getChildren%28%29)**\( nested:Boolean, tweens:Boolean, timelines:Boolean, ignoreBeforeTime:Number \) :Array**

返回由嵌套在该时间轴的所有补间动画/时间轴组成的数组。

[**getLabelTime**](https://greensock.com/docs/TimelineLite/getLabelTime%28%29)**\( label:String \) :Number**

返回特定标记所在的时间点。

[**getTweensOf**](https://greensock.com/docs/TimelineLite/getTweensOf%28%29)**\( target:Object, nested:Boolean \) :Array**

返回时间轴内特定对象的补间动画。

[**invalidate**](https://greensock.com/docs/TimelineLite/invalidate%28%29)**\( \) :\***

\[override\] 清除所有初始化数据（如补间动画的初始值/结束值）。若想重新启动补间动画而不还原任何先前记录的初始值，那么这将非常有用。

[**isActive**](https://greensock.com/docs/TimelineLite/isActive%28%29)**\( \) :Boolean**

判断动画目前是否处于活跃状态（即进度条在此实例的时间范围内移动，并且没有暂停，其祖先时间轴同理）。

[**kill**](https://greensock.com/docs/TimelineLite/kill%28%29)**\( vars:Object, target:Object \) :\***

根据参数完全取消整个或部分动画。

[**pause**](https://greensock.com/docs/TimelineLite/pause%28%29)**\( atTime:\*, suppressEvents:Boolean \) :\***

暂停实例，可快进至特定时间。

[**paused**](https://greensock.com/docs/TimelineLite/paused%28%29)**\( value:Boolean \) :\***

获取和设置动画的暂停状态，判断动画当前是否存于暂停状态。

[**play**](https://greensock.com/docs/TimelineLite/play%28%29)**\( from:\*, suppressEvents:Boolean \) :\***

开始正向播放，可快进至特定时间（默认情况下，将从当前进度开始播放）。

[**progress**](https://greensock.com/docs/TimelineLite/progress%28%29)**\( value:Number, suppressEvents:Boolean \) :\***

获取或设置动画的进度，取值范围为 0~1（不包含 repeats）。0 代表初始位置，0.5 代表中间位置，1 代表结束位置（完成）。

[**recent**](https://greensock.com/docs/TimelineLite/recent%28%29)**\( \) :Animation**

返回更近添加的补间动画/时间轴/回调函数，与其在时间轴上的位置无关。

[**remove**](https://greensock.com/docs/TimelineLite/remove%28%29)**\( value:\* \) :\***

从时间轴上移除一个补间动画、时间轴、回调函数或标记（均可为数组）。

[**removeLabel**](https://greensock.com/docs/TimelineLite/removeLabel%28%29)**\( label:String \) :\***

从时间轴上移除一个标记，并返回该标记所在的时间点。

[**render**](https://greensock.com/docs/TimelineLite/render%28%29)**\( time:Number, suppressEvents:Boolean, force:Boolean \) :**

渲染。

[**restart**](https://greensock.com/docs/TimelineLite/restart%28%29)**\( includeDelay:Boolean, suppressEvents:Boolean \) :\***

重新正向播放。

[**resume**](https://greensock.com/docs/TimelineLite/resume%28%29)**\( from:\*, suppressEvents:Boolean \) :\***

在不改变方向（正向或反向）的情况下恢复播放，可首先快进至特定时间。+

[**reverse**](https://greensock.com/docs/TimelineLite/reverse%28%29)**\( from:\*, suppressEvents:Boolean \) :\***

反向播放，动画的所有方面都反向，包括缓动函数。

[**reversed**](https://greensock.com/docs/TimelineLite/reversed%28%29)**\( value:Boolean \) :\***

获取或设置动画的反向状态，判断动画是否反向播放。

[**seek**](https://greensock.com/docs/TimelineLite/seek%28%29)**\( position:\*, suppressEvents:Boolean \) :\***

\[override\] 直接快进至特定时间（或标记），无论实例是否是暂停或反向。

[**set**](https://greensock.com/docs/TimelineLite/set%28%29)**\( target:Object, vars:Object, position:\* \) :\***

向时间轴末端添加一个过渡时间为 0 的补间动画（或通过 "position" 参数指定其它位置） ，即当进度达到特定位置时立刻设置值。其实这与 add\( TweenLite.to\(target, 0, {...}\) \) 完全相同，只不过是更少代码的实现方式。

[**shiftChildren**](https://greensock.com/docs/TimelineLite/shiftChildren%28%29)**\( amount:Number, adjustLabels:Boolean, ignoreBeforeTime:Number \) :\***

根据时间（秒/帧）和可选的 adjustLabel，调整时间轴上所有子元素的 startTime。

[**staggerFrom**](https://greensock.com/docs/TimelineLite/staggerFrom%28%29)**\( targets:Array, duration:Number, vars:Object, stagger:Number, position:\*, onCompleteAll:Function, onCompleteAllParams:Array, onCompleteScope:\* \) :\***

将目标对象数组进行初始值相同的补间动画（将当前值作为目标值），但将每个目标对象的启动时间按指定时间进行错开，从而以尽可能少的代码创建一个间隔均匀的序列。

[**staggerFromTo**](https://greensock.com/docs/TimelineLite/staggerFromTo%28%29)**\( targets:Array, duration:Number, fromVars:Object, toVars:Object, stagger:Number, position:\*, onCompleteAll:Function, onCompleteAllParams:Array, onCompleteScope:\*\) :\***

对目标对象数组指定相同的初始值和目标值，但将每个目标对象的启动时间按指定时间进行错开，从而以尽可能少的代码创建一个间隔均匀的序列。

[**staggerTo**](https://greensock.com/docs/TimelineLite/staggerTo%28%29)**\( targets:Array, duration:Number, vars:Object, stagger:Number, position:\*, onCompleteAll:Function, onCompleteAllParams:Array, onCompleteScope:\* \) :\***

将目标对象数组进行目标值相同的补间动画，但将每个目标对象的启动时间按指定时间进行错开，从而以尽可能少的代码创建一个间隔均匀的序列。

[**startTime**](https://greensock.com/docs/TimelineLite/startTime%28%29)**\( value:Number \) :\***

获取或设置动画在其父时间轴上的开始时间（在延迟时间之后）。

[**time**](https://greensock.com/docs/TimelineLite/time%28%29)**\( value:Number, suppressEvents:Boolean \) :\***

获取或设置进度条的本地位置（本质是当前时间），单位为秒（或帧，基于帧的动画），不小于 0 且不大于动画的持续时间。

[**timeScale**](https://greensock.com/docs/TimelineLite/timeScale%28%29)**\( value:Number \) :\***

用于缩放动画时间的因子，其中 1 为正常速度（默认），0.5 = 半速，2 = 两倍速度，以此类推。

[**to**](https://greensock.com/docs/TimelineLite/to%28%29)**\( target:Object, duration:Number, vars:Object, position:\* \) :\***

向时间轴末端添加一个 TweenLite.to\(\) 补间动画（或通过 "position" 参数添加到别处）。其实这与 add\( TweenLite.to\(...\) \) 完全相同，只不过是更少代码的实现方式。

[**totalDuration**](https://greensock.com/docs/TimelineLite/totalDuration%28%29)**\( value:Number \) :\***

\[override\] 获取或设置动画的总持续时间。当将其作为 setter 时，则通过调整时间轴的 timeScale 属性，以满足指定的持续时间。

[**totalProgress**](https://greensock.com/docs/TimelineLite/totalProgress%28%29)**\( value:Number, suppressEvents:Boolean \) :\***

获取或设置动画总进度（包含重复次数（repeats）），取值区间为 0~1，0 代表初始位置，0.5 代表中点位置、1 代表结束位置（完成）。

[**totalTime**](https://greensock.com/docs/TimelineLite/totalTime%28%29)**\( time:Number, suppressEvents:Boolean \) :\***

根据总持续时间（totalDuration）获取或设置进度条的位置，包含所有重复次数（repeats）和重复延迟时间（repeatDelays）（这两者仅在 TweenMax 和 TimelineMax 中可用）。

[**useFrames**](https://greensock.com/docs/TimelineLite/useFrames%28%29)**\( \) :Boolean**

\[只读\] true 代表时间轴的时刻是帧而不是秒。

