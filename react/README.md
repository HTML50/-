# react学习笔记

## https://facebook.github.io/react/docs/hello-world.html

2016年11月14日

### Hello world

https://facebook.github.io/react/docs/hello-world.html

```jsx
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

看上去好像是react自定义了一套自己的DOM机制，还有操作方法、语法。



### JSX

https://facebook.github.io/react/docs/introducing-jsx.html



```jsx
const element = <h1>Hello, world!</h1>;
```

JSX是JS的一种语法扩展，官方建议使用JSX来写React。

*const是ES6中新的变量。*https://strongloop.com/strongblog/es6-variable-declarations/



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



