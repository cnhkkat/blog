# JSX
- () {} ""

  - 用括号 () 包裹 JSX
  - 用大括号 {} 包裹表达式
  - 用双引号 "" 将属性值指定为字符串字面量

- JSX 防止 XSS 跨站脚本攻击，防止注入。React DOM 默认会**转义**用户输入的内容，转成字符串

## JSX 是对象

- `Babel` 会把 JSX 转译成一个名为 `React.createElement()` 函数调用
- **该函数实际上创建了一个开销极小的普通对象，对象被称为“React 元素”**

> 元素描述了你在屏幕上想看到的内容

- React 通过读取这些对象，然后**使用它们来构建 DOM** 以及**保持随时更新 DOM** 来与 React 元素保持一致


# 元素渲染成 DOM
- 假设你的 HTML 文件某处有一个 `<div id="root"></div>`，称为 **“根” DOM 节点** 
- **React DOM 管理**该节点内的所有内容
- 想要将**一个** React 元素渲染到根 DOM 节点中，只需把它们一起传入 `ReactDOM.render()`

```js
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

## React 只更新它需要更新的部分

# 组件
> 组件，它**接受任意的入参**（即 “props”），并返回用于**描述页面展示内容的 React 元素**

## 函数组件
```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
该函数是一个有效的 React 组件，**因为它接收唯一带有数据的 “props”（代表属性）对象并返回一个 React 元素**。这类组件被称为“函数组件”，因为它本质上就是 JavaScript 函数。


## class 组件
```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
## 渲染组件
```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
当React 元素为用户自定义组件(**组件名称以大写字母开头**)时，它会将`name="Sara"` **转换为单个对象 props 传递给组件**,
让 JSX 接收 **属性**（attributes）`name` 以及 子组件（children）

1. 我们调用 `ReactDOM.render()`函数，传入`<Welcome name="Sara" />`作为参数
2. React 调用 `Welcome`组件，将`name="Sara"`作为`props`传入
3. `Welcome`组件返回`<h1>Hello, Sara</h1>`
4. React DOM 将 DOM 高效地更新为 `<h1>Hello, Sara</h1>`

## 组合组件
```js
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
1. 组件可以在其输出中引用其他组件。这就可以让我们用同一组件来抽象出任意层次的细节。按钮，表单，对话框，甚至整个屏幕的内容：在 React 应用程序中，这些通常都会以组件的形式表示。

2. 我们可以创建一个可以多次渲染 `Welcome` 组件的 `App` 组件。

3. 通常来说，每个新的 React 应用程序的顶层组件都是 `App` 组件。但是，如果你将 React 集成到现有的应用程序中，你可能需要使用像 `Button` 这样的小组件，并自下而上地将这类组件逐步应用到视图层的每一处。

## 提取组件
将组件拆分为更小的组件
```js
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
1. 该评论组件接收 author（对象），text （字符串）以及 date（日期）作为 props。所以可以分成3块。

2. 该组件由于嵌套的关系，变得难以维护，且很难复用它的各个部分。因此，让我们从中提取一些组件出来。

- 提取 Avatar 组件
```js
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```
  - Avatar 不需知道它在 Comment 组件内部是如何渲染的。因此，我们给它的 `props` 起了一个更通用的名字：`user`，而不是 author。

  - 从**组件自身的角度命名 props**，而不是依赖于调用组件的上下文命名

- 提取 UserInfo 组件
```js
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
- 简化 Comment 组件
```js
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
## props是只读的
组件无论是使用函数声明还是通过 class 声明，都决不能修改自身的 props。

> 纯函数，不会更改入参，且多次调用下相同的入参始终返回相同的结果。

**所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。**

# state & 生命周期

> State 与 props 类似，但是 state 是私有的，并且完全**受控于当前组件**。

- `componentDidMount()` 方法会在**组件已经被渲染到 DOM 后**运行。
- 写一个`Clock` class 组件
  1. 当 `<Clock />` 被传给 `ReactDOM.render()`的时候，React 会调用 Clock 组件的构造函数,因为 Clock 需要显示当前的时间，所以它会用一个包含当前时间的对象`{date: new Date()}`来初始化 this.state。我们会在之后更新 state。
  2. 之后 React 会调用组件的 `render()` 方法。这就是 React 确定该在页面上展示什么的方式`{this.state.date.toLocaleTimeString()}`。React 更新 DOM 来匹配 Clock 渲染的输出。
  3. 当 Clock 的输出被插入到 DOM 中后，React 就会调用 `ComponentDidMount()` 生命周期方法。在这个方法中，Clock 组件向浏览器请求**设置一个计时器**来每秒调用一次组件的 `tick()` 方法。`this.timerID = setInterval(() => this.tick(),100);`
  4. 在`tick()` 方法中，Clock 组件会通过调用 `setState()` 来计划进行一次 UI 更新。得益于 setState() 的调用，React 能够知道 state 已经改变了，然后会重新调用 `render()` 方法来确定页面上该显示什么。React 也会相应的更新 DOM。
  5. 一旦 Clock 组件从 DOM 中被移除，React 就会调用 `componentWillUnmount()` 生命周期方法，这样计时器就停止了。`clearInterval(this.timerID);`

- 关于 `setState`
  - 不要直接修改 State,而是使用 `setState()`。**构造函数是唯一可以给 this.state 赋值的地方**
  - State 的更新可能是**异步**的
    - 出于性能考虑，React 可能会把多个 `setState()` 调用合并成一个调用。
    - 因为 `this.props` 和 `this.state` 可能会**异步更新**，所以不要依赖他们的值来更新下一个状态。
    ```js
    // Wrong
    this.setState({
      counter: this.state.counter + this.props.increment,
    });
    // 要解决这个问题，可以让 setState() 接收一个函数而不是一个对象
    // 这个函数用上一个 state 作为第一个参数，将此次更新被应用时的 props 做为第二个参数
    // Correct
    this.setState((state, props) => ({
      counter: state.counter + props.increment
    }));
    ```
  - State 的更新会被**合并**
    - 当你调用 setState() 的时候，React 会把你提供的对象合并到当前的 state。这里的合并是浅合并，所以 `this.setState({comments})`完整保留了 `this.state.posts`， 但是完全替换了 `this.state.comments`。
    ```js
    constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
      };
    }
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

- 数据是向下流动的
  - state 是局部的或是封装的的原因是除了拥有并设置了它的组件，其他组件都无法访问。
  - **任何的 state 总是所属于特定的组件，而且从该 state 派生的任何数据或 UI 只能影响树中“低于”它们的组件**。比如组件在`props`中接收参数 date，但是组件不知道 date 是来自 Clock 组件的state 还是 props 还是手动输入的。

# 事件处理

- React 事件的命名采用小驼峰式`onClick`
- 使用 JSX 语法时需要传入一个函数作为事件处理函数 `onClick={activateLasers}`
- 不能通过`return false` 的方式**阻止默认行为**。你必须显式的使用 `e.preventDefault()`。`e` 是一个合成事件。
- 不需要使用 `addEventListener` 为已创建的 DOM 元素添加监听器。只需要在该元素初始渲染的时候添加监听器即可。将事件处理函数声明为 class 中的**方法**。
- 在 JavaScript 中，**class 的方法默认不会绑定 this**。如果你忘记绑定 this.handleClick 并把它传入了 onClick，当你调用这个函数的时候 `this` 的值为 `undefined`
- 如果你没有在方法后面添加`()`，例如 `onClick={this.handleClick}`，你应该为这个方法绑定 `this`
  - 绑定this的方法一：在 `constructor` 里 `this.handleClick = this.handleClick.bind(this)`;
  - Create React App 默认启用 class fields 语法: `handleClick = () => {console.log('this is:', this)}`
  - 或者使用箭头函数：`onClick={() => this.handleClick()}`
    - 此语法问题在于每次渲染 LoggingButton 时都会创建不同的回调函数。在大多数情况下，这没什么问题，但如果该回调函数作为 prop 传入子组件时，这些组件可能会进行额外的重新渲染。我们通常建议在构造器中绑定或使用 class fields 语法来避免这类性能问题
  - 传参
    - `onClick={(e) => this.deleteRow(id, e)}`, React 的事件对象 e 作为第二个参数
    - `onClick={this.deleteRow.bind(this, id)}`

# 条件渲染
在 React 中，你可以创建不同的组件来封装各种你需要的行为。然后，依据应用的不同状态，你可以只渲染对应状态下的部分内容

## &&

true && expression 总是返回 expression
false && expression 总是返回 false

## 三目运算符
condition ? true : false

## 阻止组件渲染
在组件内通过 if 判断是否 `return null`，可以阻止组件渲染。在组件的 render 方法中返回 null 不会影响组件的生命周期。componentDidUpdate 依然会被调用。

# 列表 & key

使用 map 遍历 numbers 数组，将数组的每个元素变成 `<li>` 标签,最后将得到的数组赋值给 listItems。然后把整个 listItems 插入到 `<ul>` 元素中，然后渲染进 DOM 。最后页面上显示一个 1 到 5 的项目符号列表。

**在一个组件中渲染列表**
```jsx
function ListItem(props) {
  return <li>{props.value}</li>;
}
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()} value={number}>
      )}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
📢 运行这段代码，将会看到一个警告 a key should be provided for list items。也就是说当创建一个元素时，必须有一个 `key` 属性。

- key 帮助 React **识别哪些元素改变**了，比如被添加或删除。
- 一个元素的 key 最好是这个元素在列表中拥有的一个**独一无二的字符串**。通常，使用数据中的 `id` 来作为元素的 key。
- 如果列表项目的顺序可能会变化，不建议使用 index 来用作 key 值，因为这样做会导致性能变差，还可能引起组件状态的问题。
- 如果你选择不指定显式的 key 值，那么 React 将**默认使用 index** 用作为列表项目的 key 值。
- 当我们生成两个不同的数组时，我们可以使用相同的 key 值。数组元素中使用的 key 在**其兄弟节点**之间应该是独一无二的
- 组件传 key ，`<Post key={post.id} />`

✨ tips:
- 在 map() 方法中的元素需要设置 key 属性
- 元素的 key 只有放在就近的数组上下文中才有意义

# 表单
表单元素通常会保持一些内部的 state。

> 在 HTML 中，表单元素（如<`input>、 <textarea> 和 <select>`）之类的表单元素通常自己维护 state，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用 setState()来更新。

我们可以把两者结合起来，使 React 的 state 成为“**唯一数据源**”。

## 受控组件
React控制表单输入元素

```js
  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('提交的名字: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          名字:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="提交" />
      </form>
    );
  }
```
由于在表单元素上设置了 value 属性，因此显示的值将始终为 this.state.value，这使得 React 的 state 成为唯一数据源。由于 handlechange 在每次按键时都会执行并更新 React 的 state，因此显示的值将随着用户输入而更新。
对于受控组件来说，输入的值始终由 React 的 state 驱动。

> `<input type="text">`, `<textarea>` 和 `<select>` 之类的标签都非常相似,它们都接受一个 value 属性，你可以使用它来实现受控组件

### select
将数组传递到 value 属性中，以支持在 select 标签中选择多个选项`<select multiple={true} value={['B', 'C']}>`

### input
<input type="file"> 允许用户从存储设备中选择一个或多个文件，将其上传到服务器，或通过使用 JavaScript 的 File API 进行控制。因为它的 value 只读，所以它是 React 中的一个非受控组件。

### 使用name处理多个input  
当需要处理多个 input 元素时，我们可以给每个元素添加 name 属性，并让处理函数根据 event.target.name 的值选择要执行的操作

```js
this.setState({
  [name]: value
});
// ES6 计算属性名称的语法更新给定输入名称对应的 state 值
```
由于 setState() 自动将部分 state 合并到当前 state, 只需调用它 更改部分 state 

# 状态提升
> 多个组件需要反映相同的变化数据，这时我们建议将共享状态**提升到最近的共同父组件**中去

通常，state 首先添加到需要渲染数据的组件中，如果其他组件也需要这个state，就把state提升到最近的父组件中。

TemperatureInput组件，temperature = this.state.temperature;
```js
const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};
handleChange(e) {
    this.setState({temperature: e.target.value});
}

render() {
    const temperature = this.state.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
```
两个 TemperatureInput 组件均在各自内部的 state 中相互独立地保存着各自的数据
```js
// Calculator
render() {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" />
      </div>
    );
  }
```
我们希望两个输入框内的数值彼此能够同步，所以将 TemperatureInput 组件中的 state 移动至 Calculator 组件中去。 

改变:

```js
// TemperatureInput
handleChange(e) {
    // Before: this.setState({temperature: e.target.value});
    this.props.onTemperatureChange(e.target.value);
    // ...
}
render() {
    // Before: const temperature = this.state.temperature;
    const temperature = this.props.temperature;
    // ...
}
```

```js
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {temperature: '', scale: 'c'};
  }

  handleCelsiusChange(temperature) {
    this.setState({scale: 'c', temperature});
  }

  handleFahrenheitChange(temperature) {
    this.setState({scale: 'f', temperature});
  }

  render() {
    const scale = this.state.scale;
    const temperature = this.state.temperature;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange} />
        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange} />
        <BoilingVerdict
          celsius={parseFloat(celsius)} />
      </div>
    );
  }
}
```
无论你编辑哪个输入框中的内容，Calculator 组件中的 this.state.temperature 和 this.state.scale 均会被更新。其中一个输入框保留用户的输入并取值，另一个输入框始终基于这个值显示转换后的结果。

# 组合 VS 继承

## {props.children}
<FancyBorder> JSX 标签中的所有内容都会作为一个 children prop 传递给 FancyBorder 组件
```js
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}

function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
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

不用 children，自定义 prop

```js
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
`<Contacts />` 和 `<Chat />` 之类的 React 元素本质就是对象（object），所以你可以把它们当作 props，像其他数据一样传递。像 **slot**。可以将任何东西作为 props 进行传递。

# React哲学
## 1 将设计好的 UI 划分为**组件层级**
  - FilterableProductTable
    - SearchBar
    - ProductTable
        - ProductCategoryRow
        - ProductRow

## 2 用 React 创建一个静态版本
- 在构建应用的**静态**版本时，我们需要创建一些会重用其他组件的组件，然后通过 props 传入所需的数据。props 是父组件向子组件传递数据的方式。
- 由于我们构建的是静态版本，所以这些组件目前只需提供 render() 方法用于渲染。
- 最顶层的组件 FilterableProductTable 通过 props 接受你的数据模型。如果你的**数据模型发生了改变**，再次**调用 ReactDOM.render()**，**UI 就会相应地被更新**。

## 3 确定 UI state 的最小（且完整）表示
- 想要使你的 UI 具备**交互**功能，需要有触发基础数据模型改变的能力。React 通过实现 state 来完成这个任务。
- 你要编写一个任务清单应用，你只需要保存一个包含所有事项的数组，而无需额外保存一个单独的 state 变量（用于存储任务个数）。当你需要展示任务个数时，只需要利用该数组的 length 属性即可


### 示例应用

示例应用拥有如下数据：

- 包含所有产品的原始列表
- 用户输入的搜索词
- 复选框是否选中的值
- 经过搜索筛选的产品列表

检查相应的数据是否属于  state 的判断方法:
- 是否是父组件通过 props 传递的数据
- 是否是随着时间的推移保持不变的数据
- 是否可以根据其他 state 或 prop 计算出来的数据
- 如果是，则说明不属于state。

所以包含所有产品的原始列表是经由 props 传入的，所以它不是 state；搜索词和复选框的值应该是 state，因为它们随时间会发生改变且无法由其他数据计算而来；经过搜索筛选的产品列表不是 state，因为它的结果可以由产品的原始列表根据搜索词和复选框的选择计算出来。

**属于 state 的有：**
- 用户输入的搜索词
- 复选框是否选中的值

## 4 确定 state 放置的位置
我们已经确定了应用所需的 state 的最小集合。接下来，我们需要确定**哪个组件能够改变这些 state**，或者说**拥有这些 state**。

📢 
React 中的数据流是单向的，并顺着组件层级从上往下传递。

对于应用中的每一个 state：

- 找到根据这个 state 进行渲染的所有组件。**SearchBar和ProductTable**
- 找到他们的共同所有者（common owner）组件（在组件层级上高于所有需要该 state 的组件）。**FilterableProductTable**
- 该共同所有者组件或者比它层级更高的组件应该拥有该 state。
- 如果你找不到一个合适的位置来存放该 state，就可以直接创建一个新的组件来存放该 state，并将这一新组件置于高于共同所有者组件层级的位置。

我们把这些 state 存放在 FilterableProductTable 组件中。首先，将实例属性 this.state = {filterText: '', inStockOnly: false} 添加到 FilterableProductTable 的 constructor 中，设置应用的初始 state；接着，将 filterText 和 inStockOnly 作为 props 传入 ProductTable 和 SearchBar；最后，用这些 props 筛选 ProductTable 中的产品信息，并设置 SearchBar 的表单值。

## 5 添加反向数据流
们已经借助自上而下传递的 props 和 state 渲染了一个应用。现在，我们将尝试让数据反向传递：处于较低层级的表单组件更新较高层级的 FilterableProductTable 中的 state。每当用户改变表单的值，我们需要改变 state 来反映用户的当前输入。由于 state 只能由拥有它们的组件进行更改，**FilterableProductTable 必须将一个能够触发 state 改变的回调函数（callback）传递给 SearchBar**。我们可以使用输入框的 onChange 事件来监视用户输入的变化，并通知 FilterableProductTable 传递给 SearchBar 的回调函数。然后该回调函数将调用 setState()，从而更新应用。
```js
// FilterableProductTable
  handleFilterTextChange(filterText) {
    this.setState({
      filterText: filterText
    });
  }
  
  handleInStockChange(inStockOnly) {
    this.setState({
      inStockOnly: inStockOnly
    })
  }
  render() {
  return (
      <SearchBar
        filterText={this.state.filterText}
        inStockOnly={this.state.inStockOnly}
        onFilterTextChange={this.handleFilterTextChange}
        onInStockChange={this.handleInStockChange}
      />
  )
  }


// SearchBar
handleFilterTextChange(e) {
    this.props.onFilterTextChange(e.target.value);
}
<input
    type="text"
    placeholder="Search..."
    value={this.props.filterText}
    onChange={this.handleFilterTextChange}
/>
```
