[返回目录](./README.md)

#### 基于原型构建的面向对象系统
```JavaScript
(1).JavaScript语言设计思路及原理   
    -- 借鉴
        C语言的 基本语法
        JAVA的  数据类型 和 内存管理
        scheme的 函数 提升“第一等公民”
        self prototype的   继承机制
    -- JavaScript组成
        1.ECMAScript
        2.BOM
        3.DOM
    -- javascript = 函数式编程 + 面向对象编程
    -- 这种开发模式中，你的类或主对象(被称为可观察的对象)会通知其他感兴趣的类或对象(称为观察者)，并提供相关信息(事件)

(2).面向对象的基本规则
    1.对象是什么 --> 属性跟方法的综合体 
        xiaoming  {
            属性：历史不好 为人正直  严肃
            方法：吃  喝   拉    撒
        }   
        xiaoming.吃() 
    2.对象是怎么来的 --> 由类(构造函数)实例化得来的
    3.不存在独立的 属性/函数(方法) --> 都必须是某个对象的 属性/方法 
    4.JavaScript 是没有继承的概念的，继承可以看做是访问权限    
    5.先有类 后有对象，我们不能操作类，只能操作类的实例化对象。
      通过关键字new实例化，那么此时的实例对象就继承了该类的所有封装的属性和方法

(3).prototype 原型编程语言的 基本规则
    1.所有的数据都是对象 
    2.对象会记住他的原型 
    3.当请求访问对象的某个方法，无法响应时，会把这个请求委托给他自己的原型  ==>  原型链

(4).宿主环境和宿主环境对象
    -- JavaScript是一种脚本语言，所以有宿主和JS引擎
    1.宿主环境 ：浏览器
    2.宿主环境对象：Windows对象（公共对象系统）
    3.ECMA没有对宿主环境进行统一标准，所以宿主环境对象略有差异
    4.全局下定义的变量和函数都会成为window的属性和方法

(5).区别对象
    1.本地对象  ==> 构造函数
    2.内置对象  ==> Math: 区别于本地对象不需要new成实例来使用;
                   global:全局对象 http://www.w3school.com.cn/jsref/jsref_obj_global.asp
    3.宿主对象  ==> BOM->浏览器对象模型 -> 核心对象window
                    DOM->文档对象模型 -> 核心对象document,element对象,element代表着一个标签对象
      
(6)基于原型构建的面向对象系统
    1.构造函数
        @1.什么是构造函数？
           -- 在JS中可以看做是类的存在
           -- 通过new可以得到类的实例对象
           -- 并且实例对象继承类的属性和方法
        @2.构造函数的写法
            // 1. 声明 Person 类
            function Person() {
                this.name = "ddy1"
            }
            // 2.封装 Person 类的属性和方法
            Person.prototype = {
                name : "ddy2",
                age : 27,
                work : function(){
                    console.log(this.name + "正在工作")
                }
            }
        @3.构造函数的实例对象执行流程
            // 执行步骤: 
            // 1.创建 zdy对象 
            // 2.调用构造函数Person
            var zdy = new Person(); 
        @4.访问构造函数内属性和方法顺序
            // 1. 现在构造函数内部去找
            // 2. 没找到然后去构造函数的原型对象上去找
            // 3. 在没找到会去构造函数继承的类的原型对象找，知道Object的原型对象上
            console.log(zdy.name) // ddy1
            zdy.name = "ddy3"
            console.log(zdy.name) // 输出: ddy3
            zdy.work() // 输出: ddy3 正在工作

    2.构造函数与内存
        @1.使用this将属性或者方法 写在构造函数里，那么通过new创建的实例其实是复制了这个类的属性和方法，
           从而增加了内存消耗，浪费资源。
            function Person(){   
                this.age=27
                this.eat=function(){}
            }
            var ddy = new Person();
                    
        @2.如果把这些属性和方法写在prototype里，那么实例化的类只是继承了它的原型，对原型上的属性和方法
           具有了访问权限，所以这么做节约了内存。
            Person.prototype={
                age : 30,
                play : function(){}
            }

    3.普通函数 与 构造函数 的区别
        相同点：
            1.都是function来声明的,都是Function的实例对象
            2.都在内存堆中开辟一块内存空间
        不同点:
            1.作用不同构: 构造函数是用来通过new创建实例的;普通函数就是来被调用的
            2.构造函数内不会有return,因为return在这里没有任何意义
            3.在写法上一般构造函数的首字母会大写

    4.构造器->constructor 属性：**所有对象都拥有的一个属性,返回创建此对象的构造器**
        function Person() {

        }
        // 1.Person 的构造器
        console.log(Person.constructor) // f Function(){}
        // 2. Person实例ddy1的构造器
        var ddy1 = new Person();
        console.log(ddy1.constructor); // 输出:function Person(){}
        // 3. Person的原型对象的构造器
        Person.prototype = {

        }
        console.log(Person.prototype.constructor) // 输出 f Object(){}
        // 4. 拥有原型的Person的实例的构造器
        var ddy2 = new Person();
        console.log(ddy2.constructor); // 输出 f Object(){};因为上面上面将它的原型指向了{}
        // 5. 更改Person原型对象的构造器指向后Person实例的构造器
        Person.prototype.constructor = Person;
        var ddy3 = new Person();
        console.log(ddy3.constructor) // 输出 function Person(){}

    5.prototype与__proto__
        prototype 
            @1.构造函数Function的一个属性，指向一个原型对象;继承于Object的prototype
            @2.既然是一个原型对象,那么它也是Object的一个实例
        __proto__ 
            @1.每一个对象拥有的属性  
            @2.指向的是当前对象自身构造函数关联的原型对象

    6.基类与派生类 --> 派生类继承与基类
        @1.通过父类和子类理解下
            父类: 
                  动物类 {
                        属性: 大脑、消化、温度
                  }
            子类：
                  人类 {
                        属性: 名字、年龄、性别
                  }
                  var ddy = new 人类();
                  // 这里毋庸置疑 肯定输出人类属性中的名字
                  console.log(ddy.名字)
                  // 那么这里 也会输出动物类属性中的大脑，因为子类继承与父类，
                  console.log(ddy.大脑) 
            然而在JavaScript中 基类就等同于父类，派生类就等同于子类

        @2.JavaScript中的内置结构为
             基类:   Object
             派生类: 构造函数   Array  Function  String Number Boolean ...
             实例:   new xx()  []     function  "xx"   123    true/false

             所以在  OBject.prototype.xxx上扩展方法，那么所有的对象都会拥有这个方法
             例如:
                Object.prototype.work = function(){
                    console.log("我正在工作")
                }
                var str = "String"; 
                    str.work(); // 输出: 我正在工作
                var num = 10
                    num.work(); // 输出: 我正在工作
                var fn = function(){}
                    fn.work();  // 输出: 我正在工作
                var arr = []
                    arr.work(); // 输出: 我正在工作
                var bol = true
                    bol.work(); // 输出: 我正在工作
             
        @3.对象的数据类型 ( 值:也就是实例 --> 对象 ) 
            Object  --> function Object (){...}
            创建实例：
                1.字面量   --> {}
                2.构造函数 --> new Object() 
            Array  --> function Array (){...}
            创建实例：
                1.字面量   --> []
                2.构造函数 --> new Array() 
            function  --> function Function (){...}
            创建实例：
                1.字面量   --> function(){}
                2.构造函数 --> new Function()
            String  --> function String (){...}
            创建实例：
                1.字面量   --> "abc"
                2.构造函数 --> new String()    
            Number  --> function Number (){...}
            创建实例：
                1.字面量   --> 123
                2.构造函数 --> new Number()   
            Boolean --> function Boolean (){...}
            创建实例：
                1.字面量   --> true / false
                2.构造函数 --> new Boolean()

            通过上面可以看出:
            1.每个数据类型都对应着自己的构造函数
            2.每个数据类型创建实例的方式都有两种 字面量和构造函数，而且这两种方式是等同的,
              因为这两种方式创建的实例都拥有相同的属性和方法
                var a = {}
                var b = new Object()
                console.log(a)
                console.log(b)
                通过这串代码运行的结果可以看出 输出的 a和b 拥有完全相同的属性跟方法(__proto__)
            3.我们都知道通过构造函数创建出的实例都是对象，所以我们就可以理解为什么
              在JS中我们会把一切都当做对象来处理

    7.原型链
        ![](https://github.com/zhudongyu/ECMAScript-5-note/raw/master/yuanxinglian.JPG)
        @1.
            Function.prototype.x=function(){}
            function fn(){

            }
            // 输出: f (){} 因为prototype是Function的一个属性，
            console.log(Function.prototype) 
            // 输出: f(){} 因为__proto__指向的是当前自身对象的构造函数关的原型联对象
            // fn 的构造函数是Function，
            console.log(fn.__proto__) 
            // 输出: {Object..} fn这时是一个实例对象，那么它的原型对象，就是Object的一个实例对象
            console.log(fn.prototype) 
            // 输出Object fn.prototype是构造函数Object的实例对象，那么Object关联的原型对象就是{Object...}
            console.log(fn.prototype.__proto__) 
            
            // 输出: f (){}; 这里的Object是函数，它的构造函数是Function,那么Function关联的原型对象
            // 就是它的Prototype 
            console.log(Object.__proto__)
            // 输出: {Object...}
            console.log(Object.prototype)
            // 输出: null 原型链的终点
            console.log(Object.prototype.__proto__)

            console.log(Object.__proto__ === fn.__proto__) // true
            console.log(Object.__proto__ === Function.prototype) // true

        @2.
            Object.prototype.a = function(){
                console.log("aaaa")
            }
            Function.prototype.b = function(){
                console.log("bbbb")
            }
            function fn(){

            }
            fn.a() // 输出: aaaa
            fn.b() // 输出: bbbb

            var f = new fn();
            f.a() // 输出: aaaa
            f.b() // 报错 f.b() is not a function

    8.类(构造函数)之间的继承
        // Person 看做是父类
        function Person(name,age){
            this.name = name;
            this.age = age;
        }
        Person.prototype = {
            eat : function(){
                console.log(this.name + "正在吃饭")
            },
            dirnk : function(){
                console.log(this.name + "正在喝水")
            },
        }
        // Student 看做是子类
        function Student(name,age){
            // 更改Person中this指向为Student,否则指向的就是window
            Person.call(this,name,age)
        }
        // 1.因为Person类的实例具有对其原型的访问权限
        // 2.所以将Student类的原型指向Person的实例
        // 3.那么Student类的实例爬着原型链就能访问到Person的原型对象
        Student.prototype = new Person();
        Student.prototype.school = function(){
            console.log(this.name + "正在上学")
        }
        var Alice = new Student("Alice",18);
        Alice.school(); // 输出: Alice 正在上学
        /*
            也可以使用 for...in...的方法,遍历Person类原型对象的所有"可枚举属性",
            在赋值给Student类的原型。但是这样赋值行为极大的消耗了性能。
        */


```
