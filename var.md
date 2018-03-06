[返回目录](./README.md)

####变量的意义、堆栈的世界、语言执行规则、运算符、基本语句
```JavaScript
(1).内存的概念
    -- 这里不赘述官方解释，通过例子理解
        1.电脑的内存一般是8G，而硬盘可能是1T
        2.比如说QQ(200M大小)，下载是存储到了硬盘里（占用了200M），运行是在内存里
         （处理程序,占用2M -> 内存占用的大小取决于进程）
        3.再比如 电脑中毒 -> 蠕虫病毒不断繁殖不断运行，不断占用内存，卡死，关机，回收内存

(2).内存分布 : 只会给对象分布内存 --> 在JS中一切都可以理解为对象
    -- 常量池
        存放的是常量 --> 值类型值 --> 比如数字，字符串
    -- 栈
        存放的是变量 --> 变量的作用是存储对象记忆内存地址
        -- 举例说明 
        1.var a = {}  
            @1.在栈中开辟出一块内存空间，这块内存空间会有一个名字叫做a --> 变量a 在栈中
            @2.{}这个对象会存储在堆中，占用一块内存地址 --> {}对象在堆中
            @3.这时a中就引用了这个对象的内存地址
            @4.这个过程在C语言中称为指针，但是在JS语言中我们称为引用
        2.var b;
            @1.在栈中开辟了一块内存空间，命名为b
            @2.由于没有引用任何内存地址，所以b当前的值为undefined
    -- 堆
        存放的是对象 --> 引用类型的值 --> 这个对象的意思是 数据类型都为Object的对象，
        │-——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--|
        │  constant pool  │    栈     |                            堆                             │
        │                 │    var    |                          Object                           │
        │                 │           |                                                           │
        │                 │           │                                                           │
        │                 │           │                                                           │
        │                 │           │                                                           │
        │-——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--——--|

(3)语言执行规则
    1.按照从上往下的加载顺序规则，先定义后执行
        @1.从上往下
        @2.先定义后执行
           --定义代码：var 变量，function函数
           --执行代码：除了定义代码其余都是执行代码
    2.举例说明：
        @1.执行下面代码：
           console.log(a);  // 输出undefined
           var a = 1;
           conosle.log(a);  // 输出1
        @2.解释
            1.首先根据执行规则的 先定义 --> 先定义了变量a,也就是 var a;
            2.然后根据执行规则的 后执行 --> 从上往下
              -- 第一步：console.log(a) 这是变量a没有引用内存地址，所以输出undefined
              -- 第二步：var a = 1 数字1存储在堆中，占用了内存地址，并且被变量 a 引用
              -- 第三步: console.log(a) 因为此时变量 a 存储了内存地址，所以输出为 1 
        @3.在来一例子思考
            fn() // step_3: 执行函数fn  
            var a = {} // step_1: 定义变量 a; step_7: 将对象{}赋值给变量a
            console.log(a) // step_8: 输出：Object , 等同于console.log(window.a)
            function fn () { // step_2: 定义函数fn
                console.log(a) // step_5 输出：undefined
                var a = 1 // step_4: 定义变量 a; step_6: 将数字1赋值给变量 a;
            }
            console.log(a) // step_9: 输出：Object , 等同于console.log(window.a)
    3.明白了JS的执行规则，那么问题来了，为什么会这么执行？
        因为这一切都是由内存机制主导的
        -- 先开辟内存空间
        -- 然后在取内存地址 
(4).变量的命名规范
    1.数字、字母、下划线 _ 和美元符号 $
    2.不能以数字开头
    3.JavaScript是一门弱类型语言，所以严格区分大小写
    4.见名知意 --> 一定要打死那些写 aa,bb的....
    5.不能使用关键字 
    6.建议采用驼峰命名  

(5).比较运算符 ==、=== 
    相同点 : 比较的都是 是否为同一个内存地址
    不同点 : == 会进行 隐式转换，转换成相同数据类型在比较，而===不会进行隐式转换
        var a = 10
        var a1 = 1
        var b = "10"
        var c = true
        var d = "ddy"
        console.log(a == b)  // true
        console.log(a == c)  // false
        console.log(a1 == c) // true    因为 Number(true) == 1        
        console.log(a == d)  // false   

(6).for 循环和 if
    需求: 将一个数组的成员转换成另一个对象的成员
    已知: var arr = ["cat","dog","bird","pick"]
    分析: 为了满足后面所有类似这样的需求，可以封装成一个方法 --> 封装成数组对象独有的
        Array.prototype.unc = function(){
            var json = {};
            console.log(this) // 构造函数中this指向的是调用它的实例对象
            for(var i=0;i<this.length;i++){
                if(!json[this[i]]){
                    json[this[i]] = i;
                }
            }
            return json;
        }
        var result = arr.unc();
        console.log(result) // {cat:0,dog:1,bird:2,pick:3}
(7).扩展理解switch,实现极简jQuery对象
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
    $("#abc") // ID 选择

```