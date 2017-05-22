> 4月28日
>
> 这两个月写了几个小项目，整理了一下GITHUB，面了两次试。期间遇到一些基本的问题，翻了翻红宝书里面都有提到，于是接着把后半部分过完。（看的太慢了，都2017年了，ES6都快普及了）
>
> # 第十二章 DOM2 DOM3
>
> XHTML的命名空间，这个好像没有遇到过。粗略看一下。
>
> 
>
> **样式**
>
> dom.style.cssText();
>
> dom.style.setProperty('color',"#fff");
>
> dom.style.getPropertyValue('background');
>
> dom.style.removeProperty('border')
>
> 
>
> `dom.style.getPropertyCSSValue()`在Chrome 58测试失效，MDN上是这样写的`Only supported via getComputedStyle in Firefox. Returns the property value as a CSSPrimitiveValue or null for shorthand properties.`
>
> 
>
> 获取渲染后的style属性： document.defaultView.getComputedStyle(dom,null);   IE有自己的方法，dom.currentStyle.cssValue.
>
> 
>
> css规则
>
> ```css
> div.box {
> background-color: blue;
> width: 100px;
> height: 200px;
> }
> ```
>
> ```javascript
> var sheet = document.styleSheets[0];
> var rules = sheet.cssRules || sheet.rules; //取得规则列表
> var rule = rules[0]; //取得第一条规则
> alert(rule.selectorText); //"div.box"
> alert(rule.style.cssText); //完整的 CSS 代码
> alert(rule.style.backgroundColor); //"blue"
> alert(rule.style.width); //"100px"
> alert(rule.style.height); //"200px"
> ```
>
> 
>
> > 与添加规则相似，删除规则也不是实际 Web 开发中常见的做法。考虑到删除规则可能会影响 CSS层叠的效果，因此请大家慎重使用。
>
> 
>
> **元素大小**
>
> offsetTop, offsetLeft, offsetHeight, offsetWidth
>
> pageXoffset, pageYoffset
>
> 
>
> **遍历**
>
> nodeIterator, treeWalker
>
> 
>
> **范围range**
>
> 有两种方法选择一定内容，使用场景基本不多。之前写fineReader用到过一次，用于确定选取的文本。不过用到的方法是`window.getSelection()`。
>
> 
>
> # 第十三章 事件
>
> 我就比较擅长`观察员模式`。
>
> 事件流：微软是冒泡流，Netscape是事件捕获流。
>
> 事件冒泡：从最深层的元素开始，向上冒泡。
>
> 捕获：刚好相反的过程。
>
> 
>
> `<input type="button" value="Click Me" onclick="alert('Clicked')" />`这样的写法是`HTML事件处理程序`。这种写法比较便捷，存在的问题是，当onclick所指定的函数未加载完毕时，会报错。
>
> `DOM0 级事件处理程序`是这样形式的写法：
>
> ```javascript
> btn.onclick = function(){
> alert("Clicked");
> };
> btn.onclick = null; 
> ```
>
> > 通过 JavaScript 指定事件处理程序的传统方式，就是将一个函数赋值给一个事件处理程序属性。这种为事件处理程序赋值的方法是在第四代 Web 浏览器中出现的，而且至今仍然为所有现代浏览器所支持。原因一是简单，二是具有跨浏览器的优势。
>
> 
>
> `DOM2 级事件`： `addEventListener()`和` removeEventListener() `。
>
> 
>
> > 大多数情况下，都是将事件处理程序添加到事件流的冒泡阶段，这样可以最大限度地兼容各种浏览
> > 器。最好只在需要在事件到达目标之前截获它的时候将事件处理程序添加到捕获阶段。如果不是特别需要，我们不建议在事件捕获阶段注册事件处理程序。
>
> 写的很明白，我就不总结了。
>
> IE事件略过。
>
> 
>
> 理解冒泡、捕获阶段的一个例子
>
> css
>
> ```css
> #inner{
> width:100px;
> height:100px;
> background:red;
> }
> #outer{
> width:200px;
> height:200px;
> background:blue;
> }
> ```
>
> html
>
> ```html
> <div id='outer'>
>     <div id='inner'></div>
> </div>
> ```
>
> js
>
> ```javascript
> document.getElementById('inner').addEventListener('click', showId);
> document.getElementById('outer').addEventListener('click', showId,true);//强制在捕获阶段生效
>
> function showId() {
>     alert(this.id);
> }
> ```
>
> 总结下来就是，现代浏览器为了兼容原先不同的方案，采取的是折中的办法，先进行捕获阶段，再进行冒泡阶段。有一个来回的过程。
>
> 
>
> 5月3日
>
> （新买了个联想小新笔记本，折腾了1天半没安好UBUNTU，上网查了查，说硬盘模式写死成sata+raid，读不了硬盘。于是暂时先不折腾，等过一段时间有方案再研究）
>
> 
>
> **事件类型**
>
> UI事件（load, unload, select, resize, scroll)
>
> 之前总是在国外网站上遇到这种提示，关闭网页时，提示有更改未保存，是否留下。
>
> ```html
> <body onunload='alert(1)';>
> ```
>
> ```javascript
> window.addEventListner('unload',function(){alert(1)});
> window.onunload = function(){alert(1)};
> ```
>
> 这样做都是办不到的，只有这样
>
> ```javascript
> window.onbeforeunload = function(){
>   return '确定要离开吗？';
> }
> ```
>
> 直接`return`点文字，随便是什么，就可以呼出确认框。
>
> 我刚开始以为是没有测试服务器`127.0.0.1`的缘故，专门下载了一个HTTP server还，经过几番测试，就是这个原因，书本上面写的内容有些过时。
>
> 
>
> 5月4日
>
> `document.implementation.hasFeature`方法过时了。详见https://developer.mozilla.org/en-US/docs/Web/API/DOMImplementation/hasFeature
>
> 
>
> 看源码的时候看见个不常见的用法：
>
> ```javascript
> window['onclick'] = function(){
>   alert('clicked');
> }
> ```
>
> 这是给window增加eventListener的一种写法。想了想window是一个对象，这样写也没什么错误。
>
> 数组也是可以这样赋值的，但好像一般情况下都是按照默认的元素序号排列。
>
> 
>
> 5月5日
>
> （昨天折腾了一下午WINDOWS主题，最终各种软件都卸载了，还是使用最原始的Win+R）
>
> **鼠标事件、键盘事件**
>
> 监听按下功能键时的左右位置
>
> ```javascript
> test.addEventListener('keyup',function(e){
>   var loc = e.location || e.keyLocation;
>   if (loc){
>  	alert(loc);
>   }
> })
> ```
>
> 
>
> 在可输入内容处的监听，比keypress高级。
>
> ```javascript
> test.addEventListener('textInput',function(e){
>   console.log(e.data);
> })
> ```
>
> 
>
> 变动事件是节点增删改的监控。
>
> contextmenu是鼠标事件。
>
> DOMContentLoaded在DOM树形成后触发。与window.load不同的是，不考虑图像、js、css等资源的加载与否。
>
> 
>
> 
>
> 5月8日
>
> DOM.readystatechange与ajax的不同。chrome中有interactive和complete两个状态。
>
> 
>
> hashchange监控URL`#`后参数的变化。
>
> 
>
> **移动设备事件**：设备水平状态，加速度。
>
> 
>
> **手势事件**：touchstart, touchmove, touchend, touchcancel
>
> APPLE专有手势事件。
>
> 
>
> **事件委托**
>
> （前一段面试的时候，面试官问我知道事件委托吗。我一想，委托不是C#里面学的delegate吗，好像跟事件监听有关，不过都忘了。又想了想JS里面跟事件相关的不就是eventListener的概念，比较模糊，支支吾吾答不出来，今天一看名词解释，就是把多个事件的监听处理放在一起，真是好）
>
> 适合采用委托技术的事件为click, mousedown, mouseup, keydown, keyup, keypress。这样一想，监听键盘不同按键作出不同反应的事件处理就是一个委托。
>
> 
>
> 使用innerHTML移除带有事件处理的内容时，有可能无法移除内存中对其的事件处理引用。
>
> 
>
> 模拟鼠标、键盘事件都已经被规范弃用，详见https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/initKeyboardEvent
>
> 
>
> # 第十四章 表单脚本
>
> document.forms[0].elements可访问所有表单项
>
> document.forms[0].elements['name']指定name表单项访问
>
> 
>
> autofocus会自动聚焦。最早baidu就没有搜索框焦点，每次都要手动聚焦，google就不用。
>
> 
>
> 表单的keyup,，keydown，keypress事件有个bug，比如要检测当前输入的内容合法性，需要下一次操作才能检测上一次输入的内容，解决方法是使用`setTimeout`延迟函数。
>
> ```javascript
> textBox.addEventListener("keydown", function(){
>     setTimeout(function(){
>     if (/[^\d]/.test(textBox.value)){
>         textBox.style.backgroundColor = "red";
>     } else {
>         textBox.style.backgroundColor = "#fff";
>     }
>     },0)
> });
> ```
>
> 
>
> required是HTML5 api中检查字段内容的方法。
>
> ```HTML
> <input type='text' required>
> ```
>
> 还有新增的email, url type
>
> ```html
> <input type='email' required>
> <input type='url' required>
> ```
>
> 输入不合规范，会提示。
>
> 
>
> 5月9日
>
> ```javascript
> var selectbox1 = document.getElementById("selLocations1");
> var selectbox2 = document.getElementById("selLocations2");
> selectbox2.appendChild(selectbox1.options[0]);	
> ```
>
> 对于已有的元素，使用`appendChild`将其附加到其他位置时，会先移除元素原先的位置，然后再移动。
>
> 
>
> execCommand配合富文本编辑器的时候多，绑定在按钮上，对可编辑区域进行各种操作。
>
> document.queryCommandState('name')用于查看是否执行了相关操作：
>
> ```javascript
> var isBold = frames["richedit"].document.queryCommandState("bold");
> var fontSize = frames["richedit"].document.queryCommandValue("fontsize");
> //如果有执行了操作，该命令查看操作的值
> ```
>
> 
>
> execCommand('copy')在firefox中默认是没有权限执行的。
>
> 
>
> 5月10日
>
> # 第十五章 canvas
>
> 绘制文字、图形、路径、旋转。
>
> canvas的概念不多，我自己也都用过，简单过一下。
>
> 
>
> webGL略过。
>
> 
>
> # 第十六章 HTML5脚本编程
>
> **原生拖动：**
>
> ```html
> <t draggable='true' style='display:block;height:200px;width:400px;background:#ccc;'>hi there. I can fly~</t>
> ```
>
> 默认情况下，文字是可选状态，选中后才可以拖动。然后扔到，比如说地址栏，就可以进行搜索。
>
> 但是加上draggable以后，文字就不可选，拖动也无法扔到任何地方去。
>
> 这时需要设置`dataTransfer`属性：
>
> ```javascript
> t.addEventListener('dragstart',function(e){
> e.dataTransfer.setData('text','hhh')
> });
> ```
>
> 给元素增加一个`dragstart`事件的event listener，意思是，拖动开始时，设置dataTransfer中的内容。这时拖动元素，又可以扔到地址栏中搜索，不过这次搜索的关键字是上面代码设定的`"hhh"`。可以将内容设定为元素本身的内容`console.log(e.srcElement.innerText)`。
>
> 
>
> **历史状态管理:**
>
> ```html
> <a href='javascript:go(1)'>1</a>
> <a href='javascript:go(2)'>2</a>
>
> <script>
> function go(n){
> history.pushState({name:'test',number:n},'what','#'+n)
> }
>
> window.onpopstate = function(e){
> console.log(e.state)
> }
> </script>
> ```
>
> 基本上就是这两个方法放在一起使用，动态更改历史状态，而不真正的发送请求。用于解决单页面APP。比如我写的那个博客就是ajax + history.pushState来加载不同的页面，并实现前进后退。需要注意的是，pushState中的url地址要真实存在，不然报错。
>
> 
>
> # 第十七章 错误处理与调试
>
> **try catch finally**
>
> finally中的代码在任何情况下都会执行。
>
> 
>
> **错误类型**
>
> ```javascript
> throw new SyntaxError("I don’t like your syntax.");
> throw new TypeError("What type of variable do you take me for?");
> throw new RangeError("Sorry, you just don’t have the range.");
> throw new EvalError("That doesn’t evaluate.");
> throw new URIError("Uri, is that you?");
> throw new ReferenceError("You didn’t cite your references properly.");
> ```
>
> 
>
> 5月11日
>
> 面向公众的API必须要进行错误检测。
>
> 
>
> **调试**
>
> 早期没有调试工具时使用alert大法，后来使用控制台。IE常见错误略过（下面默认略过IE的内容。）
>
> 
>
> # 第十八章 javascript与XML
>
> 略过这一章，XML XSLT XPATH DOMPARSER这些词都没遇到过。
>
> 
>
> 
>
> # 第十九章 E4X
>
> 略过。
>
> > **Warning: **E4X is deprecated. It will be disabled by default for content in Firefox 16, disabled by default for chrome in Firefox 17, and removed in Firefox 18. Use [`DOMParser`](https://developer.mozilla.org/en-US/docs/Web/API/DOMParser)/[`DOMSerializer`](https://developer.mozilla.org/en-US/docs/Web/API/DOMSerializer) or a non-native JXON algorithm instead.
>
> 
>
> 
>
> # 第二十章 JSON
>
> JSON(JavaScript Object Notation)
>
> 
>
> **对象**
>
> 字符串需要用双引号`""`裹住，单引号`''`会报错。无需声明`var`。
>
> 这是JSON对象与JS字面量不太相同的地方。
>
> 
>
> **数组**
>
> ```json
> [25,"hi",true]
> ```
>
> JSON的数组是js数组的字面量表示形式。
>
> 
>
> 一个复杂JSON的例子：
>
> ```json
> [
>     {
>         "title": "Professional JavaScript",
>         "authors": [
>             "Nicholas C. Zakas"
>         ],
>         "edition": 3,
>         "year": 2011
>     },
>     {
>         "title": "Professional JavaScript",
>         "authors": [
>             "Nicholas C. Zakas"
>         ],
>         "edition": 2,
>         "year": 2009
>     },
>     {
>         "title": "Professional Ajax",
>         "authors": [
>             "Nicholas C. Zakas",
>             "Jeremy McPeak",
>             "Joe Fawcett"
>         ],
>         "edition": 2,
>         "year": 2008
>     },
>     {
>         "title": "Professional Ajax",
>         "authors": [
>             "Nicholas C. Zakas",
>             "Jeremy McPeak",
>             "Joe Fawcett"
>         ],
>         "edition": 1,
>         "year": 2007
>     },
>     {
>         "title": "Professional JavaScript",
>         "authors": [
>             "Nicholas C. Zakas"
>         ],
>         "edition": 1,
>         "year": 2006
>     }
> ]
> ```
>
> 
>
> 假设上面的例子是一个对象`book`
>
> ```javascript
> var book = [
>   ...
> ]
> ```
>
> 使用`var json = JSON.stringify(book)`，可以将对象序列化，得到JSON字符串：
>
> ```json
> [{"title":"Professional JavaScript","authors":["Nicholas C. Zakas"],"edition":3,"year":2011},{"title":"Professional JavaScript","authors":["Nicholas C. Zakas"],"edition":2,"year":2009},{"title":"Professional Ajax","authors":["Nicholas C. Zakas","Jeremy McPeak","Joe Fawcett"],"edition":2,"year":2008},{"title":"Professional Ajax","authors":["Nicholas C. Zakas","Jeremy McPeak","Joe Fawcett"],"edition":1,"year":2007},{"title":"Professional JavaScript","authors":["Nicholas C. Zakas"],"edition":1,"year":2006}]
> ```
>
> 
>
> JSON.stringify有多种参数，可以指定要序列化的对象、使用函数、设置输出格式。
>
> 比如这样：
>
> ```javascript
> JSON.stringify(json,null,"--")
> ```
>
> 得到
>
> ```json
> [
> --{
> ----"title": "Professional JavaScript",
> ----"authors": [
> ------"Nicholas C. Zakas"
> ----],
> ----"edition": 3,
> ----"year": 2011
> --},
> --{
> ----"title": "Professional JavaScript",
> ----"authors": [
> ------"Nicholas C. Zakas"
> ----],
> ----"edition": 2,
> ----"year": 2009
> --},
> --{
> ----"title": "Professional Ajax",
> ----"authors": [
> ------"Nicholas C. Zakas",
> ------"Jeremy McPeak",
> ------"Joe Fawcett"
> ----],
> ----"edition": 2,
> ----"year": 2008
> --},
> --{
> ----"title": "Professional Ajax",
> ----"authors": [
> ------"Nicholas C. Zakas",
> ------"Jeremy McPeak",
> ------"Joe Fawcett"
> ----],
> ----"edition": 1,
> ----"year": 2007
> --},
> --{
> ----"title": "Professional JavaScript",
> ----"authors": [
> ------"Nicholas C. Zakas"
> ----],
> ----"edition": 1,
> ----"year": 2006
> --}
> ]
> ```
>
> 
>
> 
>
> 5月15日
>
> # 第二十一章 ajax comet
>
> 基础的ajax方法使用过几次，没什么难度。
>
>  W3C规范中定义的 xmlhttprequest2级新增了很多方法：FormData、响应超时、进度事件、跨资源共享等。
>
> 
>
> **其他跨域技术**
>
> 图像PING
>
> JSONP
>
> ajax对于跨域的请求是禁止的。但是img, script, iframe等含有SRC属性的标签可以直接调用跨域文件，所以在script中引用跨域js便可以实现。
>
> 还有一些COMET, SSE, web socket跨域技术，还用不上，略过不看。
>
> 
>
> # 第二十二章 高级技巧
>
> **安全类型检测方法**
>
> ```javascript
> Object.prototype.toString.call('some string') == "[object Function]"
> //false
> ```
>
> 
>
> **作用域安全的构造函数**
>
> 有构造函数如下：
>
> ```javascript
> function Person(name, age, job){
> this.name = name;
> this.age = age;
> this.job = job;
> }
> var person = new Person("Nicholas", 29, "Software Engineer");
> ```
>
> 如果创建实例时忘加`new`操作符，就会导致错误。
>
> 解决方法是在构造函数中增加判断：
>
> ```javascript
> function Person(name, age, job){
> if (this instanceof Person){
> this.name = name;
> this.age = age;
> this.job = job;
> } else {
> return new Person(name, age, job);
> }
> }
> var person1 = Person("Nicholas", 29, "Software Engineer");
> alert(window.name); //""
> alert(person1.name); //"Nicholas"
> var person2 = new Person("Shelby", 34, "Ergonomist");
> alert(person2.name); //"Shelby"
> ```
>
> 
>
> 再进一步，看一下继承的情况：
>
> ```javascript
> function Polygon(sides){
>   this.sides = sides;
>   this.getArea = function(){
>     return 0;
>   }
> }
>
> function Rectangle(width, height){
>   Polygon.call(this, 2);
>   //继承Polygon的属性
>   this.width = width;
>   this.height = height;
>   this.getArea = function(){
>     return this.width * this.height;
>   };
> }
>
> var rect = new Rectangle(5, 10);
> alert(rect.sides);  //2
> ```
>
> `rect`通过构造函数中对于`Polygon`的`call`，继承了`Polygon`函数中的属性。
>
> 但是如果`Polygon`使用前一个例子中的安全作用域，如下所示：
>
> ```javascript
> function Polygon(sides){
>   if (this instanceof Polygon) {
>     this.sides = sides;
>     this.getArea = function(){
>       return 0;
>     };
>   } else {
>     return new Polygon(sides);
>   }
> }
>
> function Rectangle(width, height){
> 	...
> }
>
> var rect = new Rectangle(5, 10);
> alert(rect.sides);  //undefined
> ```
>
> 这样`rect`中就得不到`sides`的属性，因为`this`的缘故，`Polygon`会返回一个`new Polygon`。
>
> 如果想要在这个基础上，仍然继承到`sides`等属性，需要在`new Rectangle`前修改原型链，如下：
>
> ```javascript
> Rectangle.prototype = new Polygon();
> var rect = new Rectangle(5,10);
> alert(rect.sides);  //2
> ```
>
> `Rectangle`的原型被设定为`Polygon`，也就意味着`new Rectangle() instanceof Polygon == true`，于是再其构造函数中，`Polygon.call(this, 2)`对应在构造函数`Polygon`的代码：
>
> ```javascript
> if (this instanceof Polygon) {
>     this.sides = sides;
>     this.getArea = function(){
>       return 0;
>     };
> }
> ```
>
> `this`对象继承于`Polygon`，于是就可以继承`sides`, `getArea`属性。
>
> 
>
> **惰性载入函数**
>
> 当一个函数需要多个`if`分支来确定兼容性时，可以考虑惰性载入。原理就是重写函数或利用匿名立即执行函数加载时判断指定。
>
> 比如早期使用AJAX时，针对IE/FF浏览器不同的支持方式，就会使用IF先进行版本判断，然后再确定调用`XHR`或`ActiveXObject`。
>
> 代码如下：
>
> 1.重写
>
> ```javascript
>  function createXHR(){
>             if (typeof XMLHttpRequest != "undefined"){
>                 createXHR = function(){
>                     return new XMLHttpRequest();
>                 };
>             } else if (typeof ActiveXObject != "undefined"){
>                 createXHR = function(){                    
>                     if (typeof arguments.callee.activeXString != "string"){
>                         var versions = ["MSXML2.XMLHttp.6.0", "MSXML2.XMLHttp.3.0",
>                                         "MSXML2.XMLHttp"],
>                             i, len;
>                 
>                         for (i=0,len=versions.length; i < len; i++){
>                             try {
>                                 new ActiveXObject(versions[i]);
>                                 arguments.callee.activeXString = versions[i];
>                             } catch (ex){
>                                 //skip
>                             }
>                         }
>                     }
>                 
>                     return new ActiveXObject(arguments.callee.activeXString);
>                 };
>             } else {
>                 createXHR = function(){
>                     throw new Error("No XHR object available.");
>                 };
>             }
>             
>             return createXHR();
>         }
> ```
>
> 2.匿名立即执行
>
> ```javascript
>  var createXHR = (function(){
>             if (typeof XMLHttpRequest != "undefined"){
>                 return function(){
>                     return new XMLHttpRequest();
>                 };
>             } else if (typeof ActiveXObject != "undefined"){
>                 return function(){                    
>                     if (typeof arguments.callee.activeXString != "string"){
>                         var versions = ["MSXML2.XMLHttp.6.0", "MSXML2.XMLHttp.3.0",
>                                         "MSXML2.XMLHttp"],
>                             i, len;
>                 
>                         for (i=0,len=versions.length; i < len; i++){
>                             try {
>                                 new ActiveXObject(versions[i]);
>                                 arguments.callee.activeXString = versions[i];
>                                 break;
>                             } catch (ex){
>                                 //skip
>                             }
>                         }
>                     }
>                 
>                     return new ActiveXObject(arguments.callee.activeXString);
>                 };
>             } else {
>                 return function(){
>                     throw new Error("No XHR object available.");
>                 };
>             }
>         })();
> ```
>
> 
>
> 5月17日
>
> **函数绑定**
>
> 有如下代码：
>
> ```javascript
> var handler = {
> message: "Event handled",
> handleClick: function(event){
> alert(this.message);
> }
> };
> var btn = document.getElementById("my-btn");
> btn.AddEventListener('click',handler.handleClick)
> ```
>
> 给`btn`加上了一个单击事件监听，但是处理函数是`handler`对象内部的函数，涉及到`this`，在此例中，输出为`undefined`。因为`this`指向的是`btn`。
>
> 
>
> 为了修正这个输出，有多种方法：
>
> ```javascript
> var handler = {
> msg : 'event handled',
> handleClick:function(event){
>  console.log(event,this.msg)
> }
> }
>
> function bind(fn,target){
> return function(){
>   fn.apply(target);
> }
> }
>
> test.addEventListener('click',function(event){
> handler.handleClick(event)
> })
> //闭包
>
> test.addEventListener('click',bind(handler.handleClick,handler))
> //apply/call
>
> test.addEventListener('click',handler.handleClick.bind(handler))
> //原生bind()
> ```
>
> 
>
> **函数柯里化**
>
> 柯里化，据说很厉害，之前只听说过。详见https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch4.html。
>
> 下面这个代码是柯里化的应用：
>
> ```javascript
> function curry(fn){
>             var args = Array.prototype.slice.call(arguments, 1);
>             return function(){
>                 var innerArgs = Array.prototype.slice.call(arguments),
>                     finalArgs = args.concat(innerArgs);
>                 return fn.apply(null, finalArgs);
>             };
>         }
>     
>         function add(num1, num2){
>             return num1 + num2;
>         }
>         
>         var curriedAdd = curry(add, 5);
>         alert(curriedAdd(3));   //8
>
>         var curriedAdd2 = curry(add, 5, 12);
>         alert(curriedAdd2());   //17
>         
>         var curriedAdd3 = curry(add, 5, 12, 18);
>         alert(curriedAdd3());   //17
>
>         var curriedAdd4 = curry(add, 5, 12);
>         alert(curriedAdd4(5));   //17
>
>         var curriedAdd5 = curry(add, 5, 12, 18);
>         alert(curriedAdd5(1));   //17
> ```
>
> 大概意思就是通过闭包的`apply`来将多个参数转化为单个参数。
>
> 
>
> 5月19日
>
> （昨天在prototype/call这里研究了半天，本来想写个学习记录的，发现之前自己写过一遍浅的，就不再写重复的内容了）
>
> 使用柯里化写`bind()`：
>
> ```javascript
> function bind(fn,context){
>   var args = Array.prototype.slice.call(arguments,2);
>   return function(){
>     var innerArgs = Array.prototype.slice.call(arguments);
>     var finalArgs = args.concat(innerArgs);
>     fn.apply(context,finalArgs);
>   }
> }
> ```
>
> 跟上面的`add`柯里化函数类似，通过分离变量，`apply`改变执行环境来实现模拟`bind`函数：
>
> ```javascript
> var handler = {
>   message:"event handled",
>   handleClick : function(name,event){
>     alert(this.message+":"+name+":"+event.type)
>   }
> }
>
> var btn = document.getElementById('my-btn');
> btn.addEventListener('click',bind(handler.handleClick,handler,'button'))
> ```
>
> `event`是默认传递的，与绑定的内容是一致的：增加一个`click`的listener，event的类型就是`click`。
>
> 原生`bind`柯里化：
>
> ```javascript
> btn.addEventListener('click',handler.handleClick.bind(handler,'some name'))
> ```
>
> 
>
> **高级定时器**
>
> 数组分块：
>
> ```javascript
> setTimeout(function(){
> //取出下一个条目并处理
> var item = array.shift();
> process(item);
> //若还有条目，再设置另一个定时器
> if(array.length > 0){
> setTimeout(arguments.callee, 100);
> }
> }, 100);
> ```
>
> 函数节流
>
> 自定义事件
>
> 鼠标拖放
>
> 
>
> # 第二十三章 离线应用与客户端存储
>
> COOKIE
>
> localStorage
>
> sessionStorage
>
> localStorage与sessionStorage用法基本一致，区别在于时限，大小一般都是5MB，我电脑上测试的是5242875B。
>
> 
>
> # 第二十四章 最佳实践
>
> 因为JS中没有什么不可以被修改的内容，所以要尽量不去修改不属于自己的东西，于是就有了以下的规范：
>
> ```javascript
> //创建全局对象
> var Wrox = {};
> //为 Professional JavaScript 创建命名空间
> Wrox.ProJS = {};
> //将书中用到的对象附加上去
> Wrox.ProJS.EventUtil = { ... };
> Wrox.ProJS.CookieUtil = { ... };
> ```
>
> > 在这个例子中， Wrox 是全局量， 其他命名空间在此之上创建。 如果本书所有代码都放在 Wrox.ProJS命名空间，那么其他作者也应把自己的代码添加到 Wrox 对象中。只要所有人都遵循这个规则，那么就不用担心其他人也创建叫做 EventUtil 或者 CookieUtil 的对象， 因为它会存在于不同的命名空间中。
>
> 
>
> 性能
>
> ```javascript
> function updateUI(){
> var imgs = document.getElementsByTagName("img");
> for (var i=0, len=imgs.length; i < len; i++){
> imgs[i].title = document.title + " image " + i;
> }
> var msg = document.getElementById("msg");
> msg.innerHTML = "Update complete.";
> }
> ```
>
> 如果`imgs`数量很大，效率不高，因为全局对象`docment`的查找。
>
> 如果修改一下，将`document`改为局部对象：
>
> ```javascript
> function updateUI(){
> var doc = document;
> var imgs = doc.getElementsByTagName("img");
> for (var i=0, len=imgs.length; i < len; i++){
> imgs[i].title = doc.title + " image " + i;
> }
> var msg = doc.getElementById("msg");
> msg.innerHTML = "Update complete.";
> }
> ```
>
> > 这里， 首先将 document 对象存在本地的 doc 变量中； 然后在余下的代码中替换原来的 document 。与原来的的版本相比，现在的函数只有一次全局查找，肯定更快。将在一个函数中会用到多次的全局对象存储为局部变量总是没错的。
>
> 
>
> 最小化语句数
>
> ```javascript
> //使用单个var声明
> var count=5,
>     color='blue',
>     values = [1,2,3];
>
> //使用数组和对象字面量
> var arr = [1,2,3];
> var obj = {
>   name:"Nicholas",
>   age:29
> };
> ```
>
> 
>
> DOM优化
>
> 最小化现场更新