#### 1. JSON.stringify、JSON.parse、toString()

JSON.stringify()是从一个对象解析出JSON字符串，是带[]的

toString()是将数组转化成字符串，因此不带 []

JSON.parse() 是用于从一个字符串中解析出json对象

#### 2. 添加、移除、移动、复制、创建和查找节点的方法

- 创建节点：
```
creatDocumentFragment()     //创建一个DOM片段
creatElement()              //创建一个具体元素
createTextNode()            //创建一个文本节点
```

- 添加、移除、替换、插入
```
appendChild()                   //添加
removeChild()                   //移除
replaceChild()                  //替换
insertBefore(newNode, exsitNode)//插入
```

- 查找
```
getElementsByTagName()  //通过标签名称
getElementsByName()     //通过元素的Name属性值
getElementById()        //通过元素的id (唯一性)
```

- insertBefore用法
```
//html：
    <ul>
        <li>foo</li>
        <li>bar</li>
    </ul>
//js:
//创建一个li插入到ul的第一个li前面
var li = creatElement('li');
li.innerHtml = 'aaa';
var firstLi = document.getElementsByTagName('li')[0];
firstLi.parentNode.insertBefore(li, firstLi);
```

#### 3. window.onload与document.ready有什么区别

document.ready:当DOM结构加载完成即执行。

window.onload:当页面所有资源加载完毕即执行。

#### 4. "= =" 和 "= = ="有什么不同

前者会自动转换类型 后者不会

#### 5. js中检测用户客户端浏览器与操作系统类型

var ua = window.navigator.userAgent;

#### 6. 在js中什么是伪数组，如何将伪数组转换成标准数组

伪数组（类数组）：无法直接调用数组方法或期望length属性有什么特殊的行为，但仍可以对真正数组遍历方法来遍历它们。

典型的是函数的 argument参数，还有像调用getElementsByTagName,document.childNodes之类的,它们都返回 NodeList对象都属于伪数组。

可以使用Array.prototype.slice.call(fakeArray)将数组转化为真正的Array 对象。

#### 7. typeof、instanceof和Object.prototype.toString()

JavaScript 中 typeof 和 instanceof 常用来判断一个变量是否为空， 或者是什么类型的。

typeof:

1. 对于对象、数组、null 返回的值是 object 。比如typeof(window)，typeof(document)，typeof(null)返回的值都是object。

2. 对于函数类型，返回的值是 function。比如：typeof(eval)，typeof(Date)返回的值都是function。

3. 返回值是一个字符串， 用来说明变量的数据类型。

4. typeof 一般只能返回如下几个结果： number, boolean, string, function, object, undefined。

**对于 Array, Null 等特殊对象使用 typeof 一律返回 object， 这正是 typeof 的局限性。**

instaceof:

1. 返回值为布尔值;

2. instanceof 用于判断一个变量是否属于某个对象的实例。

```
var a = new Array();
alert(a instanceof Array); //true
alert(a instanceof Object) //true
//如上， 会返回 true， 同时 alert(a instanceof Object) 也会返回 true;
//这是因为 Array 是 object 的子类。
alert(b instanceof Array) b is not defined

function Test() {};
var a = new test();
alert(a instanceof test) // true
```

Object.prototype.toString():

```
let obj = {};
let arr = [];
let foo = function() {}
let num = 1;
let bool = true;
let str = 'hzt';
let udf = undefined;
let nul = null;

console.log(Object.prototype.toString.call(num).split('[object ')[1].split(']')[0])//Number
console.log(Object.prototype.toString.call(bool).split('[object ')[1].split(']')[0])//Boolean
console.log(Object.prototype.toString.call(str).split('[object ')[1].split(']')[0])//String
console.log(Object.prototype.toString.call(udf).split('[object ')[1].split(']')[0])//Undefined
console.log(Object.prototype.toString.call(arr).split('[object ')[1].split(']')[0])//Array
console.log(Object.prototype.toString.call(obj).split('[object ')[1].split(']')[0])//Object
console.log(Object.prototype.toString.call(foo).split('[object ')[1].split(']')[0])//Function
console.log(Object.prototype.toString.call(nul).split('[object ')[1].split(']')[0])//Null
```

#### 8. 强制类型转换与隐式类型转换

强制：parseInt()  parseFloat()  Number()

隐式：== - * / + <= >=

#### 9. NaN、undefined、null区别

未定义的值和定义未赋值的为undefined，null是一种特殊的object,NaN是一种特殊的number。

undefined与null是相等；NaN与任何值都不相等，与自己也不相等

值类型的“虚无”用undefined，引用类型的“虚无”，用null。

什么情况下会返回null:

1. document.getElementById(‘XXX’); 寻找一个不存在的元素，返回null

什么情况下会返回undefined:

1. 直接访问没有修改的全局变量undefined，var x = undefined 那x的值为undefined

2. 使用没有声明的变量，在IE下出错，提示”xxx”未定义；已经声明，没有赋值，类似var x; alert(x); 弹出undefined

3. 使用了一个不存在的对象的属性

#### 10. JS的_proto_、prototype、constructor

- 我们牢记两点：
  1. _proto_和constructor属性是对象独有的；
  2. prototype属性是函数独有的，因为函数也是一种对象，所以函数也拥有_proto_和constructor属性
- _proto_属性的作用就是当访问一个对象属性时，如果该对象内部不存在这个属性，那么就会去他的_proto_属性所指向的那个对象(父对象)里找，一直找，直到_proto_属性的终点null，再往上找就相当于在null上取值，就会报错。通过_proto_属性将对象连接起来的这条链路就是我们所谓的原型链
- prototype属性的作用就是让该函数所实例化的对象们都可以找到公用的属性和方法，即f1._proto_ === Foo.prototype
- constructor属性的含义就是指向该函数的构造函数，所有函数(此时看成对象了)最终的构造函数都指向Function。

#### 11. 闭包

闭包有3个特性：

1. 函数嵌套函数

2. 函数内部可以引用函数外部的参数和变量

3. 参数和变量不会被垃圾回收机制回收

好处

1. 保护函数内的变量安全 ，实现封装，防止变量流入其他环境发生命名冲突

2. 在内存中维持一个变量，可以做缓存（但使用多了同时也是一项缺点，消耗内存）

3. 匿名自执行函数可以减少内存消耗

坏处

1. 其中一点上面已经有体现了，就是被引用的私有变量不能被销毁，增大了内存消耗，造成内存泄漏，解决方法是可以在使用完变量后手动为它赋值为null；

2. 其次由于闭包涉及跨域访问，所以会导致性能损失，我们可以通过把跨作用域变量存储在局部变量中，然后直接访问局部变量，来减轻对执行速度的影响

#### 12. 垃圾回收机制

我们每创建一个对象，字符串都会占用一部分内存，用过之后很多都不会再用了，所以会造成内存泄漏，在c，c++都是手动释放，幸运的是我们js有自动管理。

工作原理:

当变量进入环境时(例如在函数中声明一个变量)，将这个变量标记为“进入环境”，当变量离开环境时，则将其标记为“离开环境”。标记“离开环境”的就回收内存

#### 13. 如何避免内存泄漏

减少不必要的全局变量，使用严格模式避免意外创建全局变量。

在你使用完数据后，及时解除引用(闭包中的变量，dom引用，定时器清除)。

组织好你的逻辑，避免死循环等造成浏览器卡顿，崩溃的问题。

但是使用闭包也会造成内存泄漏，所以当我们不用的时候可以手动释放。

减少console.log()

#### 14. bind、call和apply的区别

call和apply:

改变this指向，call传参是一个一个传，apply传参是一个数组，且是直接执行函数

但是第二个参数都可以传arguments

bind:

bind()和call与apply不同。bind是新创建一个函数，然后把它的上下文绑定到bind()括号中的参数上，然后将它返回。

所以，bind后函数不会执行，而只是返回一个改变了上下文的函数副本，而call和apply是直接执行函数。

**在IE6~IE8中创建一个bind方法：**

```
var button = document.getElementById("button"),
    text = document.getElementById("text");
button.onclick = function() {
    alert(this.id); // 弹出text
}.bind(text);


if (!function() {}.bind) {
    Function.prototype.bind = function(context) {
        let _this = this;
        let args = Array.prototype.slice.call(arguments);
            
        return function() {
            return _this.apply(context, args.slice(1));    
        }
    };
}
```

首先，我们判断是否存在bind方法，然后，若不存在，向Function对象的原型中添加自定义的bind方法。

这里面var _this = this这段代码让我很困扰，按理说，prototype是一个对象，对象的this应该指向对象本身，也就是prototype，但真的是这样吗。看看下面的代码：

```
function a(){};

a.prototype.testThis = function(){console.log(a.prototype == this);};

var b = new a();

b.testThis();//false

```

显然，this不指向prototype，而经过测试，它也不指向a，而指向b。所以原型中的this值就明朗了。指向调用它的对象。

Array.prototype.slice.call(arguments);

接下来就是上面这段代码，它会将一个类数组形式的变量转化为真正的数组。为啥呢，其实书上并没有说slice还有这样的用法，也不知道是谁发明的。slice的用法可以顺便上网查一下，就能查到。但要更正一点，网上的介绍说slice有两个参数，第一个参数不能省略。然而我不知道是我理解的问题还是咋地，上面这段代码tmd不就是典型的没传参数吗！！！arguments是传给call的那个上下文，前面讲过，不要弄混(由于arguments自己没有slice方法，这里属于借用Array原型的slice方法)。而且经过测试，若果你不给slice传参数，那就等于传了个0给它，结果就是返回一个和原来数组一模一样的副本。

这之后的代码就很好理解，返回一个函数，该函数把传给bind的第一个参数当做执行上下文，由于args已经是一个数组，排除第一项，将之后的部分作为第二部分参数传给apply，前面讲过apply的用法。

如此，我们自己的这个bind函数的行为就同es5中的bind一样了。

##### 实现call和apply

参考:https://www.cnblogs.com/echolun/p/12144344.html

#### 15. 数据类型、数据复制

- 基本数据类型：（栈内存）
1. Number
2. Boolean
3. undefined
4. String
5. null
6. symbol标记对象属性名称
7. bigInt表示一个任意精度的整数
 
- 引用数据类型：（堆内存）
1. Object（Function、Array、Date。。。）

- 对象的克隆：

1. 克隆函数：

bind()方法，在引用函数后加上.bind()即可。

```
var a = function(){};  

var b = a.bind();  a===b //fanlse
```

2. 克隆对象：循环遍历

更好的办法是对象序列化：

```
var obj = {
    a:1,
    b:2
};  

var newObj = JSON.parse(JSON.stringify(obj)); 
```

**注意： 无法实现对象中方法的深拷贝**

```
JSON.stringify({name:function(){}, age:12});
//"{"age":12}"
```

3. 克隆dom元素：

使用cloneNode()。  

```
let div = document.getElementById('box');   

let box2 = div.cloneNode(true);
```

4. ES6方法：Object.assign。   

**注意： 当对象只有一级属性为深拷贝； 当对象中有多级属性时，二级属性后就是浅拷贝**

``` 
var obj = {
    a:1,
    b:2
}; 

var newObj = Object.assign({}, obj); 
```

##### 5. 递归方式:

```
function deepClone(obj){
　　let objClone =  Array.isArray(obj) ? [] : {};
　　if (obj && typeof obj === 'object') {
　　　　for(let key in obj){
　　　　　　if (obj[key] && typeof obj[key] === 'object'){
　　　　　　　　objClone[key] = deepClone(obj[key]);
　　　　　　}else{
　　　　　　　　objClone[key] = obj[key]
　　　　　　}
　　　　}
　　}
　　return objClone;
}
```

参考:https://www.cnblogs.com/hyns/p/12405328.html

#### 16. 构造函数

- 特点：
1. 构造函数的首字母必须大写，用来区分于普通函数
2. 内部使用的this对象，来指向即将要生成的实例对象
3. 使用New来生成实例对象
	
- 缺点：
1. 所有实例都会通过原型链引用到prototype
2. 同一个对象实例之间，无法共享属性
3. prototype相当于特定类型所有实例都可以访问到的一个公共容器
4. 那么我们就将重复的东西放到公共容易就好了

#### 17. localStorage、sessionStorage和cookie、session

特性 | cookie | SessionStorage | localStorage
---|---|---|---
数据生命周期 | 生成时会被指定一个maxAge，这就是cookie的生命周期，在这个周期内cookie有效，默认关闭浏览器失效。| 页面会话期间可用 | 除非主动清除，否则一直存在
存放数据大小 | 4K左右（因为每次HTTP请求都会携带cookie） | 一般5M或更大 | 同左←
与服务器通信 | 由对服务器的请求来传递，每次都会携带在http头中，如果使用cookie保存过多数据会带来性能问题 | 数据不是由每个服务器请求传递的，而是只有在请求时使用数据，不参与和服务器的通信 | 同左←
易用性 | cookie需要自己封装setCookie、getCookie | 可以用原生接口，也可以再次封装来对Object和Array有更好的支持 | 同左←
共同点 | 都是保存在浏览器端，和服务器的session机制不同 | 同左← | 同左←

#### 18. js监听对象 Object.defineProperty()

Object.defineProperty(obj, prop, descriptor) 来劫持对象属性的 geter 和 seter 操作，当数据发生改变发出通知

obj:要操作的对象，也就是 target。

prop:要新增或者修改的属性名称（可以是 Symbol）。

descriptor:
```
interface Descriptor {
    configurable?: boolean;//指的是该属性是否可以进行配置，也就是说只有该选项为 true，属性值才能被改变或者删除。该属性默认为 false
    enumerable?: boolean;//该属性译为「数不清的」，因此我们能猜到，该属性和枚举相关。当它为 true 时，该属性才会出现在对象的枚举属性中。默认为 false。
    value?: any;//不多解释，可以为任何有效的 JavaScript 数据，是该属性对应的值。默认为 undefined
    writable?: boolean;//描述该属性是否可以修改，与 configurable 不同的在于，即使 writable 设置为 false，该属性也可以做出删除或其他修改以外的操作。该选项只对赋值运算符起作用。默认为 false
    get?: () => any;//配置了该属性的 getter 函数。每当我们访问这个属性，就会调用 get。比如 console.log(obj.a)。该函数配置的返回值就将会是访问获得的值。该函数不接受任何参数，但是存在 this 对象（但是不确定会指向谁）。默认值为 undefined
    set?: (value: any) => void;//配置该函数的 setter 函数。当属性值被修改时，就会调用该函数。函数接收一个参数，该参数就是即将修改的新值。同样，该函数也存在 this。默认值为 undefined
}
```

优点：

- 兼容性好,支持IE9

缺点：

- 无法检测到对象属性的新增或删除

- 无法监听数组变化 无法监听push pop shift unshift sort reverse splice，需要在Array.prototype上添加方法，生成新的数组赋给数据这样就会触发setter

对比ES6的Proxy：

- 可以直接监听对象而非属性
- 可以直接监听数组的变化
- 拦截方式较多
- Proxy返回一个新对象，可以只操作新对象达到目的，而Object.defineProperty只能遍历对象属性直接修改
- Proxy作为新标准将受到浏览器厂商重点持续的性能优化

#### 19. 实现instanceof

思路:右边的值的prototype是否在左边的的原型链上

1. 取constructor的原型（constructor.prototype）
2. 在原型链上取obj的原型（obj = obj._ _ proto _ ）
3. 循环比较是否相等：
（1）相等，则是该类型的实例，返回true
（2）不想等，则继续在原型链往上找该实例的原型 obj = obj. _ proto _ _
（3）找到原型链的尽头（obj === null），则不是该类型的实例，返回false

```
function instanceOf(obj,constructor){
	//取constructor的原型
	let prototype = constructor.prototype;
	//取obj的原型
	obj = obj.__proto__;
	while(true){
		if(obj === prototype)
			return true;
		obj = obj.__proto__;
		if(obj === null)
			return false;
	}
}
```

typeof中:

```
console.log(`null-->${typeof null}`);               //object
console.log(`number-->${typeof 123}`);              //number
console.log(`boolean-->${typeof true}`);            //boolean
console.log(`undefined-->${typeof undefined}`);     //undefined
console.log(`string-->${typeof "HHH"}`);            //string
console.log(`function-->${typeof function () {}}`); //function
console.log(`object-->${typeof { name: 123 }}`);    //object
console.log(`array-->${typeof [1, 2, 3]}`);         //object
// console.log(`object-->${obj.constructor}`);
// console.log(`object-->${JSON.stringify(obj.__proto__)}`);
// console.log(`array-->${arr.constructor}`);
// console.log(`object-->${arr.__proto__}`);
// console.log(arr instanceof Array)
// console.log(obj instanceof Object)
// console.log(arr.__proto__.constructor==Array)
// console.log(`function-->${fun.constructor}`); 


```

#### 20. 函数的节流和防抖

概念：

函数防抖(debounce)：触发高频事件后n秒内函数只会执行一次，如果n秒内高频事件再次被触发，则重新计算时间。

函数节流(throttle)：高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率。

函数节流（throttle）与 函数防抖（debounce）都是为了限制函数的执行频次，以优化函数触发频率过高导致的响应速度跟不上触发频率，出现延迟，假死或卡顿的现象。

1. 函数防抖(debounce)

- 实现方式：每次触发事件时设置一个延迟调用方法，并且取消之前的延时调用方法
- 缺点：如果事件在规定的时间间隔内被不断的触发，则调用方法会被不断的延迟

```
//防抖debounce代码：
function debounce(fn,delay) {
    var timeout = null; // 创建一个标记用来存放定时器的返回值
    return function (e) {
        // 每当用户输入的时候把前一个 setTimeout clear 掉
        clearTimeout(timeout); 
        // 然后又创建一个新的 setTimeout, 这样就能保证interval 间隔内如果时间持续触发，就不会执行 fn 函数
        timeout = setTimeout(() => {
            fn.apply(this, arguments);
        }, delay);
    };
}
// 处理函数
function handle() {
    console.log('防抖：', Math.random());
}
        
//滚动事件
window.addEventListener('scroll', debounce(handle,500));
```

2. 函数节流(throttle)

- 实现方式：每次触发事件时，如果当前有等待执行的延时函数，则直接return

```

//节流throttle代码：
function throttle(fn,delay) {
    let canRun = true; // 通过闭包保存一个标记
    return function () {
         // 在函数开头判断标记是否为true，不为true则return
        if (!canRun) return;
         // 立即设置为false
        canRun = false;
        // 将外部传入的函数的执行放在setTimeout中
        setTimeout(() => { 
        // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环了。
        // 当定时器没有执行的时候标记永远是false，在开头被return掉
            fn.apply(this, arguments);
            canRun = true;
        }, delay);
    };
}
 
function sayHi(e) {
    console.log('节流：', e.target.innerWidth, e.target.innerHeight);
}
window.addEventListener('resize', throttle(sayHi,500));
```

#### 21. WebSocket心跳检测和重连机制

思路是：

1. 每隔一段指定的时间（计时器），向服务器发送一个数据，服务器收到数据后再发送给客户端，正常情况下客户端通过onmessage事件是能监听到服务器返回的数据的，说明请求正常。
2. 如果再这个指定时间内，客户端没有收到服务器端返回的响应消息，就判定连接断开了，使用websocket.close关闭连接。
3. 这个关闭连接的动作可以通过onclose事件监听到，因此在 onclose 事件内，我们可以调用reconnect事件进行重连操作。

```
$(function () {
    var path = basePath;
    var jspCode = $("#userId").val();
    var websocket;
    createWebSocket();

    /**
     * websocket启动
     */
    function createWebSocket() {
        try {
            if ('WebSocket' in window) {
                websocket = new WebSocket((path + "/wsCrm?jspCode=" + jspCode).replace("http", "ws").replace("https", "ws"));
            } else if ('MozWebSocket' in window) {
                websocket = new MozWebSocket(("ws://" + path + "/wsCrm?jspCode=" + jspCode).replace("http", "ws").replace("https", "ws"));
            } else {
                websocket = new SockJS(path + "/wsCrm/sockJs?jspCode=" + jspCode.replace("http", "ws"));
            }
            init();
        } catch (e) {
            console.log('catch' + e);
            reconnect();
        }

    }

    function init() {
        //连接成功建立的回调方法
        websocket.onopen = function (event) {
            console.log("WebSocket:已连接");
            //心跳检测重置
            heartCheck.reset().start();
        };

        //接收到消息的回调方法
        websocket.onmessage = function (event) {
            showNotify(event.data);
            console.log("WebSocket:收到一条消息", event.data);
            heartCheck.reset().start();
        };

        //连接发生错误的回调方法
        websocket.onerror = function (event) {
            console.log("WebSocket:发生错误");
            reconnect();
        };

        //连接关闭的回调方法
        websocket.onclose = function (event) {
            console.log("WebSocket:已关闭");
            heartCheck.reset();//心跳检测
            reconnect();
        };

        //监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常。
        window.onbeforeunload = function () {
            websocket.close();
        };

        //关闭连接
        function closeWebSocket() {
            websocket.close();
        }

        //发送消息
        function send(message) {
            websocket.send(message);
        }
    }

    //避免重复连接
    var lockReconnect = false, tt;

    /**
     * websocket重连
     */
    function reconnect() {
        if (lockReconnect) {
            return;
        }
        lockReconnect = true;
        tt && clearTimeout(tt);
        tt = setTimeout(function () {
            console.log('重连中...');
            lockReconnect = false;
            createWebSocket();
        }, 4000);
    }

    /**
     * websocket心跳检测
     */
    var heartCheck = {
        timeout: 5000,
        timeoutObj: null,
        serverTimeoutObj: null,
        reset: function () {
            clearTimeout(this.timeoutObj);
            clearTimeout(this.serverTimeoutObj);
            return this;
        },
        start: function () {
            var self = this;
            this.timeoutObj && clearTimeout(this.timeoutObj);
            this.serverTimeoutObj && clearTimeout(this.serverTimeoutObj);
            this.timeoutObj = setTimeout(function () {
                //这里发送一个心跳，后端收到后，返回一个心跳消息，
                //onmessage拿到返回的心跳就说明连接正常
                websocket.send("HeartBeat");
                console.log('ping');
                self.serverTimeoutObj = setTimeout(function () { // 如果超过一定时间还没重置，说明后端主动断开了
                    console.log('关闭服务');
                    websocket.close();//如果onclose会执行reconnect，我们执行 websocket.close()就行了.如果直接执行 reconnect 会触发onclose导致重连两次
                }, self.timeout)
            }, this.timeout)
        }
    };
});
```

#### 22. 图片懒加载的原理

待加载图片何时开始渲染:

offsetTop - scroolTop &lt; clientHeight

元素到顶部的距离 - 屏幕滚动距离 &lt; 屏幕高度


代码:

```
<body>
    <img data-src="./images/1.jpg" alt="">
    <img data-src="./images/2.jpg" alt="">
    <img data-src="./images/3.jpg" alt="">
    <img data-src="./images/4.jpg" alt="">
    <img data-src="./images/5.jpg" alt="">
    <img data-src="./images/6.jpg" alt="">
    <img data-src="./images/7.jpg" alt="">
    <img data-src="./images/8.jpg" alt="">
    <img data-src="./images/9.jpg" alt="">
    <img data-src="./images/10.jpg" alt="">
    <img data-src="./images/1.jpg" alt="">
    <img data-src="./images/2.jpg" alt="">
</body>

var imgs = document.querySelectorAll('img');

//offsetTop是元素与offsetParent的距离，循环获取直到页面顶部
function getTop(e) {
    var T = e.offsetTop;
    while(e = e.offsetParent) {
        T += e.offsetTop;
    }
    return T;
}

function lazyLoad(imgs) {
    var H = document.documentElement.clientHeight;//获取可视区域高度
    var S = document.documentElement.scrollTop || document.body.scrollTop;
    for (var i = 0; i < imgs.length; i++) {
        if (H + S > getTop(imgs[i])) {
            imgs[i].src = imgs[i].getAttribute('data-src');
        }
    }
}

window.onload = window.onscroll = function () { //onscroll()在滚动条滚动的时候触发
    lazyLoad(imgs);
}
```

注意：offsetTop是相对于父元素的，所以上面代码有一个offsetParent。

#### 23. DOM事件流事件捕获事件冒泡

事件类型:

- UI（User Interface用户界面）事件，当用户与页面上的元素交互时触发
- 焦点事件，当元素获得或失去焦点是触发
- 鼠标事件，当用户通过鼠标在页面上执行操作时触发
- 滚轮事件，当使用鼠标滚轮（或类似设备）时触发
- 文本事件，当在文档中输入文本时触发
- 键盘事件，当用户通过键盘在页面上执行操作时触发
- 合成事件，当为IME（Input Method Edtor，输入法编辑器）输入字符时触发
- 变动（mutation）事件，当底层DOM结构发生变化时触发

有三种绑定事件的方式：

1. 直接在dom元素上指定相应的事件，例如鼠标单击事件：

```
<div onclick="alert('我是dom元素中的click')">点我</div>
```

2. 通过JavaScript获取到元素对象，然后通过对象属性的方式绑定事件

```
<div id='clickMe'>点我</div>
<script>
    var div = document.querySelector('#clickMe')
    div.onclick=function(){
        alert('我是click')
    }
</scripit>
```

3. 通过事件侦听器addEventListener(）来绑定事件

```
<div id='clickMe'>点我</div>
<script>
    var div = document.querySelector('#clickMe')
    //这里需要注意的是，用事件侦听器绑定事件是没有on的
    div.addEventListener('click',function(){
        alert('我是侦听器中的click');
    })
</scripit>
```

事件流:

事件流描述的是从页面中接收事件的顺序。事件发生时会在元素节点与根节点之间按照特定的顺序传播，路径所经过的所有节点都会收到该事件，这个传播过程即DOM事件流.

事件流的理解:

假如现在在页面上有三个div盒子组成的同心圆（如下图所示），然后我们给每个圆都绑定一个鼠标点击（click）事件。当我们点击最内侧的粉色小圆时会触发小圆的click事件，然后事件会从内到外依次向上传播直到到达document而停止。所以会依次弹出“我是粉圆”、“我是黄圆”和“我是绿圆”三个弹出框，那么如果我们给body、html和document也绑定了click事件，那么这几个元素的click事件也会被依次触发。这样就形成了一个事件流。

![avatar](https://img-blog.csdnimg.cn/20200907145548325.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpeGlhb3Nlbmxpbg==,size_16,color_FFFFFF,t_70)

实际上DOM事件流有三个阶段：

- 事件捕获阶段：捕获阶段是从外到内传播即从根节点向最内侧节点传播，与我们上面例子中讲到的刚好相反
- 事件目标阶段：目标阶段就是事件到达我们具体点击的那个元素时的阶段，在上面的案例中就是粉圆
- 事件冒泡阶段：冒泡阶段就是我们上面案例中将的一样，从内向外传播直到根节点结束

那么为什么会有捕获阶段（从外到内）和冒泡阶段（从内到外）两种事件流呢？那么是不是每次事件被触发都要经过捕获阶段和冒泡阶段，从而导致一个事件被执行两次呢（答案显然不是这样的）？

这是因为，当时IE和Netscape团队都提出了事件流的概念，但是两个团队提出的事件流的方向却是完全相反的。IE的事件流是事件的冒泡流即从内到外，而Netscape提出的是事件的捕获流即从外到内。所以会出现捕获流和冒泡流两个阶段。而实际在我们触发一个事件后对应的代码只会执行一次，这是因为默认情况下只会经过事件冒泡流传播，而不会触发捕获流，所以在触发事件后对应的代码只会执行一次，然后依次向上冒泡。

##### 事件冒泡阶段

IE的s事件流叫做事件冒泡（event bubbling），即事件开始时有最具体的元素（文档中嵌套层次最深的那个节点）接收，然后逐级向上传播到较为不具体的节点（文档）。以下面的代码为例：

```
<!DOCTYPE html>
<html>
<head>
    <title>Event Bubbling Example</title>
</head>
<body>
    <div id="myDiv">Click Me></div>
</body>
</html>
```

如果我们点击了页面中的div元素，那么这个click事件会按照如下顺序传播：

1. &lt;div&gt;
2. &lt;body&gt;
3. &lt;html&gt;
4. document

也就是说，click事件首先在div元素上发生，而这个元素就是我们单击的元素，然后click事件沿着DOM树向上传播，在每一级节点上都会发生，直至传播到document对象。下图展示了事件冒泡的过程。

![avatar](https://img-blog.csdnimg.cn/20200907174642225.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpeGlhb3Nlbmxpbg==,size_16,color_FFFFFF,t_70)

所有浏览器都支持事件冒泡，但在具体实现上还是有一些差别。IE5.5及更早版本中的事件冒泡会跳过html元素，从body直接到document，IE9、Firefox、Chrome和Safari则将事件一直冒泡到window对象。

##### 事件捕获阶段

Netscape团队提出的另一种事件流叫做事件捕获（event capturing）。事件捕获的思想是不太具体的节点应该更早接收事件，而最具体的节点应该最后接收到事件。事件捕获的用意在于在事件到达预订目标之前捕获它。如果仍以前面的HTML代码为例，那么点击div元素就会按下列顺序触发click事件。

1. documnet
2. &lt;html&gt;
3. &lt;body&gt;
4. &lt;div&gt;

在事件捕获过程中，document对象首先接收到click事件，然后事件沿着DOM树依次向下，一直传播到事件的实际目标，即div元素。下图展示了事件捕获过程

![avatat](https://img-blog.csdnimg.cn/20200907181057904.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpeGlhb3Nlbmxpbg==,size_16,color_FFFFFF,t_70)

虽然事件捕获是Netscape唯一支持的事件流模型，但IE9、Safari、Chrome、Opera和Firefox目前也都支持这种事件流模型。尽管“DOM2级事件”规范要求事件应该从document对象开始传播，但这些浏览器都是从windows对象开始传播捕获事件的。由于老版本浏览器不支持，因此很少有人使用事件捕获。我们可以放心使用事件冒泡。再有特殊需要时再使用事件捕获。

虽然现在主流的浏览器都支持事件捕获流模型，但是我们上面也提到过，默认情况下都是以事件冒泡流的模型进行事件传播。那么如果我们就是想要让 事件以捕获流模型传播应该怎么办呢？这个时候我们就必须要用事件侦听器（addEventListener）的形式来绑定事件了，事件侦听器addEventListener接收三个参数：第一个是要绑定的事件类型、第二个是当事件被触发时要执行的代码、第三个参数是一个Boolean类型的可选参数（默认是false），这个参数就是决定事件流是以捕获的形式还是以冒泡的形式传播。true表示以捕获流模式传播，false表示以冒泡流模式传播，默认为false即默认以冒泡流传播。看下面的代码：

```
<div id="clickMe"></div>
<script>
    var myDiv = document.querySelector("#clickMe");
    myDiv.addEventListener('click',function(){
        console.log('我会以捕获流模式传播');
    },true);
</script>
```

上面的代码中我们给div添加了单击事件，并传递了第三个参数true，这时事件流将以捕获流的模式进行传播。

处于目标阶段

上面我们提到，由于IE和Netscape两个团队提出了完全相反的事件流的概念，用谁弃谁都不合适，于是最终就采取了一个比较折中的办法就是：在事件流的三个阶段中，首先经过事件捕获阶段，然后是实际目标接收到事件即处于目标阶段，最后再经过事件冒泡阶段，最终事件流结束。以下是DOM事件流的完整图片：

![avatar](https://img-blog.csdnimg.cn/20200908090625234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpeGlhb3Nlbmxpbg==,size_16,color_FFFFFF,t_70)

那么这样就会引发另一个问题，既然捕获阶段和冒泡阶段都要经过，那么当我们触发一个事件时，同一段代码岂不是要执行两次？为了解决这一问题，最终规定默认情况下都是以事件冒泡流的模式进行事件传播（IE获胜）。如果想要一捕获模式传播那就需要用到我们上面介绍的方法用侦听器绑定事件并将第三个参数设置为true即可。

##### 阻止冒泡

无论事件是以冒泡模式传播还是以捕获模式传播，都会有一个问题，有时候我们不想让事件向上或向下传播，即我们触发哪个元素的事件，事件就停留在哪个元素而不进行传播。那就是接下来我们要说的阻止冒泡。

事件的冒泡是可以被阻止的，防止事件冒泡而带来不必要的错误和困扰。阻止冒泡的方法就是stopPropagation()，看如下示例代码：

```
<div id="clickMe">ClickMe</div>
<script>
    var div = document.querySelector('#clickMe');
    div.addEventListener('click', function(event){
        console.log('事件冒泡被阻止了');
        event.stopPropagation();
    });
</script>
```

上面的代码中，在事件处理函数中接收一个event参数，然后通过调用event.stopPropagation()方法就可以阻止冒泡了。

#### 24. 事件委托

通俗地来讲，就是把一个元素响应事件（click、keydown......）的函数委托到另一个元素；

一般来讲，会把一个或者一组元素的事件委托到它的父层或者更外层元素上，真正绑定事件的是外层元素，当事件响应到需要绑定的元素上时，会通过事件冒泡机制从而触发它的外层元素的绑定事件上，然后在外层元素上去执行函数。

委托的优点:

1. 减少内存消耗

试想一下，若果我们有一个列表，列表之中有大量的列表项，我们需要在点击列表项的时候响应一个事件；

```
<ul id="list">
    <li>item 1</li>
    <li>item 2</li>
    <li>item 3</li>
    ......
    <li>item n</li>
</ul>
// ...... 代表中间还有未知数个 li
```

如果给每个列表项一一都绑定一个函数，那对于内存消耗是非常大的，效率上需要消耗很多性能；

因此，比较好的方法就是把这个点击事件绑定到他的父层，也就是 `ul` 上，然后在执行事件的时候再去匹配判断目标元素；

所以事件委托可以减少大量的内存消耗，节约效率。

2. 动态绑定事件

比如上述的例子中列表项就几个，我们给每个列表项都绑定了事件；

在很多时候，我们需要通过 AJAX 或者用户操作动态的增加或者去除列表项元素，那么在每一次改变的时候都需要重新给新增的元素绑定事件，给即将删去的元素解绑事件；

如果用了事件委托就没有这种麻烦了，因为事件是绑定在父层的，和目标元素的增减是没有关系的，执行到目标元素是在真正响应执行事件函数的过程中去匹配的；

所以使用事件在动态绑定事件的情况下是可以减少很多重复工作的。

##### 实现功能

基本实现

```
<ul id="list">
  <li>item 1</li>
  <li>item 2</li>
  <li>item 3</li>
  ......
  <li>item n</li>
</ul>
// ...... 代表中间还有未知数个 li
```

我们来实现把 #list 下的 li 元素的事件代理委托到它的父层元素也就是 #list 上：

```
// 给父层元素绑定事件
document.getElementById('list').addEventListener('click', function (e) {
    // 兼容性处理
    var event = e || window.event;
    var target = event.target || event.srcElement;
    // 判断是否匹配目标元素
    if (target.nodeName.toLocaleLowerCase === 'li') {
        console.log('the content is: ', target.innerHTML);
    }
});
```

在上述代码中， target 元素则是在 #list 元素之下具体被点击的元素，然后通过判断 target 的一些属性（比如：nodeName，id 等等）可以更精确地匹配到某一类 #list li 元素之上；

##### 使用 Element.matches 精确匹配

如果改变下HTML变成:

```
<ul id="list">
  <li className="class-1">item 1</li>
  <li>item 2</li>
  <li className="class-1">item 3</li>
  ......
  <li>item n</li>
</ul>
// ...... 代表中间还有未知数个 li
```

这里，我们想把 #list 元素下的 li 元素（并且它的 class 为 class-1）的点击事件委托代理到 #list 之上；

如果通过上述的方法我们还需要在 `if (target.nodeName.toLocaleLowerCase === 'li')` 判断之中在加入一个判断 `target.nodeName.className === 'class-1'`；

但是如果想像 CSS 选择其般做更加灵活的匹配的话，上面的判断未免就太多了，并且很难做到灵活性，这里可以使用 Element.matches API 来匹配；

Element.matches API 的基本使用方法: Element.matches(selectorString)，selectorString 既是 CSS 那样的选择器规则，比如本例中可以使用 target.matches('li.class-1')，他会返回一个布尔值，如果 target 元素是标签 li 并且它的类是 class-1 ，那么就会返回 true，否则返回 false；

当然它的兼容性还有一些问题，需要 IE9 及以上的现代化浏览器版本；

我们可以使用 Polyfill 来解决兼容性上的问题：

```
if (!Element.prototype.matches) {
  Element.prototype.matches =
    Element.prototype.matchesSelector ||
    Element.prototype.mozMatchesSelector ||
    Element.prototype.msMatchesSelector ||
    Element.prototype.oMatchesSelector ||
    Element.prototype.webkitMatchesSelector ||
    function(s) {
      var matches = (this.document || this.ownerDocument).querySelectorAll(s),
        i = matches.length;
      while (--i >= 0 && matches.item(i) !== this) {}
      return i > -1;            
    };
}
```

加上 Element.matches 之后就可以来实现我们的需求了：

```
if (!Element.prototype.matches) {
  Element.prototype.matches =
    Element.prototype.matchesSelector ||
    Element.prototype.mozMatchesSelector ||
    Element.prototype.msMatchesSelector ||
    Element.prototype.oMatchesSelector ||
    Element.prototype.webkitMatchesSelector ||
    function(s) {
      var matches = (this.document || this.ownerDocument).querySelectorAll(s),
        i = matches.length;
      while (--i >= 0 && matches.item(i) !== this) {}
      return i > -1;            
    };
}
document.getElementById('list').addEventListener('click', function (e) {
  // 兼容性处理
  var event = e || window.event;
  var target = event.target || event.srcElement;
  if (target.matches('li.class-1')) {
    console.log('the content is: ', target.innerHTML);
  }
});
```

##### 局限性

当然，事件委托也是有一定局限性的；

比如 focus、blur 之类的事件本身没有事件冒泡机制，所以无法委托；

mousemove、mouseout 这样的事件，虽然有事件冒泡，但是只能不断通过位置去计算定位，对性能消耗高，因此也是不适合于事件委托的；

参考:https://zhuanlan.zhihu.com/p/26536815


#### 25. JS函数柯里化

##### 柯里化到底是什么

`维基百科上说道：柯里化，英语：Currying，是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。`

看这个解释有一点抽象，我们就拿被做了无数次示例的add函数，来做一个简单的实现。

```
// 普通的add函数
function add(x, y) {
    return x + y
}

// Currying后
function curryingAdd(x) {
    return function (y) {
        return x + y
    }
}

add(1, 2)           // 3
curryingAdd(1)(2)   // 3
```
实际上就是把add函数的x，y两个参数变成了先用一个函数接收x然后返回一个函数去处理y参数。现在思路应该就比较清晰了，就是只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。

##### Currying有哪些好处

1. 参数复用

```
// 正常正则验证字符串 reg.test(txt)

// 函数封装后
function check(reg, txt) {
    return reg.test(txt)
}

check(/\d+/g, 'test')       //false
check(/[a-z]+/g, 'test')    //true

// Currying后
function curryingCheck(reg) {
    return function(txt) {
        return reg.test(txt)
    }
}

var hasNumber = curryingCheck(/\d+/g)
var hasLetter = curryingCheck(/[a-z]+/g)

hasNumber('test1')      // true
hasNumber('testtest')   // false
hasLetter('21212')      // false
```

上面的示例是一个正则的校验，正常来说直接调用check函数就可以了，但是如果我有很多地方都要校验是否有数字，其实就是需要将第一个参数reg进行复用，这样别的地方就能够直接调用hasNumber，hasLetter等函数，让参数能够复用，调用起来也更方便。

2. 提前确认

```
var on = function(element, event, handler) {
    if (document.addEventListener) {
        if (element && event && handler) {
            element.addEventListener(event, handler, false);
        }
    } else {
        if (element && event && handler) {
            element.attachEvent('on' + event, handler);
        }
    }
}

var on = (function() {
    if (document.addEventListener) {
        return function(element, event, handler) {
            if (element && event && handler) {
                element.addEventListener(event, handler, false);
            }
        };
    } else {
        return function(element, event, handler) {
            if (element && event && handler) {
                element.attachEvent('on' + event, handler);
            }
        };
    }
})();

//换一种写法可能比较好理解一点，上面就是把isSupport这个参数给先确定下来了
var on = function(isSupport, element, event, handler) {
    isSupport = isSupport || document.addEventListener;
    if (isSupport) {
        return element.addEventListener(event, handler, false);
    } else {
        return element.attachEvent('on' + event, handler);
    }
}
```

我们在做项目的过程中，封装一些dom操作可以说再常见不过，上面第一种写法也是比较常见，但是我们看看第二种写法，它相对一第一种写法就是自执行然后返回一个新的函数，这样其实就是提前确定了会走哪一个方法，避免每次都进行判断。

3. 延迟执行

```
Function.prototype.bind = function (context) {
    var _this = this
    var args = Array.prototype.slice.call(arguments, 1)
 
    return function() {
        return _this.apply(context, args)
    }
}
```
像我们js中经常使用的bind，实现的机制就是Currying.

说了这几点好处之后，发现还有个问题，难道每次使用Currying都要对底层函数去做修改，

##### 通用的封装方法

这边首先是初步封装,通过闭包把初步参数给保存下来，然后通过获取剩下的arguments进行拼接，最后执行需要currying的函数。

```
// 支持多参数传递
function progressCurrying(fn, args) {

    var _this = this
    var len = fn.length;
    var args = args || [];

    return function() {
        var _args = Array.prototype.slice.call(arguments);
        Array.prototype.push.apply(args, _args);

        // 如果参数个数小于最初的fn.length，则递归调用，继续收集参数
        if (_args.length < len) {
            return progressCurrying.call(_this, fn, _args);
        }

        // 参数收集完毕，则执行fn
        return fn.apply(this, _args);
    }
}
```

这边其实是在初步的基础上，加上了递归的调用，只要参数个数小于最初的fn.length，就会继续执行递归。

##### Currying的性能

- 存取arguments对象通常要比存取命名参数要慢一点
- 一些老版本的浏览器在arguments.length的实现上是相当慢的
- 使用fn.apply(…)和fn.call(…)通常比直接调用fn(…) 稍微慢点
- 创建大量嵌套作用域和闭包函数会带来花销，无论是在内存还是速度上

其实在大部分应用中，主要的性能瓶颈是在操作DOM节点上，这js的性能损耗基本是可以忽略不计的，所以curry是可以直接放心的使用。

##### 一道经典面试题

```
// 实现一个add方法，使计算结果能够满足如下预期：
add(1)(2)(3) = 6;
add(1, 2, 3)(4) = 10;
add(1)(2)(3)(4)(5) = 15;

function add() {
    // 第一次执行时，定义一个数组专门用来存储所有的参数
    var _args = Array.prototype.slice.call(arguments);

    // 在内部声明一个函数，利用闭包的特性保存_args并收集所有的参数值
    var _adder = function() {
        _args.push(...arguments);
        return _adder;
    };

    // 利用toString隐式转换的特性，当最后执行时隐式转换，并计算最终的值返回
    _adder.toString = function () {
        return _args.reduce(function (a, b) {
            return a + b;
        });
    }
    return _adder;
}

add(1)(2)(3)                // 6
add(1, 2, 3)(4)             // 10
add(1)(2)(3)(4)(5)          // 15
add(2, 6)(1)                // 9
```

参考:https://www.jianshu.com/p/2975c25e4d71

#### 26. 实现一个new

new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。new 关键字会进行如下的操作：

1. 创建一个空的简单 JavaScript 对象（即{}）；

2. 链接该对象（即设置该对象的构造函数）到另一个对象 ；（ 通俗理解就是新对象隐式原型__proto__链接到构造函数显式原型prototype上。）

3. 将步骤 1 新创建的对象作为 this 的上下文 ；（ 实际是执行了构造函数 并将构造函数作用域指向新对象 ）

4. 如果该函数没有返回对象，则返回 this。（ 实际是返回一个空对象， new Object()就是返回一个空对象{} ）

```
function _new(constructor, ...arg) {
  var obj = {}; // 对应于上面的步骤 1
  obj.__proto__ = constructor.prototype; // 对应于上面的步骤 2

  var res = constructor.apply(obj, arg); // 对应于上面的步骤 3

  return Object.prototype.toString.call(res) === '[object Object]' ? res : obj; // 对应于上面的步骤 4
}

const Fun = function(name) {
  this.name = name;
};

console.log(_new(Fun, '小明'));

// Fun {name: "小明"}

//最后检验，与使用new的效果相同
//child instanceof Parent//true
//child.hasOwnProperty('name')//true
//child.hasOwnProperty('age')//true
//child.hasOwnProperty('sayName')//false
```
参考:https://www.cnblogs.com/echolun/p/10903290.html
参考:https://blog.csdn.net/qq_39953537/article/details/100524524

#### 27. 封装一个JSONP

```
function myCallback(data) {
    console.log(data)
}

function jsonp(url, data, callback) {
    if (typeof data == 'string') {
        callback = data
        data = {}
    }
    var hasParams = url.indexOf('?')
    url += hasParams ? '&' : '?' + 'callback=' + callback
    var params
    for (var i in data) {
        params += '&' + i + '=' + data[i]
    }
    url += params

    var script = document.createElement('script')
    script.setAttribute('src', url)
    document.querySelector('head').appendChild(script)
}

jsonp('http://baidu.com',{id:34},'myCallback')
```

#### 28. 用class如何实现继承？不用又如何实现？

##### 用 class 实现继承

```
class Animal{
    constructor(color){
        this.color = color
    }
    move(){}
}
class Dog extends Animal{
    constructor(color, name){
        super(color)
        this.name = name
    }
    say(){}
}
```

##### 不用 class 实现继承

```
function Animal(color){
    this.color = color
}
// 动物可以动
Animal.prototype.move = function(){} 

function Dog(color, name){
    // 或者 Animal.apply(this, arguments)
    Animal.call(this, color)      
    this.name = name
}
// 下面三行实现 Dog.prototype.__proto__ = Animal.prototype
function temp(){}
temp.prototye = Animal.prototype
Dog.prototype = new temp()

// 这行看不懂就算了，面试官也不问
Dog.prototype.constuctor = Dog 
Dog.prototype.say = function(){ console.log('汪')}

var dog = new Dog('黄色','阿黄')
```

参考:https://zhuanlan.zhihu.com/p/113263315

#### 29. 手写JS发布订阅模式

什么是eventHub

我们先看一段dom操作的代码：

```
const ele = document.getElementById('selector');

ele.addEventListener('click',() => console.log('click'))
// 可以订阅多个相同事件
ele.addEventListener('click',() => console.log('click2'))
// 可以取消订阅，这里必须要使用与订阅时相同的函数
ele.removeEventListener('click',handler)
```

上边的代码其实就是发布订阅模式，我们可以订阅DOM元素的一些事件，当用户执行相应的操作时会发布事件。当然我们也可以手动来取消对事件的订阅。

不仅仅在操作DOM的时候我们会用到发布订阅模式，在vue中我们使用自定义事件的时候也会应用到发布订阅模式：

```
<!--父组件-->
<my-component v-on:my-event="doSomething"></my-component>

// MyComponent组件
methods: {
  fn() {
    this.$emit('my-event',data)
  }
}
```

自己实现eventHub:

eventHub的api如下：

- eventHub.on()   // 订阅
- eventHub.emit() // 发布
- eventHub.off()  // 取消订阅

```
// 发布订阅模式
class EventEmitter {
    constructor() {
        // 事件对象，存放订阅的名字和事件
        this.events = {};
    }
    // 订阅事件的方法
    on(eventName,callback) {
       if (!this.events[eventName]) {
           // 注意时数据，一个名字可以订阅多个事件函数
           this.events[eventName] = [callback]
       } else  {
          // 存在则push到指定数组的尾部保存
           this.events[eventName].push(callback)
       }
    }
    // 触发事件的方法
    emit(eventName) {
        // 遍历执行所有订阅的事件
       this.events[eventName] && this.events[eventName].forEach(cb => cb());
    }
    // 移除订阅事件
    removeListener(eventName, callback) {
        if (this.events[eventName]) {
            this.events[eventName] = this.events[eventName].filter(cb => cb != callback)
        }
    }
    // 只执行一次订阅的事件，然后移除
    once(eventName,callback) {
        // 绑定的时fn, 执行的时候会触发fn函数
        let fn = () => {
           callback(); // fn函数中调用原有的callback
           this.removeListener(eventName,fn); // 删除fn, 再次执行的时候之后执行一次
        }
        this.on(eventName,fn)
    }
}
```

使用:

```
let em = new EventEmitter();
let workday = 0;
em.on("work", function() {
    workday++;
    console.log("work everyday");
});

em.once("love", function() {
    console.log("just love you");
});

function makeMoney() {
    console.log("make one million money");
}
em.on("money",makeMoney)；

let time = setInterval(() => {
    em.emit("work");
    em.removeListener("money",makeMoney);
    em.emit("money");
    em.emit("love");
    if (workday === 5) {
        console.log("have a rest")
        clearInterval(time);
    }
}, 1);

//console
work everyday
just love you
work everyday
work everyday
work everyday
work everyday
have a rest
```

参考:https://www.jianshu.com/p/e0575e17de2a

参考:https://zhuanlan.zhihu.com/p/81769898

参考:https://blog.csdn.net/qq_44818085/article/details/113836800;

#### 30. js实现斐波那契数列以及优化

首先解释下什么是斐波那契数列：

0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, ...

在种子数字 0 和 1 之后，后续的每一个数字都是前面两个数字之和。

解法1：
```
function fibonacci(n) {
    if(n==0 || n == 1)
        return n;
    return fibonacci(n-1) + fibonacci(n-2);
}
```
以上函数使用递归的方式进行斐波那契数列求和，我们可能首先想到用这个，使用递归计算大数字时，性能会非常低；

解法2：

上面递归执行太多相同运算，我们可以对中间求得的变量值，进行存储的话，就会大大减少函数被调用的次数。

```
let fibonacci = function() {
    let arr= [0, 1];
    return function(n) {
        let result = arr[n];
        if(typeof result != 'number') {
            result = fibonacci(n - 1) + fibonacci(n - 2);
            arr[n] = result; // 将每次 fibonacci(n) 的值都缓存下来
        }
        return result;
    }
}(); // 执行函数
```

解法3：递推法

```
function fibonacci(n) {
    let current = 0;
    let next = 1;
    let temp;
    for(let i = 0; i < n; i++) {
        temp = current;
        current = next;
        next += temp;
    }
    console.log(`fibonacci(${n}, ${next}, ${current + next})`);
    return current;
}
```

从下往上计算，首先根据f(0)和f(1)算出f(2),再根据f(1)和f(2)算出f(3)，依次类推我们就可以算出第n项了。比递归的效率高很多。

上述还可以借用解构赋值省略temp中间变量

```
function fibonacci(n) {
    let current = 0;
    let next = 1;
    for(let i = 0; i < n; i++) {
        [current, next] = [next, current + next];
    }
    return current;
}
```

解法4：尾调用优化

```
// 在ES6规范中，有一个尾调用优化，可以实现高效的尾递归方案。
// ES6的尾调用优化只在严格模式下开启，正常模式是无效的。
'use strict'
function fib(n, current = 0, next = 1) {
    if(n == 0) return 0;
    if(n == 1) return next; // return next
    return fib(n - 1, next, current + next);
}
```

解析：

##### 什么是尾调用？

尾调用（Tail Call）是函数式编程的一个重要概念，本身非常简单，一句话就能说清楚，就是指某个函数的最后一步是调用另一个函数。

```
function f(x){
  return g(x);
}
```

尾调用优化？

函数调用会在内存形成一个“调用记录”，又称“调用帧”（call frame），保存调用位置和内部变量等信息。如果在函数A的内部调用函数B，那么在A的调用帧上方，还会形成一个B的调用帧。等到B运行结束，将结果返回到A，B的调用帧才会消失。如果函数B内部还调用函数C，那就还有一个C的调用帧，以此类推。所有的调用帧，就形成一个“调用栈”（call stack）。

尾调用由于是函数的最后一步操作，所以不需要保留外层函数的调用帧，因为调用位置、内部变量等信息都不会再用到了，只要直接用内层函数的调用帧，取代外层函数的调用帧就可以了。

```
function g(item) {
  return item
}
// 下面是尾调用例子
function f() {
  let m = 1
  let n = 2
  return g(m + n)
}
f()
// 上面例子实际上等同于:
g(3)
```

上面代码中，如果函数g不是尾调用，函数f就需要保存内部变量m和n的值、g的调用位置等信息。但由于调用g之后，函数f就结束了，所以执行到最后一步，完全可以删除f(x)的调用帧，只保留g(3)的调用帧。

`这就叫做“尾调用优化”（Tail call optimization），即只保留内层函数(即g函数)的调用帧。如果所有函数都是尾调用，那么完全可以做到每次执行时，调用帧只有一项，这将大大节省内存。这就是“尾调用优化”的意义。`

尾递归是什么？

递归相信大家都听过，函数调用自身，称为递归，如果尾调用自身，就称为尾递归。

（递归非常耗费内存，因为需要同时保存成千上百个调用帧，很容易发生“栈溢出”错误（stack overflow）。但对于尾递归来说，由于只存在一个调用帧，所以永远不会发生“栈溢出”错误。）

```
function fib(n, current = 0, next = 1) {
    if(n == 0) return 0;
    if(n == 1) return next; // return next
    return fib(n - 1, next, current + next);
}
```

参考:https://blog.csdn.net/aliujiujiang/article/details/100145141


#### 31. for循环与for...in循环的区别

for循环我们通常用来循环一个数组、字符串

```
var array = [1,2,3,4,5,6];
var sum = 0;
for (var i=0; i<array.length; i++){
    sum+=array[i];
}
alert(sum);
```

for…in循环呢，我们通常用来循环一个对象，
```
var stu = {
    {name:"张三",
    sex:"男",
    age:13},
    {name:"李四",
    sex:"女",
    age:18},
    {name:"王五",
    sex:"男",
    age:10}
};

for(var i in stu){
    document.write(stu[i].name);
    document.write(stu[i].age);
}
```

往细节了说他们的区别，这里通过代码验证一下

for in 遍历的不是数组，而是array对象，它遍历访问的每个值其实是array的每个属性，而不是数组元素，比如：

```
var array = [1,2,3,4,5,6];
array[10] = 10;
for (var j in array){
    alert(typeof j);
    break;
}
```

输入j，j的值为String

同样的代码再来一遍

```
var array = [1,2,3,4,5,6];
for (var i=0; i<array.length; i++){
    alert(typeof  i);
    break;
}
```

输入i，i的值为Number

所以for in 和for 是有区别的。

并且，使用for in 的效率要远低于for循环




































