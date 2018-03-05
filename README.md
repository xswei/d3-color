# d3-color

浏览器可以解析很多种颜色, 但是不能帮你处理颜色. `d3-color` 模块提供各种空间的颜色表示, 并支持在各种颜色空间之间进行转换和操作. (例如使用 [d3-interpolate](https://github.com/d3/d3-interpolate) 进行颜色插值)

例如定义 “steelblue” 颜色:

```js
var c = d3.color("steelblue"); // {r: 70, g: 130, b: 180, opacity: 1}
```

将其转换到 HSL 空间:

```js
var c = d3.hsl("steelblue"); // {h: 207.27…, s: 0.44, l: 0.4902…, opacity: 1}
```

旋转 H 值, 调整饱和度, 将其转为 CSS 字符串：

```js
c.h += 90;
c.s += 0.2;
c + ""; // rgb(198, 45, 205)
```

调整透明度并转为 CSS 可用的字符串:

```js
c.opacity = 0.8;
c + ""; // rgba(198, 45, 205, 0.8)
```

除了 [RGB](#rgb) 和 [HSL](#hsl) 颜色空间之外, `d3-color` 还提供了另外两种专用于设计领域的颜色空间：

* Dave Green’s [Cubehelix](#cubehelix)
* [Lab (CIELAB)](#lab) 和 [HCL (CIELCH)](#hcl)

Cubehelix 颜色空间单调明亮, 而 Lab 和 HCL 颜色空间在视觉上更均衡.  HCL 是 Lab 的圆柱形式, 与 HSL 是 RGB 圆柱形式类似. 

## Installing

NPM 安装： `npm install d3-color`. 或者下载 [latest release](https://github.com/d3/d3-color/releases/latest). 也可以直接从 [d3js.org](https://d3js.org) 以 [standalone library](https://d3js.org/d3-color.v1.min.js) 或作为 [D3 4.0](https://github.com/d3/d3) 的一部分直接载入. 支持 AMD, CommonJS 以及最基本的标签引入形式, 如果使用标签引入会暴露全局 `d3` 变量:

```html
<script src="https://d3js.org/d3-color.v1.min.js"></script>
<script>

var steelblue = d3.rgb("steelblue");

</script>
```

[在浏览器中测试 d3-color](https://tonicdev.com/npm/d3-color)

## API Reference

<a name="color" href="#color">#</a> d3.<b>color</b>(<i>specifier</i>) [<>](https://github.com/d3/d3-color/blob/master/src/color.js "Source")

将 [CSS Color Module Level 3](http://www.w3.org/TR/css3-color/#colorunits) 指定的颜色字符串转换并返回 [RGB](#rgb) 或 [HSL](#hsl) 颜色表示. 如果指定的字符串不可用, 则返回null:

* `rgb(255, 255, 255)`
* `rgb(10%, 20%, 30%)`
* `rgba(255, 255, 255, 0.4)`
* `rgba(10%, 20%, 30%, 0.4)`
* `hsl(120, 50%, 20%)`
* `hsla(120, 50%, 20%, 0.4)`
* `#ffeeaa`
* `#fea`
* `steelblue`

由 CSS 支持的 [named colors](http://www.w3.org/TR/SVG/types.html#ColorKeywords).

注意：这个方法可以通过 `instanceof` 来判断一个对象是否为颜色实例. 各个颜色子类也是如此, 可以用来判断一个对象是否为指定的颜色空间实例. 

<a name="color_opacity" href="#color_opacity">#</a> *color*.<b>opacity</b>

设置 color 的透明度, 范围 [0,1].

<a name="color_rgb" href="#color_rgb">#</a> *color*.<b>rgb</b>() [<>](https://github.com/d3/d3-color/blob/master/src/color.js#L209 "Source")

返回颜色的 [RGB 表示](#rgb). 对于RGB颜色则返回自身.

<a name="color_brighter" href="#color_brighter">#</a> *color*.<b>brighter</b>([<i>k</i>]) [<>](https://github.com/d3/d3-color/blob/master/src/color.js#L221 "Source")

返回基于某个颜色的更亮的副本. 如果指定了 *k* 则表示调整的亮度系数. 如果没有指定 *k* 则默认为 1. 这个操作的实现依赖于具体的颜色空间. 

<a name="color_darker" href="#color_darker">#</a> *color*.<b>darker</b>([<i>k</i>]) [<>](https://github.com/d3/d3-color/blob/master/src/color.js#L225 "Source")

返回基于某个颜色的更暗的副本. 如果指定了 *k* 则表示调整的暗度系数. 如果没有指定 *k* 则默认为 1. 这个操作的实现依赖于具体的颜色空间. 

<a name="color_displayable" href="#color_displayable">#</a> *color*.<b>displayable</b>() [<>](https://github.com/d3/d3-color/blob/master/src/color.js#L169 "Source")

当且仅当在标准的硬件上支持显示该颜色时才会返回 `true`. 例如当使用 RGB 颜色空间时, 其中一个颜色通道值小于 0 或大于 255 或者透明度值不在 [0, 1] 范围内都会返回 `false`.

<a name="color_toString" href="#color_toString">#</a> *color*.<b>toString</b>() [<>](https://github.com/d3/d3-color/blob/master/src/color.js#L172 "Source")

根据 [CSS Object Model specification(对象模型规范)](https://drafts.csswg.org/cssom/#serialize-a-css-component-value) 返回颜色的字符串标识, 比如 `rgb(247, 234, 186)`. 如果颜色不可显示, 则会返回一个经过调整的可显示的值. 比如 RGB 颜色空间中某个通道值大于 255 时会自动将其调整为 255.

<a name="rgb" href="#rgb">#</a> d3.<b>rgb</b>(<i>r</i>, <i>g</i>, <i>b</i>[, <i>opacity</i>]) [<>](https://github.com/d3/d3-color/blob/master/src/color.js#L209 "Source")<br>
<a href="#rgb">#</a> d3.<b>rgb</b>(<i>specifier</i>)<br>
<a href="#rgb">#</a> d3.<b>rgb</b>(<i>color</i>)<br>

构建一个 [RGB](https://en.wikipedia.org/wiki/RGB_color_model) 颜色通道的颜色对象. 返回的颜色对象中包含 `r`, `g` 和 `b` 三个属性. 可以使用 [RGB color picker](http://bl.ocks.org/mbostock/78d64ca7ef013b4dcf8f) 来了解三个值之间的相互影响. 

如果指定了 *r*, *g* and *b*, 则表示返回颜色对象的三个通道值. 可选的 *opacity* 用来表示透明度. 如果指定的是一个 CSS Color Module Level 3 字符串,则会将其转换为 RGB 颜色空间. 参考[color](#color). 如果指定的是一个 [*color*](#color) 实例则使用 [*color*.rgb](#color_rgb) 将其转换为 RGB 颜色空间值. 与 [*color*.rgb](#color_rgb) 不同的是这个方法总是返回一个新的实例, 尽管传入的 *color* 已经是 RGB 空间值. 

<a name="hsl" href="#hsl">#</a> d3.<b>hsl</b>(<i>h</i>, <i>s</i>, <i>l</i>[, <i>opacity</i>]) [<>](https://github.com/d3/d3-color/blob/master/src/color.js#L281 "Source")<br>
<a href="#hsl">#</a> d3.<b>hsl</b>(<i>specifier</i>)<br>
<a href="#hsl">#</a> d3.<b>hsl</b>(<i>color</i>)<br>

构建一个 [HSL](https://en.wikipedia.org/wiki/HSL_and_HSV) 颜色通道的颜色对象. 返回的颜色对象中包含 `h`, `s` 和 `l` 三个属性. 可以使用 [HSL color picker](http://bl.ocks.org/mbostock/debaad4fcce9bcee14cf)来了解三个值之间的相互影响. 

如果指定了 *h*, *s* and *l*, 则表示返回颜色对象的通道值. 可选的 *opacity* 用来表示透明度. 如果指定的是一个 CSS Color Module Level 3 字符串,则会将其转换为 HSL 颜色空间. 参考 [color](#color). 如果指定的是一个 [*color*](#color) 实例则使用 [*color*.rgb](#color_rgb) 然后再将其转换为 HSL 颜色空间值. (如果传入的颜色已经是 HSL 颜色空间则会跳过将其转为 RGB 的步骤). 

<a name="lab" href="#lab">#</a> d3.<b>lab</b>(<i>l</i>, <i>a</i>, <i>b</i>[, <i>opacity</i>]) [<>](https://github.com/d3/d3-color/blob/master/src/lab.js#L30 "Source")<br>
<a href="#lab">#</a> d3.<b>lab</b>(<i>specifier</i>)<br>
<a href="#lab">#</a> d3.<b>lab</b>(<i>color</i>)<br>

构建一个 [Lab](https://en.wikipedia.org/wiki/Lab_color_space#CIELAB) 颜色通道的颜色对象. 返回的颜色对象中包含 `l`, `a` 和 `b` 三个属性. 可以使用 [Lab color picker](http://bl.ocks.org/mbostock/9f37cc207c0cb166921b) 来了解三个值之间的相互影响. 

如果指定了 *l*, *a* and *b*, 则表示返回颜色对象的通道值. 可选的 *opacity* 用来表示透明度. 如果指定的是一个 CSS Color Module Level 3 字符串,则会将其转换为Lab颜色空间. 参考 [color](#color). 如果指定的是一个 [*color*](#color) 实例则使用 [*color*.rgb](#color_rgb) 然后再将其转换为 Lab 颜色空间值. (如果传入的颜色已经是 Lab 颜色空间则跳过将其先转为 RGB 的步骤, 如果传入的颜色为 HCL 颜色空间值则直接将其转为 Lab 颜色空间值)

<a name="hcl" href="#hcl">#</a> d3.<b>hcl</b>(<i>h</i>, <i>c</i>, <i>l</i>[, <i>opacity</i>]) [<>](https://github.com/d3/d3-color/blob/master/src/lab.js#L87 "Source")<br>
<a href="#hcl">#</a> d3.<b>hcl</b>(<i>specifier</i>)<br>
<a href="#hcl">#</a> d3.<b>hcl</b>(<i>color</i>)<br>

构建一个 [HCL](https://en.wikipedia.org/wiki/HCL_color_space) 颜色通道的颜色对象. 返回的颜色对象中包含 `h`, `c` 和 `l` 三个属性. 可以使用 [HCL color picker](http://bl.ocks.org/mbostock/3e115519a1b495e0bd95) 来了解三个值之间的相互影响. 

如果指定了 *h*, *c* and *l*, 则表示返回颜色对象的通道值. 可选的 *opacity* 用来表示透明度. 如果指定的是一个 CSS Color Module Level 3 字符串,则会将其转换为HCL颜色空间. 参考 [color](#color). 如果指定的是一个 [*color*](#color) 实例则使用 [*color*.rgb](#color_rgb) 然后再将其转换为 HCL 颜色空间值. (如果传入的颜色已经是 HCL 颜色空间则跳过将其先转为 RGB 的步骤, 如果传入的颜色为 Lab 颜色空间值则直接将其转为 HCL 颜色空间值)

<a name="cubehelix" href="#cubehelix">#</a> d3.<b>cubehelix</b>(<i>h</i>, <i>s</i>, <i>l</i>[, <i>opacity</i>]) [<>](https://github.com/d3/d3-color/blob/master/src/cubehelix.js#L32 "Source")<br>
<a href="#cubehelix">#</a> d3.<b>cubehelix</b>(<i>specifier</i>)<br>
<a href="#cubehelix">#</a> d3.<b>cubehelix</b>(<i>color</i>)<br>

构建一个 [Cubehelix](https://www.mrao.cam.ac.uk/~dag/CUBEHELIX/) 颜色通道的颜色对象. 返回的颜色对象中包含`h`, `s` 和 `l`三个属性. 可以使用[Cubehelix color picker](http://bl.ocks.org/mbostock/ba8d75e45794c27168b5)来了解三个值之间的相互影响. 

如果指定了 *h*, *s* and *l*, 则表示返回颜色对象的通道值. 可选的 *opacity* 用来表示透明度. 如果指定的是一个 CSS Color Module Level 3 字符串,则会将其转换为 Cubehelix 颜色空间. 参考 [color](#color). 如果指定的是一个 [*color*](#color) 实例则使用 [*color*.rgb](#color_rgb) 然后再将其转换为 Cubehelix 颜色空间值. (如果传入的颜色已经是 Cubehelix 颜色空间则会跳过将其先转为 RGB 的步骤). 
