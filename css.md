### CSS
#### Semicolons(分号)
> While the semicolon is technically a separator in CSS, always treat it as a terminator.

虽然在CSS中分号被视作一个分隔号，但请始终将它视作终结号。
```css
/* bad */
div {
  color: red
}

/* good */
div {
  color: red;
}
```

#### Box model(盒子模型)
> The box model should ideally be the same for the entire document. A global * { box-sizing: border-box; } is fine, but don't change the default box model on specific elements if you can avoid it.

对于整个文档来说，理论上来说盒子模型应该是相同的，全局设置盒子模型(`* { box-sizing: border-box; }`)是很好的。如果可以的话，请不要改变特定元素的默认盒子模型。

```css
/* bad */
div {
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
}

/* good */
div {
  padding: 10px;
}
```

#### Flow(文档流)
> Don't change the default behavior of an element if you can avoid it. Keep elements in the natural document flow as much as you can. For example, removing the white-space below an image shouldn't make you change its default display:

如果可以的话，请不要改变一个元素的默认表现行为。尽可能地保留原生文档流中元素的默认行为。例如，删除一个图像下面的`white-space`不会让你改变它的默认表现行为：
```css
/* bad */
img {
  display: block;
}

/* good */
img {
  vertical-align: middle;
}
```
> Similarly, don't take an element off the flow if you can avoid it.

同样的，如果可以的话，请不要让元素脱离文档流。
```css
/* bad */
div {
  width: 100px;
  position: absolute;
  right: 0;
}

/* good */
div {
  width: 100px;
  margin-left: auto;
}
```

#### Positioning(定位)
> There are many ways to position elements in CSS but try to restrict yourself to the properties/values below. By order of preference:

在CSS中有许多种定位元素的方法，但是对于定位属性值的使用上尽量严格要求你自己按照以下使用顺序：

```
display: block;
display: flex;
position: relative;
position: sticky;
position: absolute;
position: fixed;
```

#### Selectors(选择器)
> Minimize selectors tightly coupled to the DOM. Consider adding a class to the elements you want to match when your selector exceeds 3 structural pseudo-classes, descendant or sibling combinators.

请将选择器与`DOM`节点尽量紧密的联系在一起(耦合)。如果你想为某个元素添加样式的时候你的选择器超过了三层结构的伪类、后代或者兄弟选择器，请考虑直接添加一个`class`。

```css
/* bad */
div:first-of-type :last-child > p ~ *

/* good */
div:first-of-type .info
```
> Avoid overloading your selectors when you don't need to.

如果你不需要，请不要重载你的选择器。
```css
img[src$=svg], ul > li:first-child {
  opacity: 0;
}

/* good */
[src$=svg], ul > :first-child {
  opacity: 0;
}
```
#### Specificity(特异化)
> Don't make values and selectors hard to override. Minimize the use of id's and avoid !important.

不要让属性值和选择器变得难以重写，尽量减少`id`的使用和避免`!important`。

```css
/* bad */
.bar {
  color: green !important;
}
.foo {
  color: red;
}

/* good */
.foo.bar {
  color: green;
}
.foo {
  color: red;
}
```

#### Overriding(复写)
> Overriding styles makes selectors and debugging harder. Avoid it when possible.

复写样式会使得选择器和调试变得困难，尽可能避免这样做。

```css
/* bad */
li {
  visibility: hidden;
}
li:first-child {
  visibility: visible;
}

/* good */
li + li {
  visibility: hidden;
}
```

#### Inheritance(继承)
> Don't duplicate style declarations that can be inherited.

不要对可以继承的样式声明进行重复声明。

```css
/* bad */
div h1, div p {
  text-shadow: 0 1px 0 #fff;
}

/* good */
div {
  text-shadow: 0 1px 0 #fff;
}
```

#### Brevity(简洁性)
> Keep your code terse. Use shorthand properties and avoid using multiple properties when it's not needed.

请保持代码的简洁性。尽量使用简写属性，当没有必要的时候应当避免使用多个属性。

```css
/* bad */
div {
  transition: all 1s;
  top: 50%;
  margin-top: -10px;
  padding-top: 5px;
  padding-right: 10px;
  padding-bottom: 20px;
  padding-left: 10px;
}

/* good */
div {
  transition: 1s;
  top: calc(50% - 10px);
  padding: 5px 10px 20px;
}
```

#### Language(语言)
> Prefer English over math.

相对于数学表达式更提倡使用英语，能使用英语表达的，尽量别使用数学表达式。
```css
/* bad */
:nth-child(2n + 1) {
  transform: rotate(360deg);
}

/* good */
:nth-child(odd) {
  transform: rotate(1turn);
}
```

#### Vendor prefixes(浏览器属性前缀)
> Kill obsolete vendor prefixes aggressively. If you need to use them, insert them before the standard property.

强烈建议抛弃那些已经过时的浏览器属性前缀。如果你需要使用他们，请在标准属性前面插入它们。
```
/* bad */
div {
  transform: scale(2);
  -webkit-transform: scale(2);
  -moz-transform: scale(2);
  -ms-transform: scale(2);
  transition: 1s;
  -webkit-transition: 1s;
  -moz-transition: 1s;
  -ms-transition: 1s;
}

/* good */
div {
  -webkit-transform: scale(2);
  transform: scale(2);
  transition: 1s;
}
```

#### Animations(动画)
> Favor transitions over animations. Avoid animating other properties than `opacity` and `transform`.

相对于`animations`来说更提倡使用`transitions`。除了 `opacity` 和 `transform`，`animate`过程中请避免使用其它属性。

```
/* bad */
div:hover {
  animation: move 1s forwards;
}
@keyframes move {
  100% {
    margin-left: 100px;
  }
}

/* good */
div:hover {
  transition: 1s;
  transform: translateX(100px);
}
```

#### Units(单位)
> Use unitless values when you can. Favor rem if you use relative units. Prefer seconds over milliseconds.

当你能够使用无单位值的时候，你最能能够这样做。如果你需要使用相对单位，推荐`rem`。
相对于毫秒来说，推荐使用秒。

```
/* bad */
div {
  margin: 0px;
  font-size: .9em;
  line-height: 22px;
  transition: 500ms;
}

/* good */
div {
  margin: 0;
  font-size: .9rem;
  line-height: 1.5;
  transition: .5s;
}
```

#### Colors(颜色)
> If you need transparency, use rgba. Otherwise, always use the hexadecimal format.

如果你需要使用透明度，请使用`rgba`。否则，请使用十六进制的形式。

```
/* bad */
div {
  color: hsl(103, 54%, 43%);
}

/* good */
div {
  color: #5a3;
}
```

#### Drawing(图片绘制)
> Avoid HTTP requests when the resources are easily replicable with CSS.

如果一个图形资源你你能用css轻松绘制出来进行代替，那么请不要使用`HTTP`请求。
注:  white-circle   白圈
```
/* bad */
div::before {
  content: url(white-circle.svg);
}

/* good */
div::before {
  content: "";
  display: block;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #fff;
}
```

#### Hacks(css hack技巧)
> Don't use them.

不要使用他们。
```
/* bad */
div {
  // position: relative;
  transform: translateZ(0);
}

/* good */
div {
  /* position: relative; */
  will-change: transform;
}
```
# CSS

CSS文件命名

　　　　　　• 全局样式表常用命名：public.css

　　　　　　• 模块通用样式表命名：模块名_basic.css

　　　　　　• 独立样式表：模块名_页面名.css


良好的注释是非常重要的。请留出时间来描述组件（component）的工作方式、局限性和构建它们的方法。不要让你的团队其它成员 来猜测一段不通用或不明显的代码的目的。

汇总：

以 Components 的角度思考，以两个单词命名（.screenshot-image）
Components 中的 Elements，以一个单词命名（.blog-post .title）
Variants，以中划线-作为前缀（.shop-banner.-with-icon）
Components 可以互相嵌套
记住，你可以通过继承让事情变得更简单
一，class和ID命名

使用语义化、通用的命名方式；
使用连字符 - 作为 ID、Class 名称界定符，不要驼峰命名法和下划线；
避免选择器嵌套层级过多，尽量少于 3 级；
避免选择器和 Class、ID 叠加使用；
避免class，ID和选择器混合式使用，若改了class名称还要去改CSS代码，不利于后期的维护

二，声明块格式

选择器分组时，保持独立的选择器占用一行；
声明块的左括号 { 前添加一个空格；
声明块的右括号 } 应单独成行；
声明语句中的 : 后应添加一个空格；
声明语句应以分号 ; 结尾；
一般以逗号分隔的属性值，每个逗号后应添加一个空格；
rgb()、rgba()、hsl()、hsla() 或 rect() 括号内的值，逗号分隔，但逗号后不添加一个空格；
对于属性值或颜色参数，省略小于 1 的小数前面的 0 （例如，.5 代替 0.5；-.5px 代替 -0.5px）；
十六进制值应该全部小写和尽量简写，例如，#fff 代替 #ffffff；
避免为 0 值指定单位，例如，用 margin: 0; 代替 margin: 0px;；
三，声明顺序

相关属性应为一组，推荐的样式编写顺序

Positioning（由于定位（positioning）可以从正常的文档流中移除元素，并且还能覆盖盒模型（box model）相关的样式，因此排在首位）
Box model（盒模型决定了组件的尺寸和位置，因此排在第二位）
Typographic（排版相关）
Visual（视觉）
Other（其他）
四，引号的使用

url() 、属性选择符、属性值使用双引号。
五，媒体查询（Media query）的位置

　　将媒体查询放在尽可能相关规则的附近。不要将他们打包放在一个单一样式文件中或者放在文档底部。如果你把他们分开了，将来只会被大家遗忘。

六，不要使用 @import

七，Components 最少以两个单词命名，通过 - 分离

点赞按钮 (.like-button)
搜索框 (.search-form)
文章卡片 (.article-card)
八，Elements 是 Components 中的元素 Elements 命名就用一个单词；
```
 .search-form {
    > .field { /* ... */ }
    > .action { /* ... */ }
  }
```
对于倘若需要两个或以上单词表达的 Elements 类名，不应使用中划线和下划线连接，应直接连接。

```
  .profile-box {
    > .firstname { /* ... */ }
    > .lastname { /* ... */ }
    > .avatar { /* ... */ }
  }
```
任何时候尽可能使用 classnames。标签选择器在使用上没有问题，但是其性能上稍弱，并且表意不明确。避免使用标签选择器。

倘若你需要为组件设置定位，应将在组件的上下文（父元素）中进行处理，比如以下例子中，将 widths 和 floats 应用在 list component(.article-list) 当中，而不是 component(.article-card) 自身。

当出现多个嵌套的时候容易失去控制，应保持不超过一个嵌套。

关于CSS的性能优化

一，慎重选择高消耗的样式

box-shadows
border-radius
transparency（透明）
transforms（动画）
CSS filters（滤镜 性能杀手）
二，避免重排列

width
height
padding
margin
display
border-width
position
top
left
right
bottom
font-size
float
text-align
overflow-y
font-weight
overflow
font-family
line-height
vertical-align
clear
white-space
min-height
三，正确的使用display属性

Display 属性会影响页面的渲染，请合理使用。

display: inline后不应该再使用 width、height、margin、padding 以及 float；

display: inline-block 后不应该再使用 float；

display: block 后不应该再使用 vertical-align；

display: table-* 后不应该再使用 margin 或者 float；

四，不滥用float

五，动画性能的优化

动画的基本概念：

帧：在动画过程中，每一幅静止画面即为一“帧”;
帧率：即每秒钟播放的静止画面的数量，单位是fps(Frame per second);
帧时长：即每一幅静止画面的停留时间，单位一般是ms(毫秒);
跳帧(掉帧/丢帧)：在帧率固定的动画中，某一帧的时长远高于平均帧时长，导致其后续数帧被挤压而丢失的现象。
一般浏览器的渲染刷新频率是 60 fps，所以在网页当中，帧率如果达到 50-60 fps 的动画将会相当流畅，让人感到舒适。

如果使用基于 javaScript 的动画，尽量使用 requestAnimationFrame. 避免使用 setTimeout, setInterval.
避免通过类似 jQuery animate()-style 改变每帧的样式，使用 CSS 声明动画会得到更好的浏览器优化。
使用 translate 取代 absolute 定位就会得到更好的 fps，动画会更顺滑。
六，多利用硬件能力，如通过 3D 变形开启 GPU 加速

一般在 Chrome 中，3D或透视变换（perspective transform）CSS属性和对 opacity 进行 CSS 动画会创建新的图层，在硬件加速渲染通道的优化下，GPU 完成 3D 变形等操作后，将图层进行复合操作（Compesite Layers），从而避免触发浏览器大面积重绘和重排。

七，提升CSS选择器的性能

理解了CSS选择器从右到左匹配的机制后，明白只要当前选择符的左边还有其他选择符，样式系统就会继续向左移动，直到找到和规则匹配的选择符，或者因为不匹配而退出。我们把最右边选择符称之为关键选择器。

1、避免使用通用选择器

2、避免使用标签或 class 选择器限制 id 选择器

3、避免使用标签限制 class 选择器

4、避免使用多层标签选择器。使用 class 选择器替换，减少css查找

```
/* Not recommended */
treeitem[mailfolder="true"] > treerow > treecell {…}
/* Recommended */
.treecell-mailfolder {…}
```
5、避免使用子选择器

```
/* Not recommended */
treehead treerow treecell {…}
/* Recommended */
treehead > treerow > treecell {…}
/* Much to recommended */
.treecell-header {…}
```
6、使用继承

```
/* Not recommended */
#bookmarkMenuItem > .menu-left { list-style-image: url(blah) }
/* Recommended */
#bookmarkMenuItem { list-style-image: url(blah) }
```