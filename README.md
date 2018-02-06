## 开始使用文档

Animating with code may seem intimidating at first, but don’t worry, our platform was engineered to make it simple and intuitive.

“通过代码生成动画”的这种方式给人的第一感觉是：“不是吧？！”。但不用担心，本平台就是让这种方式变得简单、直观和可行。

#### 查看文档主菜单

左侧菜单涵盖了 GreenSock 所有类的 API（HTML5 版本）。任意一个类都提供了概述和它所拥有的方法和属性，而每个方法和属性均拥有各自的详情页。另外，一定要看看 Plugins 和 Utilities 里的工具，你会被 GSAP 核心动画能力以外的东西所惊讶到。

#### 我们最流行的工具

了解 GreenSock 中最流行的工具的技术细节

* TweenLite

  TweenLite 是一款超级快速、轻量且灵活的工具。它是 GreenSock 动画平台（GSAP）的基础。一个 TweenLite 实例能一次性处理任意对象（或对象数组）的一个或多个属性。

* TweenMax

  TweenMax 对 TweenLite 进行了扩展，添加了很多有用（但非必要）的特性，如 repeate\(\)、repeatDelay\(\)、yoyo\(\) 和 updateTo\(\) 等。它同时默认引入了很多额外的插件，以保证自身的全面性。当然，这些插件也适用于 TweenLite。只不过 TweenMax 帮你省了加载通用插件这一步，这些插件有：CSSPlugin、RoundPropsPlugin、BezierPlugin、AttrPlugin、DirectionalRotationPlugin、EasePack、TimelineLite 和 TimelineMax。

* TimelineLite

  TimelineLite 是一款轻量、直观的时间轴类，用于创建和管理由 TweenLite、TweenMax、TimelineLite 和 TimelineMax 实例组成的队列。Timeline 实例好比一个容器，你根据时间放置 tween（或其他 timeline）。

* TimelineMax

  TimelineMax 对 TimelineLite 进行了扩展，在 TimelineLite 的基础上增加了很多有用（但非必要）的特性，如 repeat、repeateDelay、yoyo、currentLabel\(\)、tweenTo\(\)、tweenFromTo\(\)、getLabelAfter\(\)、getLabelBefore\(\) 和 getActive\(\) 等。

* Draggable

  Draggable 通过使用鼠标/触摸事件，让几乎所有 DOM 元素变得可拖拽、可旋转、可投掷，甚至弹性滚动。另外，它还能与 ThrowPropsPlugin（可选）完美集成，使用户能进行回弹和基于动量的减速过渡。

#### GreenSock 官方练习（视频和电子书）

如果你想知道 GSAP 能做什么，可以看看官方视频教程[《Learn HTML5 Animation with GreenSock》](https://www.nobledesktop.com/html5-greensock-video-class-gsap?a=ox7)。它超过 5 小时，由我们的极客大使 Carl Schooff 录制。该教程基于真实项目，详细说明了实现过程中每一步。

