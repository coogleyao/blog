# javascript的闭包

今天又在无忧看到闭包的使用了，整理一下闭包的东西。

闭包的定义非常晦涩——闭包，是指语法域位于某个特定的区域，具有持续参照（读写）位于该区域内自身范围之外的执行域上的非持久型变量值能力的段落。这些外部执行域的非持久型变量神奇地保留它们在闭包最初定义（或创建）时的值（深连结）。简单来说，闭包就是在另一个作用域中保存了一份它从上一级函数或作用域取得的变量（键值对），而这些键值对是不会随上一级函数的执行完成而销毁。周爱民说得更清楚，闭包就是“属性表”，闭包就是一个数据块，闭包就是一个存放着“Name=Value”的对照表。就这么简单。但是，必须强调，闭包是一个运行期概念。

在Javascript中闭包(Closure)，有两个特点：

作为一个函数变量的一个引用 - 当函数返回时，其处于激活状态。
一个闭包就是当一个函数返回时，一个没有释放资源的栈区。
现在比较让人认同的闭包实现有如下三种:

```javascript 
with(obj){
  //这里是对象闭包
}

(function(){
  //函数闭包
})()

try{
  //...
} catch(e) {
  //catch闭包 但IE里不行
}
```

几个有用的示例:
```javascript
//*************闭包uniqueID*************
uniqueID = (function(){ //这个函数的调用对象保存值
  var id = 0; //这是私有恒久的那个值
  //外层函数返回一个有权访问恒久值的嵌套的函数
  //那就是我们保存在变量uniqueID里的嵌套函数.
  return function(){return id++;};  //返回,自加.
})(); //在定义后调用外层函数. 
document.writeln(uniqueID()); //0
document.writeln(uniqueID()); //1
document.writeln(uniqueID()); //2
document.writeln(uniqueID()); //3
document.writeln(uniqueID()); //4
//*************闭包阶乘*************
var a = (function(n){
  if (n<1) { alert("invalid arguments"); return 0; }
  if (n==1) {
    return 1;
  } else {
    return n * arguments.callee(n-1);
  }
})(4);
document.writeln(a);
function User( properties ) {
  //这里一定要声明一个变量来指向当前的instance
  var objthis = this;
  for ( var i in properties ) {
    (function(){
      //在闭包内，t每次都是新的，而 properties[i] 的值是for里面的
      var t = properties[i];
      objthis[ "get" + i ] = function() {return t;};
      objthis[ "set" + i ] = function(val) {t = val;};
    })();
  }
}

//测试代码
var user = new User({
  name: "Bob",
  age: 44
});

alert( user.getname());
alert( user.getage());

user.setname("Mike");
alert( user.getname());
alert( user.getage());

user.setage( 22 );
alert( user.getname());
alert( user.getage());
```



附上今天在无忧看到的问题：

要求：

让这三个节点的Onclick事件都能正确的弹出相应的参数。

1.使用函数闭包
```javascript
var lists = document.getElementsByTagName("li");
for(var i=0,l=lists.length; i < l; i++){
  lists[i].onclick = (function(i){ // 保存于外部函函数
    return function(){
      alert(i);
    }
  })(i);
}

var lists = document.getElementsByTagName("li");
for(var i=0,l=lists.length; i < l; i++){
  (function(i){ // 保存于外部函函数
    ists[i].onclick = function(){
      return function(){
          alert(i);
        }
  })(i)
}

var lists = document.getElementsByTagName("li");
for(var i=0,l=lists.length; i < l; i++){
  lists[i].onclick = new function(){
    var t = i;
    return function(){
      alert(t+1)
    }
  }
}
```

2.利用事件代理
```javascript
var ul = document.getElementsByTagName("ul")[0];
ul.onclick = function(){
  var e = arguments[0] || window.event,
  target = e.srcElement ? e.srcElement : e.target;
  if(target.nodeName.toLowerCase() == "li"){
    alert(target.id.slice(-1))
  }
}
```

3.将暂时变量保留于元素节点上
```javascript
var lists = document.getElementsByTagName("li");
for(var i=0,t=0,el; el = list[i++];){
  el.i = t++
  el.onclick = function(){
    alert(this.i)
  }
}
```

4.使用with语句造成的对象闭包
```javascript
var els = document.getElementsByTagName("li");
for(var i=0,n=els.length;i&lt;n;i++){
  with ({i:i})
  els[i].onclick = function() { alert(this.innerHTML+i) };
}
```

5.使用try...catch语句构造的异常闭包
```javascript
var lists = document.getElementsByTagName("li");
for(var i=0,l=lists.length; i < l; i++){
  try{
    throw i;
  }catch(i){
    lists[i].onclick =  function(){
      alert(i)
    }
  }
}
```

6.还是使用函数闭包，虽然看起来有点不一样
```javascript
var els = document.getElementsByTagName("li");
(''+Array(els.length+1)).replace(/./g,function(a,i){
  els[i].onclick=function(){alert(i)}
})
```