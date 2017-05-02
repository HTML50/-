4月28日

这两个月写了几个小项目，整理了一下GITHUB，面了两次试。期间遇到一些基本的问题，翻了翻红宝书里面都有提到，于是接着把后半部分过完。（看的太慢了，都2017年了，ES6都快普及了）

# 第十二章 DOM2 DOM3

XHTML的命名空间，这个好像没有遇到过。粗略看一下。



**样式**

dom.style.cssText();

dom.style.setProperty('color',"#fff");

dom.style.getPropertyValue('background');

dom.style.removeProperty('border')



`dom.style.getPropertyCSSValue()`在Chrome 58测试失效，MDN上是这样写的`Only supported via getComputedStyle in Firefox. Returns the property value as a CSSPrimitiveValue or null for shorthand properties.`



获取渲染后的style属性： document.defaultView.getComputedStyle(dom,null);   IE有自己的方法，dom.currentStyle.cssValue.



css规则

````css
div.box {
background-color: blue;
width: 100px;
height: 200px;
}
````

```javascript
var sheet = document.styleSheets[0];
var rules = sheet.cssRules || sheet.rules; //取得规则列表
var rule = rules[0]; //取得第一条规则
alert(rule.selectorText); //"div.box"
alert(rule.style.cssText); //完整的 CSS 代码
alert(rule.style.backgroundColor); //"blue"
alert(rule.style.width); //"100px"
alert(rule.style.height); //"200px"
```



> 与添加规则相似，删除规则也不是实际 Web 开发中常见的做法。考虑到删除规则可能会影响 CSS层叠的效果，因此请大家慎重使用。



**元素大小**

offsetTop, offsetLeft, offsetHeight, offsetWidth

pageXoffset, pageYoffset



**遍历**

nodeIterator, treeWalker



**范围range**

有两种方法选择一定内容，使用场景基本不多。之前写fineReader用到过一次，用于确定选取的文本。不过用到的方法是`window.getSelection()`。



# 第十三章 事件

我就比较擅长`观察员模式`。

事件流：微软是冒泡流，Netscape是事件捕获流。

事件冒泡：从最深层的元素开始，向上冒泡。

捕获：刚好相反的过程。



`<input type="button" value="Click Me" onclick="alert('Clicked')" />`这样的写法是`HTML事件处理程序`。这种写法比较便捷，存在的问题是，当onclick所指定的函数未加载完毕时，会报错。

`DOM0 级事件处理程序`是这样形式的写法：

```javascript
btn.onclick = function(){
alert("Clicked");
};
btn.onclick = null; 
```

> 通过 JavaScript 指定事件处理程序的传统方式，就是将一个函数赋值给一个事件处理程序属性。这种为事件处理程序赋值的方法是在第四代 Web 浏览器中出现的，而且至今仍然为所有现代浏览器所支持。原因一是简单，二是具有跨浏览器的优势。



`DOM2 级事件`： `addEventListener()`和` removeEventListener() `。



>大多数情况下，都是将事件处理程序添加到事件流的冒泡阶段，这样可以最大限度地兼容各种浏览
>器。最好只在需要在事件到达目标之前截获它的时候将事件处理程序添加到捕获阶段。如果不是特别需要，我们不建议在事件捕获阶段注册事件处理程序。

写的很明白，我就不总结了。

IE事件略过。



理解冒泡、捕获阶段的一个例子

css

```css
#inner{
width:100px;
height:100px;
background:red;
}
#outer{
width:200px;
height:200px;
background:blue;
}
```

html

```html
<div id='outer'>
    <div id='inner'></div>
</div>
```

js

```javascript
document.getElementById('inner').addEventListener('click', showId);
document.getElementById('outer').addEventListener('click', showId,true);//强制在捕获阶段生效

function showId() {
    alert(this.id);
}
```

