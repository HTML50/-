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



**defer与async**

```javascript
<script type='text/javascript' src='test.js' defer='defer'></script>
<script type='text/javascript' src='test.js' async></script>
```

defer是告诉浏览器，下载完成全部资源后，再执行js，就是单词本身意思，延迟执行。

async是异步下载，页面不会等待js文件下载的完成，完成异步加载页面。



xhtml的用法略过没看。

文档模式<!docutype>也粗略的扫了一眼，现在都使用html5了。还有<noscript>也是之前的概念了。



___

# **第三章 基本概念**



ES5中提出了严格模式，可以在某个函数中具体应用。

```javascript
function test(){
  'use strict';
  //do something
}
```



推荐使用`;`放在每一个语句的结尾，避免错误，增强可读性。

推荐`{}`包裹住代码块，即使只有一条语句。



**保留字，关键字**

从这里看到，ES5中的保留字`let`,`super`,`class`等在ES6中都用到了。



**变量**

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



**数据类型**

String,Number,Null,Boolean,Undefined,Object



**typeof操作符**

typeof不是一个函数。

`typeof null`返回`object`



**undefined类型**

```javascript
var msg;
//var age

alert(typeof msg); //undefined
alert(typeof age); //undefined
```

虽然age没有声明，但依然显示undefined





**null类型**

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



**Boolean类型**

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



2月13日

 **浮点数**

`0.1 + 0.2 != 0.3`

具体为何如此，要看IEEE 754标准，十进制转为二进制。



**数值范围**

由于内存限制，ECMAscript有数值的范围。Number.MIN_VALUE和Number.MAX_VALUE，前者几乎是0，后者几乎是无穷大(Infinity)。



**NaN**

`NaN == NaN`,false。



**数值转换**

Number(),parseInt(),parseFloat()。

Number('')  is 0;

parseInt('') is NaN;



**转化为字符串**

var.toString() //除了null undefined不能使用这个方法。

String()



2月15日



**位操作符**



**~ 按位非(NOT)操作**

本质是操作数的负值减1

例如25按位非，得到-26

因为-25的二进制是25的补码，也是25原码的反码+1（等于25按位非+1)

所以-25 就是 25按位非 + 1，非操作就是 -25 -1 = -26



按位与(AND) &

按位或(OR) |

按位异或(XOR) ^

左移 << 右移 >>



**布尔操作符**

! && ||



**乘性操作符**

/ * %

该类操作都会先自动进行类型转化Number()。



**加性操作符**

+-



相等 == 、全等 ===

赋值 =

不仅有 += , -= ,还有 *=,/,%=,<<=,>>=,>>>=多种符合赋值操作符。



**语句**



**Label**

和break,continue配合使用，有点goto的意思

```javascript
var num = 0;

outermost:
for (var i=0; i < 10; i++) {
  for (var j=0; j < 10; j++) {
    if (i == 5 && j == 5) {
      continue outermost;
    }
    num++;
  }
}

alert(num);    //95
```



**Switch**

switch()中的内容不仅可以是一个数值，也可以是表达式。

case也可是表达式。

```javascript
var num = 25;
switch (true) {
  case num < 0: 
    alert("Less than 0.");
    break;
  case num >= 0 && num <= 10: 
    alert("Between 0 and 10.");
    break;
  case num > 10 && num <= 20: 
    alert("Between 10 and 20.");
    break;
  default: 
    alert("More than 20.");
}
```

我还测试了使用函数作为switch的内容：

```javascript
switch (test()) {
  case 1: 
    alert("1");
    break;
  case 2: 
    alert("2");
    break;
  default: 
    alert("not 1");
}

function test(){
  return 1;
}
```



**函数**

在严格模式下，函数的参数不可以重复，不能使用eval,arguments作为参数名。

```javascript
function test(e,e){
'use strict'
console.log(e);
}
test(1,2)
```



# **第四章 变量、作用域和内存问题**

引用类型值、基本类型值

```javascript
var name = "Nicholas";
name.age = 27;
alert(name.age); //undefined
```

基本类型值（这里是字符串类型）是无法添加属性的。



```javascript
var obj1 = new Object();
var obj2 = obj1;
obj1.name = "Nicholas";
alert(obj2.name); //"Nicholas"
```

引用类型值（对象Object）的赋值，相当于指针。obj1,obj2都指向同一个对象。



2月16日

传递参数这里讲的很拗口。

```javascript
function test(num){
  return num + 20;
}

var count = 20;
var rs = test(count);
alert(rs); 	  // 40
alert(count); // 20
```

就是说，函数的参数(num)与传入参数(count)是独立的，并不会因参数的改变影响传入参数。



如果传入值是一个object，参数也是将object的指向地址复制，改变参数的指向，无法改变传入的object的指向。



instanceof, typeof

typeof用来检测进本数据类型，instanceof用于检测是何种引用类型。



**执行环境 作用域**

作用域链



**延长作用域链**

```javascript
function buildUrl(){
var qs = "?debug=true";

with(location){
var url = href+qs;
}
return url;
}

alert(buildUrl())
//显示当前页面地址+'?debug=true'
```



**没有块级作用域**

```javascript
if(true){
  var color='red'
}
alert(color) //red


for(var i=0;i<10;i++){
  //to do
}
alert(i)
```

这些看似在{}块内包裹的局部变量，都可以在全局作用域访问得到，因为JS不存在块级作用域。



# **第五章 引用类型**

**Object**

一个对象中的属性，有两种访问方式：

```javascript
var person = {
"name" : 'Tom'
}

alert(person.name);
alert(person[name]);
```

点表示法，方括号表示法。

括号表示法可以使用变量，特殊字符，关键字保留字等。点表示法不行。



toString(),toLocaleString()

一般object在输出时都自动执行的是toString()。

toLocaleString()是按本地的习惯输出内容。

举个显而易见的例子：

```javascript
var date = new Date();
console.log(date.toLocaleString())  //2017/2/16 下午8:14:52
console.log(date.toString())		//Thu Feb 16 2017 20:14:52 GMT+0800 (中国标准时间)
```



**Array**

array.reverse() 将数组反向

array.sort() 排序，比较的是元素toString()后的字符串大小。

如果要想自定义排序，需要自己写一个比较函数。

```javascript
function compare(a, b) {
  if(a>b){
  return -1;
  }
  else if(a<b){
  return 1;
  }
  else{
  return 0;
  }
}

var values = [0,2,6,9,22,1,5,10,15];
var arr = ['a','b','c','A','B','C','aa','ab','ac','ba','bb','bc','Aa','AA'];
values.sort(compare);
arr.sort(compare);

console.log(values); 
console.log(arr); 
```

> 比较函数接收两个参数，如果第一个参数应该位于第二个之前则返回一个负数，如果两个参数相等则返回 0，如果第一个参数应该位于第二个之后则返回一个正数。

即，如果按降序排列，比较函数的a>b就要返回-1，升序就要返回1。

如果数组是纯数字或字符串形式的数字，那么可以不判断a和b的大小之后再return，直接`return a-b;`或者`return b-a`来按升降序排列。



2月17日

**操作方法**

Array.concat();

连接参数至数组。

```javascript
var colors = ["red", "green", "blue"];
var colors2 = colors.concat("yellow", ["black", "brown"]);
alert(colors); //red,green,blue
alert(colors2); //red,green,blue,yellow,black,brown
```

之前写笔试，其中有一个是复制数组。我用的是for循环，把每一项数组元素push到新建的数组内。效率太低了。使用无参数的concat()可以直接获得一个数组。



Array.slice();

```javascript
var colors = ["red", "green", "blue", "yellow", "purple"];
var colors2 = colors.slice(1);
var colors3 = colors.slice(1,4);
alert(colors2); //green,blue,yellow,purple
alert(colors3); //green,blue,yellow
```

>在只有一个参数的情况下， slice() 方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和**结束位置之间**的项——但**不包括**结束位置的项。注意， slice() 方法不会影响原始数组。



Array.splice();

```javascript
var values = [0,2,6,9,22,1,5,10,15];
var rs = values.splice(0);
console.log(values,rs)
```

删除和插入两种方法。有返回值。



**位置查找**

indexOf(),lastIndexOf()

正向，反向查找一个值在数组中的位置。

可以指定从第几位开始查找。

```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
alert(numbers.indexOf(1,1)); //8
alert(numbers.indexOf(1,2)); //8
alert(numbers.indexOf(1,8)); //8
```

从numbers中查找1，分别从1,2,8位查找，结果都是在数组numbers下标8中找到。不管从第几位开始找，最终找到的位置（元素唯一的情况下，本例就找后面的1，如果后面有两个1，就返回第一个1的位置）是确定的。





> 在比较第一个参数与数组中的每一项时， 会使用全等操作符； 也就是说， 要求查找的项必须严格相等 （就像使用===一样） 。

```javascript
var person = { name: "Nicholas" };
var people = [{ name: "Nicholas" }];
var morePeople = [person];

console.log(people.indexOf(person)); //-1
console.log(morePeople.indexOf(person)); //0
```

有个概念需要明白，`{ name: "Nicholas" }  === { name: "Nicholas" }`的结果是false，因为对象只有和自己相等、全等。内容一样的两个对象是不等的，因为在内存中的地址不同。



**迭代方法**

every() ：对数组中的每一项运行给定函数，如果该函数对每一项都返回 true ，则返回 true 。
some() ：对数组中的每一项运行给定函数，如果该函数对任一项返回 true ，则返回 true 。
filter() ：对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。
forEach() ：对数组中的每一项运行给定函数。这个方法没有返回值。
map() ：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。

这几种方法大同小异

```javascript
var arr = [1,2,3,4,5]

arr.forEach(function(item,index,array){
console.log(item*2);
})
```



reduce(),reduceRight()

```javascript
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
return prev + cur;
});
alert(sum); //15
```

> 第一次执行回调函数， prev 是 1， cur 是 2。第二次， prev 是 3（1 加 2 的结果） ， cur 是 3（数组的第三项） 。这个过程会持续到把数组中的每一项都访问一遍，最后返回结果。



2月18日

（

今天顺着一些小问题上网搜了搜。比如eval的用法，引出了一大堆新的疑问，发现自己真的是在入门阶段，随着逐层深入，理论也会越发复杂。有一些提问者就说，学习某本书的时候，有些代码看不懂。我就发现，自己不用急着实验摸索，先把书本老实学习完再说。之前也写过简单的博文，分析一下代码，现在看来，书上应该比我自己研究的更精确。以后有什么尝试，又不写文章论述了，反正有些地方自己也讲不明白，而且还有错的地方，意识到还要去修改。就借着学习笔记总结总结，就可以了。

看代码：

```javascript
function test(a){
  console.log(a === arguments[0]) //true
  console.log(a,arguments[0])
  var a=1;
  console.log(a,arguments[0])
}
  
test(2)  //2,2
		//1,1
```

这说明，函数的参数变量，作用域在函数内（局部作用域），和arguments是等价的。

在内部如果有和参数名相同的变量，就会起冲突，这里的例子就把参数变量覆盖了。



之前写过一篇博文，是关于var,this的实验，里面得到的结论是var和this新建的变量，本质都是一样的，只是this是改变了作用域。

今天在研究new Object和new Function出来的对象属性区别时，就又遇到了同样的问题。

先写了一个这样的代码：

```javascript
  
function person(name,age){
  this.name = name;
  this.age = age;
}

var joy = new person('joy',12);
var tom = {
name:'tom',
age:13
}


console.log(tom,joy)
//Object {name: "tom", age: 13} person {name: "joy", age: 12}
```

两个对象大同小异。

我又尝试了：

```javascript
function person(name1,age1){
  var name = name1;
  var age = age1;
}
var joy = new person('joy',12);
console.log(joy)
//person {}
```

这说明，name,age没有成为joy的属性。

只能带上this

```javascript
function person(name1,age1){
  this.name = name1;
  this.age = age1;
  console.log(this.abc) //这里是undefined，而不是 error:  abc is not defined.
}
var joy = new person('joy',12);
console.log(joy)
//person {name: "joy", age: 12}
```

也就是说，new出来新对象时，this起了关键作用，已经指向对象本身了，和仅仅执行函数时的this是不同的。

具体的细节，我相信在后面的学习中会更清楚。

）



**RegExp**

等价写法，但是new里面的pattern是字符串形式，需要双转义。

var expression = /pattern/flags;

var expression1 = new RegExp("pattern","flags");



**捕获组模式exec()**

```javascript
var text = "mom and dad and baby";
var pattern = /mom( and dad( and baby)?)?/gi;
var matches = pattern.exec(text);
alert(matches); //["mom and dad and baby", " and dad and baby", " and baby", index: 0, input: "mom and dad and baby"]
alert(matches.index); // 0
alert(matches.input); // "mom and dad and baby"
alert(matches[0]); // "mom and dad and baby"
alert(matches[1]); // " and dad and baby"
alert(matches[2]); // " and baby"
```

多观察，多记忆。



```javascript
var text = "cat, bat, sat, fat";

var pattern1 = /.at/;
var matches = pattern1.exec(text);
alert(matches.index); //0
alert(matches[0]); //cat
alert(pattern1.lastIndex); //0

matches = pattern1.exec(text);
alert(matches.index); //0
alert(matches[0]); //cat
alert(pattern1.lastIndex); //0

var pattern2 = /.at/g;
var matches = pattern2.exec(text);
alert(matches.index); //0
alert(matches[0]); //cat
alert(pattern2.lastIndex); //3

matches = pattern2.exec(text);
alert(matches.index); //5
alert(matches[0]); //bat
alert(pattern2.lastIndex); //8
```

表达式中带`g`标志，和不带`g`标志的区别：继续在上一次的基础上查找。

注意`pattern1.lastIndex`，查找的位置是放在pattern内。3和8都代表字符串的位置，意思是前面的均为匹配内容。



**test()**

> 正则表达式的第二个方法是 test() ，它接受一个字符串参数。在模式与该参数匹配的情况下返回true ；否则，返回 false 。



2月20日

(面试——作业：设计一种算法，保证一个抽奖活动的用户，一二三等奖的中奖情况均匀分布在一天之中，最好能保证中奖的概率会根据访问量动态修正，人越多中奖概率越大）