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



