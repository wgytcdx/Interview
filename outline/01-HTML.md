<!--
 * @Author: your name
 * @Date: 2021-01-04 16:31:13
 * @LastEditTime: 2021-04-12 17:40:11
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /Interview Files/1-HTML.md
-->
#### 1. 跨域请求

- iframe + location.hash/window.name
- postMessage
- JSONP
- websockets
- 服务器改header

#### 2. HTML 的 dom 树如何生成的

浏览器解析HTML文档，在head中发现了link引入文件，于是向服务器请求文件，注意link文件在请求和下载文件过程中将继续向下解析HTML，当引入文件下载完成后会通知浏览器回头来解析；是非阻塞的。而script文件会等待文件下载完成，立即执行，执行完毕后再向下解析，是阻塞的。因此要将css文件放置于顶端，将script标签置于body底端。

如果浏览器在代码中发现一个img标签引用了一张图片，向服务器发出请求。此时浏览器同样不会等到图片下载完，而是继续渲染后面的代码；

解析HTML文档构建DOM树：

- 标签解析
- DOM树构建

##### 3. 标签解析

这部分完成从HTML字符串中解析出标签的功能。主要使用标记化算法。

标记化算法的输入结果是HTML标记，使用状态机表示。状态机一共有4个状态：数据状态(Data)、标记打开状态(Tag open)、标记名称状态(Tag name)、关闭标记打开状态(Close tag open state)。

初始状态是数据状态。

当标记是处于数据状态时，

- 遇到字符<时，状态更改为“标记打开状态”：

  a. 接收一个a-z字符会创建“起始标记”，状态更改为“标记名称状态”，并保持到接收>字符。此期间的字符串会形成一个新的标记名称。接收到>标记后，将当前的新标记发送给树构造器，状态改回“数据状态”

  b. 接收下一个输入字符/时，会创建关闭标记打开状态，并更改为“标记名称状态”。直到接收>字符，将当前的新标记发送给树构造器，并改回“数据状态”。

- 遇到a-z字符时，会将每个字符创建成字符标记，并发送给树构造器。

##### 4. DOM树构建

当标签解析器解析出标签后会发送到DOM树构建器，我们可以认为DOM树构建器主要有以下两部分组成：

- DOM树
- 一个存放签名的栈
  
首先树构建器接收到标签解析器发来的起始标签名后，会加入到栈中，图1是解析到h1标签的栈中压入的内容，共有html body h1三个标签，此时还未向DOM树中添加任何结点（图中黑色实线框代表开始标签，红色虚线框代表结束标签，结束标签不会入栈）。

继续向下解析，接收到一个/h1结束标签，此时查询栈顶元素，如果和传入的结束标签属于同种类型的p标签（如图2），则将栈顶元素弹出，向DOM树中加入此节点，然后继续向下解析（如图3）。

（HTML中的标签从闭合的角度可以分为闭合标签和空标签。而HTML中大部分标签都是闭合标签，其他少数为空标签，常见的空标签有input、img、area、base、link）

如果遇到的是没有封闭标签的元素如img/，则直接加入DOM树中即可，无需入栈。

依次向下解析，当栈为空，即html根节点也加入到DOM树中，DOM树构建完毕。

#### 5. 请说出XHTML和HTML的区别

1. 文档顶部doctype声明不同，xhtml的doctype顶部声明中明确规定了xhtml DTD的写法；

2. html元素必须正确嵌套，不能乱；

3. 属性必须是小写的；

4. 属性值必须加引号；

5. 标签必须有结束，单标签也应该用  “/” 来结束掉；

#### 6. 很多网站不常用table  iframe这两个元素，知道原因吗？

因为浏览器页面渲染的时候是从上至下的，而table 和 iframe 这两种元素会改变这样渲染规则，他们是要等待自己元素内的内容加载完才整体渲染。用户体验会很不友好。

#### 7. jpg和png格式的图片有什么区别？

jpg是有损压缩格式，png是无损压缩格式。所以，相同的图片，jpg体积会小。比如我们一些官网的banner图，一般都很大，所以适合用jpg类型的图片。但png分8位的和24位的，8位的体积会小很多，但在某些浏览器下8位的png图片会有锯齿。

#### 8. form标签上定义请求类型的是哪个属性？定义请求地址的是哪个属性？

form表单定义请求类型的是method属性，定义请求地址的是action属性

#### 9. JOSNP原理及优缺点

- 优点：
  1.不像ajax请求受同源限制，兼容性更好，可以再古老版本浏览器运行，不需要XHLHttpRequest和ActiveX的支持，并且在请求完毕后可以通过调用callback方式返回结果

- 缺点：
  1.只支持GET请求不支持POST等其他类型请求，只支持跨域请求http，不能解决不同域下两个页面的JavaScript调用问题

- 原理：
  1.动态添加一个 script 标签，因为标签的src属性是不受跨域限制的，所以与ajax的XMLHttpRequset无关了(在$.ajax中 dataType参数设置为'jsonp'即可)


#### 10. 流程

1. 项目立项
2. 理解业务
3. 探究后台连接规范(接口类型定义, 请求定义, 返回定义)
4. 定制前端开发规范(管理代码, 框架选择, 代码规范, 包管理工具)
5. 前端工作:
  - 产品经理提出需求,并和开发人员讨论需求的具体细节和可行性
  - 需求确定之后, 发布出原型图和需求文档
  - 开发人员根据原型图和需求文档进行开发, 前端还需要UI提供设计图, 项目总监使用JIRA等项目管理工具来跟踪工作流
  - 搭建测试环境来测试代码, 开发人员完成功能开发并自测通过后, 提交给测试人员进行测试
  - 测试人员和需求方通过后, 将代码合并到远程的master分支, 并部署上线
6. 开发
  - 本地部署
  - 创建分支(svn, git)
  - 选择框架/工具, 定制规范/公共组件库等
  - 测试
  - 开发文档
  - 上线


#### 11. DTD

DTD 是一套关于标记符的语法规则。它是XML1.0版规格得一部分,是html文件的验证机制,属于html文件组成的一部分。

DTD：三种文档类型：S（Strict）、T（Transitional）、F（Frameset）。

- Strict：如果您需要干净的标记，免于表现层的混乱，请使用此类型。请与层叠样式表（CSS）配合使用
- Transitional：DTD 可包含 W3C 所期望移入样式表的呈现属性和元素。如果您的读者使用了不支持层叠样式表（CSS）的浏览器以至于您不得不使用 HTML 的呈现特性时使用
- Frameset： DTD 应当被用于带有框架的文档。除 frameset 元素取代了 body 元素之外，Frameset DTD 等同于 Transitional DTD

html5基本上没有XHTML 1.0 Transitional严格的要求，并且简化了很多东西可以直接使用 
```
<!DOCTYPE HTML>
```

#### 12. 浏览器的怪异模式和标准模式的区别

何时触发怪异模式和标准模式

- 浏览器根据doctype是否存在和哪种DOT格式来决定怪异模式还是标准模式，如果XHTML文档包含完整的DOCTYPE，那么它一般以标准模式呈现
- 代码对于html4.0文档，包含严格dtd的doctype和包含过渡dtd和URI的doctype常常导致页面以标准模式呈现高亮
- 任何新的或位置的doctype将导致标准模式呈现
- 不存在doctype会导致怪异模式
- 但是有过渡dtd没有URI会导致页面以怪异模式呈现
- doctype不存在或形式不正确也会导师html和xhtml文档以怪异模式呈现。
- IE中，如果doctype声明在xml之后，会导致怪异模式

怪异模式和标准模式的区别

- 对 CSS IE盒模型缺陷的处理。在第 6 版之前，Internet Explorer 曾经使用一种决定一个元素的盒模型的宽度和高度的，与 CSS 规范所指定相冲突的算法，而且由于 Internet Explorer 的流行，很多依赖于这种不正确的算法的网页被创建。而在版本 6, Internet Explorer 在标准模式下渲染时使用了 CSS 规范的算法，而在 quirks 模式下使用先前的，不规范的算法。
- 某些行内 (inline) 元素的垂直对齐；很多早期的浏览器对齐图片至包含它们的盒子的下边框，虽然 CSS 的规范要求它们被对齐至盒内文本的基线。标准模式下，基于 Gecko 的浏览器将会对齐至基线，而在 quirks 模式下它们会对齐至底部。
- 很多早期的浏览器对表格内部的字体样式不实施继承；结果是，字体样式必须为整个文档作为一个整体指定一次，并且为表格再次指定一次，尽管 CSS 规范要求字体样式被继承进表格。如果字号使用相对单位指定，一个标准兼容的浏览器会继承基准字号，然后应用于表格内的相对字号：比如，一个声明了基准字号为 80% 的页面内声明表格为 80% （以保证在不正确继承字号的浏览器中有 80% 的字号）的字号将会，在一个标准兼容的浏览器里，显示具有 64% 字号的表格。结果是，浏览器在怪异模式典型的不对表格继承字号。
- IE6不认识!important声明，IE7、IE8、Firefox、Chrome等浏览器认识；而在怪异模式中，IE6/7/8都不认识!important声明

#### 13. HTML5新增表单元素

input type:

1. email

email 类型用于应该包含 e-mail 地址的输入域。

在提交表单时，会自动验证 email 域的值，要求必须包含@符号，同时必须包含服务器名称，如果不能满足验证，则会阻止当前的数据提交

```
邮箱：<input type="email" name="user_email" />
```

2. tel

tel 类型并不是来实现验证。它的本质目的是为了能够在移动端打开数字键盘。意味着数字键盘限制了用户只能输入数字。

```
电话：<input type="tel" name="user_tel" />
```

3. url

url 类型用于应该包含 URL 地址的输入域。

在提交表单时，会自动验证 url 域的值。验证只能输入合法的网址：必须包含http://。

```
网址：<input type="url" name="user_url">
```

4. number:类型用于应该包含数值的输入域。

```
数量：<input type="number" name="points" value="10" max="20" min="0" />
```

属性|值|描述
- | - | -
max|number|规定允许的最大值
min|number|规定允许的最小值
value|number|规定默认值
step|number|规定合法的数字间隔（如果 step=“3”，则合法的数是 -3,0,3,6 等）

**tips:iPhone 中的 Safari 浏览器支持 number 输入类型，并通过改变触摸屏键盘来配合它（显示数字）。**

5. search

search 类型用于搜索域，比如站点搜索或 Google 搜索。

search 域显示为常规的文本域。

```
search:<input type="search" />
```

6. color:类型颜色选择器

```
color:<input type="color" />
```

7. range

range 类型用于应该包含一定范围内数字值的输入域。

range 类型显示为滑动条。

```
range：<input type="range" name="points" min="1" max="10" />
```

属性|值|描述
- | - | -
max|number|规定允许的最大值
min|number|规定允许的最小值
value|number|规定默认值

8. date pickers

HTML5 拥有多个可供选取日期和时间的新输入类型：
- date - 选取日、月、年
- month - 选取月、年
- week - 选取周和年
- time - 选取时间（小时和分钟）
- datetime - 选取时间、日、月、年（UTC 时间）
- datetime-local - 选取时间、日、月、年（本地时间）

```
选取日、月、年：<input type="date" name="user_date"/>
选取月、年：<input type="month" name="user_date"/>
选取周、年：<input type="week" name="user_date"/>
选取时间（小时和分钟）：<input type="time" name="user_date"/>
选取时间、日、月、年（UTC 时间）：<input type="datetime" name="user_date"/>
选取时间、日、月、年（本地时间）：<input type="datetime-local" name="user_date"/>
```

9. placeholder:提示文本，提示占位

```
用户名：<input type="text" name="user_name" placeholder="请输入文本" />
```

10. autofocus:自动获取焦点

```
用户名：<input type="text" name="user_name" autofocus />
```

11. autocomplete

autocomplete:自动完成：on：打开 off：关闭
 
- 必须成功提交过:提交过才会记录
- 当前添加autocomplete的元素必须有name属性

```
用户名：<input type="text" name="user_name" autocomplete="on" />
```

12. required:必须输入，如果没有输入则会阻止当前数据提交

```
<input type="text" name="user_name" required />
```

13. pattern:正则表达式验证

```
手机号：<input type="tel" name="userPhone" pattern="^(\+86)?1\d{10}$">
```

14. multiple:可以选择多个文件

email:有默认验证 在email中，multiple允许输入多个邮箱地址，以逗号分隔

```
文件：<input type="file" name="photo" multiple />
邮箱：<input type="email" name="email" multiple />
```

#### 14. data-属性的作用是什么

data-* 属性用于存储页面或应用程序的私有自定义数据。

data-* 属性赋予我们在所有 HTML 元素上嵌入自定义 data 属性的能力。

存储的（自定义）数据能够被页面的 JavaScript 中利用，以创建更好的用户体验（不进行 Ajax 调用或服务器端数据库查询）。

data-* 属性包括两部分：

- 属性名不应该包含任何大写字母，并且在前缀 “data-” 之后必须有至少一个字符
- 属性值可以是任意字符串
- 用户代理会完全忽略前缀为 “data-” 的自定义属性。

```
<ul>
  <li οnclick="showDetails(this)" id="owl" data-animal-type="人">女人</li>
  <li οnclick="showDetails(this)" id="salmon" data-animal-type="鱼类">食人鱼</li>  
  <li οnclick="showDetails(this)" id="tarantula" data-animal-type="家禽">小鸡鸡</li>  
</ul>
```
#### 15. script标签async和defer的区别及作用

作用：

1. 没有 defer 或 async，浏览器会立即加载并执行指定的脚本，也就是说不等待后续载入的文档元素，读到就加载并执行。

2. async 属性表示异步执行引入的 JavaScript，与 defer 的区别在于，如果已经加载好，就会开始执行——无论此刻是 HTML 解析阶段还是 DOMContentLoaded 触发之后。需要注意的是，这种方式加载的 JavaScript 依然会阻塞 load 事件。换句话说，async-script 可能在 DOMContentLoaded 触发之前或之后执行，但一定在 load 触发之前执行。

3. defer 属性表示延迟执行引入的 JavaScript，即这段 JavaScript 加载时 HTML 并未停止解析，这两个过程是并行的。整个 document 解析完毕且 defer-script 也加载完成之后（这两件事情的顺序无关），会执行所有由 defer-script 加载的 JavaScript 代码，然后触发 DOMContentLoaded 事件。

区别

defer 与相比普通 script，有两点区别：载入 JavaScript 文件时不阻塞 HTML 的解析，执行阶段被放到 HTML 标签解析完成之后。

在加载多个JS脚本的时候，async是无顺序的加载，而defer是有顺序的加载。

#### 16. 解释白屏和 FOUC

白屏:

如果把样式放在文档底部,浏览器会等HTML和CSS完全加载完成之后再绘制到屏幕上去，譬如我们打开某些国外的网站可能出现加载时间过长，页面会出现白屏,而不是内容逐步展现。

FOUC:

如果把样式放在底部,对于FireFox浏览器，会逐步加载无样式的内容，等CSS加载完成之后突然展现样式。这是由于FireFox的渲染逻辑是解析HTML就会直接画到页面上，这时你会看到没有样式的内容，CSS再通过不断的解析将页面重绘一遍，也就是闪烁一下突然展现样式，这就是FOUC。

#### 17. 为什么通常推荐将 CSS link 放置在 head 之间，而将 JS script 放置在 body 之前？你知道有哪些例外吗？

如果把javascript放在head里的话，则先被解析,但这时候body还没有解析。（常规html结构都是head在前，body在后）如果head的js代码是需要传入一个参数（在body中调用该方法时，才会传入参数），并需调用该参数进行一系列的操作，那么这时候肯定就会报错，因为函数该参数未定义（undefined）。

而为什么我们经常看到有很多的人把js脚本放到head里面都不担心出问题？

因为通常把javascript放在head里的话，一般都会绑定一个监听，当全部的html文档解析完之后，再执行代码

#### 18. 浏览器渲染中 回流（reflow）与重绘（repaint）

##### 回流reflow:

增删DOM节点，修改一个元素的宽高，页面布局发生变化，DOM树结构发生变化，那么肯定要重新构建DOM树，而DOM树与渲染树是紧密相连的，DOM树构建完，渲染树也会随之对页面进行再次渲染，这个过程就叫回流。

##### 重绘repaint:

一个元素更换颜色，这样的行为是不会影响页面布局的，DOM树不会变化，但颜色变了，渲染树得重新渲染页面，这就是重汇。

回流的代价要远大于重绘。且回流必然会造成重绘，但重绘不一定会造成回流。

##### 重绘和回流有什么区别:

结合上面的解释，引起DOM树结构变化，页面布局变化的行为叫回流，且回流一定伴随重绘。

只是样式的变化，不会引起DOM树变化，页面布局变化的行为叫重绘，且重绘不一定会便随回流。

##### 怎么减少回流reflow:

1. DOM的增删行为

比如你要删除某个节点，给某个父元素增加子元素，这类操作都会引起回流。如果要加多个子元素，最好使用documentfragment。

2. 几何属性的变化

比如元素宽高变了，border变了，字体大小变了，这种直接会引起页面布局变化的操作也会引起回流。如果你要改变多个属性，最好将这些属性定义在一个class中，直接修改class名，这样只用引起一次回流。

3. 元素位置的变化

修改一个元素的左右margin，padding之类的操作，所以在做元素位移的动画，不要更改margin之类的属性，使用定位脱离文档流后改变位置会更好。

4. 获取元素的偏移量属性

例如获取一个元素的scrollTop、scrollLeft、scrollWidth、offsetTop、offsetLeft、offsetWidth、offsetHeight之类的属性，浏览器为了保证值的正确也会回流取得最新的值，所以如果你要多次操作，最取完做个缓存。

5. 页面初次渲染

这样的回流无法避免

6. 浏览器窗口尺寸改变

resize事件发生也会引起回流。

参考:https://www.cnblogs.com/echolun/p/10105223.html

#### 19. 什么属性能让浏览器直接使用ES6 Module

#### 20. 移动端适配时对viewport的理解

viewport的概念:

viewport就是设备的屏幕上能用来显示我们的网页的那一块区域。

```
<meta name="viewport"   content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
```

- viewport标记，用于指定用户是否可以缩放Web页面，并对相关的选项进行设定。

- width 和height 指令分别指定视区的逻辑宽度和高度。它们的值可以是以像素为单位的数字，也可以是一个特殊的标记符号。如上文代码中device-width即表示，视区宽度应为设备的屏幕宽度。类似的，device-height即表示设备的屏幕高度。

- initial-scale用于设置Web页面的初始缩放比例。默认的初始缩放比例值因智能手机浏览器的不同而有所差异，通常情况下，设备会在浏览器中呈现出整个Web页面。设为1.0则显示未经缩放的Web页面。

- maximum-scale和minimum-scale用于设置用户对于Web页面缩放比例的限制。值的范围为0.25~10.0之间

- user-scalable指定用户是否可以缩放视区，即缩放Web页面的视图。值为yes时允许用户进行缩放，值为no时不允许缩放。

参考:https://blog.csdn.net/weixin_45685544/article/details/109715595

参考:https://blog.csdn.net/u013448372/article/details/109227134

#### 21. Cookie, SessionStorage, LocalStorage区别

特性 | Cookie | SessionStorage | localStorage
---|---|---|---
数据生命周期 | 生成时会被指定一个maxAge，这就是cookie的生命周期，在这个周期内cookie有效，默认关闭浏览器失效。| 页面会话期间可用 | 除非主动清除，否则一直存在
存放数据大小 | 4K左右（因为每次HTTP请求都会携带cookie） | 一般5M或更大
与服务器通信 | 由对服务器的请求来传递，每次都会携带在http头中，如果使用cookie保存过多数据会带来性能问题 | 数据不是由每个服务器请求传递的，而是只有在请求时使用数据，不参与和服务器的通信
易用性 | cookie需要自己封装setCookie、getCookie | 可以用原生接口，也可以再次封装来对Object和Array有更好的支持
共同点 | 都是保存在浏览器端，和服务器的session机制不同

#### 22. 移动端300ms延迟原因及解决方案

解决方案:

1. faskclick https://github.com/ftlabs/fastclick

原理: 在检测到touchend事件的时候，会通过DOM自定义事件立即出发模拟一个click事件，并把浏览器在300ms之后真正的click事件阻止掉

缺点: 脚本相对较大, 不建议使用

2. 禁用游览器缩放

当HTML文档头部包含如下meta标签时：

```
<meta name="viewport" content="user-scalable=no">
<meta name="viewport" content="initial-scale=1, maximum-scale=1">
```

`表明这个页面是不可缩放的，那双击缩放的功能就没有意义了，此时浏览器可以禁用默认的双击缩放行为并且去掉300ms的点击延迟。
这个方案有一个缺点，就是必须通过完全禁用缩放来达到去掉点击延迟的目的，然而完全禁用缩放并不是我们的初衷，我们只是想禁掉默认的双击缩放行为，这样就不用等待300ms来判断当前操作是否是双击。但是通常情况下，我们还是希望页面能通过双指缩放来进行缩放操作，比如放大一张图片，放大一段很小的文字。`

3. 更改默认的视口宽度

```
<meta name="viewport" content="width=device-width">
```

`一开始，因为双击缩放主要是用来改善桌面站点在移动端浏览体验的。 随着发展现在都是专门为移动开发专门的站点，这个时候就不需要双击缩放了，所以移动端浏览器就可以自动禁掉默认的双击缩放行为并且去掉300ms的点击延迟。如果设置了上述meta标签，那浏览器就可以认为该网站已经对移动端做过了适配和优化，就无需双击缩放操作了。
这个方案相比方案一的好处在于，它没有完全禁用缩放，而只是禁用了浏览器默认的双击缩放行为，但用户仍然可以通过双指缩放操作来缩放页面。`

4. 通过 touchstart 和 touchend模拟实现

`能不能直接用touchstart代替click呢，
答案是不能，使用touchstart去代替click事件有两个不好的地方。
第一：touchstart是手指触摸屏幕就触发，有时候用户只是想滑动屏幕，却触发了touchstart事件，这不是我们想要的结果；
第二：使用touchstart事件在某些场景下可能会出现点击穿透的现象。`

什么是点击穿透？

假如页面上有两个元素A和B。B元素在A元素之上。我们在B元素的touchstart事件上注册了一个回调函数，该回调函数的作用是隐藏B元素。我们发现，当我们点击B元素，B元素被隐藏了，随后，A元素触发了click事件。

这是因为在移动端浏览器，事件执行的顺序是touchstart > touchend > click。而click事件有300ms的延迟，当touchstart事件把B元素隐藏之后，隔了300ms，浏览器触发了click事件，但是此时B元素不见了，所以该事件被派发到了A元素身上。如果A元素是一个链接，那此时页面就会意外地跳转。

参考:https://blog.csdn.net/fhjdzkp/article/details/100918720

#### 23. Web前端性能优化经验分享

1. 请减少HTTP请求
2. 请正确理解 Repaint(重绘) 和 Reflow(重排)
3. 请减少对DOM的操作
4. 使用JSON格式来进行数据交换
5. 高效使用HTML标签和CSS样式
6. 使用CDN加速(内容分发网络)
7. 将CSS和JS放到外部文件中引用，CSS放头，JS放尾
8. 精简CSS和JS文件
9. 压缩图片和使用图片Spirit技术
10. 注意控制Cookie大小和污染

参考:https://www.cnblogs.com/zhangyublogs/p/5026569.html





