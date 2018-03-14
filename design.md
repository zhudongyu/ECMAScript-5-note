```JavaScript
/*---------------------------------------------------------------------------*/
/*                    使用工厂模式方式创建用户对象（N个对象）                    */
/*---------------------------------------------------------------------------*/
    /*通过函数封装创建对象的细节*/
    function createUser(name,pwd){
        var user = new Object();
        //定义对象的属性
        user.name = name;
        user.pwd = pwd;
        //定义对象的方法
        user.showInfo=function(){
            document.write(this.name+"-"+this.pwd+"<br/>")
        };
        return user;
    }
    //创建用户对象
    //弊端1：看不出数据类型
    var user1 = createUser("张三","123456");
    var user2= createUser("李四","654321");
    user1.showInfo();
    user2.showInfo();
    //弊端2：函数重复，浪费资源
    document.write(user1.showInfo==user2.showInfo);



```

```JavaScript
constructor.js

//两种创建新对象的方式  -> 空对象
var newObject = {};//var newObject = new Object();

//两种常用方法将键值赋给一个对象
//1.点语法
//设置属性
newObject.somekey = 'hello world';
//获取属性的值
var key = newObject.somekey;

//2.中括号语法
//设置属性
newObject['somekey1'] = 'hello world -1'
//获取属性的值
var key1 = newObject['somekey1']


//虽然JavaScript不支持类的概念，但是它支持与对象一起用的函数 ---> 构造器函数
//通过在构造器前面加new关键字，告诉JavaScript像使用构造器一样实例化一个新对象，并且对象的成员由还函数来定义
//在构造器内，关键字this引用新创建的对象


//构造函数的名称首字母大写 ---> 一个约定俗成的规矩
function Car(name,year,miles){
	this.name = name;
	this.year = year;
	this.miles = miles;
	this.currentState = function(){
		return this.name + ' has done ' + this.miles + ' miles';
	};
};
//创建两个实例对象
var BYD = new Car('BYD',2014,8888);
console.log(BYD.currentState()); //BYD has done 8888 miles
var AudiR8 = new Car('AudiR8',2015,6666);
console.log(AudiR8.currentState()); //AudiR8 has done 6666 miles
//这样的构造器是有问题的！

//1.它使继承变得困难????????????????????????????????????????????????????

//2.currentState()这样的函数是为每个使用Car构造器创建的新对象而分别重新定义的，这么做不仅是不科学的，而且浪费了内存！
//因为这种函数应该在所有的Car类型实例之间共享，也就是当你需要的时候才会去访问该函数
//解决方案：-->prototype
//调用Car构造器创建一个对象后，新对象就会具有构造器原型的所有属性，
//通过这种方式，可以创建多个Car对象，并且访问相同原型,科学的做法而且节约内存
function Car(name,year,miles){
	this.name = name;
	this.year = year;
	this.miles = miles;
};
Car.prototype.currentState = function(){
	return this.name + ' has done ' + this.miles + ' miles'
}
var BYD = new Car('BYD',2014,8888);
console.log(BYD.currentState()); //BYD has done 8888 miles
var AudiR8 = new Car('AudiR8',2015,6666);
console.log(AudiR8.currentState()); //AudiR8 has done 6666 miles
```
```JavaScript

observer.js

//自定义事件构造函数
function EventTarget(){
  //事件处理程序数组集合
  this.handlers = {};
}
//自定义事件的原型对象
EventTarget.prototype = {
  //设置原型构造函数链
  constructor: EventTarget,
  //注册给定类型的事件处理程序，
  //type -> 自定义事件类型， handler -> 自定义事件回调函数
  addEvent: function(type, handler){
    //判断事件处理数组是否有该类型事件
    if(typeof this.handlers[type] == 'undefined'){
      this.handlers[type] = [];
    }
    //将处理事件push到事件处理数组里面
    this.handlers[type].push(handler);
  },
  //触发一个事件
  //event -> 为一个js对象，属性中至少包含type属性，
  //因为类型是必须的，其次可以传一些处理函数需要的其他变量参数。（这也是为什么要传js对象的原因）
  fireEvent: function(event){
    //模拟真实事件的event	
    if(!event.target){
      event.target = this;
    }
    //判断是否存在该事件类型
    if(this.handlers[event.type] instanceof Array){
      var handlers = this.handlers[event.type];
      //在同一个事件类型下的可能存在多种处理事件，找出本次需要处理的事件
      for(var i = 0; i < handlers.length; i++){
        //执行触发
        handlers[i](event);
      }
    }
  },
  //注销事件
  //type -> 自定义事件类型， handler -> 自定义事件回调函数
  removeEvent: function(type, handler){
    //判断是否存在该事件类型
    if(this.handlers[type] instanceof Array){
      var handlers = this.handlers[type];
      //在同一个事件类型下的可能存在多种处理事件
      for(var i = 0; i < handlers.length; i++){
        //找出本次需要处理的事件下标
        if(handlers[i] == handler){
          break;
        }
      }
      //从事件处理数组里面删除
      handlers.splice(i, 1);
    }
  }
};
function b(){
  console.log(123);
}
 
var target = new EventTarget();
target.addEvent("eat", b);
 
target.fireEvent({
  type: "eat"
});                 //123



// var pubsub = {};
// (function(q){
// 	var topics = {},
// 		subUid = -1;
// 	//发布或广播事件，包含特定的topic名称和参数（比如传递的数据）
// 	q.publish = function(topic,args){
// 		if(!topics[topic]){
// 			return false
// 		}
// 		var subscribers = topics[topic],
// 			len = subscribers ? subscribers.length :0 ;
// 		while(len--){
// 			subscribers[len].func(topic,args);
// 		}
// 		return this;
// 	};

// 	//通过特定的名称和回调函数订阅事件，topic/event触发时执行事件
// 	q.subscribe = function(topic,func){
// 		if(!topics[topic]){
// 			topics[topic]=[]
// 		}

// 		var token = (++subUid).toString();
// 		topics[topic].push({
// 			token : token,
// 			func : func
// 		})
// 		return token
// 	};

// 	//基于订阅上的标记引用，通过特定topic取消订阅
// 	q.unsubscribe = function(token){
// 		for(var m in topics){
// 			if(topics[m]){

// 			}
// 		}
// 	}



// })()




// //立即执行函数  参考https://segmentfault.com/q/1010000000442042
// (function exportTxt(){document.write('hello world !')})()


// var eventList = {
	
// }
// var eventFunction = function(){
// 	console.log('this is a callback function')
// }




// //var eventList = {
// //	eventName_1 : [fn1,fn2,fn3],
// //	eventName_2 : [fn4,fn5]
// //}
// //自定义事件的接口
// var Event = {
// 	eventList : {},
// 	listen : function(eventName,callback){
// 		if(!this.eventList[eventName]){
// 			this.eventList[eventName] = []
// 		}
// 		this.eventList[eventName].push(callback)
// 	},
// 	trigger : function(eventName){
// 		if(this.eventList)
// 	}
// }

// Event.listen('test',function(){
// 	console.log('test')
// })



















// // var Event = (function(){
// // 	var list = {};
// // 	var listen;
// // 	var trigger;
// // 	var remove;
// // 	listen = function(key,fn){
// // 		if(!list[key]){
// // 			list[key] =[];
// // 		}
// // 		list[key].push(fn)
// // 	}

// // 	trigger = function(){
// // 		var key = Array.prototype.shift.call(arguments)
// // 		msg = list[key]
// // 		if(!msg || msg.length === 0){
// // 			return false;
// // 		}
// // 		for(var i=0;i<msg.length;i++){
// // 			msg[i].apply(this,arguments)
// // 		}
// // 	}

// // 	remove = function(key,fn) {
// // 		var msg = list[key];
// // 		if(!msg){
// // 			return false
// // 		}
// // 		if(!fn){
// // 			delete list[key]
// // 		}else{
// // 			for(var i=0;i<msg.length;i++){
// // 				if(fn === msg[i]{
// // 					msg.splice(i,1) 
// // 				})
// // 			}
// // 		}
// // 	}

// // 	return {
// // 		listen : listen,
// // 		trigger : trigger,
// // 		remove : remove
// // 	}

// // })()


```