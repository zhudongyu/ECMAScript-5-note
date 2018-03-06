[返回目录](./README.md)

#### 数据类型的意义、本质、值
```JavaScript
(1).JavaScript是弱类型语言
    强类型：
        -- 变量类型决定了数据存储的类型
        -- 比如Java中， int a = 1,写成int a = "1",那么程序就会报错，因为声明的是数字类型却写成了字符串类型
    弱类型：
        -- 在JavaScript中 var a = 1; var a = "1",两种写法都是可以的，在JS中一切都是对象，
           变量 a 的数据类型取决于它引用的值，

(2).数据类型的作用是什么？
    -- 为了让我们更加方便而好的分配和管理我们的内存，会根据不同的数据类型的值分配不同的内存大小。
    -- 限制行为 不同数据类型的对象会有不同的属性和方法的操作
(3).对象的数据类型有哪些？
    -- 基本数据类型 ：String Number Boolean undefined 

    -- 特殊的基本数据类型：null

    null 与 undefined 的区别
        1.相同点：他们只有一个值，就是他们自身
            null --> null
            undefined --> undefined
        2.不同点:
          -- 校验他们的数据类型的时候
                typeof undefined --> undefined
                typeof null --> Object
          -- 他们自身所代表的的意义
             -- undefined: 代表的意思是缺少值
                @1.缺少内存地址
                    var a;
                    console.log(a) // undefined 缺少内存地址
                @2.函数的默认返回值
                    function fn () {
                        console.log("1")
                    }
                    var result = fn();
                    console.log(result) // undefined 函数fn的默认返回值
                @3.访问不存在的对象属性
                    var obj = {}
                    console.log(obj.name) // undefined 
                @4.函数参数未传时，默认undefined
                    function fn(name,age){
                        console.log(name) // ddy
                        console.log(age)  // undefined
                    }
                    fn("ddy");
             -- null --> 代表的意思是空对象
                @1.在内存的堆中是唯一的
                    var a = null;
                    var b = null;
                    console.log(a == b) // true
                @2.作为原型链的终点：参考后面的原型链


    -- 引用数据类型：只有一种 Object
        1.因为 Array和function的值 是Object的实例
        2.所以 Array和function的值 也是引用类型
        3.运行下面代码来理解下
          Object.prototype.work = function(){}
          function fn() {}
          console.log(fn.work) // 输出：function (){} ; 
        4.值得注意：
          var a = []
          console.log(typeof a) // Object 
          function fn (){}
          console.log(typeof fn) // Function ,这是个特殊的存在，但仍然要把它看做是对象

    -- 检测数据类型的方法 typeof 

```