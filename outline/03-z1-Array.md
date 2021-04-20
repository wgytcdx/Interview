<!--
 * @Author: your name
 * @Date: 2021-01-05 10:37:33
 * @LastEditTime: 2021-03-30 17:07:04
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /Interview Files/03-01-Array.md
-->
#### 1. 添加

1. push() 从后面添加
   
2. unshift() 从前面添加

#### 2. 删除、移除

1. pop() 移除最后一位

2. shift() 移除第一位

#### 3. 截取

slice() 以数组的形式返回数组的一部分，注意不包括 end 对应的元素，如果省略 end 将复制 start 之后的所有元素

#### 4. 合并

concat() 将多个数组（也可以是字符串，或者是数组和字符串的混合）连接为一个数组，返回连接好的新的数组

#### 5. 复制、拷贝

1. arrayObj.slice(0) 返回数组的拷贝数组，注意是一个新的数组，不是指向

2. arrayObj.concat() 返回数组的拷贝数组，注意是一个新的数组，不是指向

#### 6. 排序

reverse(); //反转元素（最前的排到最后、最后的排到最前），返回数组地址

sort(); //对数组元素排序，返回数组地址

#### 7. 字符串化

arrayObj.join(separator) 返回字符串，这个字符串将数组的每一个元素值连接在一起，中间用 separator 隔开。

#### 8. Splice

- 删除：arrayObj.splice(Pos, Count) 删除从指定位置Pos开始的指定数量Count的元素，数组形式返回所移除的元素

- 添加：arrayObj.splice(insertPos,0,item1) 将一个或多个新元素插入到数组的指定位置，插入位置的元素自动后移，返回""。

- 替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，splice (2,1,4,6)会删除当前数组位置 2 的项，然后再从位置 2 开始插入4和6。

#### 9. 数组去重

1. 使用双重for循环，再利用数组的splice方法去重（ES5常用）
```
function unique1(arr) {
    for (let i = 0; i < arr.length; i++) {
        for (let j = i + 1; j < arr.length; j++) {
            if (arr[i] === arr[j]) {
                arr.splice(j, 1);
                j--;
            }
        }
    }
    return arr
}
```
2. 利用数组的indexOf方法去重
```
function unique2(arr) {
    let arr1 = [];
    for (let i = 0; i < arr.length; i++) {
        if (arr1.indexOf(arr[i]) == -1) {
            arr1.push(arr[i])
        }
    }
    return arr1
}
```
3. 利用数组的sort方法去重（相邻元素对比法）
```
function unique3(arr) {
    arr = arr.sort();
    let arr1 = [arr[0]];
    for (let i = 1; i < arr.length; i++) {
        if (arr[i] !== arr[i - 1]) {
            arr1.push(arr[i])
        }
    }
    return arr1;
}
```
4. 利用ES6中的Map方法去重
```
function unique8(arr) {
    let map = new Map();
    let arr1 = [];
    for (let i = 0; i < arr.length; i++) {
        if (map.has(arr[i])) {
            map.set(arr[i], true)
        } else {
            map.set(arr[i], false);
            arr1.push(arr[i]);
        }
    }
    return arr1;
}
```
5. 利用ES6中的Set方法去重
```
function unique(arr) {
    return Array.from(new Set(arr))
}
console.log('使用ES6方法去重：' + unique(JSON.parse(JSON.stringify(a))));
```

#### 10. 冒泡排序

```
let b = [9, 1, 3, 5, 7, 2, 8, 4, 6, 0];
unction sort1(arr) {
    for (let i = 0; i < arr.length - 1; i++) {
        for (let j = 0; j < arr.length - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                let tmp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = tmp;
            }
        }
    }
    return arr
}
console.log('使用冒泡排序：' + sort1(JSON.parse(JSON.stringify(b))));
```

#### 11. 伪数组转换

let array  = Array.prototype.slice.call(fakeArray);

#### 12. 数组扁平化的6种方式

```
var arr = [1,2,[3,4,5,[6,7,8],9],10,[11,12]];
```

1. 递归实现

```
function fn(arr){　　　　
	 let arr1 = []
	 
     arr.forEach((val)=>{
         if(val instanceof Array){
             arr1 = arr1.concat(fn(val))
         }else{
             arr1.push(val)
         }
      })
      
      return arr1
 }
```

2. 递归+reduce

```
function fn(arr){
    return arr.reduce((prev,cur)=>{
        return prev.concat(Array.isArray(cur)?fn(cur):cur)
    },[])
}
```

3. ES6 flat方式

flat(n)

如果不传递参数n的话，flat函数默认只展开一层；n的值为多少就展开多少层；如果想无论多少层都展开为一维数组，则将参数值设为Infinity。flat函数在拍平过程中还会自动跳过数组中的空位。

```
arr.flat(Infinity)
```

4. 扩展运算符

一层一层展开

```
function fn(arr){
    let arr1 = [];
    let bStop = true;
    arr.forEach((val)=>{
        if(Array.isArray(val)){
            arr1.push(...val);
            bStop = false
        }else{
            arr1.push(val)
        }
    })
    if(bStop){
        return arr1;
    }
    return fn(arr1)
}
```

5. toString

```
let arr1 = arr.toString().split(',').map((val)=>{
            return parseInt(val)
        })
console.log(arr1)
```

6. apply

```
function flatten(arr){
     while(arr.some(item => Array.isArray(item))){
           arr =  [].concat.apply([],arr);
     }
      return arr;
}
```


