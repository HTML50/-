

# react学习笔记 2017年1月

2017年1月9日

休息了两个星期没有看。看了点电影，看了点人文的资料，看了点新闻。总觉得要学习的东西不只是技术，还有很多很多。把握时间，多多学习。



### Lists and Keys

https://facebook.github.io/react/docs/lists-and-keys.html

___

先看一段代码

```jsx
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```

猛的一看，有map又有箭头函数，有点不适应。我上网复习了一下ES6，这里的map作用是遍历numbers数组。(number) => number*2，括号可有可无。



### Rendering Multiple Components 

在React中，也可以使用上面的方法来渲染多个组件：

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) => 
<li>number</li>
);
                              
ReactDOM.render(
{listItems},
document.getElementById('root')
)
```

将上面的代码改写成一个Component

```JSX
function List(props){
const number=props.number;
const list  = number.map(x=><li key={x}>{x}</li>)
return (
<ul>{list}</ul>
)
}

const numbers = [1,2,3,4,5]

ReactDOM.render(
<List number={numbers} />,
document.getElementById('root')
)
```

这里需要注意的是，在JSX写法规则下，key={x}不要加引号`"`或者`'`，加了渲染出来是`key='{x}'`这样的，会报错说使用相同的key。



### Keys

Keys是React用来判断项目增删改的依据。

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```

不太理解为什么key要转成string类型。

最好的方法就是使用独一无二的字符串来作为key，通常情况下都是使用ID作为key的值，如下：

```jsx
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

当没有固定ID时，也可以使用item的index值来作为key。

```jsx
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
);
```

不建议使用index，发生排序时效率低下。详情查看[该文章](https://facebook.github.io/react/docs/reconciliation.html#recursing-on-children)，大意是在序列底部插入元素时，效率正常，当从顶部或其他位置插入元素时，效率就低，因为无法按照index的原有顺序进行比对了。



1月10日

### Extracting Components with Keys

Keys只在有同胞元素相邻时才有意义，比如：

- <li key='1'></li>
- <li key='2'></li>
- <li key='3'></li>

如果，变成这样，就没有意义了：

- <div><li key='1'></li></div>
- <div><li key='2'></li></div>
- <div><li key='3'></li></div>

这时，就需要把key值加给div才行。

如果将List的Component拆分，分为List与Item两部分，就需要注意上面的情况，要把key值放在List（最外层）内，而不能放在Item（内层），来保证相邻元素的关系。

```jsx
function Item(props){
  const number = props.value;
  return <li>{number}</li>;
}

function List(props){
  const value = props.value;
  return (
  <ul>
  {value.map(x =>
  <Item key={x.toString()} value={x} />
  )}
  </ul>
  );
}

const numbers= [1,2,3,4,5];
ReactDOM.render(
  <List value={numbers} />,
    document.getElementById('root')
 )
```

打这段代码的时候遇到几个问题，首先List中return时，要给<ul>标签，不然渲染出的<Item>项好几个，会出现多个同级别的标签，这在React中是不允许的，会出现`react.js:19287 Warning: flattenChildren(...): Encountered two children with the same key, .$function toString() { [native code] }. Child keys must be unique; when two children share a key, only the first child will be used.`。

JSX语法中，任何return的变量都要带上`{}`。



---

（休息分割线）

上午有个同学给了一道面试题。


> 有一栋楼共100层，一个鸡蛋从第N层及以上的楼层落下来会摔破，在第N层以下的楼层落下不会摔破。给你2个鸡蛋，设计方案找出N，并且保证在最坏情况下，最小化鸡蛋下落的次数。 


刚开始我认为这是个拆半查找，我觉得2个鸡蛋怎么够用。

然后我又想了想，觉得这怎么能够确定多少次呢，很混乱。

最后大家讨论了一会儿，明确了题意，想要解题，必须按照要求来，一个蛋碎了，另一个只能从1楼开始扔，一直到蛋碎的楼层。

我就想，最坏的情况下，就是怎么扔都很难如愿，只有更费劲没有最费劲。

我从1楼开始扔，最坏情况就是100楼才碎，我要扔100次。

先那一个鸡蛋来试探，如果我从50楼开始扔，碎了，第二个鸡蛋就从1楼开始尝试。没碎，就从51楼开始。这样，最快就是2次，最慢就是50次，平均一下25次。我觉得这是个不错的答案。

然后就开始baidu一番，看看自己的答案如何。大跌眼镜，感觉自己好无知，算个错误的答案还沾沾自喜。



最终，有一个易读的答案（其他各种动态规划、二叉树、算法忽略不计），看完我才明白，这个题求得是最少扔多少次，而不是求层数N是多少，方法是假设最少需要扔x次能确定出多少层。

第一个鸡蛋先从x楼开始扔，如果碎了，则第二个鸡蛋可以确定楼层在1到x-1之间，第二个鸡蛋最多扔x-1次必碎，这样1+(x-1)得到x次确定层数。

如果第一个鸡蛋没碎，就继续在x+(x-1)楼扔，碎了，继续在x+1到x+(x-2)的区间去扔第二个鸡蛋，最多x-2次必碎。得到2+(x-2)，共x次确定层数。

以此类推。
...




还是看别人写的吧


> 正确的方法是先假设最少判断次数为x，则第一个鸡蛋第一次从第x层扔（不管碎没碎，还有x-1次尝试机会）。如果碎了，则第二个鸡蛋在1～x-1层中线性搜索，最多x-1次；如果没碎，则第一个鸡蛋第二次从x+(x-1)层扔（现在还剩x-2次尝试机会）。如果这次碎了，则第二个鸡蛋在x+1～x+(x-1)-1层中线性搜索，最多x-2次；如果还没碎第一个鸡蛋再从x+(x-1)+(x-2)层扔，依此类推。x次尝试所能确定的最高楼层数为x+(x-1)+(x-2)+...+1=x(x+1)/2。
> 比如100层的楼，只要让x(x+1)/2>=100，得x>=14，最少判断14次。具体地说，100层的楼，第一次从14层开始扔。碎了好说，从第1层开始试。不碎的话还有13次机会，再从14+13=27层开始扔。依此类推，各次尝试的楼层依次为
>
> 14
> 27 = 14 + 13
> 39 = 27 + 12 
> ... 
> 99 = 95 + 4
> 100

这样一来，最快11次能确定，最慢也是14次。

---



### Keys Must Only Be Unique Among Siblings 

Keys只需在同胞数组保持唯一，若使用同一组id作为不同Component的Keys也是可以的。

```jsx
function Blog(props){
  const sidebar=(
  <ul>
   {props.posts.map(post=>
      <li key={post.id}>{post.title}</li> 
   )}
    </ul>
  )
  const content=props.posts.map(post=>
  <div key={post.id}>
      <h3>{post.title}</h3> 
      <p>{post.content}</p>
   </div>)

return(
<div>
{sidebar}
<hr />
{content}
</div>
)
}

const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
);
```

这里遇到的问题是，`const content=props.posts.map`这里是不需要加`{}`的，不然会报错。但是上面的

```jsx
const sidebar=(
  <ul>
   {props.posts.map(post=>
      <li key={post.id}>{post.title}</li> 
   )}
    </ul>
  )
```

就需要加`{}`，即使写成

```jsx
 const content=(
   {props.posts.map(post=>
  <div key={post.id}>
      <h3>{post.title}</h3> 
      <p>{post.content}</p>
   </div>)}
)
```

也会报错，应该是=后直接跟变量，就不需要加`{}`了。



1月11日

keys是React内部的一个参数，只用于React操作，是无法传递给Component，呈现在渲染出来的结构中的。使用Chrome的F12功能，并且下载React插件，拿上面的content片段为例，可以看到，如果一个Component只有Keys值，得到的结果如下：

> <div>		($r in the console)
>
> ___
>
> key "1"
>
> ___
>
> Props read-only
>
> children: Array[2]
>
> 0: {...}
>
> 1: {..}

属性中，只有相应的元素，如果将content代码段改为如下，增加一个id：

```jsx
 const content=(
   {props.posts.map(post=>
  <div key={post.id} id={post.id}>
      <h3>{post.title}</h3> 
      <p>{post.content}</p>
   </div>)}
)
```

这时从F12工具可观察到
> <div>		($r in the console)
>
> ___
>
> **key** "1"
>
> ___
>
> **Props** read-only
>
> children: Array[2]
>
>  -0: {...}
>
>  -1: {..}
>
> id: 1

属性中多了一个id的内容。

如果想传递原始数据的id或者其他内容到Component，光设定keys是不行的，可以自行添加其他字段。



### Embedding map() in JSX 

前面的代码中在变量定义中使用了map函数

```jsx
const listItems = numbers.map((number) =>
    <ListItem key={number.toString()}
              value={number} />
  );
```

在jsx语法中，一个Component的return里，`{}`内可以使用任意的表达式（上面这个只在其中放了变量），比如：

```jsx
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```

`const variable`是可以直接跟变量的，加`{}`反倒不行，而在`return`里，必须加`{}`才能识别变量。



### Forms

https://facebook.github.io/react/docs/forms.html

------

HTML中的表单是这样的

```html
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

点击提交，会把name的值传递给下一个页面。在React中，更常见的做法是使用一个handle function来处理提交的事件，叫做"controlled components"。



###Controlled Components

HTML中的Form，如`<input> <select> <textarea>`等均有自己实现状态改变与更新的方法。在React中，Form的中的值均由`state`属性保存，并且使用`setState()`更新。input值由这样方法控制的Component就叫做“controlled component”。

例如，React中的Form可以这样写：

```jsx
class Form extends React.Component{
  constructor(props){
    super(props);
    this.state={value:''};
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }
  
  handleChange(event){
    this.setState({value:event.target.value})
  }
  
  handleSubmit(event){
    alert('the name is '+this.state.value);
    event.preventDefault();
  }
  
  render(){
    return(
      <form onSubmit={this.handleSubmit}>
        <label>
        Name:
          <input type='text' value={this.state.value} onChange={this.handleChange} />
        </label>
      <input type='submit' value='submit' />
      </form>
    )
  }
}
```

这个逻辑跟JS是一样的，只不过换了一种方法，用`state`作为中间值来连接事件和数据更新。



### The textarea Tag 

在HTML中，textarea的内容是这样定义的：

```jsx
<textarea>
  Hello there, this is some text in a text area
</textarea>
```

在React中有所区别，内容也放在`value`中，和操作`<input>`的方法是一致的：

```jsx
render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
```



### The select Tag

同样的，React中对于`<select>`也是采取统一、简便的处理方法。（代码都一样，我直接复制了）

```jsx
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite La Croix flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

也有个叫“Uncontrolled Components”的模式，很简单，暂时略过不研究。



1月12日

（今天休息，没有看）

上午在想一个js库，用来管理一个函数顺序执行表，可以等待上一个函数执行完毕，再继续执行下一个。这些函数里可能包含setTimeout，与css的animation，transition等异步操作。想了一上午，没啥结果。

然后就学习了一下时间复杂度，自己算了算这个x++一共执行了多少次。

```javascript
for(x=1;x<=100,i++){
  for(j=1;j<x-1;j++){
    x++;
  }
}
```

也是上来就看晕了，最终才明白。



下午继续想，看了不少文章，说的最多的就是新规范里面的Promise。好像直接写没有什么可借鉴的经验，js的单线程机制貌似也不允许这样。日后再想想。

最后说到线程阻塞，我想知道alert能不能阻塞ajax，做实验发现。是可以的，阻塞发生时，浏览器对当前页面线程的js代码都不解释了。