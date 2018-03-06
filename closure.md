[返回目录](./README.md)

#### 闭包
```JavaScript
(1).自执行函数介绍
    写法: (function(){})()
        1.第一个()里是以函数式声明的形式写的函数表达式 
        2.第二个()是调用该匿名函数及传参
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
    1.什么是闭包: 闭包是一个函数在创建时允许访问自身作用域之外的作用域中的变量。
      这是纯函数式编程语言中的一个特征
        function parent(){     
            var msg="hello world";   
            function child(){    
                console.log(msg)   
            }
            child();
        }
        parent(); // 输出 hello world , 因为child访问了parent中的变量
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

(4).封装静态变量的思想
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
        
(5).思考题走一波：
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