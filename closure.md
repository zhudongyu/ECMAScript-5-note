[返回目录](./README.md)

#### 闭包
```JavaScript
(1).自执行函数介绍
    写法: (function(){})()
        1.第一个()里是以函数式声明的形式写的函数表达式,这个括号使得我们的解析器吧代码看成表达式，
        2.第二个()是执行,调用该匿名函数及传参
    
    来个例子在理解下：
        1. function(){alert('11')}() 运行后会报错的，前面的function只是一个声明，后面的()无法执行。
        2. console.log(function(){alert('11')}()) 弹出11，因为此时的代码被编译器看成了是一个执行语句。所以执行了
    执行语句:
        1.只要能让编译器理解成是执行语句，而非函数式声明的，都可以执行
        2.比如前面加 ！+ ？........等 。只是在不同的符号返回值是不一样的
        3.还有一个效率问题，识别代码的时候(）是输出最快的，性能最好的。而 + - 号在Google中是最好的

    执行流程: 定义匿名函数 --> 调用 --> 销毁(动态临时开辟的内存空间被销毁掉)
    好处: 规划了局部作用域，能够防止变量污染
    场景: 当下在框架开发中经常用到


(2).看下Vue框架搭建的起手式
    (function(root,factory){
        root.Vue = factory();
    })(window,function(){ // 工厂函数
        var Vue = function(){

        }
        return Vue;
    })
(3).闭包
    1.什么是闭包: 
        官方:闭包是一个函数在创建时允许访问自身作用域之外的作用域中的变量,这是纯函数式编程语言中的一个特征
            function parent(){     
                var msg="hello world";   
                function child(){    
                    console.log(msg)   
                }
                child();
            }
            parent(); // 输出 hello world , 因为child访问了parent中的变量
        我的:
            闭包的产生:
                1.一个作用域访问另一个作用域的变量,
                2.但是会让另一个作用域得不到释放，得不到释放就会产生内存泄漏(不在用到的内存没有及时释放 --> 内存泄漏)

            闭包的现象:
                // 全局作用域  window,
                // 全局作用域下占用的内存会一直存在直到关闭浏览器
                var a = 1;   
                function fn(){  // 局部作用域  
                    console.log(a) // 作用域链是向上冒泡的，所以这里找不到a就会去上一级全局作用域去找
                }
                fn();
                1.函数 fn 在被调用之前就是可空壳子
                2.只有当它被调用时才会在其作用域内开辟一块临时空间(堆栈)
                3.当执行完之后就会立即销毁这块空间，
                4.但是当他开辟的这块内存空间与另一个作用域内的内存地址存在引用关系时，就不会被销毁掉
                5.从而内存就得不到释放，长期占用造成内存泄漏

    2.理解闭包的作用域:
        @1.我们现在都知道 函数在调用完之后会 销毁 临时开辟的内存空间也就是说自身作用域会销毁
        @2.看下面的例子,思考如果函数在内存中销毁掉会发生什么？
            function parent(){     
                var msg="hello world";  
                return function(){  
                    console.log(msg);
                }
            }
            // 这时result 接收了一个返回值,按照上面说的规则，这块内存会销毁...
            var result = parent(); 
            // 输出 hello world 
            // 这是为什么呢？
            // 因为两个作用域之间有依赖关系,下层作用域能访问上层作用域中的变量【作用域链】
            // 闭包是在作用域链之上的一个升级,所以上一级的作用域不可能被销毁【变量缓存】
            result();
    3.闭包会使函数中的变量都保存在内存中,这对内存的消耗会加大,所以我们不能滥用闭包
(4).经典闭包
    //这段代码每隔一秒输出执行1次的10，执行10次
    for(var i=0;i<10;i++){
        setTimeout(function(){
            console.log(i)
        },i *1000)
    }
    //这段代码会每隔一秒输出0~9的i
    for(var i=0;i<10;i++){
        setTimeout((function(i){
            return function(){
                console.log(i)
            }
        })(i),i * 1000)
    }

(5).封装静态变量的思想
    1.什么是静态变量(static) : 你只能来访问我，但是却不能修改我
    2.JS 中没有static关键字，也没这个概念
    3.JS中的实现
        (function(){
            var someObject={
                max:1000,
                min:100
            }
            var util = function(){}
      	    var others = function(){}
            // 通过 return 暴露我们希望暴露出去的内容
            return {
                // 只能让你获取
                get:function(name){
                    return someObject[name]?someObject[name]:null;
                },
                util:util,
                others:others
            }
        })();
        
(6).思考题走一波：
    var foo=function(m,n){
        console.log(n);
        return {
            foo:function(o){
                console.log(o);
                // 注意这里 return foo(o,m) --> return window.foo(o,m)
                // 所以首先在内存中的栈中查找，结果是找到了，然后在执行   
                // 如果访问的是 return 对象中foo,需要用到this,this.foo() 
                return foo(o,m);
            }           
        }
    }
    1.问题一:
        var result=foo(1); // undefined 
        result.foo(2); // 2 1
        result.foo(3); // 3 1
        result.foo(4); // 4 1
    
    2.问题二:
        var result=foo(2).foo(3).foo(4).foo(5);// undefined 3 2 4 3 5 4
        
    3.问题三:
        var result=foo(1);    // undefined
        result.foo(2).foo(3); // 2 1 3 2
        result.foo(4).foo(5); // 4 1 5 4



```