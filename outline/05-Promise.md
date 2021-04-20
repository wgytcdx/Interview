<!--
 * @Author: your name
 * @Date: 2021-01-04 16:34:54
 * @LastEditTime: 2021-04-20 10:24:22
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /Interview Files/大纲/05-Promise.md
-->
#### 1. 什么是Promise

1. 主要用于异步计算
2. 可以将异步操作队列化，按照期望的顺序执行，返回符合预期的结果
3. 可以在对象之间传递和操作promise，帮助我们处理队列

#### 2. 特点

1. promise是一个对象，对象和函数的区别就是对象可以保存状态，函数不可以（闭包除外）
2. 并未剥夺函数return的能力，因此无需层层传递callback，进行回调获取数据
3. 代码风格，容易理解，便于维护
4. 多个异步等待合并便于解决

```
new Promise(
  function (resolve, reject) {
    // 一段耗时的异步操作
    resolve('成功') // 数据处理完成
    // reject('失败') // 数据处理出错
  }
).then(
  (res) => {console.log(res)},  // 成功
  (err) => {console.log(err)} // 失败
)
```

- resolve作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；

- promise有三个状态：

1. pending[待定]初始状态
2. fulfilled[实现]操作成功
3. rejected[被否决]操作失败

当promise状态发生改变，就会触发then()里的响应函数处理后续步骤；

promise状态一经改变，不会再变。

- Promise对象的状态改变，只有两种可能：

从pending变为fulfilled

从pending变为rejected。

这两种情况只要发生，状态就凝固了，不会再变了。

#### 3. Promise.all

Promise.all([p1, p2, p3])用于将多个promise实例，包装成一个新的Promise实例，返回的实例就是普通的promise,

它接收一个数组作为参数,

数组里可以是Promise对象，也可以是别的值，只有Promise会等待状态改变,

当所有的子Promise都完成，该Promise完成，返回值是全部值得数组,

有任何一个失败，该Promise失败，返回值是第一个失败的子Promise结果


#### Promise.race() 类似于Promise.all() ，区别在于它有任意一个完成就算完成

#### 4. 实现Promise

前言:

Promise都不会陌生, JavaScript异步流程从最初的的CallBack, 到Promise, 到Generator, 再到目前使用最多的Async/Await, 这不仅仅是技术实现的发展, 更是思想上对于如何控制异步的递进. Promise作为后续方案的基础, 是重中之重, 也是面试最常问到的. 

今天我们就一起从0到1实现一个基于A+规范的Promise, 过程中也会对Promise的异常处理, 以及是否可手动终止做一些讨论, 最后会对我们实现的Promise做单元测试, 完整代码已经上传到Github. 

##### 1.基础框架

new Promise()时接受一个executor函数作为参数, 该函数会立即执行, 函数中有两个参数, 他们也是函数, 分别是resolve和reject, 函数同步执行一定要放在try...catch中, 否则无法进行错误捕获

**MyPromise**

```
function MyPromise(executor){
  function resolve(value){

  }
  function reject(reason){

  }
  try{
    executor(resolve, reject)
  } catch(reason){
    reject(reason)
  } 
}
```

resolve()接受Promise成功值value, reject()接受Promise的失败值reason

**test.js**

```
let promise = new MyPromise(function(resolve, reject){
  resolve(123)
})
```

##### 2. 添加状态机

目前存在的问题:

Promise是一个状态机的机制, 初始状态为`pending`, 成功状态为`fulfilled`, 失败状态为`rejected`.

只能从`pending`=&gt;`fulfilled`, 或者`pending`=&gt;`rejected`, 并且状态一旦转变, 就永远不会再变了

所以需要添加一个状态流转机制

**MyPromise**

```
function MyPromise(executor){
  let PENDING = 'pending',
      FULFILLED = 'fulfilled',
      REJECTED = 'rejected',
      _this = this;
  _this.status = PENDING;

  function resolve(value){
    if(_this.status == PENDING){
      _this.status = FULFILLED;
    }
  }
  function reject(reason){
    if(_this.status == PENDING){
      _this.status = REJECTED;
    }
  }
  try{
    executor(resolve, reject)
  } catch(reason){
    reject(reason)
  } 
}
```

**test.js**

```
let promise = new MyPromise(function(resolve, reject){
  resolve(123)
})

promise.then(function(value){
  console.log(value)
}, function(reason){
  console.log(reason)
})
```

##### 3. 添加then方法

Promise拥有一个`then`的方法, 接受两个函数`onFulfilled`和`onRejected`, 分别作为Promise成功和失败的回调. 所以在`then`方法中我们需要对状态status进行判断, 如果是`fulfilled`, 则执行`onFulfilled(value)`方法, 如果是`rejected`, 则执行`onRejected(reason)`方法.

由于成功值`value`和失败原因`reason`是由用户在`executor`中通过`resolve(value)`和`reject(reason)`传入的, 所以我们需要一个全局的`value`和`reason`供后续方法获取.

**MyPromise**

```
function MyPromise(executor){
  let PENDING = 'pending',
      FULFILLED = 'fulfilled',
      REJECTED = 'rejected',
      _this = this;
  _this.status = PENDING;
  _this.value = null;
  _this.reason = null;

  function resolve(value){
    if(_this.status == PENDING){
      _this.status = FULFILLED;
      _this.value = value
    }
  }
  function reject(reason){
    if(_this.status == PENDING){
      _this.status = REJECTED;
      _this.reason = reason;
    }
  }
  try{
    executor(resolve, reject)
  } catch(reason){
    reject(reason)
  } 
}

MyPromise.prototype.then = function(onFulfilled, onRejected){
  let _this = this;
  if(_this.status == 'fulfilled'){
    onFulfilled(_this.value)
  }
  if(_this.status == 'rejected'){
    onRejected(_this.reason)
  }
}
```

##### 4. 实现异步调用resolve

目前存在的问题

同步调用`resolve()`没有问题, 但如果是异步调用, 比如放到`setTimeout`中, 因为目前的代码在调用`then()`方法时, `status`仍然是`pending`的状态, 当timer到时候调用`resolve()`把status修改为`fulfilled`状态, 但是onFulfilled()函数已经没有时机调用了. 

针对上述问题, 进行修改

```
function MyPromise(executor){
  let PENDING = 'pending',
      FULFILLED = 'fulfilled',
      REJECTED = 'rejected',
      _this = this;
  _this.status = PENDING;
  _this.value = null;
  _this.reason = null;
  _this.onFulfilledCallbacks = [];
  _this.onRejectedCallbacks = [];

  function resolve(value){
    if(_this.status == PENDING){
      _this.status = FULFILLED;
      _this.value = value;
      _this.onFulfilledCallbacks.forEach(function(fulfilledCallback){
        fulfilledCallback();
      })
    }
  }
  function reject(reason){
    if(_this.status == PENDING){
      _this.status = REJECTED;
      _this.reason = reason;
      _this.onRejectedCallbacks.forEach(function(rejectedCallback){
        rejectedCallback();
      })
    }
  }
  try{
    executor(resolve, reject)
  } catch(reason){
    reject(reason)
  } 
}
MyPromise.prototype.then = function(onFulfilled, onRejected){
  let _this = this;
  if(_this.status == 'pending'){
    _this.onFulfilledCallbacks.push(()=>{
      onFulfilled(_this.value)
    })

    _this.onRejectCallbacks.push(()=>{
      onRejected(_this.reason)
    })
  }

  if(_this.status == 'fulfilled'){
    onFulfilled(_this.value)
  }
  if(_this.status == 'rejected'){
    onRejected(_this.reason)
  }
}
```
我们添加了两个回调函数数组, `onFulfilledCallbacks`和`onRejectedCallbacks`, 用来存储`then()`方法中传入的成功和失败的回调. 然后, 当用户调用`resolve()`或`reject()`的时候, 修改`status`状态, 并从响应的回调数组中依次取出回调函数并执行. 

同时, 通过这种方式我们也实现了可以注册多个`then()`函数, 并且成功或者失败时按照注册顺序依次执行.

**test.js**

```
let MyPromise = require('./MyPromise.js');

let promise = new MyPromise(function(resolve, reject) {
  setTimeout(function() {
    resolve(123);
  }, 1000);
});

promise.then(function(value) {
  console.log('value1', value);
}, function(reason) {
  console.log('reason1', reason);
});

promise.then(function(value) {
  console.log('value2', value);
}, function(reason) {
  console.log('reason2', reason);
});
```

##### 5. then返回的仍然是Promise

Promise的A+规范中, `then()`方法返回的仍是一个Promise, 并且返回的Promise的`resolve`的值是上一个Promise的`onFulfilled()`函数或`onRejected()`函数的返回值. 如果上一个Promise的`then()`方法回调函数的执行过程中发生了错误, 那么会将其捕获到, 并作为返回的Promise的`onRejected`函数的参数传入. 比如:

```
let promise = new Promise((resolve, reject)=>{
  resolve(123);
})

promise.then((value)=>{
  console.log(`value1 ${value}`);
  return 456;
}).then((value)=>{
  console.log(`value2 ${value}`)
})

let promise = new Promise((resolve, reject)=>{
  resolve(123)
})
```

打印结果为:

value1 123

value2 456

```
let promise = new Promise((resolve, reject)=>{
  resolve(123)
})

promise.then(value=>{
  console.log(`value1 ${value}`);
  a.b = 2;// 这里存在语法错误
  return 456;
}).then(value=>{
  console.log(`value2 ${value}`)
}, reason=>{
  console.log(`reason2 ${reason}`)
})
```

打印结果为

value1 123

reason2 ReferenceError: a is not defined

可以看到, `then()`方法回调函数如果发生错误, 会被捕获到, 那么`then()`返回的Promise会自动变为`onRejected`, 执行`onRejected()`回调函数. 

```
let promise = new Promise((resolve, reject)=>{
  reject(123)
})

promise.then(value=>{
  console.log(`value1 ${value}`);
  return 456;
}, reason=>{
  console.log(`reason1 ${reason}`);
  return 456;
}).then(value=>{
  console.log(`value2 ${value}`)
}, reason=>{
  console.log(`reason2 ${reason}`)
})
```

打印结果为:

reason1 123

value2 456

**下面我们就实现一个`then()`方法依然返回一个Promise**. 

**MyPromise**

```
MyPromise.prototype.then = function(onFulfilled, onRejected){
  let _this = this,
      promise2 = null;

  promise2 = new MyPromise((resolve, reject)=>{
    if(_this.status == 'pending'){
      _this.onFulfilledCallbacks.push(()=>{
        try{
          let x = onFulfilled(_this.value)
          _this.reslovePromise(promise2, x, reslove, reject);
        }catch(reason){
          reject(reason)
        }
      });
      _this.onRejectedCallbacks.push(()=>{
        try{
          let x = onRejected(_this.reason);
          _this.resolvePromise(promise2, x, resolve, reject);
        }catch(reason){
          reject(reason)
        }
      })
    }
    if(_this.status == 'fulfilled'){
      try{
        let x = onFulfilled(_this.value);
        _this.resolvePromise(promise2, x, resolve, reject);
      } catch(reason){
        reject(reason)
      }
    }
    if(_this.status == 'rejected'){
      try{
        let x = onRejected(_this.reason);
        _this.resolvePromise(promise2, x, resolve, reject);
      }catch(reason){
        reject(reason)
      }
    }
  })
  return promise2;
}
```

新增一个`promise2`作为`then()`方法的返回值. 通过`let x = onFulfilled(_this.value)`或者`let x = onRejected(_this.reason)`拿到`then()`方法回调函数的返回值, 然后调用`_this.resolvePromise(promise2, x, resolve, reject)`, 将新增的`promise2`, `x`, `promise2`的`resolve`和`reject`传入到`resolvePromise`中. 

**所以, 下面我们重点看一下`resolvePromise()方法`**

**MyPromise**

```
MyPromise.prototype.resolvePromise = function(promise2, x, resolve, reject){
  let _this = this;
  let called = false;//防止多次调用

  if(promise2 === x){
    return reject(new TypeError('循环引用'));
  }

  if(x !== null && (Object.prototype.toString.call(x) === '[object Object]' || Object.prototype.toString.call(x) === '[object Function]')){
    // 判断x类型
    try{
      let then = x.then;

      if(typeof then === 'function'){
        then.call(x, (y)=>{
          if(called)return;
          called = true;
          _this.resolvePromise(promise2, y, resolve, reject)
        }, reason=>{
          if(called) return;
          called = true;
          reject(reason)
        })
      } else {
        if(called) return;
        called = true;
        resolve(x);
      }
    }catch(reason){
      if(called) return;
      called = true;
      reject(reason)
    }
  } else {
    // x是普通值 返回resolve
    resolve(x)
  }
}
```

`resolvePromise()`是用来解析`then()`回调函数中返回的仍是一个`Promise`, 这个`Promise`有可能是我们自己的, 有可能是别的库来实现的, 也有可能是一个具有`then()`方法的对象, 所以这里靠`resolvePromise()`来实现统一处理. 

下面是翻译自`PromiseA+规范`关于`resolvePromise()`的要求

**Promise解决过程**

Promise的解决过程是一个抽象操作, 其需输入一个Promise和一个值, 我们表示为[ [Resolve] ](promise, x), 如果x有then方法且看上去像一个Promise, 解决程序即尝试使 promise 接受 x 的状态；否则其用 x 的值来执行 promise 。

这种 thenable 的特性使得 Promise 的实现更具有通用性：只要其暴露出一个遵循 Promise/A+ 协议的 then 方法即可；这同时也使遵循 Promise/A+ 规范的实现可以与那些不太规范但可用的实现能良好共存。

运行 [ [Resolve] ](promise, x) 需遵循以下步骤：

- x 与 promise 相等

如果 promise 和 x 指向同一对象，以 TypeError 为据因拒绝执行 promise

- x 为 Promise

如果 x 为 Promise ，则使 promise 接受 x 的状态:

```
- 如果 x 处于等待态， promise 需保持为等待态直至 x 被执行或拒绝
- 如果 x 处于执行态，用相同的值执行 promise
- 如果 x 处于拒绝态，用相同的据因拒绝 promise
```

- x 为对象或函数

如果 x 为对象或者函数：

```
- 把 x.then 赋值给 then
- 如果取 x.then 的值时抛出错误 e ，则以 e 为据因拒绝 promise
- 如果 then 是函数，将 x 作为函数的作用域 this 调用之。传递两个回调函数作为参数，第一个参数叫做 resolvePromise ，第二个参数叫做 rejectPromise:
    - 如果 resolvePromise 以值 y 为参数被调用，则运行 [[Resolve]](promise, y)
    - 如果 rejectPromise 以据因 r 为参数被调用，则以据因 r 拒绝 promise
    - 如果 resolvePromise 和 rejectPromise 均被调用，或者被同一参数调用了多次，则优先采用首次调用并忽略剩下的调用
    - 如果调用 then 方法抛出了异常 e：
        - 如果 resolvePromise 或 rejectPromise 已经被调用，则忽略之
        - 否则以 e 为据因拒绝 promise
    - 如果 then 不是函数，以 x 为参数执行 promise
- 如果 x 不为对象或者函数，以 x 为参数执行 promise
```

如果一个 promise 被一个循环的 thenable 链中的对象解决，而 [ [Resolve] ](promise, thenable) 的递归性质又使得其被再次调用，根据上述的算法将会陷入无限递归之中。算法虽不强制要求，但也鼓励施者检测这样的递归是否存在，若检测到存在则以一个可识别的 TypeError 为据因来拒绝 promise。

测试：

**test.js**

```
let promise = new MyPromise(function(resolve, reject) {
  setTimeout(function() {
    resolve(123);
  }, 1000);
});

promise.then((value) => {
  console.log('value1', value);
  return new MyPromise((resolve, reject) => {
    resolve(456);
  }).then((value) => {
    return new MyPromise((resolve, reject) => {
      resolve(789);
    })
  });
}, (reason) => {
  console.log('reason1', reason);
}).then((value) => {
  console.log('value2', value);
}, (reason) => {
  console.log('reason2', reason);
});
```

打印结果:

value1 123

value2 789

##### 6. 让`then()`方法的回调函数总是异步调用

官方`Promise`实现的回调函数总是异步调用的

```
console.log('start');

let promise = new Promise((resolve, reject)=>{
  console.log('step-');
  resolve(123);
});

promise.then(value=>{
  console.log('step--');
  console.log(`value1 ${value}`)
});

console.log('end')
```

打印结果:

start

step-

end

step--

value1 123

Promise属于微任务, 我们这里为了方便用宏任务 `setTimeout`来代替异步

**MyPromise**

```
MyPromise.prototype.then = function(onFulfilled, onRejected){
  let _this = this;
  let promise2 = null;
  promise2 = new MyPromise((resolce, reject)=>{
    if(_this.status == 'pending'){
      _this.onFulfilledCallbacks.push(()=>{
        setTimeout(()=>{
          try{
            let x = onFulfilled(_this.value);
            _this.resolvePromise(promise2, x, resolve, reject);
          } catch(reason){
            reject(reason)
          }
        }, 0)
      })
      _this.onRejectedCallbacks.push(()=>{
        setTimeout(()=>{
          try{
            let x = onRejected(_this.reason);
            _this.reslovePromise(promise2, x, resolve, reject)
          }catch(reason){
            reject(reason)
          }
        }, 0)
      })

      
    }
    if(_this.status == 'fulfilled'){
      setTimeout(()=>{
        try{
          let x = onFulfilled(_this.value);
          _this.resolvePromise(promise2, x, resolve, reject);
        }catch(reason){
          reject(reason)
        }
      }, 0)
    }
    if(_this.status == 'rejected'){
      setTimeout(()=>{
        try{
          let x = onRejected(_this.reason);
          _this.resolvePromise(promise2, x, resolve, reject);
        }catch(reason){
          reject(reason)
        }
      }, 0)
    }
  });
  return promise2
}
```

测试:

**test.js**

```
let MyPromise = require('./MyPromise.js');

console.log('start');

let promise = new MyPromise((resolve, reject) => {
  console.log('step-');
  setTimeout(() => {
    resolve(123);
  }, 1000);
});

promise.then((value) => {
  console.log('step--');
  console.log('value', value);
});

console.log('end');
```

打印结果：

start

step-

end

step--

value1 123

**经过以上步骤，一个最基本的Promise就已经实现完了，下面我们会实现一些不在PromiseA+规范的扩展方法**

##### 10. 实现Promise.all方法

`Promise.all()`接受一个包含多个`Promise`的数组, 当所有`Promise`均为`fulfilled`状态时, 返回一个结果数组, 数组中结果的顺序和传入的`Promise`顺序一一对应. 如果有一个`Promise`为`rejected`状态, 则整个`Promise.all`为`rejected`

**MyPromise**

```
MyPromise.all = function(promiseArr){
  return new MyPromise((resolve, reject)=>{
    let result = [];

    promiseArr.forEach((promise, index)=>{
      promise.then(value=>{
        result[index] = value;
        if(result.length === promiseArr.length){
          resolve(result);
        }
      }, reject)
    })
  })
}
```

**test**

```
let promise1 = new MyPromise((resolve, reject) => {
  console.log('aaaa');
  setTimeout(() => {
    resolve(1111);
    console.log(1111);
  }, 1000);
});

let promise2 = new MyPromise((resolve, reject) => {
  console.log('bbbb');
  setTimeout(() => {
    reject(2222);
    console.log(2222);
  }, 2000);
});

let promise3 = new MyPromise((resolve, reject) => {
  console.log('cccc');
  setTimeout(() => {
    resolve(3333);
    console.log(3333);
  }, 3000);
});

Promise.all([promise1, promise2, promise3]).then((value) => {
  console.log('all value', value);
}, (reason) => {
  console.log('all reason', reason);
})
```

打印结果：

aaaa

bbbb

cccc

1111

2222

all reason 2222

3333

https://www.jianshu.com/p/d39f9d3168df;

https://www.jianshu.com/p/327e38aec874;






































































