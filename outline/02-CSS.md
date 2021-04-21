#### 1. 浮动与清除浮动

##### 浮动:

浮动元素会脱离文档的普通流,根据float的值向左或向右移动,直到它的外边界碰到父元素的内边界或另一个浮动元素的外边界为止。由于浮动框不在文档的普通流中,所以文档的普通流中的块级元素表现得就像浮动元素不存在一样。

##### 浮动的影响

- 浮动元素会造成父元素塌陷
- 浮动元素的左(右)外边界不能超出其父元素的左(右)内边界。
- 浮动元素不会重叠。
- 浮动元素的顶端不能比其父元素的内顶端更高,不能比之前出现的浮动元素顶端高。

1. 子元素添加clear:both（不推荐）
   - 优点：通俗易懂，方便
   - 添加无意义标签，语义化差

2. 父元素添加overfloat:hidden（不推荐）
   - 优点：代码简洁
   - 缺点：内容增多的时候容易造成不会自动换行导致内容被隐藏掉，无法显示要溢出的元素
3. 使用after伪元素清除浮动（推荐使用）
   - 优点：符合闭合浮动思想，结构语义化正确
   - 缺点：ie6-7不支持伪元素：after，使用zoom:1触发hasLayout
```
.clearfix:after{/*伪元素是行内元素 正常浏览器清除浮动方法*/
    content: "";
    display: block;
    height: 0;
    clear:both;
    visibility: hidden;
}
.clearfix{
    *zoom: 1;/*ie6清除浮动的方式 *号只有IE6-IE7执行，其他浏览器不执行*/
}
 
<body>
    <div class="fahter clearfix">
        <div class="big">big</div>
        <div class="small">small</div>
        <!--<div class="clear">额外标签法</div>-->
    </div>
    <div class="footer"></div>
</body>
```
4. 使用before和after双伪元素清除浮动
   - 优点：代码更简洁
   - 缺点：ie6-7不支持伪元素，用zoom:1触发hasLayout.
```
.clearfix:after,.clearfix:before{
    content: "";
    display: table;
}
.clearfix:after{
    clear: both;
}
.clearfix{
    *zoom: 1;
}
 
 <div class="fahter clearfix">
    <div class="big">big</div>
    <div class="small">small</div>
</div>
<div class="footer"></div>
```

参考:https://segmentfault.com/a/1190000005905564

#### 2. 屏幕水平垂直居中

1. css3:

```
div{
    position: absolute;
    left:50%;
    top:50%;
    transform: translate(-50%, -50%);
}
```

2. position:

```
.father{
    position:relative
}
.child{
    position:absolute;
    left:0;
    top: 0;
    bottom: 0;
    right: 0;
    margin: auto;
}
```

3. flex:

```
.div{
    display:flex;
    justify-content:center;
    align-item:center
}
```

4. table-cell:

```
<!--html-->
<div class="table-cell">
    <p>我爱你</p>
</div>

/**css**/
.table-cell {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
    width: 240px;
    height: 180px;
}
```

5. calc: 计算

#### 3. CSS布局方式

1. 静态布局

布局概念: 最传统、原始的Web布局设计。网页最外层容器(outer)有固定的大小,所有的内容以该容器为标准,超出宽高的部分用滚动条(overflow:scroll)来实现滚动查阅。

优点: 采用的是css2之前的写法,不存在浏览器兼容性。布局简单。

缺点: 但是移动端不可以使用pc端的页面,两个页面的布局不一致,移动端需要自己另外设计一个布局并使用不同域名呈现。

2. 流式布局

布局概念: 流式布局也叫百分比布局

这边引入一下自适应布局:

分别为不同的屏幕设置布局格式，当屏幕大小改变时，会出现不同的布局，意思就是在这个屏幕下这个元素块在这个地方，但是在那个屏幕下，这个元素块又会出现在那个地方。只是布局改变，元素不变。可以看成是不同屏幕下由多个静态布局组成的。

而流式布局的特点是随着屏幕的改变，页面的布局没有发生大的变化，可以进行适配调整，这个正好与自适应布局相补。

优点: 元素的宽高用百分比做单位，元素宽高按屏幕分辨率调整，布局不发生变化.

缺点: 屏幕尺度跨度过大的情况下，页面不能正常显示。

3. 弹性布局:

布局概念: 弹性布局是CSS3引入的强大的布局方式，用来替代以前Web开发人员使 的一些复杂易错的hacks方法(如float实现流式布局)。

优点: 简单、方便、快速.

缺点: CSS3新特性,浏览器兼容性非常头疼。而且手机浏览器对flex的支持也不是很理想。

4. 响应式布局

采用自适应布局和流式布局的综合方式，为不同屏幕分辨率范围创建流式布局

现在优秀的页面都追求一套代码可以实现三端的浏览;

从概念可以看出来,自适应布局的诞生是为了实现不同屏幕分辨率的终端上浏览网页的不同展示方式。

通过响应式设计能使网站在手机和平板电脑上有更好的浏览阅读体验。屏幕尺寸不一样展示给用户的网页内容也不一样.

利用媒体查询可以检测到屏幕的尺寸(主要检测宽度)，并设置不同的CSS样式，就可以实现响应式的布局。
 
#### 4. BFC布局

BFC 应该可以抽象成一个 独立的个体，出淤泥而不染的白莲花。

是的不受其他元素的影响，始终保持自己独立。就是正常的普通流布局规则，设置的样式是什么样的，就按正常的普通流布局的表现。

内部的子元素 不受外界的影响，外界的元素，也不受内部的元素的影响。各自独立生活在html这片空间下。

BFC触发条件：

【1】根元素，即HTML元素

【2】float的值不为none

【3】overflow的值不为visible

【4】display的值为inline-block、table-cell、table-caption

【5】position的值为absolute或fixed 

BFC布局规则：

1. 内部的Box会在垂直方向，一个接一个地放置（不设置浮动，设置浮动那肯定是 左右一行排列了）。

2. Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠（正常的不设置浮动，两个块元素margin重叠,仅仅是垂直方向，左右不是这个样子的）

3. 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。（不设置浮动，不设置左边距，块级子元素，一律靠左竖直向下排列，内联子元素一律从左向右排列，想想，正常写代码，都是这样，设置左浮动的靠近父元素的左边，设置右浮动，靠近父元素的右边。）

4. BFC的区域不会与float box重叠。BFC区域的子元素不受外面的影响，外面的也不受BFC区域里面的影响（这个挺重要的，设置的浮动的元素，脱离了文档流，正常的相邻的元素会跑到它下面（靠左）。设置成BFC，自己独成一块，不会跑到它下面，而是保持洁身自好，自己依然占一块。）

6. 计算BFC的高度时，浮动元素也参与计算（就是子元素设置浮动，脱离文档流，父元素高度塌陷，给父元素设置BFC，那么父元素高度就不会忽略浮动的子元素，从而高度就不会塌陷，这样理解，好像是BFC又把脱离文档流的元素，又拉回来了，但保持了浮动的特点。）

#### 5. position针对父元素定位

https://blog.csdn.net/chen_zw/article/details/8741365

https://blog.csdn.net/chen_zw/article/details/8741365?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=1328740.49500.16170691462183103&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control

absolute的英文意思是绝对的意思,实际上是针对父级元素元素定位,如果父级元素没有position:relative|absolute,则追至再上一个父级元素,直至相对于文档的左上角定位，按照我们中国人的理解观念，这个其实是相对定位，是脱离文档流的。用了abolute属性，原有float属性将失效；

relative的英文意思是相对的意思，实际上是相对于对象当前位置的定位。而且是不脱离文档流的，就算用top、lef、bottom、right或margin将其移动位置，它也会在原来的文档流中占有自己实际大小的一块位置。

absolute则会找他的父元素,直到找到一个position属性不是static的父元素,可能是它的上一层,也可能是好几层,如果都没有定义position属性的话,那将根据body定位.
还有一个区别是:relative的元素,不管你怎么改变top或是left,他原来的那一块div的位置还是存在着的,会占着那边.如果有人挤掉了那一块,那他的位置也会改变.

#### 6. 两种盒模型

什么是盒模型:

所有HTML元素可以看作盒子。在 CSS 中，”box model” 这一术语是用来设计和布局时使用

而这个盒子，由四个部分组成：

- margin	外边距，可以当做盒子与盒子之间的距离
- border	边框，可以当做是盒子的厚度
- padding	内边距，可以当做盒子边框于其中容纳物的距离
- content	内容，即盒子中存放文本或图片的区域

标准模型和 IE模型（怪异盒子）:

在 CSS 中，我们可以通过

box-sizing: conent-box;
box-sizing: border-box;

区别:

标准模型:

元素的width = 内容的width。

IE盒模型

元素的width = 内容的width + padding + border。

切换两种盒模型方法

box-sizing: content-box;//标准盒模型（默认）

box-sizing: border-box;//IE盒模型

#### 7. 什么是外边框重叠

相邻的两个盒子margin可结合为一个单独外边距

- margin都为正数时，折叠结果取两者最大值

- margin都为负数时，折叠结果取绝对值较大值

- margin一正一负时，结果为两者和

如何避免外边框重叠:

属于同一个BFC的两个相邻的Box会发生margin重叠，所以我们可以设置，两个不同的BFC，也就是我们可以让把第二个p用div包起来，然后激活它使其成为一个BFC;

```
p {
    width: 200px;
    height: 150px;
    margin: 30px;
    background-color: rgba(123, 123, 123, 0.2);
}

div {
    overflow: hidden;
}

<body>
    <p></p>
    <div>
        <p></p>
    </div>
</body>
```

#### 8. rgba和opacity有什么区别


rgba()只作用于元素color于background属性透明，但子级不会继承属性。

opacity()针对元素所有样式透明效果

#### 9. Grid布局

#### 10. CSS 选择器优先级

CSS 的继承性:

CSS 的继承特性指的是应用在一个标签上的那些 CSS 属性被传到其子标签上。看下面的 HTML 结构：

```
<div>
    <p></p>
</div>
```

如果 &lt;div&gt; 有个属性 color: red，则这个属性将被 &lt;p&gt; 继承，即 &lt;p&gt; 也拥有属性 color: red。

由上可见，当网页比较复杂， HTML 结构嵌套较深时，一个标签的样式将深受其祖先标签样式的影响。影响的规则是：

##### 1. CSS 优先规则1： 最近的祖先样式比其他祖先样式优先级高。

```
<!-- 类名为 son 的 div 的 color 为 blue -->
<div style="color: red">
    <div style="color: blue">
        <div class="son"></div>
    </div>
</div>
```

如果我们把一个标签从祖先那里继承来的而自身没有的属性叫做"祖先样式"，那么"直接样式"就是一个标签直接拥有的属性。又有如下规则：

##### 2. CSS 优先规则2："直接样式"比"祖先样式"优先级高。

```
<!-- 类名为 son 的 div 的 color 为 blue -->
<div style="color: red">
    <div class="son" style="color: blue"></div>
</div>
```

##### 选择器的优先级

上面讨论了一个标签从祖先继承来的属性，现在讨论标签自有的属性。在讨论 CSS 优先级之前，先说说 CSS 7 种基础的选择器：

- ID 选择器， 如 #id{}

- 类选择器， 如 .class{}

- 属性选择器， 如 a[href="segmentfault.com"]{}

- 伪类选择器， 如 :hover{}

- 伪元素选择器， 如 ::before{}

- 标签选择器， 如 span{}

- 通配选择器， 如 *{}

##### 3. CSS 优先规则3：

优先级关系：内联样式 > ID 选择器 > 类选择器 = 属性选择器 = 伪类选择器 > 标签选择器 = 伪元素选择器

```
// HTML
<div class="content-class" id="content-id" style="color: black"></div>

// CSS
#content-id {
    color: red;
}
.content-class {
    color: blue;
}
div {
    color: grey;
}
```

最终的 color 为 black，因为内联样式比其他选择器的优先级高。

所有 CSS 的选择符由上述 7 种基础的选择器或者组合而成，组合的方式有 3 种：

- 后代选择符： .father .child{}

- 子选择符： .father > .child{}

- 相邻选择符: .bro1 + .bro2{}

当一个标签同时被多个选择符选中，我们便需要确定这些选择符的优先级。我们有如下规则：

##### 4. CSS 优先规则4：

计算选择符中 ID 选择器的个数（a），计算选择符中类选择器、属性选择器以及伪类选择器的个数之和（b），计算选择符中标签选择器和伪元素选择器的个数之和（c）。按 a、b、c 的顺序依次比较大小，大的则优先级高，相等则比较下一个。若最后两个的选择符中 a、b、c 都相等，则按照"就近原则"来判断。

```
// HTML
<div id="con-id">
    <span class="con-span"></span>
</div>

// CSS
#con-id span {
    color: red;
}
div .con-span {
    color: blue;
}
```

由规则 4 可见，&lt;span&gt; 的 color 为 red。

如果外部样式表和内部样式表中的样式发生冲突会出现什么情况呢？这与样式表在 HTML 文件中所处的位置有关。样式被应用的位置越在下面则优先级越高，其实这仍然可以用规则 4 来解释。

例5：

```
// HTML
<link rel="stylesheet" type="text/css" href="style-link.css">
<style type="text/css">
@import url(style-import.css); 
div {
    background: blue;
}
</style>

<div></div>

// style-link.css
div {
    background: lime;
}

// style-import.css
div {
    background: grey;
}
```

从顺序上看，内部样式在最下面，被最晚引用，所以 &lt;div&gt; 的背景色为 blue。

上面代码中，@import 语句必须出现在内部样式之前，否则文件引入无效。当然不推荐使用 @import 的方式引用外部样式文件，原因见另一篇博客：CSS 引入方式[https://www.runoob.com/w3cnote/html-import-css-method.html]。

CSS 还提供了一种可以完全忽略以上规则的方法，当你一定、必须确保某一个特定的属性要显示时，可以使用这个技术。

##### 5. CSS 优先规则5：

属性后插有 !important 的属性拥有最高优先级。若同时插有 !important，则再利用规则 3、4 判断优先级。

```
// HTML
<div class="father">
    <p class="son"></p>
</div>

// CSS
p {
    background: red !important;
}
.father .son {
    background: blue;
}
```
虽然 .father .son 拥有更高的权值，但选择器 p 中的 background 属性被插入了 !important， 所以 &lt;p&gt; 的 background 为 red。

#### 11. Normalize.css

Normalize.css 只是一个很小的CSS文件，但它在默认的HTML元素样式上提供了跨浏览器的高度一致性。相比于传统的CSS reset，Normalize.css是一种现代的、为HTML5准备的优质替代方案。Normalize.css现在已经被用于Twitter Bootstrap、HTML5 Boilerplate、GOV.UK、Rdio、CSS Tricks 以及许许多多其他框架、工具和网站上。

##### 综述

Normalize.css是一种CSS reset的替代方案。

我们创造normalize.css有下面这几个目的：

- 保护有用的浏览器默认样式而不是完全去掉它们
- 一般化的样式：为大部分HTML元素提供
- 修复浏览器自身的bug并保证各浏览器的一致性
- 优化CSS可用性：用一些小技巧
- 解释代码：用注释和详细的文档来

Normalize.css支持包括手机浏览器在内的超多浏览器，同时对HTML5元素、排版、列表、嵌入的内容、表单和表格都进行了一般化。尽管这个项目基于一般化的原则，但我们还是在合适的地方使用了更实用的默认值。

##### Normalize vs Reset

知道Normalize.css和传统Reset的区别是非常有价值的. 

1. Normalize.css 保护了有价值的默认值
2. Normalize.css 修复了浏览器的bug
3. Normalize.css 不会让你的调试工具变的杂乱
4. Normalize.css 是模块化的
5. Normalize.css 拥有详细的文档

无论从适用范畴还是实施上，Normalize.css与Reset都有极大的不同。尝试一下这两种方法并看看到底哪种更适合你的开发偏好是非常值得的。这个项目在Github上以开源的形式开发。任何人都能够提交问题报告或者提交补丁。整个项目发展的过程对所有人都是可见的，而每一次改动的原因也都写在commit信息中，这些都是有迹可循的。

参考:https://jerryzou.com/posts/aboutNormalizeCss/

#### 12. 描述下z-index和叠加上下文是如何形成的？

##### z-index是什么？

z-index 属性设置元素的堆叠顺序。拥有更高堆叠顺序的元素总是会处于堆叠顺序较低的元素的前面。
该属性设置一个定位元素沿 z 轴的位置，z 轴定义为垂直延伸到显示区的轴。如果为正数，则离用户更近，为负数则表示离用户更远。

##### z-index和叠加上下文是如何形成的?

传送:https://blog.csdn.net/cat_sky/article/details/80302245

##### 使用z-index有什么需要注意的地方？

- 在开发中尽量避免层叠上下文的多层嵌套，因为层叠上下文嵌套过多的话容易产生混乱，如果对层叠上下文理解不够的话是不好把控的。
- 非浮层元素（对话框等）尽量不要用z-index（通过层叠顺序或者dom顺序或者通过层叠上下文进行处理）
- z-index设置数值时尽量用个位数
- 让absolute元素覆盖正常文档流内元素（不用设z-index，自然覆盖）
- 让后一个absolute元素覆盖前一个absolute元素（不用设z-index，只要在HTML端正确设置元素顺序即可）

##### 什么情况下使用z-index？

当absolute元素覆盖另一个absolute元素，且HTML端不方便调整DOM的先后顺序时，需要设置z-index: 1。非常少见的情况下多个absolute交错覆盖，或者需要显示最高层次的模态对话框时，可以设置z-index > 1。

#### 13. 什么是css sprites

对于浏览器来说，他们的请求方式都是发起一个Http请求，经历三次握手，并把文件拉取回来，一般的浏览器内核只能同时并发4，5个网络请求，所以大量的小图片特别影响性能，不但网页加载完成时间慢，还可能影响一些重要的JS逻辑，使得网页响应也变慢，卡死等等，对于浏览器来说，发起一个Http请求，来回几百毫秒的耗时，已经是相当高的资源耗费。然后很多人就想到了一种办法，把网页中一些背景图片整合到一张图片文件中，再利用CSS的“background-image”，“background- repeat”，“background-position”的组合进行背景定位，background-position可以用数字精确的定位出背景图片的位置，这样可以将原本需要多张图像文件分别显示，整并为单一图像文件的分区显示技术，借由减少下载图像文件数量，提高网页的显示性能。

1. 使用雪碧图的优点：

- 利用CSS Sprites能很好地减少网页的http请求，从而大大的提高页面的性能，这也是CSS Sprites最大的优点，也是其被广泛传播和应用的主要原因；

- CSS Sprites能减少图片的字节，曾经比较过多次3张图片合并成1张图片的字节总是小于这3张图片的字节总和。

- 解决了网页设计师在图片命名上的困扰，只需对一张集合的图片上命名就可以了，不需要对每一个小元素进行命名，从而提高了网页的制作效率。

2. 使用雪碧图的缺点

- 在图片合并的时候，你要把多张图片有序的合理的合并成一张图片，还要留好足够的空间，防止板块内出现不必要的背景；这些还好，最痛苦的是在宽屏，高分辨率的屏幕下的自适应页面，你的图片如果不够宽，很容易出现背景断裂。

-  至于可维护性，这是一般双刃剑。可能有人喜欢，有人不喜欢，因为每次的图片改动都得往这个图片删除或添加内容，显得稍微繁琐。而且算图片的位置（尤其是这种上千px的图）也是一件颇为不爽的事情。

- 由于图片的位置需要固定为某个绝对数值，这就失去了诸如center之类的灵活性。

#### 14. SVG

SVG 是一种基于 XML 语法的图像格式，全称是可缩放矢量图（Scalable Vector Graphics）。其他图像格式都是基于像素处理的，SVG 则是属于对图像的形状描述，所以它本质上是文本文件，体积较小，且不管放大多少倍都不会失真。此外SVG 是万维网联盟的标准，SVG 与诸如 DOM 和 XSL 之类的 W3C 标准是一个整体。

参考:https://zhuanlan.zhihu.com/p/54088196

#### 15. 浏览器兼容常见问题

参考:https://blog.csdn.net/gaoqiang1112/article/details/77891969

#### 16. 什么叫优雅降级和渐进增强

渐进增强 progressive enhancement：
针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。

优雅降级 graceful degradation：
一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。

区别：

- 优雅降级是从复杂的现状开始，并试图减少用户体验的供给

- 渐进增强则是从一个非常基础的，能够起作用的版本开始，并不断扩充，以适应未来环境的需要

- 降级（功能衰减）意味着往回看；而渐进增强则意味着朝前看，同时保证其根基处于安全地带

#### 17. CSS3媒体查询

1. 什么是媒体查询
媒体查询就是可以根据浏览器的特性（窗口大小、横竖屏、屏幕比例等）为此设定不同的 CSS 样式。

2. 为什么要媒体查询
我们在制作网页的时候，希望可以根据用户的屏幕特性而展现不同的网页内容，让用户体验更好。

3. 媒体查询具体用法
早在 CSS2 开始就已经支持媒体查询了，只不过和 CSS3 不太一样。以下是 CSS2 用法：
// 写在HTML的 head 标签中, 表示：屏幕小于等于1200px时才使用style.css样式
```
<link rel="stylesheet" type="text/css" media="screen and (max-width: 1200px)" href="style.css">

<link rel="stylesheet" type="text/css" media="(orientation: portrait)" href="main.css">
```

CSS3用法:

```
@media screen and (max-width: 960px) {  // 屏幕小于960px时显示灰色
    body {
        background: #999;
    }
}

@media screen and (max-width: 768px) {  // 屏幕小于768px时显示#03a9f4（蓝色）
    body {
        background: #03a9f4;
    }
}

@media screen and (max-width: 320px){  // 屏幕小于768px时显示#ff5722（橘黄）
    body {
        background: #ff5722;
    }
}
```

#### 18. 什么是伪元素

伪元素可以比作一个假的元素，但不存在与dom树中，通常用来向元素添加小图标、清除浮动影响等

#### 19. 为什么提倡使用 translate() 而非 不是 absolute

改变transform或opacity不会触发浏览器重新布局（reflow）或重绘（repaint），只会触发复合（compositions）(复合是什么，我也不懂，没听说过，有知道的朋友可以在留言区告诉我)。

transform使浏览器为元素创建一个 GPU 图层

translate改变位置时，元素依然会占据其原始空间

而改变绝对定位会触发重新布局，进而触发重绘和复合。

改变绝对定位会使用到 CPU。

因此translate()更高效，可以缩短平滑动画的绘制时间。

#### 20. 圣杯布局，双飞翼布局

参考:https://www.jianshu.com/p/81ef7e7094e8

参考:https://zhuanlan.zhihu.com/p/103774667

#### 21. px、em、rem、vw、百分比的区别

- px是固定单位，其他几种都是相对单位。当我们把电脑屏幕的分辨率调为1440*900时，css里设置的1px实际的物理尺寸就是**屏幕宽度**的1/1440。
- em：默认字体大小的倍数。比如给元素设置font-size: 2em，这里的默认字体大小实际上是继承自父亲的大小，font-size: 2em表示当前元素字体大小是父亲的2倍。当给元素设置width: 2em，这里的默认字体大小是该元素自身的实际字体大小。
- rem：根元素(html 节点)字体大小的倍数。比如一个元素设置 width: 2rem 表示该元素宽度为html节点的font-size 大小的2倍。 如果html未设置font-size的大小，默认是16px。
- 1vw 代表**浏览器**视口宽度的1%。
- 1% 对不同属性有不同的含义。 font-size: 200% 和font-size: 2em 一样，表示字体大小是默认（继承自父亲）字体大小的2倍。 line-height: 200% 表示行高是自己字体大小的2倍。 width: 100%表示自己content的宽度等于父亲content宽度的1倍。
- 需要注意的是chrome浏览器下文字最小是12px，设置低于12px的值最终也会展示12px。

参考:https://blog.csdn.net/weixin_36074841/article/details/112355541

#### 22. 物理像素、逻辑像素、CSS像素、PPI、设备像素比是什么

参考:https://blog.csdn.net/qq_33834489/article/details/79247119

#### 23. 解决前端浏览器字体小于12px办法`transform:scale()`

```
.Num{
    font-size: 14px;
    -webkit-transform: scale(0.8);
 }
.Numsize-font{
  font-size: 14*0.8px;
}
```

**注意**:transform:scale()这个属性只可以缩放可以定义宽高的元素，而行内元素是没有宽高的，我们可以加上一个display:inline-block;


















