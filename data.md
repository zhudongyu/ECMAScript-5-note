[返回目录](./README.md)

#### 数据类型的意义、本质、值

> 一、JavaScript是弱类型语言
 
(1).强类型：

    1.变量类型决定了数据存储的类型
    2.比如Java中， int a = 1,写成int a = "1",那么程序就会报错，因为声明的是数字类型却写成了字符串类型
(2).弱类型：

    1.在JavaScript中 var a = 1; var a = "1",两种写法都是可以的，在JS中一切都是对象，
    2.变量 a 的数据类型取决于它引用的值，


> 二、数据类型的作用是什么？JS中的数据类型有哪些？

(1).数据类型的作用

    1.为了让我们更加方便而好的分配和管理我们的内存，会根据不同的数据类型的值分配不同的内存大小。
    2.限制行为 不同数据类型的对象会有不同的属性和方法的操作

(2).JS中的数据类型

    1.基本数据类型 ：String Number Boolean undefined 

    2.特殊的基本数据类型：null
    `null --> false !!null --> false !null --> true`

    3.引用数据类型：只有一种 Object
      因为 `Array` 和 `function` 的值是 `Object` 的实例
      所以 `Array` 和 `function` 的值 也是引用类型
      运行下面代码来理解下
```JavaScript
Object.prototype.work = function(){}
function fn() {}
console.log(fn.work) // 输出：function (){} ; 
```
        
    值得注意：
```JavaScript
var a = []
console.log(typeof a) // Object 
function fn (){}
console.log(typeof fn) // Function ,这是个特殊的存在，但仍然要把它看做是对象
```

    4.检测数据类型的方法 `typeof` 



> 三、null 与 undefined 的区别

(1).相同点：
```javaScript
// 他们只有一个值，就是他们自身
null --> null
undefined --> undefined
```

(2).不同点：

    1.校验他们的数据类型的时候
```javaScript
typeof undefined // undefined
typeof null // Object
```
    2.他们自身所代表的的意义
```javaScript
// undefined: 代表的意思是缺少值
// 缺少内存地址
var a;
console.log(a) // undefined 缺少内存地址

// 函数的默认返回值
function fn () {
    console.log("1")
}
var result = fn();
console.log(result) // undefined 函数fn的默认返回值

// 访问不存在的对象属性
var obj = {}
console.log(obj.name) // undefined 

// 函数参数未传时，默认undefined
function fn(name,age){
    console.log(name) // ddy
    console.log(age)  // undefined
}
fn("ddy");
```

```javaScript
// null: 代表的意思是空对象
// 在内存的堆中是唯一的
var a = null;
var b = null;
console.log(a == b) // true

// 作为原型链的终点：参考后面的原型链
Object.prototype.__proto__  // null

```
               
