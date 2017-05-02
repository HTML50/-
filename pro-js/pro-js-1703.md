3月1日

# 第十章 DOM

nodeList转换为数组：

`Array.prototype.slice.call(NodeList,0)`

```html
<div id='parent'>
<div id='child1'>child1-text</div>
<div id='child2'>child2-text</div>
</div>


<script>
var node = document.getElementById('parent'),
    arr = Array.prototype.slice.call(node.childNodes,0);
  
alert(node.childNodes);		//nodeList[5]
alert(arr);					//Array[5]

//[text, div#child1, text, div#child2, text]

</script>
```

数组里的元素类型还是HTMLDivElement。



3月2日

```html
<div id='parent'><div id='child1'>child1-text</div>
<div id='child2'>child2-text</div>
</div>

<script>
var node = document.getElementById('parent');

alert(node.firstChild,node.firstChild.nodeType);//<div id="child1">child1-text</div> 1
alert(node.lastChild,node.lastChild.nodeType);//#text 3
</script>
```

上面的3行div结构是：

div#parent 

​	div#child1

​	text: '换行符'

​	div#child2

​	text: '换行符'

所以node.firstChild和node.lastChild并不像看上去那样是两个div。换行符也会被当做是一个节点。

对节点进行操作时，严谨的做法是先对节点的类型进行判断，如果是元素节点，1，再操作。之前我就犯过这样的错误，取值取不到想要的内容。



document.createDocumentFragment();文档片段可以提高渲染效率，比如将多个LI添加到UL。先生成一个文档片段，再添加至文档。



动态添加script的方法有两种：载入js文件、设定script内容，然后appendChild到body。

css类似。



> NodeList 对象都是“动态的” ，这就意味着每次访问NodeList 对象，都会运行一次查询。有鉴于此，最好的办法就是尽量减少 DOM 操作。



# 第十一章 DOM扩展

 **querySelector()**

```javascript
//取得 body 元素
var body = document.querySelector("body");
//取得 ID 为"myDiv"的元素
var myDiv = document.querySelector("#myDiv");
//取得类为"selected"的第一个元素
var selected = document.querySelector(".selected");
//取得类为"button"的第一个图像元素
var img = document.body.querySelector("img.button");
```

**querySelectorAll()**

```javascript
//取得某<div>中的所有<em>元素（类似于 getElementsByTagName("em")）
var ems = document.getElementById("myDiv").querySelectorAll("em");
//取得类为"selected"的所有元素
var selecteds = document.querySelectorAll(".selected");
//取得所有<p>元素中的所有<strong>元素
var strongs = document.querySelectorAll("p strong");
```



**非文本节点（换行）元素遍历**

childElementCount ：返回子元素（不包括文本节点和注释）的个数。
firstElementChild ：指向第一个子元素； firstChild 的元素版。
lastElementChild ：指向最后一个子元素； lastChild 的元素版。
previousElementSibling ：指向前一个同辈元素； previousSibling 的元素版。
nextElementSibling ：指向后一个同辈元素； nextSibling 的元素版。



**HTML5**

 **getElementsByClassName()**

**classList**

```html
<div class="bd user disabled">删除这个div class中的user属性</div>

//以前的方法
<script>
//有未完善的地方。比如查找时className必须完全一致，多一个空格都不行。
Document.prototype.getElementByClassName = function(name){
var nodes = document.getElementsByTagName('div'); //不区分大小写
for(var i=0;i<nodes.length;i++){
if(nodes[i].className == name){
return nodes[i];
}
}
}

//创建HTMLElement原型方法
HTMLElement.prototype.removeClass = function(name){
var arr = this.className.split(' ');
for(var i=0;i<arr.length;i++){
if(name == arr[i]){
arr.splice(i,1);
break;
}
}
this.className = arr.join(' ');
}

var target = document.getElementByClassName('bd user disabled');
target.removeClass('user');
alert(target);
</script>

//html5方法
<script>
var target = document.querySelector('.bd');
target.classList.remove('user');
alert(target);
</script>
```

展示了删除className多项中的某一项，html5新增的api只需两行。



**document.hasFocus()** 

**readyState**

```javascript
if (document.readyState == "complete"){
//执行操作
}
else if (document.readyState == "loading"){
//执行操作
}
```

**compatMode**

**document.head**

**document.charset**

**自定义数据属性**

```html
<div id="myDiv" data-appId="12345" data-myname="Nicholas"></div>
```

可用dataset访问自定义属性。在最新版本的CHROME中测试，不加data-前缀也可以正常自定义数据。



innerText会改变所有子文本节点中的内容；会对Html语法编码。
