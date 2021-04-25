<!--
 * @Author: your name
 * @Date: 2021-01-04 16:32:19
 * @LastEditTime: 2021-04-25 10:19:57
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /Interview Files/大纲/07-Vue.md
-->
#### 1. 介绍Vue

Vue是构建用户界面的**渐进式框架**，他并没有完全遵循 MVVM 模型，但是 Vue 的设计也受到了它的启发。因此在文档中经常会使用 vm (ViewModel 的缩写) 这个变量名表示 Vue 实例。

主要功能：

- SPA应用(single page application也就是Vue-router)
- 双向数据绑定(数据监听 object.defineProperty、Proxy)
- MVVM模式

#### 2. Vue生命周期

1. beforeCreate 创建Vue实例
2. created 创建data
3. beforeMounted 创建$el
4. mounted $el加载到Dom上
5. beforeUpdate
6. Updated
7. beforeDestroy
8. destroyed
   
#### 3. Vue-Router传值

1. props
```
//组件定义
props:['id']

//路由配置 props：true
{
    path: '/user/:id',
    component: User,
    props: true
}   

// 跳转传参：
// 1.标签跳转
<router-link to="/user/1"></router-link>

// 2.函数跳转
// 直接调用$router.push 实现携带参数的跳转
this.$router.push({
    path: `/describe/${id}`,
})

//获取参数

<div>{{ id }}</div>
```
2. 配置url的?前面
```
//路由配置
{
    path: '/user/:id',
    component: User
},
//1.标签跳转
<router-link to="/user/1"></router-link>

// 2.函数跳转
// 直接调用$router.push 实现携带参数的跳转
this.$router.push({
    path: `/user/${id}`,
})

//获取参数：
<div>{{$route.params.id}}</div>

```
3. 配置url不显示参数
```
//路由配置
{
    path: '/user',
    component: User
},　　

// 1.标签跳转
<router-link :to="{name:'c', params:{id:1}}"></router-link>

// 2.函数跳转
// 直接调用$router.push 实现携带参数的跳转
this.$router.push({
    path: `/user`, params:{
　　　　　　　　id:id
　　　　　　}
})

// 获取参数：
<div>{{$route.params.id}}</div>
```
4. 配置url的?后面

```
//路由配置
{
    path: '/user',
    component: User
},　

// 1.标签跳转
<router-link :to="{name:'c', query:{id:1}}"></router-link>

// 2.函数跳转
// 直接调用$router.push 实现携带参数的跳转
this.$router.push({
    path: `/user`, query:{
　　　　　　　　id:id
　　　　　　}
})

//获取参数
<div>{{$route.query.id}}</div>
```

#### 4. Vue-Router钩子函数

全局:

1. router.beforeEach((to, from, next)=>{})
2. router.afterEach((to, from)=>{})

配置:

1. beforeEnter:(to, from, next)=>{}

组件:

1. beforeRouteEnter(to, from, next){}
2. beforeRouteUpdate(to, from, next){}
3. beforeRouteLeave(to, from, next){}

#### 5. 路由、组件懒加载

1. vue异步组件实现路由懒加载
```
component：resolve=>(['需要加载的路由的地址'，resolve])
```

2. es提出的import(推荐使用这种方式)
```
const HelloWorld = （）=>import('需要加载的模块地址')
```

#### 6. computed，watch的区别

computed是计算属性,类似于过滤器,对绑定到view的数据进行处理,有getter,setter属性,setter接受,getter返回

特性:

1. 是计算值，
2. 应用：就是简化tempalte里面{{}}计算和处理props或$emit的传值
3. 具有缓存性，页面重新渲染值不变化,计算属性会立即返回之前的计算结果，而不必再次执行函数

watch是监听,有newval, oldval两个参数,一个新值,一个旧值

特性:

1. 是观察的动作，
2. 应用：监听props，$emit或本组件的值执行异步操作
3. 无缓存性，页面重新渲染时值不变化也会执行

1. 简单数据监听

```
data(){
    return{
    'first':2
    }
},
watch:{
    first(){
    console.log(this.first)
    }
},
```

2. 复杂数据监听

```
data(){
    return{
    'first':{
        second:0
    }
    }
},
watch:{
    secondChange:{
    handler(oldVal,newVal){
        console.log(oldVal)
        console.log(newVal)
    },
    deep:true
    }
},
```

3. 监听对象的单个属性


方法一：可以直接对用对象.属性的方法拿到属性

```
data(){
    return{
        'first':{
            second:0
        }
    }
},
watch:{
    first.second:function(newVal,oldVal){
        console.log(newVal,oldVal);
    }
},
```

方法二：watch如果想要监听对象的单个属性的变化,必须用computed作为中间件转化,因为computed可以取到对应的属性值

```
data(){
    return{
        'first':{
            second:0
        }
    }
},
computed:{
    secondChange(){
        return this.first.second
    }
},
watch:{
    secondChange(){
        console.log('second属性值变化了')
    }
},
```


#### 7. Vue通讯方式

- 组件传值:

1. 父组件与子组件传值

父组件传给子组件：子组件通过props方法接受数据; 子组件传给父组件：$emit方法传递参数

2. 非父子组件间的数据传递，兄弟组件传值

eventBus，就是创建一个事件中心，相当于中转站，可以用它来传递事件和接收事件。项目比较小时，用这个比较合适。

- VueX传值
- Storage传值
- 路由传值
- 数据库传值
- window.name
- websocket

#### 8. computed

1. 使得数据处理结构清晰；
2. 依赖于数据，数据更新，处理结果自动更新；
3. 计算属性内部this指向vm实例；
4. 在template调用时，直接写计算属性名即可；
5. 常用的是getter方法，获取数据，也可以使用set方法改变数据；
6. 相较于methods，不管依赖的数据变不变，methods都会重新计算，但是依赖数据不变的时候computed从缓存中获取，不会重新计算。

#### 9. 对Vue.js的template编译的理解

首先，通过compile编译器把template编译成AST语法树（abstract syntax tree 即 源代码的抽象语法结构的树状表现形式），compile是createCompiler的返回值，createCompiler是用以创建编译器的。另外compile还负责合并option。

然后，AST会经过generate（将AST语法树转化成render funtion字符串的过程）得到render函数，render的返回值是VNode，VNode是Vue的虚拟DOM节点，里面有（标签名、子节点、文本等等）

#### 10. vuex

- State:存放状态
- Mutations:操作State
- Actions:异步操作State
- Getter:加工State传输(类似于computed)
- Models:模块化状态管理

#### 11. Vue3与Vue2的区别

Vue-cli3.0于 8月11日正式发布，看了下评论，兼容性不是很好，命令有不少变化，不是特别的乐观

vue3.0 的发布与 vue2.0 相比，优势主要体现在：更快、更小、更易维护、更易于原生、让开发者更轻松；

更快
 
1. virtual DOM 完全重写，mounting & patching 提速 100%；
2. 更多编译时 （compile-time）提醒以减少 runtime 开销；
3. 基于 Proxy 观察者机制以满足全语言覆盖以及更好的性能；
4. 放弃 Object.defineProperty ，使用更快的原生 Proxy；
5. 组件实例初始化速度提高 100%;
6. 提速一倍/内存使用降低一半；

更小

1. Tree-shaking 更友好；
2. 新的 core runtime：~ 10kb gzipped；

#### 12. mixin

可以理解成类似于注册了一个Vue的公共方法, 可以绑定在多个组件或多个Vue实例中使用. 另一点, 类似于在原型对象中注册方法, 实例对象即组件或者Vue实例对象中, 仍然可以定义相同函数名的方法进行覆盖, 有点像子类和父类的感觉. 

合并规则

1. 数据对象： 在内部会进行递归合并，并在发生冲突时以组件数据优先
2. 同名钩子函数： 合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子之前调用
3. 值为对象的选项：例如 methods、components 和 directives，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。

#### 13. Vue阻止事件冒泡

.stop
```
<div @click="test1()"> 
   <span @click.stop="test2()">按钮1</span> 
   <span>按钮2</span> 
</div>
```

#### 14. 为什么data必须是函数

因为对于组件有一个很明显的特性是在于它是可以被复用

那么注册了一个组件本质上就是创建了一个组件构造器的引用，而真正当我们使用组件的时候才会去将组件实例化

虽然data是在构造器的原型链上被创建的，但是实例化的component1和component2确是共享同样的data对象,当你修改一个属性的时候，data也会发生改变，这明显不是我们想要的效果。

当我们的data是一个函数的时候，每一个实例的data属性都是独立的，不会相互影响了。

参考:https://www.jianshu.com/p/f3e774c57356


#### 15. Vue响应式原理

参考:https://www.jianshu.com/p/4dff7c2cdaaa

#### 17. diff算法和时间复杂度

参考:https://blog.csdn.net/qq_40413670/article/details/107895096

#### 18. Vuex工作原理

参考:https://www.jianshu.com/p/d95a7b8afa06

#### 19. $nextTick

Vue 实现响应式**并不是数据发生变化之后 DOM 立即变化**，而是按一定的策略进行 DOM 的更新。

Vue事件循环说明:

简单来说，Vue 在修改数据后，视图不会立刻更新，而是等**同一事件循环**中的所有数据变化完成之后，再统一进行视图更新。

用法: 在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

使用`Vue.nextTick()`是为了可以获取更新后的DOM 。

触发时机：在同一事件循环中的数据变化后，DOM完成更新，立即执行`Vue.nextTick()`的回调。

同一事件循环中的代码执行完毕 =&gt; DOM 更新 =&gt; nextTick callback触发

应用场景：

1. 在Vue生命周期的`created()`钩子函数进行的DOM操作一定要放在`Vue.nextTick()`的回调函数中。

原因：是`created()`钩子函数执行时DOM其实并未进行渲染。

2. 在数据变化后要执行的某个操作，而这个操作需要使用随数据改变而改变的DOM结构的时候，这个操作应该放在`Vue.nextTick()`的回调函数中。

原因：Vue异步执行DOM更新，只要观察到数据变化，Vue将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变，如果同一个watcher被多次触发，只会被推入到队列中一次。


- 例: 点击按钮显示原本以 v-show = false 隐藏起来的输入框，并获取焦点。

```
showsou(){
  this.showit = true //修改 v-show
  document.getElementById("keywords").focus()  //在第一个 tick 里，获取不到输入框，自然也获取不到焦点
}
```
修改为：
```
showsou(){
  this.showit = true
  this.$nextTick(function () {
    // DOM 更新了
    document.getElementById("keywords").focus()
  })
}
```









