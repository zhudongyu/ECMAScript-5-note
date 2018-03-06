```JavaScript
正则(RegExp) --> 校验字符串 --> 从左到右校验

(1)创建正则对象:
    1.字面量: /(正则模式)/(修饰符)
    2.构造函数: new RegExp("正则模式","修饰符")
(2).知识点:
    1. [ ] -> 一个字符的范围: 
       [0-9] -> 这个字符的范围是0-9
       [a-z] -> 这个字符的范围是a-z
       [A-Z] -> 这个字符的范围是A-Z
    2. ^ $ -> 匹配所有字符: 也就是说每一个字符都都应该按照 ^ 和 $ 之间的正则模式去匹配
       var str = "abc123"
       var exp = /^[0-9]$/ 
       console.log(exp.test(str)) // false 应为 str 的第一个字符是字母,不符合exp的正则模式 
    3. g -> 修饰符: 代表着匹配全局,
    4. i -> 修饰符: 默认区分大小写,可以通过修饰符 i 让正则模式不区分大小写
    5. \d -> 元字符: 等价于 [0-9]
    6. \w -> 元字符: 等价于 [0-9 a-z A-Z]
    7. {} -> 设置匹配字符的数量: 
       {10} 代表匹配的字符数为10个
       {3,10} 代表匹配的字符数量为 3 到 10 个 
    8. + -> 代表的是 1 到 N位 之间的字符
    9. * -> 代表的是 0 到 N位 之间的字符
   10. . -> 在正则中代表任意字符
   11. \ -> 转义符: 
       \. 代表转义了 .
       \{\} 代表转义了 {}
   12. ? -> 代表着当前的字符要么没有，要么就出现一次
   13. | -> 逻辑或: 一般跟着正则表达式()使用  ([\d+])|([\w*])
  
(3).支持正则表达式的string对象的方法：
    1.match --> 以数组形式返回指定的值
        var str = "1.hello world;2.hello javascript"
        console.log(str.match(/\d+/g)) //['1','2']

    2.replace --> 返回替换过后的字符串，常用于关键字屏蔽
        1.replace的第二参数为字符串
            var str = "小明说你他妈的再看我一眼试试，我就打死你！"
            console.log(str.replace(/他妈的|打死/g,"**"))
        2.replace的第二参数为匿名函数
            var str = "[0]你爱我，[1]我也爱你，那我们在一起吧！"
            var result = str.replace(/\[(\d+)\]/g,function(a,b){
                // ["[0]"->a, "0"->b, 0->下标,str的文本]
                // ["[1]"->a, "0"->b, 7->下标,str的文本]
                // 所以可以看出replace是循环执行的
                console.log(arguments)
                console.log(a) // 输出: [0] [1]; 参数 a 代表着与整个正则模式相匹配的字符 
                console.log(b) // 输出: 0 1 ; 参数 b 代表着与正则模式中的表达式相匹配的字符
                return "如果"
            })
            console.log(result) // 如果你爱我，如果我也爱你，那我们在一起吧！

    3.search --> 搜索返回下标
        var str = "javaScript"
        console.log(str.search(/java/)) // 0

    4.split 切割后以数组形式返回
        var str = "如果11我爱你,如果22你也爱我,那33我们在一起吧！" 
        str.split(/\d+/g) // ["如果", "我爱你，如果", "你也爱我，那", "我们在一起吧！"]
        str.split(" ") // ["如果11我爱你，如果22你也爱我，那33我们在一起吧！"]
        str.split(",") // ["如果11我爱你", "如果22你也爱我", "那33我们在一起吧！"]

(4).RegExp对象的方法
    1.test --> 返回布尔值
        // 匹配座机号
        var str = "020-82531020"
        var exp = /^0\d{2,3}-\d{7,8}$/
        console.log(exp.test(str)) // true

    2.exec
        @1."模板标识符" 比如说 Vue 中的 {{ }}
        @2.获取模板标识符里的内容
            var user = "{{1001}},{{1002}},{{1003}}"
            var exp = /\{\{(\d+)\}\}/g;
            while((result=exp.exec(user)) !== null){
                // ["{{1001}}", "1001", index: 0,  input: "{{1001}},{{1002}},{{1003}}"]
                // ["{{1002}}", "1002", index: 9,  input: "{{1001}},{{1002}},{{1003}}"]
                // ["{{1003}}", "1003", index: 18, input: "{{1001}},{{1002}},{{1003}}"]
                console.log(result) 
            }
        @3.浅浅析一下正则在模板引擎中的玩法
            HTML:
                <div id="box"></div>

            JavaScript:
                // View层: html模板  
                <script type="text/html" id="template">
                    <p>name:{{ name }}</p>
                    <p>age:{{ age }}</p>
                </script>

                <script type="text/javascript">
                    // Control层
                    var updateViewMethod = function(temp,data){
                        var temp = document.getElementById(temp).innerHTML;
                        var exp = /\{\{(\w+)\}\}/g;
                        while((result=exp.exec(temp))!==null){  
                            if(result[1]){   
                                temp = temp.replace(result[0],date[result[1]]);
                            }
                        }
                        return tp1;
                    }
                    // Model层数据
                    var date={
                        name:"ddy",
                        age:27,
                    }
                    document.getElementById("box").innerHTML = updateViewMethod("template",date);
                </script>

```