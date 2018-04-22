[返回目录](./README.md)

#### 堆栈的世界、变量的意义、语言执行规则、作用域、作用域链、运算符、基本语句


> 一、堆栈的世界 -> 内存

(1).内存的概念

    1.电脑的内存一般是8G，而硬盘可能是1T
    2.比如说QQ(200M大小)，下载是存储到了硬盘里（占用了200M），运行是在内存里
     （处理程序,占用2M -> 内存占用的大小取决于进程）
    3.再比如 电脑中毒 -> 蠕虫病毒不断繁殖不断运行，不断占用内存，卡死，关机，回收内存

(2).内存分布 : JS中一切都是对象，所以只会给对象分布内存

    1.常量池
      存放的是常量 --> 值类型值 --> 比如数字，字符串
    2.栈
      存放的是变量 --> 变量的作用是存储对象记忆内存地址
    3.堆
      存放的是对象 --> 引用类型的值 --> 这个对象的意思是 数据类型都为Object的对象
    4.运行时内存
      函数在运行时占用的内存，运行完毕之后会销毁掉
      
      │-——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--|
      │  constant pool  │    栈     |                            堆                    │    运   │
      │                 │    var    |                          Object                 │     行   │
      │                 │           |                                                 │     时   │
      │                 │           │                                                 │     内   │
      │                 │           │                                                 │     存   │
      │                 │           │                                                 │         │
      │-——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--|

(3).JS在内存的表现

    1.看看 var a = {} 在内存中是怎么体现的
      @1.变量 a 在栈中开辟出一块内存空间 --> 这块内存空间会有一个名字叫做 a 
      @2.{} 这个对象会存储在堆中 --> {} 占用一块内存地址
      @3.这时 a 中就引用了这个对象的内存地址
      @4.这个过程在C语言中称为指针，但是在JS语言中我们称为引用
    2.var b;
      @1.在栈中开辟了一块内存空间，命名为 b
      @2.由于没有引用任何内存地址，所以b当前的值为 undefined


> 二、变量、作用域、作用域链

(1).作用域 -> 作用域指的是对某一变量 || 方法有访问权限

    1.全局作用域 -> 在 window 环境下创建,此时的变量和函数为全局变量和全局函数
    2.局部作用域 -> 在 function 下创建，其内部的变量和函数,就是局部变量和函数  

(2).function局部作用域是怎么创建的？为什么访问不到局部变量？

    1.局部作用域通过 function 来声明，但是声明不等于创建。虽然声明了，在没被调用时就是一个空壳子。
    2.只有当它被调用的时候才会被创建。然后在其内部临时开辟一块内存空间，同样会有堆栈的分布（但是没有常量池，内存中只有一块常量池)
    3.所以这样就形成了局部作用域。
    
    4.当执行完毕之后这块临时创建的内存空间就会被销毁掉了。
    5.所以说在全局环境下是无法访问局部作用域的,因为没有被创建.
    6.就算被创建了,按照代码执行规则,先定义后执行,但是当执行完之后,这块就销毁了,所以全局还是无法访问局部的。


(3).变量的 就近原则/作用域链/生命周期？

    1.就近原则 -> 会先在自身所在作用域进行检索，找到后进行输出，
    2.作用域链 -> 找不到会向上冒泡直到找到对应的变量 自底向上的原则
```javaScript
var a = 10,b = 5;
function fn() {
    console.log(a) // undefined 
    console.log(b) // 5 
    var a = 1; 
    console.log(a) // 1
}
fn();
```
    3.生命周期
      全局变量生命周期：全局作用域中，关闭浏览器进程内存空间回收他随之也被回收。
      局部变量生命周期：定义在函数中 调用即存在  调用完毕即销毁。
    

    
> 三、JavaScript语言执行规则

(1).JS语言执行的三条黄金法则

    1.从上往下 -> 先定义所有定义代码 -> 定义代码是 : 定义变量和函数式声明
    2.从上往下 -> 后执行所有执行代码 -> 执行代码是 : 除了定义代码其余都是执行代码
    3.赋值操作 -> 从右往左 (.运算符优先于赋值操作 )
    

(2).JS语言是如何在内存中运行的

```javaScript
console.log(a);  // step_2: 输出 undefined;因为这时变量 a 没有引用内存地址,所以输出 undefined
var a = 1;       // step_1: 定义变量 a; step_3: 给变量 a 赋值 1,也就是此时变量 a 存储了引用地址;
conosle.log(a);  // step_4: 输出 1;因为此时变量 a 存储了内存地址,所以输出为 1 
```
```javaScript
var a = {n:1}
a.x = b = {n:2}
console.log(b)   // {n:2}
console.log(a.x) // {n:2}
b.n = 10
console.log(b)   // {n:10}
console.log(a.x) // {n:10}
```
```javaScript
var a = {name:b}
var b = {name:a}
console.log(a) // {name:undefined}
console.log(b) // {name:{name:undefined}}
```

(3).函数式声明函数和变量式函数表达式

    1.函数式声明 -> 只要是函数式声明，在内存中就会有一个引用名称，和内存地址
```javaScript
function fn () {
    ....
}
```
    2.变量式函数表达式 -> 匿名函数只有内存地址没有引用名称
```javaScript
var fn = function  () {
    ....
}
```
(4).函数执行规则

    先看下下面几段代码的运行结果

```javaScript
function fn(){   
    console.log(1)
}
fn(); // 输出 1
var fn = function(){  
    console.log(2)
};

```
```javaScript
function fn(){   
    console.log(1)
}
var fn = function(){  
    console.log(2)
};
fn(); // 输出 2


```
```javaScript
fn(); // fn is not a function
var fn = function(){  
    console.log(2)
};
```
    我们可以总结出:
        1.当我们调用一个函数 fn 的时候，会先去栈里检索，查看有没有定义 fn 这个变量。
        2.有 fn 这个变量:
          根据 fn 变量存储的内存地址找到对应的内存空间。
          如果 fn 变量没有存储内存地址，就会去堆找 fn 的引用名称。
        3.没有 fn 这个变量: 就去堆中找 fn 的引用名称

    那问题又来了:为什么先去栈中找？
        1.因为这一切都是由内存机制主导的，先开辟内存空间，然后在取内存地址 
        2.栈在内存中占据的很小很小的空间。
        3.堆却占据了庞大的空间
        4.举个栗子
          假如现在有一个超级大的水缸和一个小碗，里面都放了一个鸡蛋，但是你不知道。
          现在要你以最快的速度找到这个鸡蛋，那么你会先去哪里找呢？
          显而易见你会先看下小碗，因为你看一眼就能得出结论了，有的话就找到了，没有的话再去大水缸里去找。
          人的思维都这么聪明，何况人开发出的程序呢，

(5).再次深入理解 JS 的运行规则

```javaScript
fn()               // step_3: 执行函数fn  
var a = {}         // step_1: 定义变量 a;  step_7: 将对象{}赋值给变量a
console.log(a)     // step_8: 输出：Object , 等同于console.log(window.a)
function fn () {   // step_2: 定义函数fn
    console.log(a) // step_5: 输出：undefined
    var a = 1      // step_4: 定义变量 a;  step_6: 将数字1赋值给变量 a;
}
console.log(a)     // step_9: 输出：Object , 等同于console.log(window.a)

```

> 四、运算符 -> 比较运算符 ==、=== 

```javaScript
// 相同点 : 比较的都是 是否为同一个内存地址
// 不同点 : == 会进行 隐式转换，转换成相同数据类型在比较，而===不会进行隐式转换
var a = 10
var a1 = 1
var b = "10"
var c = true
var d = "ddy"
console.log(a == b)  // true
console.log(a == c)  // false
console.log(a1 == c) // true    因为 Number(true) == 1        
console.log(a == d)  // false   

```

> 五、基本语句

(1).for 循环 -> 一般处理数字的判断

    需求: 将一个数组的成员转换成另一个对象的成员
    已知: var arr = ["cat","dog","bird","pick"]
    分析: 为了满足后面所有类似这样的需求，可以封装成一个方法 --> 封装成数组对象独有的
```JavaScript
Array.prototype.unc = function(){
    var json = {};
    for(var i = 0;i < this.length;i++){
        if(!json[this[i]]){
            json[this[i]] = i;
        }
    }
    return json;
}
var result = arr.unc();
console.log(result) // {cat:0,dog:1,bird:2,pick:3}
```


(2).while循环 -> 一般处理非数字的判断
```javaScript
// 数值的判断
var a = 0;
while(a < 10){
    console.log(a);
    a++;
}
// 非数值的判断
var a = 1;
while(true){
    a++;
    if(a > 5) {
        break; 
    }
    console.log(a);
}

```
(3).do while 是先执行一次在进行判断
```javaScript
var a=11;
do{
    console.log(a);
    a++;
}while(a<10);
```

(4).扩展理解switch,实现极简jQuery对象

```JavaScript
(function(root){ 
    var jQuery = function(selector) {
        switch(typeof selector){
            case "function":
                console.log("我要等页面加载完毕在执行回调")
                break;
            case "object":
                console.log("我要找对象")
                break;
            case "string":
                switch(selector.charAt(0)){
                    case "#":
                        console.log("ID 选择")
                        break;
                    case ".":
                        console.log("CLASS 选择")
                        break;
                    default:
                        console.log("TAG 选择")
                }
                break;
        }
    }
    root.$ = root.jQuery = jQuery;
})(this) // this 也就是 window

console.log($)      // f jQuery(){...}
console.log(jQuery) // f jQuery(){...}
$("#abc")           // ID 选择

```

(5).break 和 continue

    break 语句用于跳出循环。
    continue 用于跳过循环中的一个迭代。
    break 和 continue 语句的不同之处
    break 语句可以立即退出循环，阻止再次反复执行任何代码。
    而 continue 语句只是退出当前循环，根据控制表达式还允许继续进行下一次循环。

