## TweenMax

TweenMax 对 TweenLite 进行了扩展，添加了很多有用（但非必要）的特性，如 `repeat()`、`repeatDelay()`、`yoyo()` 等。它同时默认引入了很多额外的插件，以保证自身的全面性。当然，这些插件也适用于 TweenLite。只不过 TweenMax 帮你省了加载通用插件这一步骤，这些插件有：CSSPlugin、RoundPropsPlugin、BezierPlugin、AttrPlugin、DirectionalRotationPlugin、EasePack、TimelineLite 和 TimelineMax。因为 TweenMax 是 TweenLite 的扩展，所以能在其基础上做更多事情。两者的语法一致，可在项目中混合使用。但若需要考虑文件大小，则最好坚持使用 TweenLite，除非是 TweenMax 的专属特性。

> 译者注：对于与 TweenLite 重复的文档，在 TweenMax 中就尽量省去。避免冗余，保证文档精简。

### 用法

补间动画最常用的方式是 TweenMax.to\(\)，它允许你定义目标值：

```js
var photo = document.getElementById("photo");
TweenMax.to(photo, 2, {width:"200px", height:"150px"});
```



### 特殊属性、回调函数与缓动



