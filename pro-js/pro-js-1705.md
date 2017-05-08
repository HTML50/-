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

```css
div.box {
background-color: blue;
width: 100px;
height: 200px;
}
```

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



> 大多数情况下，都是将事件处理程序添加到事件流的冒泡阶段，这样可以最大限度地兼容各种浏览
> 器。最好只在需要在事件到达目标之前截获它的时候将事件处理程序添加到捕获阶段。如果不是特别需要，我们不建议在事件捕获阶段注册事件处理程序。

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

总结下来就是，现代浏览器为了兼容原先不同的方案，采取的是折中的办法，先进行捕获阶段，再进行冒泡阶段。有一个来回的过程。



5月3日

（新买了个联想小新笔记本，折腾了1天半没安好UBUNTU，上网查了查，说硬盘模式写死成sata+raid，读不了硬盘。于是暂时先不折腾，等过一段时间有方案再研究）



**事件类型**

UI事件（load, unload, select, resize, scroll)

之前总是在国外网站上遇到这种提示，关闭网页时，提示有更改未保存，是否留下。

```html
<body onunload='alert(1)';>
```

```javascript
window.addEventListner('unload',function(){alert(1)});
window.onunload = function(){alert(1)};
```

这样做都是办不到的，只有这样

```javascript
window.onbeforeunload = function(){
  return '确定要离开吗？';
}
```

直接`return`点文字，随便是什么，就可以呼出确认框。

我刚开始以为是没有测试服务器`127.0.0.1`的缘故，专门下载了一个HTTP server还，经过几番测试，就是这个原因，书本上面写的内容有些过时。



5月4日

`document.implementation.hasFeature`方法过时了。详见https://developer.mozilla.org/en-US/docs/Web/API/DOMImplementation/hasFeature



看源码的时候看见个不常见的用法：

```javascript
window['onclick'] = function(){
  alert('clicked');
}
```

这是给window增加eventListener的一种写法。想了想window是一个对象，这样写也没什么错误。

数组也是可以这样赋值的，但好像一般情况下都是按照默认的元素序号排列。



5月5日

（昨天折腾了一下午WINDOWS主题，最终各种软件都卸载了，还是使用最原始的Win+R）

鼠标事件、键盘事件

监听按下功能键时的左右位置

```javascript
test.addEventListener('keyup',function(e){
  var loc = e.location || e.keyLocation;
  if (loc){
 	alert(loc);
  }
})
```



在可输入内容处的监听，比keypress高级。

```javascript
test.addEventListener('textInput',function(e){
  console.log(e.data);
})
```



变动事件是节点增删改的监控。

contextmenu是鼠标事件。

DOMContentLoaded在DOM树形成后触发。与window.load不同的是，不考虑图像、js、css等资源的加载与否。





5月8日

DOM.readystatechange与ajax的不同。chrome中有interactive和complete两个状态。



hashchange监控URL`#`后参数的变化。



移动设备事件：设备水平状态，加速度。



手势事件：touchstart, touchmove, touchend, touchcancel

APPLE专有手势事件。



**事件委托**

（前一段面试的时候，面试官问我知道事件委托吗。我一想，委托不是C#里面学的delegate吗，好像跟事件监听有关，不过都忘了。又想了想JS里面跟事件相关的不就是eventListener的概念，比较模糊，支支吾吾答不出来，今天一看名词解释，就是把多个事件的监听处理放在一起，真是好）

适合采用委托技术的事件为click, mousedown, mouseup, keydown, keyup, keypress。这样一想，监听键盘不同按键作出不同反应的事件处理就是一个委托。



使用innerHTML移除带有事件处理的内容时，有可能无法移除内存中对其的事件处理引用。



模拟鼠标、键盘事件都已经被规范弃用，详见https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/initKeyboardEvent



# 第十四章 表单脚本

document.forms[0].elements可访问所有表单项

document.forms[0].elements['name']指定name表单项访问



autofocus会自动聚焦。最早baidu就没有搜索框焦点，每次都要手动聚焦，google就不用。



表单的keyup,，keydown，keypress事件有个bug，比如要检测当前输入的内容合法性，需要下一次操作才能检测上一次输入的内容，解决方法是使用`setTimeout`延迟函数。

```javascript
textBox.addEventListener("keydown", function(){
    setTimeout(function(){
    if (/[^\d]/.test(textBox.value)){
        textBox.style.backgroundColor = "red";
    } else {
        textBox.style.backgroundColor = "#fff";
    }
    },0)
});
```



required是HTML5 api中检查字段内容的方法。

```HTML
<input type='text' required>
```

还有新增的email, url type

```html
<input type='email' required>
<input type='url' required>
```

输入不合规范，会提示。