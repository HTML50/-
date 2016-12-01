# react学习笔记 12月

2016年12月1日

除了使用Class的方法创建Events Handler，还有两种不推荐的方法，我跳过去不学了。



## Conditional Rendering

https://facebook.github.io/react/docs/conditional-rendering.html

___

在不同的条件下可以渲染不同的内容，比如使用if语句或者`条件操作符`。看下面这个例子：

```jsx
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```

这是两个不同的信息提示Components。

我们来建立一个Greeting Component来对上面的代码进行不同条件的渲染。

```jsx
function Greeting(props){
  const isLogin = props.isLoggedIn;
  if(isLogin){
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDom.Render(
  // <Greeting isLoggedIn={false} /> 也可以是false
  <Greeting isLoggedIn={true} />,
  document.getElement('root')
)
```

根据isLoggedIn的真假，渲染不同的内容。