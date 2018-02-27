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

* tweens（Array）：

* align（String）：

* stagger（Number）：

* onStart（Function）：动画启动时的回调函数（即其 time 从 0 变到其他值，可触发多次，即补间动画多次重新启动）。

* onStartScope（Object）：指定 `onStart` 函数的作用域（即函数内 `"this"` 的引用）。

* onReverseComplete（Function）：动画反向运动到初始开始位时所触发的回调函数。例如，当 `reverse()` 被调用时，补间动画将反向运动到初始位置，当其 `time` 为 0 时，`onReverseComplete` 函数会被触发。即使将补间动画实例放置在 TimelineLite 或 TimelineMax 实例中，若 Timeline 实例反向运动且到达初始位置时（译者注：补间动画实例的初始位置）也会触发该函数（译者注：补间动画实例的 onReverseComplete 函数）。

* onReverseCompleteScope（Object）：指定 onReverseComplete 函数的作用域（即函数内 `"this"` 的引用）。

* onUpdate（Function）：动画每次更新都会触发的回调函数（即活跃状态下动画的每一帧）

* onUpdateScope（Object）：指定 `onUpdate` 函数的作用域（即函数内 `"this"` 的引用）

* autoRemoveChildren（Boolean）：

* smoothChildTiming（Boolean）：

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

### 构造函数

### 属性

### 方法



