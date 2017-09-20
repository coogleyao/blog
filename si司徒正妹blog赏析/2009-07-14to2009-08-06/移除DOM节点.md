# remove DOM node

在IE中移除容器类节点，会引起内存泄露，最好是创建一个新的节点，比如div，然后将要删除的节点放入这个div中，再将div的innerHTML清空。其它的直接removeChild就可以了

```javascript
// '\v'在ie中不会被转义,仍然是 `'\v'` ; other browser 中是垂直制表符，会被解释为 `''`
var removeNode = ! + '\v1' ? function () {
  
  // create `div` once in currentPage lifeCycle
  var div;
  
  return function (node) {
    if (node && node.tagName != 'BODY') {
      div = div || document.createElement('DIV');
      div.appendChild(node);
      div.innerHTML = '';
    }
  }
}() : function (node) {
        if (node && node.parentNode && node.tagName != 'BODY'){
          node.parentNode.removeChild(node);
        }
      }
```

jQuery 中的实现: 

```javascript
var removeNode = function (node) {
  if (node && node.parentNode) {
    node.parentNode.removeChild(node);
  }
  
  // release  memory in IE
  node = null;
}
```