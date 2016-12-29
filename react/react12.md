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



2016年12月29日

前几天学的太无聊，跑去写了两个代码。我觉得虽说上来学习官方文档是没错的，但是这个react的教程略显枯燥，一直都在讲用法，讲模块，我只模糊明白了这是用来完成什么工作的。也许真是前端技术发展太快了吧。



把1号的代码敲了一遍，复习一下。把UserGreeting中的props去掉，也是可以运行的。需要牢记React的语法规范，这样才能慢慢理解它的设计逻辑。

## Element Variables

有下面一段代码，根据`this.state`的不同，结合上面的登入、登出提示语，加上一个控制按钮，来继续深化不同条件下渲染的概念。

```JSX

function UserGreeting() {
	return <h1>Welcome back!</h1>;
}

function GuestGreeting() {
	return <h1>Please sign up.</h1>;
}
function Greeting(props) {
	const isLoggedIn = props.isLoggedIn;
	if (isLoggedIn) {
		return <UserGreeting />;
	}
	return <GuestGreeting />;
}
function LoginButton(props) {
	return (
		<button onClick={props.onClick}>
			Login
		</button>
	);
}

function LogoutButton(props) {
	return (
		<button onClick={props.onClick}>
			Logout
		</button>
	);
}

	class LoginControl extends React.Component{
		constructor(props){
			 super(props);
			 this.handleLoginClick = this.handleLoginClick.bind(this);
			 this.handleLogoutClick = this.handleLogoutClick.bind(this);
			 this.state= {isLoggedIn : false};
		}

		handleLoginClick(){
			this.setState({isLoggedIn:true});
		}

		handleLogoutClick(){
			this.setState({isLoggedIn:false});
		}

		render(){
			const isLoggedIn=this.state.isLoggedIn;
			let button = null;
			if(isLoggedIn){
				button = <LogoutButton onClick={this.handleLogoutClick} />;
			}else{
				button = <LoginButton onClick={this.handleLoginClick} />;
			}

		return (
			<div>
			<Greeting isLoggedIn={isLoggedIn} />{button}
			</div>
			);
		}
	}
	
	ReactDOM.render(
	<LoginControl />,
	document.getElementById('demo')
	);
```



我自己敲完运行，先是提示了一个奇怪的错误，看了半天是class extends component没有开头字母大写。然后可以运行后，点按钮没有变化，仔细对照官网代码，发现onclick有没有大写。我把props改为p也是可以正常运行的，参数不是固定的。

## Inline If with Logical && Operator

简写if的方法

```jsx
function Mailbox(props){
		const unreadMessages = props.unreadMessages;
		return(
			<div>
				Hello,
				{
					unreadMessages.length>0 &&
					<h2>you have {unreadMessages.length} unread messages.</h2>
				}
			</div>
			);
	}
	
	const message = ['welcome','re:welcome']
	ReactDOM.render(
	<Mailbox unreadMessages={message} />,
	document.getElementById('demo')
	);
```

这里使用了`{true or flase && expression}`，在js中，true&&“语句”的结果肯定是等价于“语句”，相反false&&“语句”得到的是false。这句话有个问题，如果把<div>去掉，那么将报错，估计是JSX语法有什么要求，貌似只要在临近{变量}的地方加上html标签包裹内容就不会出错。



## Inline If-Else with Conditional Operator

还有一种条件化的简写方法，使用三目运算符`condition ? true : false`。

```JSX
function RenderText(props){
		return(<h1>you are {props.isLoggedIn?'login':'not login'}</h1>)
	}
	
	ReactDOM.render(
	<RenderText isLoggedIn={false}/>,
	document.body
	);
```
还可以填充得复杂点

```JSX
function Button1(){
	return <button>btnTrue</button>
}
function Button2(){
	return <button>btnFalse</button>
}

	function RenderText(props){
		return(
			<h1>
            {props.isLoggedIn?(
				<Button1 />
				):(
				<Button2 />
				)}
         	 </h1>
			)
	}
	
	ReactDOM.render(
	<RenderText isLoggedIn={false}/>,
	document.body
	);
```

官方提示：如果Component太过复杂，就把其拆分。



12月30日

## Preventing Component from Rendering

