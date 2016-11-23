# react学习笔记 11月

## https://facebook.github.io/react/docs/hello-world.html

2016年11月14日

### Hello world

https://facebook.github.io/react/docs/hello-world.html

------

```jsx
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

看上去好像是react自定义了一套自己的DOM机制，还有操作方法、语法。



### JSX

https://facebook.github.io/react/docs/introducing-jsx.html

------

jsx写法示例

```jsx
const element = <h1>Hello, world!</h1>;
```

JSX是JS的一种语法扩展，官方建议使用JSX来写React。

*const是ES6中新的变量。*https://strongloop.com/strongblog/es6-variable-declarations/



用 {} 输出变量

```jsx
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

JSX写法怪怪的，可以不加双引号包裹内容，element就是一个JSX的声明。这段代码是为了展示如何使用JSX语法来输出变量，使用花括号，{ function or variable }来输出变量。



创建元素

```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);

const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

这两句的效果是一样的，一种是直接写JSX，另一种是使用`createElement`方法。该方法内置检查机制，生成的代码安全高效。



2016年11月15日

### Elements

https://facebook.github.io/react/docs/rendering-elements.html

------



元素是React apps中最小的单位，是components的组成部分。



在root中render内容

```jsx
<div id="root"></div>
```

```jsx
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

这还是hello world的代码。在这个id为root的div中，所有的内容将由React DOM管理，我们约定其为“root”。在实际项目中可以有一个或多个这样的root DOM。



更新render过的内容

React Element are immutable。元素的状态一旦创建，便无法更改，唯一更新render过的内容的方法是，重新创建element，使用`ReactDOM.render()`更新。

```jsx
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

这是一个显示当前时间的例子，使用`setInterval`，每秒更新ReactDOM。



React只更新必需的部分

在上面的例子中，React会比较内容变化时的前后不同，只把更改的内容更新在ReactDOM中。F12可以观察到，只有时间部分发生了重绘。

![](https://facebook.github.io/react/img/docs/granular-dom-updates.gif)



2016年11月16日

### Components and Props

https://facebook.github.io/react/docs/components-and-props.html

------

components将UI分为可复用的部分，且相互独立。

从概念上来看，components很像js中的函数，接收参数（在React中称为Props），返回React Elements规定的内容。



最简单的components是在内部写一个js函数。

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

我为了这个Components纠结了半天，我在codepen上测试了一下。

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

ReactDOM.render(
Welcome('hhhh'),
 document.getElementById('root')
);
//这样只显示hello

ReactDOM.render(
Welcome({'test':[{"name":"im test name"}]}),
 document.getElementById('root')
);
//构造一个JSON参数，这样什么都不显示
```

上网上一查，想输出props.name，要这样写

```jsx
var HelloMessage = React.createClass({
  render: function() {
    return <h1>Hello {this.props.name}</h1>;
  }
});

ReactDOM.render(
  <HelloMessage name="Runoob" />,
  document.getElementById('root')
);
```

使用了`React.createClass()`方法，这应该在随后会学到，暂且放一边。



另一种定义component的方法，ES6中的类，暂且用不到。

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```



渲染props的方法

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

最终结果，Hello, Sara

发生了什么？

1. `ReactDOM.render`调用了element元素内的`<Welcome name="Sara" />`参数。

2. React调用`Welcome`这个component内 `{name: 'Sara'}`作为props。

3. `Welcome`这个component返回`<h1>Hello, Sara</h1>`元素作为结果。

4. React DOM高效的更新了匹配的DOM内容。

*(似懂非懂的样子)*



可以看到，此例中的element，渲染时要经过Welcome方法的加工。官网上写的比较模糊，我还没分清component和element的区别，只知道，这个例子中的element表示的是一个自定义component，称为“props”。

component的名字要以大写字母开头。Welcome。



component的组成？复用？

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

这个例子设定了`App`这个component，打印了多遍的`Welcome`。

但是render调用时，写法为`<App />`，这和调用element时是不同的，这应该是调用component独有的方法。`<ComponentName />`



component的分解

先看一个例子

```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

这个`Comment`component返回了好几项内容，有object(author)，string，date。如果想单独使用其中的一部分，需要把这个component拆开。

来看component的分解，先分解Avatar

```jsx
function Avatar(props){
  return (
    <img className="Avatar"
          src={props.user.avatarUrl}
          alt={props.user.name}
        />
  );
}
```

这时，`Comment`就可以简化为

```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />//这里
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

使用`Avatar`这个component替换了`Comment`中的一部分。

还可以继续简化。

```jsx
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

Extracting (分解、拆分) component的核心思想就是制作可以复用的小片代码。



props是只读的，不可以被component修改

```jsx
function sum(a, b) {
  return a + b;
}
```

这个`sum`函数就是纯净的，相反

```jsx
function withdraw(account, amount) {
  account.total -= amount;
}
```

React component规定不能修改props的内容。下节State可以达到这个目的。



2016年11月17日

### State

https://facebook.github.io/react/docs/state-and-lifecycle.html

___

前面的例子里有一个显示时间的

```jsx
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

这个例子中没有component，只有element

现在，我们可以把它改为component，提高一下复用率。

```jsx
function Clock(props){
  return (
     <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.data.toLocalTimeString()}.</h2>
    </div>
  );
}

function tick(){
 ReactDOM.render(
  <Clock date={new Date()} />,
  document.getElementById('root')
 );
}

setInterval(tick,1000);
```

好像这样写还不太完美。

下面就引入了State的概念：component可以定义为一个具有功能的类，而state是类似于component但他的功能**只对类有效**。



首先，将component的函数形式的写法转化为Class形式。

```jsx
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
对比一下，发现几点改动
1. 使用class开头，声明Clock
2. 多写了一个render()在开头
3. return的内容props改为this.props



然后在这个基础上增加Local State。

第一步，将`props`改为`state`

```jsx
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

第二步，增加类的constructor。这都是ES6类的新规范。

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

第三步，去掉`date`prop

```jsx
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

最终形成的代码是

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```



mounting unmounting

上面的例子只输出一次时间。因为没有加setInterval。在React中，当你想设定/清除一个计时器时，这个操作叫做`mounting`和`unmounting`，具体写法是

```jsx
  componentDidMount() {

  }

  componentWillUnmount() {

  }
```

 这两种方法称为"lifecycle hooks"

为了使时间动起来，我们这样写一个mounting

```jsx
 componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

完整的代码

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

`tick()`中有`this.setState()`是用来更新state内容的，`componentDidMount`中还有箭头函数。*容我消化消化*



2016年11月18日

继续这一节的内容，没有实际的例子，实在不易懂。

不论如何，上面的例子，从写法上又比之前的例子结构清晰、逻辑明确了，同时渲染结果正确地显示了每一秒的时间变化。

观察代码，可以发现。首先这是一个类的形式的component，它含有两个React的原生方法`componentDidMount()`和`componentWillUnmount()`。第二个方法清除定时器暂时还没使用。最后是`tick()`内的`setState()`方法，从字面上看这就可以看出是设定更新state的。



正确使用state，有三个要点。

1.不能直接修改state。直接修改内容并不能使component重绘。

```jsx
// Wrong
this.state.comment = 'Hello';
// Correct
this.setState({comment: 'Hello'});
```

2.setState有时是异步的，更新`counter`时需要使用`prevState`

```jsx
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

3.使用`setState`更新State时，可以单独设置每一个参数。比如state内有两个参数：

```jsx
constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

可以分开：

```jsx
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```


2016年11月21日

过了个周末，再看上面的箭头函数又看不懂了~

```jsx
response => {
      this.setState({
        posts: response.posts
      });
    }
```

再复习一下：

箭头函数相当于匿名函数

```javascript
'use strict';
//只有一个参数

x => x*x;
//等同于
function(x){
  return x*x;
}

//没有参数
() => '此时没参数'
//等同于
function(){
  return '此时没参数';
}

//多个参数
(x,y) => x+y;
//等同于
function(x,y){
  return x,y;
}
```



那怎么执行箭头函数呢？和执行匿名函数的方法是一样的。

第一种方法，用括号包住，再紧跟`()`执行。

```javascript
'use strict';
//普通匿名
;(function (){
alert('我是匿名函数测试')
})
();

//箭头函数
;(()=>alert('我是箭头函数测试'))();
```

第二种方法，将匿名函数赋值给变量，调用变量执行函数。

```javascript
'use strict';
//普通匿名
var b = function(){
alert('我是匿名函数')
}
b();

//箭头函数
var a=() => alert('箭头函数 \\(^o^)/')
a();
```

那么，带有参数的匿名函数如何调用？

```javascript
'use strict';
var c = function(x){
  return x*x;
}
console.log(c(6))

//箭头
var d = x => x*x;
console.log(d(7))
```

我觉得调用含参的匿名函数不好记住写法，太拗口了，但是从匿名函数推算过来还是可以记住的。



好了，至此，最上面的那个`response`箭头函数，就是。。。还是看不太懂，response不是函数参数的名字么。为何函数里面反而调用了`response.posts`。我觉得这一定跟React的机制有关，后面肯定会遇到。暂且跳过。



### Handling Events

https://facebook.github.io/react/docs/handling-events.html

___

在React中处理事件Event的方式与原生DOM是类似的。

有两点不同：

- React事件使用驼峰命名法`camelCase`。

- 事件名是React中函数的写法，用{}括住。


普通的HTML

```html
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

JSX写法

```jsx
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

还有一点不同的是，在React中无法使用`return false`来阻止默认行为，只能使用`e.preventDefault()`，在HTML里可以这样写：

```html
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

React是这样的：

```jsx
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```



2016年11月23日
