# JavaScript高级程序设计（第3版）
2月9日

昨天从网上看了看大家推荐的书籍，从今天开始学习热度较高的《JavaScript高级程序设计》，希望从中查漏补缺，获得进步。



上网查了一下，第一版是2006年出的。到现在也有10个年头，开篇讲的就是本书使用ES3作为代码规范。有些地方可能稍显过时。



___

**第一章**讲的是JS的历史，这个看了看大概。





___

**第二章**是JS在HTML中的引用。

其中类似这样的代码：

```javascript
<script type='text/javascript'>
  alert('</script>');
</script>
```

`</scirpt>`是不能在JS代码片段中出现的，会直接闭合`<script>`，强行结束解释器对代码的解释。



defer与async

```javascript
<script type='text/javascript' src='test.js' defer='defer'></script>
<script type='text/javascript' src='test.js' async></script>
```

defer是告诉浏览器，下载完成全部资源后，再执行js，就是单词本身意思，延迟执行。

async是异步下载，页面不会等待js文件下载的完成，完成异步加载页面。



xhtml的用法略过没看。

文档模式<!docutype>也粗略的扫了一眼，现在都使用html5了。还有<noscript>也是之前的概念了。



___

**第三章** 基本概念



ES5中提出了严格模式，可以在某个函数中具体应用。

```javascript
function test(){
  'use strict';
  //do something
}
```



推荐使用`;`放在每一个语句的结尾，避免错误，增强可读性。

推荐`{}`包裹住代码块，即使只有一条语句。



保留字，关键字

从这里看到，ES5中的保留字`let`,`super`,`class`等在ES6中都用到了。



变量

在这里，第一次提出变量作用域。

```javascript
function test(){
  var message ='hi';
}
test();
alert(message); //error: message is not defined
```

函数退出后，变量被摧毁。或者说，定义(var)在函数体内的变量，是定义该变量的作用域中的**局部变量**。



但是这样，去掉var，就是**全局变量**

```javascript
function test(){
  message ='hi';
}
test();
alert(message); //hi
```
但是，**不推荐**去掉`var`，严格模式下也会出错。



数据类型

String,Number,Null,Boolean,Undefined,Object



typeof操作符

typeof不是一个函数。

`typeof null`返回`object`



undefined类型

```javascript
var msg;
//var age

alert(typeof msg); //undefined
alert(typeof age); //undefined
```

虽然age没有声明，但依然显示undefined





null类型

很好的一个用途就是用来保存object

```javascript
var obj=null;

//将obj指定为object
//some code here
//eg:
//obj= car
if(obj != null){
  alert('已经有内容了！')
}
```



null派生自undefined,所以有`null == undefined`为`true`。



Boolean类型

```javascript
var message = 'hello world';
var messageAsBoolean = Boolean(message); // (Boolean) true
```

对于任何非空(`''`)的字符串，都可通过布尔转换为true



对于Number类型，任何非0，包括无穷大Infinity，转化为true。NaN和0，转化为false。



对于Object类型，任何对象为true，null为false。



对于Undefined类型，只能转换为false。


2月10日

（

上午面试去了，项目经理面的，看了看GITHUB和博客，对我那个小游戏比较感兴趣，然后让去做一道上机题：输入框，按钮，输入形如"1,2,3,4,5,1"的参数，去重排序，结果显示在页面上。

我一看比较简单，没听清要求不让使用Array.sort()排序。写的时候，刚开始把document.getElementById('input').value直接放在了`<script>`顶部，导致点击按钮，取得的value是空的，我竟然没检查出来。然后写出来的，忘加排序了。最后排序用的sort()，这一连串的粗心大意，导致直接pass。

下次一定要听清要求，看清楚问题考虑好了再下笔。。。



我回来又重新写了一遍，代码如下

```html
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
</head>
<body>
<div>
	请输入: <input id='inputData' type='text'>
	<input type='button' onclick='sort()' value='去重排序'>
	
	<div id='rsArea'></div>
</div>

<script>
function sort(){
var input= toArray(document.getElementById('inputData').value),
	output = document.getElementById('rsArea');
	output.innerText = '结果是:' +arrSort(arrDup(input));
}

function toArray(str){
	return eval('['+str+']');
}

function arrDup(arr){
var rs=[];
for(i=0;i<arr.length;i++){
if(rs.indexOf(arr[i])==-1){
rs.push(arr[i]);
}
}
return rs;
}

function arrSort(arr){
var flag=true,
	k=arr.length;

while(flag){
	flag = false;
	for(i=0;i<k;i++){
		if(arr[i]>arr[i+1]){
			var temp = arr[i+1];
			arr[i+1] =arr[i];
			arr[i] = temp;
			flag = true;
		}
	}
	k--;
}
return arr;
}
</script>

</body>
</html>
```



冒泡排序写的时候又忘了，看来算法要经常练。

字符串转数组也需要复习一下(eval)。

）


