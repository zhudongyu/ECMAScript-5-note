##一、获取本周周一的日期
STEP-1
```JavaScript
//获取本周周一时间
var date = new Date()
var getFirstDayOfWeek = function(date) {
    let day = date.getDay() || 7;
    return new Date(date.getFullYear(), date.getMonth(), date.getDate() + 1 - day);
}
```
STEP-2
```JavaScript
//将时间戳转化 xxxx-xx-xx格式
var changeTheDate = function (timeStamp) {
    var date = timeStamp ? new Date(timeStamp) : new Date();
    var Y = date.getFullYear() + '-';
    var M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1) + '-';
    var D = date.getDate() < 10 ? '0'+ date.getDate() : date.getDate() + '';
    var theDayIs = Y+M+D;
    return theDayIs;
}
```
##二、获取本月1号是周几
```JavaScript
var date = new Date()                   // date : 2017-11-09
var nowYear = date.getFullYear()        // nowYear : 2017
var nowMonth = date.getMonth()          // nowMonth : 10
var nowDay = date.getDate()             // nowDay : 9

date.setFullYear(nowYear)               // date : 2017-11-09
date.setDate(1)                         // date : 2017-11-01
date.setMonth(nowMonth)                 // date : 2017-11-01
var firstDayOfMonthIs = date.getDay()   // firstDayOfMonthIs : 3(周三)
```
##三、获取本月最后一天日期
```JavaScript
//当前时间是 2017-11-09
var date = new Date()
var year = date.getFullYear()
var month = date.getMonth()+1
var lastDayOfMonth = new Date(year,month,0) // 2017-11-30
```

