# javascript测试函数运行速度
通常我们需要对函数进行优化，一般的做法是开始的时候获得时间，结束的时候再获得一次时间，两次时间相减就能到到花费的时间。而函数运行速度之快，基本上都是毫秒级的。下面给出的函数就是就此准备的。

// 时间转为时间戳（毫秒）
```javascript
function time2stamp(){
    var d = new Date();
    return Date.parse(d)+d.getMilliseconds();
}
```

用法:
```javascript
 var t1 = time2stamp();
// 比较各游览器的DOM运行速度。
var divs = document.getElementByTagName("div"); 
var t2 = time2stamp();
alert("耗时：" + (t2 - t1) + " 毫秒");
```

新的方法:
```javascript
var time1 = new Date
// 比较各游览器的DOM运行速度。
var divs = document.getElementByTagName("div"); 
alert("耗时：" + (new Date - time1) + " 毫秒");
```

