

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



1月13日

(不务正业分割线)

___


6种primitive数据类型

- [Boolean](https://developer.mozilla.org/en-US/docs/Glossary/Boolean)
- [Null](https://developer.mozilla.org/en-US/docs/Glossary/Null)
- [Undefined](https://developer.mozilla.org/en-US/docs/Glossary/Undefined)
- [Number](https://developer.mozilla.org/en-US/docs/Glossary/Number)
- [String](https://developer.mozilla.org/en-US/docs/Glossary/String)
- [Symbol](https://developer.mozilla.org/en-US/docs/Glossary/Symbol) (new in ECMAScript 6)



其中，`typeof(null)`得到的是object。

为什么，网上有人问 ：

> If `null` is a primitive, why does `typeof(null)` return `"object"`?

答案是：

> Because [the spec says so](http://www.ecma-international.org/ecma-262/5.1/#sec-11.4.3).



仔细研究这几个类型还是很有意思的。比如：

```javascript
var num = Number(123);
var num1 = 123;
var num2 = new Number(123)
var num3 = new Number(123)


console.log(typeof(num));
console.log(typeof(num2));

console.log(num === num1);		//true
console.log(num1 === num2);		//false
console.log(num2 == num3);		//false
```





这些都是上午看关于不同类型数据比较时学到的，规范文档中的规则也挺拗口，不太容易记。比如：

```javascript
console.log(null==undefined)		//true
```



当然还有意外：

```javascript
console.log(!Boolean == false);		//true
console.log(!Boolean == true);		//false
console.log(Boolean == true);		//false
console.log(Boolean == false);		//false
```

这是因为typeof(Boolean)是function，根据NOT(`!`)运算符的规则，对于Object类型，转化为Boolean的值是true。所以这里的!Boolean就应该是false。是NOT`!`在这里起到的转化作用。

对于`==`的比较，规范中没有提到function类型的比较，我估计应该按照Object的规则应用，也就是说大部分结果都是false。



1月16日

上午复习了一下冒泡排序。发现如果在纸上写，很容易就会出现错误，太依赖调试了，需要多看些算法，锻炼逻辑。

### Lifting State Up

https://facebook.github.io/react/docs/lifting-state-up.html

___

通常情况下，多个Component会控制同一个数据区域，这时需要lift state up，将共享的state放到最接近的公共ancestor中去。

在本例中，我们将写一个温度计算器，来计算给定的温度是否能让水沸腾。

首先，创建一个BoilingVerdict，接收温度作为prop，返回是否沸腾。

```jsx
function BoilingVerdict(props){
  if(props.temperature>=100){
    return <p>水沸腾了！</p>;
  }
  return <p>水还不太热！</p>;
}
```

然后，写一个Caculator，用于渲染input接收输入，然后用state来保存，并通过上面的BoilingVerdict输出结果。

```jsx
class Caculator extends React.Component{
  constructor(props){
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state ={value:''};
  }
  
  handleChange(e){
    this.setState({value:e.target.value})
  }
  
  render(){
    const value = this.state.value;
    return(
    <div>
      <input type='text' value={value} onChange={this.handleChange} />
        <BoilingVerdict temperature={parseInt(value)} />
      </div>
    )
  }
}
```



1月17日

### Adding a Second Input 

 增加另一个input，将两个input分别设置为华氏温度、摄氏温度。

先从`Caculator`中分解出来一个`TemperatureInput` 。建立一个新的prop，叫做`scale`，用来传递不同种类的温度`c`,`f`。

大概看了一遍官网的代码，自己先写了一个，完成了两个input，但是标准代码是两个Component，包含state，用于下一步的相互转化。

```jsx
const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {value: ''};
  }

  handleChange(e) {
    this.setState({value: e.target.value});
  }

  render() {
    const value = this.state.value;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={value}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}

class Calculator extends React.Component {
  render() {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" />
      </div>
    );
  }
}
```

还是COPY官方的吧。此时，两个不同的温度不会自动转化，也没有关于水是否沸腾的判断，以为温度值是在不同的Component内部的state保存的，下一步就来Lifting state up。



### Lifting State Up

先写个温度互相转换的函数

```jsx
function toCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5 / 9;
}

function toFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32;
}
```

然后写一个函数，整合上面的函数

```jsx
function tryConvert(value, convert) {
  const input = parseFloat(value);
  if (Number.isNaN(input)) {
    return '';
  }
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000;
  return rounded.toString();
}
```

下面，把state从`TemperatureInput`中删除，并且把`input`的`onChange`设置为props的handler。`input`的`value`来自于props。

```jsx
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    this.props.onChange(e.target.value);
  }

  render() {
    const value = this.props.value;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={value}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

最后，将state的值放在调用`TemperatureInput`的`Calculator`中，这种思想就是lifting state up，共享的state放在最近的祖先中。



完整代码如下：

```jsx
function BoilingVerdict(props) {
  if (props.celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water would not boil.</p>;
}

function toCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5 / 9;
}

function toFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32;
}

const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

function tryConvert(value, convert) {
  const input = parseFloat(value);
  if (Number.isNaN(input)) {
    return '';
  }
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000;
  return rounded.toString();
}

class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    this.props.onChange(e.target.value);
  }

  render() {
    const value = this.props.value;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={value}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}


class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {value: '', scale: 'c'};
  }

  handleCelsiusChange(value) {
    this.setState({scale: 'c', value});
  }

  handleFahrenheitChange(value) {
    this.setState({scale: 'f', value});
  }

  render() {
    const scale = this.state.scale;
    const value = this.state.value;
    const celsius = scale === 'f' ? tryConvert(value, toCelsius) : value;
    const fahrenheit = scale === 'c' ? tryConvert(value, toFahrenheit) : value;

    return (
      <div>
        <TemperatureInput
          scale="c"
          value={celsius}
          onChange={this.handleCelsiusChange} />
        <TemperatureInput
          scale="f"
          value={fahrenheit}
          onChange={this.handleFahrenheitChange} />
        <BoilingVerdict
          celsius={parseFloat(celsius)} />
      </div>
    );
  }
}

ReactDOM.render(
<Calculator />,
document.getElementById('root')
)
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/ozdyNg?editors=0010)

![Monitoring State in React DevTools](https://facebook.github.io/react/img/docs/react-devtools-state.gif)



小结：这个有些复杂了，卡壳了半天。其中`e.target.value`与`onChange`函数自身能传递input的`value`在官网上都没有交待，自己做了点小实验才弄明白逻辑。自己写的代码绑定的state有问题，逻辑还有待加强训练。另外看了一些毕业react作品，我感觉学一个东西环境影响是比较大的，当什么都不知道的时候，直接灌输react的思想是很容易进入脑中的。当熟练了另外一种工具，再去学习不太相同的东西，前者的固有思维是常常出现的。

明天再把这个例子复习一遍。



1月18日

复习。

把昨天模糊的地方自己探索了一下。

```jsx
class Input extends React.Component{
	constructor(props){
		super(props)
		this.handle = this.handle.bind(this)
	}

	handle(e){
		this.props.onChange(e.target.value)
	}

	render(){
		const value=this.props.value;
		return(
			<input value={value} onChange={this.handle} />
		)
	}
}

class Calculator extends React.Component{
	constructor(props){
		super(props)
		this.state = {value:'test'}
		this.handle = this.handle.bind(this)
	}

	handle(value){
	this.setState({value:value})
	}

	render(){
		const value =this.state.value;
		return(
			<Input value={value} onChange={this.handle} />
		)
	}
}

ReactDOM.render(
	<Calculator />,
	document.getElementById('root')
)
```

弄明白了`e.target.value`是WEB API的内容，意思是当鼠标单击input产生事件时，`e.target`就是事件来自哪里，那么`e.target.value`这个就是值。

`this.props.onChange(e.target.value)`这句是调用Component赋值时的`onChange`事件，可以把props看做是`<Input value={value} onChange={this.handle} />`这个。

刚开始我忘记把handle bind(this)了，导致输入时报错。我还在想为什么必须要bind(this)，最后按照规定写，就不报错了。



自己又写了一遍。初始化时，有一个input显示NaN，经过调查，是因为`tryConvert`没有作参数预处理。





1月19日

### Composition vs Inheritance

https://facebook.github.io/react/docs/composition-vs-inheritance.html

___

React有强有力的组合方法，建议多使用Component间的组合，而不要使用继承的方法。

本节将展示几个初学者常见的，想寻求继承方法的例子，使用组合的方法来解决。



### Containment 

有些Components是事先不知道他们的子元素的，比如``Sidebar` `Dialog`这样代表'boxes'的Component。

对于这类Component，做法是，直接将props传递给children元素，`{props.children}`一句话搞定，看代码：

```jsx
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
      //这里的意思就是把props的children内容拿过来用了
    </div>
  );
}
```

注意，这是另外一个**Component**的代码：

```jsx
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      //这里使用了上面代码中的Component,FancyBorder，下面的内容是WelcomeDialog的children
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```

从控制台也可以看到，FancyBorder的props中，有一个children的属性。



有时，你需要Component中有多个”接口“来自定义内容，可以使用Component来代替：

```jsx
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

这里的App使用了SplitPane，并且使用了left和right两个属性，每个属性的内容均为一个Component。观察到，App()是没有props参数的，如果不需要传入参数，这样的写法是正确的。



### Specialization 

根据前面的例子，我们可以看到`WelcomeDialog`是`Dialog`的一种特殊情况，在React中，我们通过组合的方法，使用相对特殊的Component，调用相对一般的Component，比如使用`WelcomeDialog`来render`Dialog`，并且自定义props：

```jsx
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```

这里`WelcomeDialog`调用了(render)`Dialog`，并且自定义了title与message的内容，在`Dialog`的定义中，输出的内容是由props.title与props.message决定的。



同样，使用class声明的Component也可以这样做，下面的代码是个简单的alert输入内容的例子：

```jsx
function LegendBox(props){
return (
<fieldset>
<legend>NASA</legend>
{props.children}
</fieldset>
)
}

function Dialog(props){
return(
<LegendBox>
<h1>{props.title}</h1>
<p>{props.msg}</p>
{props.children}
</LegendBox>
)
}

class Program extends React.Component{
constructor(props){
super(props)
this.state={value:''};
this.handleChange = this.handleChange.bind(this);
this.handleClick = this.handleClick.bind(this);
}

handleChange(e){
this.setState({value:e.target.value})
}

handleClick(){
alert('恭喜你！欢迎加入我们，'+this.state.value)
}

render(){
return(
<Dialog title='火星探索计划' msg='我们怎么称呼您？'>
<input value={this.state.value} onChange={this.handleChange} />
<button onClick={this.handleClick}>报名!</button>
</Dialog>
)
}
}

ReactDOM.render(
  <Program />,
  document.getElementById('root')
);
```

直接用notepad++写jsx，不会自动调整格式。

facebook中有上千个使用Component构建的部件，但没有一处需要使用继承方法的地方。如果需要复用，最好的方法就是分解成不同的块，使用本节的方法来实现。



### Thinking in React

https://facebook.github.io/react/docs/thinking-in-react.html

___

我们认为，React是使用javascript，用来构建庞大的、高效的Web app的好工具。Facebook和Instagram就用React构建的很好。

本节例子将展示一个完整的思考过程，关于使用React制作一个产品数据搜索功能的组件。

### Start With A Mock 

假设现在有一个JSON API和一个设计草稿，如下所示：

![Mockup](https://facebook.github.io/react/img/blog/thinking-in-react-mock.png)

JSON API得到的数据如下：

```JSON
[
  {category: "Sporting Goods", price: "$49.99", stocked: true, name: "Football"},
  {category: "Sporting Goods", price: "$9.99", stocked: true, name: "Baseball"},
  {category: "Sporting Goods", price: "$29.99", stocked: false, name: "Basketball"},
  {category: "Electronics", price: "$99.99", stocked: true, name: "iPod Touch"},
  {category: "Electronics", price: "$399.99", stocked: false, name: "iPhone 5"},
  {category: "Electronics", price: "$199.99", stocked: true, name: "Nexus 7"}
];
```

