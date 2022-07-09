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
> 组件，它接受任意的入参（即 “props”），并返回用于描述页面展示内容的 React 元素

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
