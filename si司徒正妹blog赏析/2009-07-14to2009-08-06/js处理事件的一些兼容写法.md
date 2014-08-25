# javascript处理事件的一些兼容写法

### 绑定事件
```javascript
var addEvent = function (obj, type, fn) {
  if (obj.addEventListener){
    obj.addEventListener(type, fn, false);
  }else if (obj.attachEvent) {
    obj['e'+type+fn] = fn;
    obj.attachEvent( 'on'+type, function() {
      obj['e'+type+fn]();
    });
  }
}
```
另一个实现
```javascript
var addEvent = (function (obj, type, fn) {
  if (document.addEventListener){
    return function (obj, type, fn) {
      obj.addEventListener(type, fn, false)
    }
  }else{
    return function (obj, type, fn) {
      obj.attachEvent('on'+type, function(){
        fn.call(obj, window.event)
      })
    }
  }
})()
```

### 绑定onpropertychange事件
onpropertychange是微软制造的一个事件，它在一个元素的属性发生变化的时候触发，常见的有文本的长度改变，样长改变等，FF大致和它相似的属性为oninput事件，不过它只针对textfield与textarea的value属性。safari，firefox，chrome与opera都支持此属性。

```javascript
var addPropertyChangeEvent = function (obj,fn) {
  if(window.ActiveXObject){
    obj.onpropertychange = fn;
  }else{
    obj.addEventListener("input",fn,false);
  }
}
```

### 移除事件
```javascript
var removeEvent = function( obj, type, fn ) {
  if (obj.removeEventListener)
    obj.removeEventListener( type, fn, false );
  else if (obj.detachEvent) {
    obj.detachEvent( "on"+type, obj["e"+type+fn] );
    // release  memory in IE
    obj["e"+type+fn] = null;
  }
};
```

另一个实现
```javascript
var removeEvent = (function (obj, type, fn) {
  if (document.removeEventListener){
    return function (obj, type, fn) {
      obj.removeEventListener(type, fn, false)
    }
  }else{
    return function (obj, type, fn) {
      obj.detachEvent('on'+type, function(){
        fn.call(obj, window.event);
      })
      // release  memory in IE
      obj["e"+type+fn] = null;
    }
  }
})()
```

### 加载事件
```javascript
var loadEvent = function(fn) {
  var oldonload = window.onload;
  if (typeof window.onload != 'function') {
    window.onload = fn;
  }else {
    window.onload = function() {
      oldonload();
      fn();
    }
  }
}
```

### 阻止事件
```javascript
var stopEvent = function(e){
  e = e || window.event;
  if(e.preventDefault) {
    e.preventDefault();
    e.stopPropagation();
  }else{
    e.returnValue = false;
    e.cancelBubble = true;
  }
}
```

如果仅仅是阻止事件冒泡

```javascript
var stopEvent = function(e){
  e = e || window.event;
  if(!+'\v1') {
    e.cancelBubble = true;
  }else{
    e.stopPropagation();
  }
}
```

### 取得事件源对象
相当于Prototype.js框架的Event.element(e)
```javascript
var getEvent = function(e){
  e = e || window.event;
  var target = event.srcElement ? event.srcElement : event.target;
  return target;
}
function getEvent() {
  if (window.event) return window.event;
  var c = getEvent.caller;
  while (c.caller) c = c.caller;
  return c.arguments[0];
}
```

或者这个功能更强大，我在开发datagrid时开发出来的，详细用法见《一步步教你实现表格排序（第二部分）》。

```javascript
var getEvent = function(e) {
  var e = e || window.event;
  if (!e) {
    var c = this.getEvent.caller;
    while (c) {
      e = c.arguments[0];
      if (e && (Event == e.constructor || MouseEvent  == e.constructor)) {
        break;
      }
      c = c.caller;
    }
  };
  var target = e.srcElement ? e.srcElement : e.target,
  currentN = target.nodeName.toLowerCase(),
  parentN  = target.parentNode.nodeName.toLowerCase(),
  grandN = target.parentNode.parentNode.nodeName.toLowerCase();
  return [e,target,currentN,parentN,grandN];
}
```
