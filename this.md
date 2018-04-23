[返回目录](./README.md)

## 挥舞this的正确的姿势、apply、call和bind原理及实现

### 一、如何理解 this

        1.this 在 JS 中是一种关键字,
        2.它代表着函数运行时自动生成的一个内部对象
        3.随着函数的使用场合不同this的值也会发生变化
        4.但总体有一个原则就是 **** this 指的是调用这个函数的那个对象 ****



### 二、全局环境下(window):

```JavaScript
console.log(this) // window 对象
// 在函数中
function fn() {
    console.log(this) 
}
// 输出window 对象, 
// 因为不存在独立的函数 所有函数都必须是某个对象的方法
// 所以相当于 window.fn();
fn(); 
```


### 三、在对象中

```JavaScript
var name = "ddy"
var obj = {
    name : "donyk",
    getName : function(){
        console.log(name) // ddy
        console.log(this.name) // donyk
        function fn(){
            console.log(this.name) // ddy
        }
        fn() // 相当于 window.fn()
    }
}
obj.getName();
```

### 四、在事件函数中: this 指向触发当前事件的 Element 对象

```JavaScript
var box = document.getElementById("box")
box.onclick = function() {
    console.log(this) // Object HTMLDivElement
}
```

### 五、在构造函数中:指向它的实例对象,参考后面面向对象中的构造函数

### 六、在apply、call和bind中

(1).这三个方法不是天上掉下来的...都是 Function 对象上的方法
```JavaScript
Function.prototype.apply
Function.prototype.call
Function.prototype.bind   
```
    所以 Function 的实例对象(new Function/function(){})都是可以调用的。

(2).这三个方法有一个共同的作用 --> 改变当前函数对象里面 this 原本的指向

(3).apply、call的执行规则

    1.立即调用当前函数对象  
    2.改变原本函数对象里面 this 指向
```JavaScript
var name="ddy";
function setName(){
    console.log(this.name);   
}
var obj={
    name:"donyk",
    fn:function(){
        console.log(this)  // obj 对象
        setName(); // 输出 ddy;
        setName.call(this); // 输出 donyk; 
        setName.apply(this); // 输出 donyk;
    }
};
obj.fn();
```
(4).apply和call的区别

```JavaScript
call(this,1,2)    // call 的第二个参数开始随便传，对应函数的第1.2，3个参数
aplly(this,[1,2]) // apply 的第二个参数开始必须是数组，数组
```

(5).bind 

    1.*** bind 不会立即调用函数对象***，会返回一个新的函数，它和原本的函数对象是一样的
    2.新函数对象中的 this 绑定了bind中的**第一个参数**
```JavaScript
var color = "red";
var obj = {color:"blue"};
function getColor(a,b){
    console.log(arguments);  // [1,2,3,4,5,6,7]
    console.log("a : " + a)  // a : 1
    console.log("b : " + b)  // b : 2
    console.log(this.color); // blue
}
// bind后返回的新函数result; 
// 并且bind从第二个参数开始传进去的参数位置比调用result时传进去的参数靠前
var result = getColor.bind(obj,1,2,3,4,5); 
result(6,7); 
```
