<!--
 * @Author: your name
 * @Date: 2021-01-04 16:33:08
 * @LastEditTime: 2021-03-08 11:04:33
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /Interview Files/大纲/04-ECMAScript.md
-->
#### 1. async/await

提供了在不阻塞主线程的情况下使用同步代码实现异步访问资源的能力, 使得代码逻辑更加清晰

**promise和async|await的区别？**

Promise是将回调写法标准化了. 实际上早期的JS并没有Promise对象, 是社区Promise规范出来后, 才被吸收到了ECMAScript中.

Promise的问题:

Promise没能解决以同步方式写异步代码的问题, 嵌套依然存在, 仍然会面临大量的then回调

Async和Await:

为了解决上述Promise的问题, await横空出世, 然鹅await是新语法, 引擎改动太大, 因此要求使用了await的函数前面, 必须增加async关键字

#### 2. Set

ES6中提供了Set数据容器，这是一个能够存储无重复值的有序列表。

- 创建Set

通过new Set()可以创建Set，然后通过add方法能够向Set中添加数据项：

```
//Set
let set= new Set();
set.add(1);
set.add('1');
console.log(set.size);//2       
```

**Set内部使用Object.is()方法来判断两个数据项是否相等，唯一不同的是+0和-0在Set中被判断为是相等的。**

同时可以使用数组来构造Set，或者说具有迭代器的对象都可以用来构造Set，并且Set构造器会确保不会存在重复的数据项：

```
let set = new Set([1,2,3,3,3,3]);
console.log(set.size);//3
```

- 检查某个值是否存在于Set中

可以使用has()方法来判断某个值是否存在于Set中：

```
let set = new Set([1,2,3,3,3,3]);
console.log(set.has(5)); //false
```

- 删除值

使用delete()方法从Set中删除某个值，或者使用clear()方法从Set中删除所有值：

```
let set = new Set([1,2,3,3,3,3]);
console.log(set.size);//3
console.log(set.has(5)); //false

set.delete(1);

console.log(set.has(1)); //false
console.log(set.size); //2
```

- forEach()方法

可以使用forEach方法来遍历Set中的数据项，该方法传入一个回调函数callback，还可以传入一个this，用于回调函数之中：

回调函数callback中有三个参数：

1. 元素值；
2. 元素索引；
3. 将要遍历的对象；

```
 let set = new Set([1,2,3,3,3,3]);
 set.forEach(function (value,key,ownerSet) {
     console.log(value);
     console.log(key);           
 })
```

Set中的value和key是相同的，这是为了让Set的forEach方法和数组以及Map的forEach方法保持一致，都具有三个参数。

在forEach方法中传入this，给回调函数使用：

```
let set = new Set([1,2,3,3,3,3]);
let operation ={

    print(value){
        console.log(value);
    },

    iterate(set=[]){
        set.forEach(function(value,key,ownerSet){
            this.print(value);
            this.print(key);
        },this);
    }

}

operation.iterate(set);

输出：1 1 2 2 3 3
```

如果回调函数使用箭头函数的话，就可以省略this的入参，这是因为箭头函数会通过作用域链找到当前this对象，将上面的示例代码使用箭头函数来写：

```
let set = new Set([1,2,3,3,3,3]);
let operation ={

    print(value){
        console.log(value);
    },

    iterate(set=[]){
        set.forEach((value,key)=>{
            this.print(value);
            this.print(key);
        })
    }

}

operation.iterate(set);
```

- 将Set转换成数组

将数组转换成Set十分容易，可以将数组传入Set构造器即可；而将Set转换成数组，需要使用扩展运算符。扩展运算符能将数组中的数据项切分开，作为独立项传入到函数，如果将扩展运算符用于可迭代对象的话，就可以将可迭代对象转换成数组：

```
let [...arr]=set;
console.log(arr); // [1,2,3]
```

- Weak Set

Set在存放对象时，实际上是存放的是对象的引用，即Set也被称之为Strong Set。如果所存储的对象被置为null，但是Set实例仍然存在的话，对象依然无法被垃圾回收器回收，从而无法释放内存：

```
let set = new Set();
let key={};
let key2 = {};
set.add(key);
set.add(key2);
console.log(set.size); //2

key=null;
console.log(set.size); //2
```

可以看出就算对象key置为null，但是由于是强引用的方式，Set实例还存在，对象key依然不会被回收. 

如果想让对象key正常释放的话，可以使用Weak Set，此时，**存放的是对象的弱引用，当对象只被Set弱引用的话，并不会阻止对象实例被回收。**Weka Set同Set的用法几乎一致。可以使用add()方法增加数据项，使用has()方法检查Weak Set中是否包含某项，以及使用delete()方法删除某一项。

```
let set = new WeakSet();
let key = {};   
set.add(key);
console.log(set.has(key)); //true
set.delete(key);
console.log(set.has(key)); //false
```

但需要注意的是：Weak Set构造器不接受基本类型数据，只接受对象。同样的可以使用可迭代的对象如数组，来作为构造器参数，来创建Weak Set。

- Weak Set和Set之间的差异

对于Weak Set和Set之间的重要差异：

1. 对于Weak Set实例，若调用了add()方法时传入了非对象的参数，则会抛出错误。如果在has()或者delete()方法中传入了非对象的参数则会返回false；
2. Weak Set不可迭代，因此不能用于for-of循环；
3. Weak Set 无法暴露出任何迭代器（例如 keys() 与 values() 方法） ，因此没有任何编程手段可用于判断 Weak Set 的内容；
4. Weak Set没有forEach()方法；
5. Weak Set没有size属性；


#### 3. Map






































