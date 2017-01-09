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

这里需要注意的是，在JSX写法规则下，key={x}不要加引号`"`或者`'`，加了会报错说使用相同的key。



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



### Extracting Components with Keys





