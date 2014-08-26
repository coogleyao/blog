# 转：ie6与firefox操作iframe中DOM节点的一点不同

原地址：[http://bluehua.org/2008/11/01/96.html](http://bluehua.org/2008/11/01/96.html)

依次在两个浏览器中运行以下代码


<html>
  <body>
    <iframe id="myiframe"></iframe>
  </body>
</html>
<script type="text/javascript">
```javascript
  var doc = document.getElementById('myiframe').contentWindow.document;
  var textNode = document.createTextNode('yes~');
  doc.open();
  doc.write('<html><body></body></html>');
  doc.close();
  doc.body.appendChild(textNode);
```
</script>


```javascript
<html>
	<body>
		<iframe id="myiframe"></iframe>
	</body>
</html>
<script type="text/javascript">
	var doc = document.getElementById('myiframe').contentWindow.document;
	var textNode = doc.createTextNode('yes~');
	doc.open();
	doc.write('<html><body></body></html>');
	doc.close();
	doc.body.appendChild(textNode);
</script>
```

```javascript
<html>
	<body>
		<iframe id="myiframe"></iframe>
	</body>
</html>
<script type="text/javascript">
	var doc = document.getElementById('myiframe').contentWindow.document;
	doc.open();
	doc.write('<html><body></body></html>');
	doc.close();
	var textNode = doc.createTextNode('yes~');
	doc.body.appendChild(textNode);
</script>
```

三段代码在firefox下面都是ok的,但是只有第三段在ie6下面能正常运行,前两段都会报参数无效的错误…… 这说明在ie6下只有使用iframe当前document生成的节点才能被append到DOM中,其他insertBfore..同理

IE8已和其他游览器一致了！

________
以上是原文。
只有中间那段在ie7，8不能运行。
其实真正的原因是:
  在ie中，当`doc.open()`的时候，基于*iframe*创建的文本节点被移除，`文本节点对象` 变成了 `空对象null`，所以会报`参数无效`的错误。
  firefox 和 chrome 无此问题。

  另外，ie 必需手动`doc.open();doc.close();`，firefox 和 chrome 无需手动 `doc.open();doc.close();`，不过调用也不会影响什么。

  当然，要是能在节点操作完之后 `textNode = null;`，那一定是极好的了。

由于是在mac上运行的pd，没对ie6进行测试，只测试ie7，ie8。ie6应该同理。

