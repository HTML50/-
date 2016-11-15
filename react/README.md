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

```

```

