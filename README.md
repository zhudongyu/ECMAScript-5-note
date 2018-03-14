# ECMAScript-5-note
> 本文档为 ES5 的深入研究汇总

> 知识越学越多，总是要花点时间来整下,梳理思路

> 将散落在各地的知识碎片组织起来,形成知识网络系统,方便自己查询和补充,同时也利于他人学习成长！

> I love JavaScript ！！！

## 目录

* [数据类型的意义、本质、值](./data.md)
* [变量的意义、堆栈的世界、语言执行规则、运算符、基本语句](./var.md)
* [闭包](./closure.md)
* [以正确的姿势挥舞this](./this.md)
* [正则 RegExp 模板引擎及模板替换](./RegExp.md)
* [基于prototype原型构建的面向对象系统、原型链探索](./prototype.md)
* [日期操作大全](./date.md)
* [双向数据绑定原理分析及实现](./viewModel.md)
* [数据校验插件开发](./install.md)
* [javaScript 设计模式](./design.md)
* [javaScript的垃圾回收机制和内存泄漏](./memoryLeak.md)


> 函数的执行规则
```javascript
1.调用一个函数的时候,首先会根据名称去栈中查找这个变量，如果这个变量的引用是函数就执行，不是函数就报错！
2.如果在栈中没有找到这个变量或者变量的值为undefined，就会去堆中查找函数式声明的函数，找到并执行


1.
var fn = function（）{
    console.log(2)
}
function fn(){
    console.log(1)
}
fn(); // 2

2.
fn()  // 1
var fn = 1;
function fn (){
    console.log(1)
}
fn() // TypeError: fn is not a function

```