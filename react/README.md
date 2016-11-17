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

