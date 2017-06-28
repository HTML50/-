# React

6月24日

这两天把之前学习的官方文档复习了一遍，然后学习阮一峰老师的[React入门实例教程](http://www.ruanyifeng.com/blog/2015/03/react.html)，可能是由于文章写作时间较早，中间实例代码风格跟官方文档不大一致，不过我反倒觉得挺合我的口味，写起来无比顺畅。

例子都比较简单，通过之前的学习，代码码出来都没什么困难，再回头看之前的笔记时，有些当时不懂的问题，现在都已经明白原因了。

来看一个例子：

```jsx
var Input = React.createClass({
	render:function(){
		return <input type="text" placeholder="type something.." />
	}
})

ReactDOM.render(
	<Input />,
	root
)
```

定义了一个`Input`组件，渲染得到一个输入框。

对于React来说，管理变量用到了`state`机制，相当于把组件看成一个状态机，在上面的代码上增加一些内容：

```jsx
var Input = React.createClass({
  	getInitialState:function(){
		return {value:''}
	},
	inputHandler:function(e){
		this.setState({value:e.target.value})
	},
	render:function(){
		return <input type="text" value={this.state.value} onChange={this.inputHandler} placeholder="type something.." />
	}
})

ReactDOM.render(
	<Input />,
	root
)
```

新增了初始化`state`与输入内容改变时的事件处理方法。

<del>这时再操作输入框，`Input`组件的状态`state`就会随之改变，和Angular不同的是这种绑定是单向的，如果直接更改`state`的内容，`Input`不会有任何改变。</del>

把`Input`组件的`value`设定为`this.state.value`就可以实现输入框与`state`的内容同步。



最后两个例子是Ajax和Promise的用例，就不研究了。



6月27日

https://hulufei.gitbooks.io/react-tutorial

今天顺了顺翻译的教程，知道了我写的顺手的方法都是ES6之前的写法，ES6方法新增添了`class`等，所以React也更新了写法。比如写一个`LikeButton`：

```jsx
 class LikeButton extends React.Component{
   constructor(props){
     super(props);
     this.state = {liked:false};
     this.handleClick = this.handleClick.bind(this);
   }
   
   handleClick(e){
   	this.setState({liked:!this.state.liked})
   }

   render(){
   	var value = this.state.liked?'liked' : 'haven\'t liked';
     return	<button onClick={this.handleClick}>you {value} this, click to toggle</button> 
   }
 }


ReactDOM.render(<LikeButton />,root)
```

区别不太大，我把ES6的Class又复习了一遍，`constructor`中事件`bind(this)`的`this`指向的是谁我还没研究明白。姑且先这样记着，用到的时候自然就明白了。



官方文档学习笔记中最后一个例子没写完，对照着源码写完整了：

```jsx

var productData = [
  {category: 'Sporting Goods', price: '$49.99', stocked: true, name: 'Football'},
  {category: 'Sporting Goods', price: '$9.99', stocked: true, name: 'Baseball'},
  {category: 'Sporting Goods', price: '$29.99', stocked: false, name: 'Basketball'},
  {category: 'Electronics', price: '$99.99', stocked: true, name: 'iPod Touch'},
  {category: 'Electronics', price: '$399.99', stocked: false, name: 'iPhone 5'},
  {category: 'Electronics', price: '$199.99', stocked: true, name: 'Nexus 7'}
];
 
class App extends React.Component{
  constructor(props){
    super(props);
    this.state ={
      filterText:'',
      inStock: false 
    }
    this.handleInput = this.handleInput.bind(this);
    this.handleChecked = this.handleChecked.bind(this);
  }

  handleInput(filterText){
    this.setState({
      filterText:filterText
    })
  }

  handleChecked(inStockOnly){
    this.setState({
      inStock:inStockOnly
    })
  }

  render(){
  return(
    <div>
      <SearchTable inStock={this.state.inStock} filterText={this.state.filterText}  filterTextChange={this.handleInput} onCheckedChange={this.handleChecked} />
      <ProductTable inStock={this.state.inStock} filterText={this.state.filterText}  data={productData}/>
    </div>
  )
  }
}


class SearchTable extends React.Component{
constructor(props){
  super(props);
  this.handleInput = this.handleInput.bind(this);
  this.handleIsStockd = this.handleIsStockd.bind(this);
}

handleInput(e){
  this.props.filterTextChange(e.target.value);
}

handleIsStockd(e){
  this.props.onCheckedChange(e.target.checked);
}

render(){
return(
<div>
<input placeholder='Search...' value={this.props.filterText} onChange={this.handleInput} />
<p>
<input type='checkbox' checked={this.props.inStock} onChange={this.handleIsStockd}/> 只显示有货商品
</p>
</div>
)
}
}

class ProductTable extends React.Component{
render(){
var row = [],name;
this.props.data.forEach((product) =>{
  if(product.name.indexOf(this.props.filterText) === -1 || !product.stocked && this.props.inStock){ 
    return;
  }
  name = product.stocked ? product.name : <span style={{color:'red'}}>{product.name}</span>
  row.push(<tr key={product.name}><td>{name}</td><td>{product.price}</td></tr>);
})

return(
<table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Price</th>
          </tr>
        </thead>
        <tbody>{row}</tbody>
      </table>
)
}
}


ReactDOM.render(
  <App />,
  document.getElementById('root')
);

```

这算是一个完整的例子，其中用到了子组件向父组件传值`this.props.fn(arg)`，写的过程中明白了状态机的含义，每当`state`改变时，组件会自动重绘UI。