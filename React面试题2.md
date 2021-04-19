##### React面试点2

###### 1.

##### 什么是 MVVM？比之 MVC 有什么区别？

首先先来说下 View 和 Model

- View 很简单，就是用户看到的视图
- Model 同样很简单，一般就是本地数据和数据库中的数据

基本上，我们写的产品就是通过接口从数据库中读取数据，然后将数据经过处理展现到用户看到的视图上。当然我们还可以从视图上读取用户的输入，然后又将用户的输入通过接口写入到数据库中。但是，如何将数据展示到视图上，然后又如何将用户的输入写入到数据中，不同的人就产生了不同的看法，从此出现了很多种架构设计。

传统的 MVC 架构通常是使用控制器更新模型，视图从模型中获取数据去渲染。当用户有输入时，会通过控制器去更新模型，并且通知视图进行更新。

但是 MVC 有一个巨大的缺陷就是**控制器承担的责任太大**了，随着项目愈加复杂，控制器中的代码会越来越**臃肿**，导致出现不利于**维护**的情况。

在 MVVM 架构中，引入了 **ViewModel** 的概念。ViewModel 只关心数据和业务的处理，不关心 View 如何处理数据，在这种情况下，View 和 Model 都可以独立出来，任何一方改变了也不一定需要改变另一方，并且可以将一些可复用的逻辑放在一个 ViewModel 中，让多个 View 复用这个 ViewModel。

对于 MVVM 来说，其实最重要的并不是通过双向绑定或者其他的方式将 View 与 ViewModel 绑定起来，**而是通过 ViewModel 将视图中的状态和用户的行为分离出一个抽象，这才是 MVVM 的精髓**。

##### 前端路由原理？两种实现方式有什么区别？

前端路由实现起来其实很简单，本质就是**监听 URL 的变化**，然后匹配路由规则，显示相应的页面，并且无须刷新页面。目前前端使用的路由就只有两种实现方式

- Hash 模式
- History 模式

###### Hash 模式

`www.test.com/#/` 就是 Hash URL，当 `#` 后面的哈希值发生变化时，可以通过 `hashchange` 事件来监听到 URL 的变化，从而进行跳转页面，并且无论哈希值如何变化，服务端接收到的 URL 请求永远是 `www.test.com`。

```
window.addEventListener('hashchange', () => {
  // ... 具体逻辑
})
```

Hash 模式相对来说更简单，并且兼容性也更好。

###### History 模式

History 模式是 HTML5 新推出的功能，主要使用 `history.pushState` 和 `history.replaceState` 改变 URL。

通过 History 模式改变 URL 同样不会引起页面的刷新，只会更新浏览器的历史记录。

// 新增历史记录
history.pushState(stateObject, title, URL)
// 替换当前历史记录
history.replaceState(stateObject, title, URL)

###### 两种模式对比

- Hash 模式只可以更改 `#` 后面的内容，History 模式可以通过 API 设置任意的同源 URL
- History 模式可以通过 API 添加任意类型的数据到历史记录中，Hash 模式只能更改哈希值，也就是字符串
- Hash 模式无需后端配置，并且兼容性好。History 模式在用户手动输入地址或者刷新页面的时候会发起 URL 请求，后端需要配置 `index.html` 页面用于匹配不到静态资源的时候

##### Vue 和 React 之间的区别

Vue 的表单可以使用 `v-model` 支持双向绑定，相比于 React 来说开发上更加方便，当然了 `v-model`其实就是个语法糖，本质上和 React 写表单的方式没什么区别。

改变数据方式不同，Vue 修改状态相比来说要简单许多，React 需要使用 `setState` 来改变状态，并且使用这个 API 也有一些坑点。并且 Vue 的底层使用了依赖追踪，页面更新渲染已经是最优的了，但是 React 还是需要用户手动去优化这方面的问题。

React 16以后，有些钩子函数会执行多次，这是因为引入 Fiber 的原因，这在后续的章节中会讲到。

React 需要使用 JSX，有一定的上手成本，并且需要一整套的工具链支持，但是完全可以通过 JS 来控制页面，更加的灵活。Vue 使用了模板语法，相比于 JSX 来说没有那么灵活，但是完全可以脱离工具链，通过直接编写 `render` 函数就能在浏览器中运行。

在生态上来说，两者其实没多大的差距，当然 React 的用户是远远高于 Vue 的。

在上手成本上来说，Vue 一开始的定位就是尽可能的降低前端开发的门槛，然而 React 更多的是去改变用户去接受它的概念和思想，相较于 Vue 来说上手成本略高。

###### 2.

##### 生命周期

在之前的版本中，如果你拥有一个很复杂的复合组件，然后改动了最上层组件的 `state`，那么调用栈可能会很长。

调用栈过长，再加上中间进行了复杂的操作，就可能导致长时间阻塞主线程，带来不好的用户体验。Fiber 就是为了解决该问题而生。

Fiber 本质上是一个虚拟的堆栈帧，新的调度器会按照优先级自由调度这些帧，从而将之前的同步渲染改成了异步渲染，在不影响体验的情况下去分段计算更新。

对于如何区别优先级，React 有自己的一套逻辑。对于动画这种实时性很高的东西，也就是 16 ms 必须渲染一次保证不卡顿的情况下，React 会每 16 ms（以内） 暂停一下更新，返回来继续渲染动画。

对于异步渲染，现在渲染有两个阶段：`reconciliation` 和 `commit` 。前者过程是可以打断的，后者不能暂停，会一直更新界面直到完成。

**Reconciliation** 阶段

- `componentWillMount`
- `componentWillReceiveProps`
- `shouldComponentUpdate`
- `componentWillUpdate`

**Commit** 阶段

- `componentDidMount`
- `componentDidUpdate`
- `componentWillUnmount`

因为 Reconciliation 阶段是可以被打断的，所以 Reconciliation 阶段会执行的生命周期函数就可能会出现调用多次的情况，从而引起 Bug。由此对于 Reconciliation 阶段调用的几个函数，除了 `shouldComponentUpdate` 以外，其他都应该避免去使用，并且 V16 中也引入了新的 API 来解决这个问题。

##### `getDerivedStateFromProps` 用于替换 `componentWillReceiveProps` ，该函数会在初始化和 `update` 时被调用

##### `getSnapshotBeforeUpdate` 用于替换 `componentWillUpdate` ，该函数会在 `update` 后 DOM 更新前被调用，用于读取最新的 DOM 数据。

##### setState

`setState` 在 React 中是经常使用的一个 API，但是它存在一些的问题经常会导致初学者出错，核心原因就是因为这个 API 是异步的。

`setState` 异步的原因我认为在于，`setState` 可能会导致 DOM 的重绘，如果调用一次就马上去进行重绘，那么调用多次就会造成不必要的性能损失。设计成异步的话，就可以将多次调用放入一个队列中，在恰当的时候统一进行更新过程。

因为多次调用会合并为一次，只有当更新结束后 `state` 才会改变

##### 性能优化

这小节内容集中在组件的性能优化上，这一方面的性能优化也基本集中在 `shouldComponentUpdate` 这个钩子函数上做文章。

在 `shouldComponentUpdate` 函数中我们可以通过返回布尔值来决定当前组件是否需要更新。这层代码逻辑可以是简单地浅比较一下当前 `state` 和之前的 `state` 是否相同，也可以是判断某个值更新了才触发组件更新。一般来说不推荐完整地对比当前 `state` 和之前的 `state` 是否相同，因为组件更新触发可能会很频繁，这样的完整对比性能开销会有点大，可能会造成得不偿失的情况。

当然如果真的想完整对比当前 `state` 和之前的 `state` 是否相同，并且不影响性能也是行得通的，可以通过 immutable 或者 immer 这些库来生成不可变对象。这类库对于操作大规模的数据来说会提升不错的性能，并且一旦改变数据就会生成一个新的对象，对比前后 `state` 是否一致也就方便多了，同时也很推荐阅读下 immer 的源码实现。

另外如果只是单纯的浅比较一下，可以直接使用 `PureComponent`，底层就是实现了浅比较 `state`。

##### 通信

其实 React 中的组件通信基本和 Vue 中的一致。同样也分为以下三种情况：

- 父子组件通信
- 兄弟组件通信
- 跨多层级组件通信
- 任意组件

###### 父子通信

父组件通过 `props` 传递数据给子组件，子组件通过调用父组件传来的函数传递数据给父组件，这两种方式是最常用的父子通信实现办法。

这种父子通信方式也就是典型的单向数据流，父组件通过 `props` 传递数据，子组件不能直接修改 `props`， 而是必须通过调用父组件函数的方式告知父组件修改数据。

###### 兄弟组件通信

对于这种情况可以通过共同的父组件来管理状态和事件函数。比如说其中一个兄弟组件调用父组件传递过来的事件函数修改父组件中的状态，然后父组件将状态传递给另一个兄弟组件。

###### 跨多层次组件通信

如果你使用 16.3 以上版本的话，对于这种情况可以使用 Context API。

###### 任意组件

这种方式可以通过 Redux 或者 Event Bus 解决，另外如果你不怕麻烦的话，可以使用这种方式解决上述所有的通信情况

##### HOC 是什么？相比 mixins 有什么优点？

实现一个函数，传入一个组件，然后在函数内部再实现一个函数去扩展传入的组件，最后返回一个新的组件，这就是高阶组件的概念，作用就是为了更好的复用代码。

其实 HOC 和 Vue 中的 mixins 作用是一致的，并且在早期 React 也是使用 mixins 的方式。但是在使用 class 的方式创建组件以后，mixins 的方式就不能使用了，并且其实 mixins 也是存在一些问题的，比如：

- 隐含了一些依赖，比如我在组件中写了某个 `state` 并且在 `mixin` 中使用了，就这存在了一个依赖关系。万一下次别人要移除它，就得去 `mixin` 中查找依赖
- 多个 `mixin` 中可能存在相同命名的函数，同时代码组件中也不能出现相同命名的函数，否则就是重写了，其实我一直觉得命名真的是一件麻烦事。。
- 雪球效应，虽然我一个组件还是使用着同一个 `mixin`，但是一个 `mixin` 会被多个组件使用，可能会存在需求使得 `mixin` 修改原本的函数或者新增更多的函数，这样可能就会产生一个维护成本

##### 事件机制

const Test = ({ list, handleClick }) => ({

  list.map((item, index) => (

​      <span onClick={handleClick} key={index}>{index}</span>

  ))

})

React 其实自己实现了一套事件机制

以上类似代码想必大家经常会写到，但是你是否考虑过点击事件是否绑定在了每一个标签上？事实当然不是，JSX 上写的事件并没有绑定在对应的真实 DOM 上，而是通过事件代理的方式，将所有的事件都统一绑定在了 `document` 上。这样的方式不仅减少了内存消耗，还能在组件挂载销毁时统一订阅和移除事件。

另外冒泡到 `document` 上的事件也不是原生浏览器事件，而是 React 自己实现的合成事件（SyntheticEvent）。因此我们如果不想要事件冒泡的话，调用 `event.stopPropagation` 是无效的，而应该调用 `event.preventDefault`。

那么实现合成事件的目的是什么呢？总的来说在我看来好处有两点，分别是：

- 合成事件首先抹平了浏览器之间的兼容问题，另外这是一个跨浏览器原生事件包装器，赋予了跨浏览器开发的能力
- 对于原生浏览器事件来说，浏览器会给监听器创建一个事件对象。如果你有很多的事件监听，那么就需要分配很多的事件对象，造成高额的内存分配问题。但是对于合成事件来说，有一个事件池专门来管理它们的创建和销毁，当事件需要被使用时，就会从池子中复用对象，事件回调结束后，就会销毁事件对象上的属性，从而便于下次复用事件对象。

在 JavaScript 中，**true && expression** 总是返回 **expression**，而 **false && expression** 总是返回 **false**。

因此，如果条件是 **true**，**&&** 右侧的元素就会被渲染，如果是 **false**，React 会忽略并跳过它。

在JavaScript中，true && expression总是返回expression，而false && expression总是返回false。

因此，如果条件是true，&&右侧的元素就会被渲染，如果是false，React会忽略并跳过它。

###### 3.

#### react 迷惑的点

##### 为什么要引入 React

因为从本质上讲，JSX 只是为 `React.createElement(component, props, ...children)` 函数提供的语法糖。

##### 为什么要用 className 而不用 class

1.React 一开始的理念是想与浏览器的 DOM API 保持一直而不是 HTML，因为 JSX 是 JS 的扩展，而不是用来代替 HTML 的，这样会和元素的创建更为接近。在元素上设置 `class` 需要使用 `className` 这个 API

2.浏览器问题，ES5 之前，在对象中不能使用保留字。

3.解构问题，当你在解构属性的时候，如果分配一个 `class` 变量会出问题

##### 为什么属性要用小驼峰

因为 JSX 语法上更接近 JavaScript 而不是 HTML，所以 React DOM 使用 `camelCase`（小驼峰命名）来定义属性的名称，而不使用 HTML 属性名称的命名约定。

##### 为什么 constructor 里要调用 super 和传递 props

**为什么要调用 super**

其实这不是 React 的限制，这是 JavaScript 的限制，在构造函数里如果要调用 this，那么提前就要调用 super，在 React 里，我们常常会在构造函数里初始化 state，`this.state = xxx` ，所以需要调用 super。

**为什么要传递 props**

要是构造函数中调用了某个访问 `props` 的方法，那这个 bug 就更难定位了。**因此我强烈建议始终使用super(props)，即使这不是必须的**

##### 为什么调用方法要 bind this

你必须谨慎对待 JSX 回调函数中的 `this`，在 JavaScript 中，class 的方法默认不会[绑定](https://link.juejin.im?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_objects%2FFunction%2Fbind)`this`。如果你忘记绑定 `this.handleClick` 并把它传入了 `onClick`，当你调用这个函数的时候 `this` 的值为 `undefined`。

这并不是 React 特有的行为；这其实与 [JavaScript 函数工作原理](https://link.juejin.im?target=https%3A%2F%2Fwww.smashingmagazine.com%2F2014%2F01%2Funderstanding-javascript-function-prototype-bind%2F)有关。通常情况下，如果你没有在方法后面添加 `()`，例如 `onClick={this.handleClick}`，你应该为这个方法绑定 `this`。

##### React是如何处理事件的？

React的事件是合成事件。

简单的理解react如何处理事件的，React在组件加载（mount）和更新（update）时，将事件通过addEventListener统一注册到document上，然后会有一个事件池存储了所有的事件，当事件触发的时候，通过dispatchEvent进行事件分发。

##### 四种事件处理对比

1.直接bind this型

```
class Foo extends React.Component{
	handleClick(){
		this.setState({xxx:aaa})
	}
	render(){
		return(
			<button onClick={this.handleClick.bind(this)}>
				Click me
			</button>
		)
	}
}
```

优点：写起来顺手，一口气就能把这个逻辑写完，不用移动光标到其他地方。

缺点：性能不太好，这种方式跟 react 内部帮你 bind 一样的，每次 render 都会进行 bind，而且如果有两个元素的事件处理函数是同一个，也还是要进行 bind，这样会多写点代码，而且进行两次 bind，性能不是太好。(其实这点性能往往不会是性能瓶颈的地方，如果你觉得顺手，这样写完全没问题)

2.constructor 手动bind型

```
class Foo extends React.Component{
	constructor(props){
		super(props)
		this.handleClick = this.handleClick.bind(this);
	}
	handleClick(){
		this.setState({xxx:aaa})
	}
	render(){
		return(
			<button onCick={this.handleClick}>Click me</button>
		)
	}
}
```

优点：相比于第一种性能更好，因为构造函数只执行一次，那么只会bind一次，而且如果有多个元素都需要调用这个函数，也不需要重复bind，基本上解决了第一种的两个缺点。

缺点：没有明显的缺点，硬要说的话就是太丑了，然后不顺手

3.箭头函数型

```
class Foo extends React.Component{
	handleClick(){
		this.setState({xxx:aaa})
	}
	render(){
		return(
			<button onClick={(e)=>this.handleClick(e)}>
				Click me
			</button>
		)
	}
}
```

优点：顺手，好看

缺点：每次render都会重复创建函数，性能会差一点

4.利用class fields(类字段)型

```
class Foo extends React.Component{
	handleClick = ()=>{
		this.setState({xxx:aaa})
	}
	render(){
		return(
			<button onClick={this.handleClick}>click me</button>
		)
	}
}
```

优点：好看，性能好

##### 为什么要setState，而不是直接this.state.xx = oo

setState做的事情不仅仅只是修改了this.state的值，另外最重要的是它会触发React的更新机制，会进行diff，然后将patch部分更新到真实dom里。

如果你直接this.sate.xx=oo的话，state的值确实会改，但是改了不会触发UI的更新，那就不是数据驱动了。

##### setState是同步还是异步相关问题

1.setState时候同步还是异步？

执行过程代码同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形成了所谓的“异步”，所以表现出来有时是同步，有时是“异步”。

2.何时是同步，何时是异步呢？

只在合成事件和钩子函数中是“异步”的，在原生事件和setTimeout/setInterval等原生API中都是同步的。简单的可以理解为被React控制的函数里面就会表现出“异步”，反之表现为同步。

3.那为什么会出现异步的情况呢？

为了做性能优化，将 state 的更新延缓到最后批量合并再去渲染对于应用的性能优化是有极大好处的，如果每次的状态改变都去重新渲染真实 dom，那么它将带来巨大的性能消耗。

4.那如何在表现出异步的函数里可以准确拿到更新后的 state 呢？

通过第二个参数 `setState(partialState, callback)` 中的 callback 拿到更新后的结果。

或者可以通过给 setState 传递函数来表现出同步的情况：

```
this.setState((state) => {
	return { val: newVal }
})
```

5.那表现出异步的原理是怎么样的呢？

我这里还是用最简单的语言让你理解：在 React 的 setState 函数实现中，会根据 isBatchingUpdates(默认是 false) 变量判断是否直接更新 this.state 还是放到队列中稍后更新。然后有一个 batchedUpdate 函数，可以修改 isBatchingUpdates 为 true，当 React 调用事件处理函数之前，或者生命周期函数之前就会调用 batchedUpdate 函数，这样的话，setState 就不会同步更新 this.state，而是放到更新队列里面后续更新。

这样你就可以理解为什么原生事件和 setTimeout/setinterval 里面调用 this.state 会同步更新了吧，因为通过这些函数调用的 React 没办法去调用 batchedUpdate 函数将 isBatchingUpdates 设置为 true，那么这个时候 setState 的时候默认就是 false，那么就会同步更新。

##### JSX是什么？

全称：javascript and XML

定义：可扩展（自定义）标记性语言，基于javascript，融入了XML，我们可以在js中书写xml，使用JSX可以很好的描述UI在页面中应该呈现它应有的交互形式

```
ReactDOM.render(要渲染的组件，组件要挂载的位置)
```

JSX其实就是javascript对象，是用来描述UI结构信息的。

JSX是JavaScript语言的一种语法扩展，长得像HTML，但并不是HTML，附加了原生HTML标签不具备的能力，例如：自定义属性，以及后续的组件传值

UI界面显示什么样，取决于JSX对象结构，换句话说，取决于render()函数里面的return关键字后面返回的JSX内容结构

引入React.js库是为了解析识别JSX语法，同时创建虚拟DOM，而引入react-dom是为了渲染组件，将组件挂载到特定的位置上，同时将虚拟DOM转换为真实DOM，插入到页面中

使用扩展运算符...在JSX中传递整个props对象

JSX中添加属性的命名方式是camelCase 

React中定义组件，组件名称的首字母大写。

##### React的工作方式及优点

使用React的方式，就可以避免构建这样复杂的程序结构，无论何种事件，引发的都是React组件的重新渲染，它只会修改数据变化的DOM部分，并不需要去关心怎么去操作DOM

![img](https://user-gold-cdn.xitu.io/2019/7/26/16c2c557affceade?imageslim)



构建组件，本质上就是编写javascript函数，而组件中最重要的是数据，在React中数据分两种：props和state。当定义一个组件时，它接收任意的形参（即props），并用于返回描述页面展示内容的React元素

无论props还是state，当它们任何一个发生改变时，都会引发render函数的重新渲染

一个UI组件所渲染的结果，就是通过props和state这两个属性在render方法里面映射生成对应的HTML结构

##### Props

当通过函数声明或class自定义一个组件时，它会将JSX所接受的属性转换为以对象传递给该定义时的组件，这个接收的对象就是props。props就是组件定义属性的集合，它是组件对外的接口，由外部通过JSX属性传入设置（也就是从外部传递给内部组件的数据）

在React中，你可以将prop类似于HTML标签元素的属性。而在React中，Prop的属性值类型可以任何数据类型。当然如果是非字符串数据类型，在JSX中，必须要用{}把prop值包裹起来。

父组件向子组件传值是通过设置JSX属性的方式实现的，而在子组件内部获取父组件数据是通过this.props来获取的。也可以这么认为，props就是对外提供的数据接口。

##### State

state代表的是当前组件的内部状态，可以把组件看成一个‘状态机’，它是能够随着时间变化的数据，更多的是应当在实现交互时使用，根据状态state的改变呈现不同的UI展示。

当需要记录组件自身数据变化时，想要使组件具备交互的能力，那么需要有触发该组件基础数据模型改变的能力，那么此时就需要使用state

通过在React中封装的事件，例如;onChange、onClick、onKeyDown、onFocus、onBlur等这些事件类型里面绑定事件方法内的setState都是异步的。

如果是React控制的事件处理程序以及在它的钩子函数内调用setState，它不会同步的更新state

在这里，只需要知道，对于在React中的JSX绑定的事件处理函数中调用setState方法是异步的就可以了。

如果你需要基于当前的state来计算出新的值，那么setState就应该传递一个函数，而不是一个对象，它可以确保每次调用的都是使用最新的state。

##### 多个setState调用会合并处理

当在事件处理方法内多次调用setState方法时，render函数只会执行一次，并不会导致组件的重复渲染，因为React会将多个this.setState产生的修改放在一个队列里面进行批量延迟处理，所以从这点上讲，React设计这个setState是非常高效的，结合了函数式编程，不用考虑性能的问题。

##### 那么究竟什么样的数据属性可以视为状态？

状态（state）应该是会随着时间产生变化的数据，当更改这个状态（state），需要更新组件的UI，就可以将它定义成state，更多是在实现页面的交互时使用的

写静态，没有任何交互页面时，用props进行数据的填充

##### 无状态的组件（UI组件/函数式组件）

可以用纯粹的函数来定义，所谓纯函数，只有输入和输出，无状态，无生命周期钩子函数，只是用作于接受父组件传来的props值渲染生成DOM结构，无交互，无逻辑层的数据展示

无状态（函数式）组件，在性能上是最高效的，开销很低，因为没有那些生命周期函数

##### props与state的灵魂对比

相同点：都是组件内的数据,是一普通的javascript对象,都是用来保存信息的,这些信息可以控制组件的形态

不同点：

props是由父组件传入的(类似形参),用于定义外部组件的接口,是React组件的输入,它是从父组件传递给子组件的数据对象,在父(外部)组件JSX元素上,以自定义属性的形式定义，传递给当前组件,而在子组件内部,则以this.props或者props进行获取

props只具备读的能力,不能直接被修改,如果想要修改某些值,用来响应用户的输入或者输出响应,可以借用React内提供的setState函数进行触发,并用state来作为替代

state是当前组件的内部状态,它的作用范围只局限于当前组件,它是当前组件的一个私有变量.用于记录组件内部状态的,如果组件中的一些数据在某些时刻发生变化,或者做一些页面逻辑交互时,需要更新UI,这个时候就需要使用state来跟踪状态(例如控制一元素的显示隐藏来回切换等状态),它由组件本身管理,可以通过setState函数修改state

##### 向事件处理程序中传递参数

以下两种方式都可以向事件处理函数传递参数

```
<button onClick={this.handleBtnDelete.bind(this,id)}>删除</button>
或者
<button onClick={(e)=>this.handleBtnDelete(id,e)}>删除</button>
```

使用箭头函数，React的事件对象会被作为第二个参数传递，而且也必须显示的传递进去

而通过bind的方式，事件对象以及更多的参数将会被隐式的传递进去

###### 4.

##### React灵魂23问

###### 1.setState是异步还是同步？

1.合成事件中是异步（合成事件：react为了解决跨平台，兼容性问题，自己封装了一套事件机制，代理了原生的事件，像在jsx中常见的onClick、onChange这些都是合成事件）

2.钩子函数中的是异步

3.原生事件中是同步（原生事件：是指非react合成事件，原生自带的事件监听addEventListener，或者也可以用原生js、jq直接document.querySelector().onclick这种绑定事件的形式都属于原生事件。）

4.setTimeout中是同步（在setTimeout中去setState并不算是一个单独的场景，它是随着你外层去决定的，因为你可以在合成事件中setTimeout，可以在钩子函数中setTimeout，也可以在原生事件setTimeout，但是不管是哪个场景下，基于event loop的模型下，setTimeout中里去setState总能拿到最新的state值）。

###### setState中的批量更新

在setState的时候react内部会创建一个updateQueue，通过firstUpdate、lastUpdate、lastUpdate.next去维护一个更新的队列，在最终的performWork中，相同的key会被覆盖，只会对最后一次的setState进行更新。

1、setState只在合成事件和钩子函数中是异步的，在原生事件和setTimeout中都是同步的。

2、setState的异步并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形成了所谓的“异步”，当然可以通过第二个参数setState(partialState, callback)中的callback拿到更新后的结果。

3、setState的批量更新优化也是建立在“异步”（合成事件、钩子函数）之上的，在原生事件和setTimeout中不会批量更新，在“异步”中如果对同一个值进行多次setState，setState的批量更新策略会对其进行覆盖，取最后一次的执行，如果是同时setState多个不同的值，在更新时会对其进行合并批量更新。

###### 2.react@16.4+的生命周期

![preview](https://pic3.zhimg.com/v2-34344a720b3e38ff64e2a26c2089680a_r.jpg)

###### 3.useEffect(fn, [])和componentDidMount有什么差异？

useEffect会捕获props和state。所以即便在回调函数里，你拿到的还是初始的props和state。如果想得到“最新”的值，可以使用ref。

###### 4.hooks为什么不能放在条件判断里？

以useState为例，在react内部，每个组件（Fiber）的hooks都是以链表的形式存在memoizeState属性中。update阶段，每次调用useState，链表就会执行next向后移动一步。如果将useState写在条件判断中，假设条件判断不成立，没有执行里面的useState方法，会导致接下来所有的useState的取值出现偏移，从而导致异常发生。

###### 5.fiber是什么？

React Fiber是一种基于浏览器的单线程调度算法。

React Fiber 用类似requestIdleCallback的机制来做异步diff。但是之前数据结构不支持这样的实现异步diff，于是React实现了一个类似链表的数据结构，将原来的递归diff变成了现在的遍历diff，这样就能做到异步可更新了。

![preview](https://pic4.zhimg.com/v2-8e691022bf76747599b85f7c7797206b_r.jpg)

###### 6.聊一聊diff算法

传统diff算法的时间复杂度是O(n^3)，这在前端render中是不可接受的。为了降低时间复杂度，react的diff算法做了一些妥协，放弃了最优解，最终将时间复杂度降低到了O(n)。

那么react diff算法做了哪些妥协呢？参考如下：

1.tree diff:只对比同一层的dom节点，忽略dom节点的跨层级移动

如下图，react只会对相同颜色方框内的DOM节点进行比较，即同一个父节点下的所有子节点。当发现节点不存在时，则该节点及其子节点会被完全删除掉，不会用于进一步的比较。

这样只需要对树进行一次遍历，便能完成整个DOM树的比较。

![preview](https://pic3.zhimg.com/v2-d2cb592bc4da53a85dbf6cef2dd7c9ee_r.jpg)

这就意味着，如果dom节点发生了跨层级移动，react会删除旧的节点，生成新的节点，而不会复用。

2.component diff:如果不是同一类型的组件，会删除旧的组件，创建新的组件

![preview](https://pic2.zhimg.com/v2-d8d3646da454398e0defacb61b5e1081_r.jpg)

3.element diff:对于同一层级的一组子节点，需要通过唯一id进行区分。

如果没有id来进行区分，一旦有插入动作，会导致插入位置之后的列表全部重新渲染。这也是为什么渲染列表时为什么要使用唯一的key。

###### 7.调用setState之后发生了什么？

1.在setState的时候，React会为当前节点创建一个updateQueue的更新列队。

2.然后会触发reconciliation过程，在这个过程中，会使用名为Fiber的调度算法，开始生成新的Fiber树，Fiber算法的最大特点是可以做到异步中断的执行。

3.然后React Scheduler会根据优先级高低，先执行优先级高的节点，具体是执行doWork方法。

4.在doWork方法中，React会执行一遍updateQueue中的方法，以获得新的节点。然后对比新旧节点，为老节点打上更新、插入、替换等Tag。

5.当前节点doWork完成后，会执行performUnitOfWork方法获得最新节点，然后再重复上面的过程。

6.当所有节点都doWork完成后，会触发commitRoot方法，React进入commit阶段。

7.在commit阶段中，React会根据前面为各个节点打的Tag，一次性更新整个dom元素。

###### 8.为什么虚拟dom会提高性能？

虚拟dom相当于在JS和真实dom中间加了一个缓存，利用diff算法避免了没有必要的dom操作，从而提高性能。

###### 9.错误边界是什么？它有什么用？

在React中，如果任何一个组件发生错误，它将破坏整个组件树，导致整页白屏。这时候我们可以用错误边界优雅地降级处理这些错误。

###### 10.什么事Portals?

Portal提供了一种将子节点渲染到存在于父组件以外的DOM节点的优秀的方案。

```
ReactDOM.createPortal(child, container)
```

###### 11.React组件间有哪些通信方式？

1.父组件向子组件通信

通过props传递

2.子组件向父组件通信

主动调用通过props传过来的方法，并将想要传递的信息，作为参数，传递到父组件的作用域中

3.跨层级通信

1）使用react自带的Context进行通信，createContext创建上下文，useContext使用上下文。

2）使用Redux或者Mobx等状态管理库

3）使用订阅发布模式

###### 12.React父组件如何调用子组件中的方法？

1、如果是在方法组件中调用子组件（`>= react@16.8`），可以使用 `useRef` 和 `useImperativeHandle`:

```text
const { forwardRef, useRef, useImperativeHandle } = React;

const Child = forwardRef((props, ref) => {
  useImperativeHandle(ref, () => ({
    getAlert() {
      alert("getAlert from Child");
    }
  }));
  return <h1>Hi</h1>;
});

const Parent = () => {
  const childRef = useRef();
  return (
    <div>
      <Child ref={childRef} />
      <button onClick={() => childRef.current.getAlert()}>Click</button>
    </div>
  );
};
```

2、如果是在类组件中调用子组件（`>= react@16.4`），可以使用 `createRef`:

```text
const { Component } = React;

class Parent extends Component {
  constructor(props) {
    super(props);
    this.child = React.createRef();
  }

  onClick = () => {
    this.child.current.getAlert();
  };

  render() {
    return (
      <div>
        <Child ref={this.child} />
        <button onClick={this.onClick}>Click</button>
      </div>
    );
  }
}

class Child extends Component {
  getAlert() {
    alert('getAlert from Child');
  }

  render() {
    return <h1>Hello</h1>;
  }
}
```

###### 13.React有哪些优化性能的手段？

###### 类组件中的优化手段

1）使用纯组件PureComponent作为基类

2）使用React.memo高阶函数包装组件

3）使用shouldComponentUpdate生命函数来定义渲染逻辑

###### 方法组件中的优化手段

1）使用useMemo

2)使用useCallBack

###### 其他方式

1.在列表需要频繁变动时，使用唯一id作为key，而不是数组下标。

2.必要时通过改变CSS样式隐藏显示组件，而不是通过条件判断显示隐藏组件

3.使用Suspense和lazy进行懒加载

###### 14.为什么React元素有一个$$typeof属性？

![preview](https://pic2.zhimg.com/v2-1d9e3361bd9ec0f42db7af5192497eb1_r.jpg)

目的是为了防止XSS攻击。因为Symbol无法被序列化，所以React可以通过有没有$$typeof属性来断出当前的element对象是从数据库来的还是自己生成的。

如果没有$$typeof这个属性，react会拒绝处理该元素。

###### 15.React如何区分Class组件和Function组件？

一般的方式是借助typeof和Function.prototype.toString来判断当前是不是class,如下：

```
function isClass(func) {
  return typeof func === 'function'
    && /^class\s/.test(Function.prototype.toString.call(func));
}
```

但是这个方式有它的局限性，因为如果用了babel等转换工具，将class写法全部转为function写法，上面的判断就会失效。

React区分Class组件和Function组件的方式很巧妙，由于所有的类组件都要继承React.Component,所以只要判断原型链上是否有React.Component就可以了。

```
AComponent.prototype instanceof React.Component
```

###### 16.HTML和React事件处理有什么区别？

在HTML中事件名必须小写：

```
<button onclick='activateLasers'>
```

而在React中需要遵循驼峰写法：

```
<button onClick={activateLasers}>
```

在HTML中可以返回false以阻止默认的行为：

```
<a href="#" onclick='console.log("The link was clicked."); return false;'></a>
```

而在React中必须明确地调用preventDefault();

```
function handleClick(event){
	event.preventDefault();
	console.log('The link was clicked.');
}
```

###### 17.什么事suspense组件？

Suspense让组件“等待”某个异步操作，直到该异步操作结束即可渲染。

```
const resource = fetchProfileData();

function ProfilePage() {
  return (
    <Suspense fallback={<h1>Loading profile...</h1>}>
      <ProfileDetails />
      <Suspense fallback={<h1>Loading posts...</h1>}>
        <ProfileTimeline />
      </Suspense>
    </Suspense>
  );
}

function ProfileDetails() {
  // 尝试读取用户信息，尽管该数据可能尚未加载
  const user = resource.user.read();
  return <h1>{user.name}</h1>;
}

function ProfileTimeline() {
  // 尝试读取博文信息，尽管该部分数据可能尚未加载
  const posts = resource.posts.read();
  return (
    <ul>
      {posts.map(post => (
        <li key={post.id}>{post.text}</li>
      ))}
    </ul>
  );
}
Suspense 也可以用于懒加载，参考下面的代码：

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

###### 18.为什么JSX中的组件名要以大写字母开头？

因为React要直到当前渲染的是组件还是HTML元素。

###### 19.redux是什么？

Redux是一个为JavaScript应用设计的，可预测的状态容器。

它解决了如下问题：

- 跨层级组件之间的数据传递变得很容易
- 所有对状态的改变都需要dispatch，使得整个数据的改变可追踪，方便排查问题。

但是它也有缺点：

- 概念偏移，理解起来不容易
- 样板代码太多

###### 20.react-redux的实现原理？

通过redux和 react context配合使用，并借助高阶函数，实现了 react-redux。

###### 21.redux和mobx的区别？

得益于Mobx的observable，使用mobx可以做到精准更新；对应的Redux是用dispatch进行广播，通过Provider和connect来比对前后差别控制更新粒度

###### 22.redux有哪些异步中间件？

1.redux-thunk

源代码简短优雅，上手简单

2.redux-saga

借助JS的generator来处理异步，避免了回调的问题

3.redux-observable

借助了RxJS流的思想以及其各种强大的操作符，来处理异步问题

###### 5.

#### React笔记

###### React是什么？

React是一个声明式，高效且灵活的用于构建用户界面的JavaScript裤。使用React可以将一些简短、独立的代码片段组合成复杂的UI界面，这些代码片段被称作“组件”。

React中拥有多种不同类型的组件。

一个组件接收一些参数，我们把这些参数叫做props('props'是‘properties’简写)，然后通过render方法返回需要展示在屏幕上的视图的层次结构。

render方法的返回值描述了你希望在屏幕上看到的内容。React根据描述，然后把结果展示出来。更具体地说，render返回了一个React元素，这是一种对渲染内容的轻量级描述。

在JSX中你可以任意使用JavaScript表达式，只需要用一个大括号把表达式括起来。每一个React元素事实上都是一个JavaScript对象，你可以在你的程序中把它当保存在变量中或者作为参数传递。

在React应用中，数据通过props的传递，从父组件流向子组件。

```
class Square extends React.Component {
	render(){
		<button className='square' onClick={()=>alert('click')>
			{this.props.value}
		</button>
	}
}
```

注意，此处使用了 onClic={()=>alert('click')}的方式向onClick这个prop传入一个函数。React将在单击时调用此函数。很多人经常忘记编写()=>,而写成了onClick={alert('click')}，这种常见的错误会导致每次这个组件渲染的时候都会触发弹出框。

可以通过在React组件的构造函数中设置this.state来初始化state。this.state应该被视为一个组件的私有属性。

在JavaScript中，每次你定义其子类的构造函数时，都需要调用super方法。因此，在所有含有构造函数的React组件中，构造函数必须以super(props)开头。

每次在组件中调用setState时，React都会自动更新其子组件。

**当你遇到需要同时获取多个子组件数据，或者两个组件之间需要相互通讯的情况时，需要把子组件的 state 数据提升至其共同的父组件当中保存。之后父组件可以通过 props 将状态数据传递到子组件当中。这样应用当中所有组件的状态数据就能够更方便地同步共享了。**

###### 为什么不可变性在 React 中非常重要

一般来说，有两种改变数据的方式。第一种方式是直接*修改*变量的值，第二种方式是使用新的一份数据替换旧数据。

直接修改数据

```
var player = {score: 1, name: 'Jeff'};
player.score = 2;
//player修改后的值为 {score: 2, name: 'Jeff'}
```

新数据替换旧数据

```
var player = {score: 1, name: 'Jeff'};
var newPlayer = Object.assign({},player,{score:2});
//player的值没有改变，但是newPlayer的值是{score: 2, name: 'Jeff'}
//使用对象展开语法，就可以写成：
var newPlayer = {...player,score:2};
```

不直接修改（或改变底层数据）这种方式和前一种方式的结果是一样的，这种方式有以下几点好处：

###### 简化复杂的功能

不可变性使得复杂的特性更容易实现。在后面的章节里，我们会实现一种叫做“时间旅行”的功能。“时间旅行”可以使我们回顾井字棋的历史步骤，并且可以“跳回”之前的步骤。这个功能并不是只有游戏才会用到——撤销和恢复功能在开发中是一个很常见的需求。不直接在数据上修改可以让我们追溯并复用游戏的历史记录。

###### 跟踪数据的改变

如果直接修改数据，那么就很难跟踪到数据的改变。跟踪数据的改变需要可变对象可以与改变之前的版本进行对比，这样整个对象树都需要被遍历一次。

跟踪不可变数据的变化相对来说就容易多了。如果发现对象变成了一个新对象，那么我们就可以说对象发生改变了。

###### 确定在 React 中何时重新渲染

不可变性最主要的优势在于它可以帮助我们在 React 中创建 *pure components*。我们可以很轻松的确定不可变数据是否发生了改变，从而确定何时对组件进行重新渲染。

###### 函数组件

如果你想写的组件只包含一个 `render` 方法，并且不包含 state，那么使用**函数组件**就会更简单。我们不需要定义一个继承于 `React.Component` 的类，我们可以定义一个函数，这个函数接收 `props` 作为参数，然后返回需要渲染的元素。函数组件写起来并不像 class 组件那么繁琐，很多组件都可以使用函数组件来写。

```
function Square(props){
	return(
		<button className="square" onClick={props.onClick}>
			{props.value}
		</button>
	)
}
```

`concat()` 方法可能与你比较熟悉的 `push()` 方法不太一样，它并不会改变原数组，所以我们推荐使用 `concat()`。

**我们强烈推荐，每次只要你构建动态列表的时候，都要指定一个合适的 key。**

组件的 key 值并不需要在全局都保证唯一，只需要在当前的同一级元素之前保证唯一即可。

###### 一组JavaScript构建工具链通常由这些组成：

- 一个package管理器，比如Yarn或npm 。它能让你充分利用庞大的第三方package的生态系统，并且轻松地安装或更新它们。
- 一个打包器，比如webpack或Parcel。它能让你编写模块化代码，并将它们组合在一个成为小的package，以优化加载时间。
- 一个编译器，例如Babel。它能让你编写的新版本JavaScript代码，在旧版本浏览器中依然能够工作。

###### JSX简介

```
const element = <h1>Hello,world!</h1>;
```

这个有趣的标签语法既不是字符串也不是 HTML。

它被称为 JSX，是一个 JavaScript 的语法扩展。我们建议在 React 中配合使用 JSX，JSX 可以很好地描述 UI 应该呈现出它应有交互的本质形式。JSX 可能会使人联想到模版语言，但它具有 JavaScript 的全部功能。

JSX 可以生成 React “元素”。

###### 为什么使用JSX?

React 认为渲染逻辑本质上与其他 UI 逻辑内在耦合，比如，在 UI 中需要绑定处理事件、在某些时刻状态发生变化时需要通知到 UI，以及需要在 UI 中展示准备好的数据。

React 并没有采用将*标记与逻辑进行分离到不同文件*这种人为地分离方式，而是通过将二者共同存放在称之为“组件”的松散耦合单元之中，来实现[*关注点分离*](https://en.wikipedia.org/wiki/Separation_of_concerns)。

React [不强制要求](https://reactjs.bootcss.com/docs/react-without-jsx.html)使用 JSX，但是大多数人发现，在 JavaScript 代码中将 JSX 和 UI 放在一起时，会在视觉上有辅助作用。它还可以使 React 显示更多有用的错误和警告消息。

###### 在JSX中嵌入表达式

在 JSX 语法中，你可以在大括号内放置任何有效的 [JavaScript 表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions)。例如，`2 + 2`，`user.firstName` 或 `formatName(user)` 都是有效的 JavaScript 表达式。

为了便于阅读，我们会将 JSX 拆分为多行。同时，我们建议将内容包裹在括号中，虽然这样做不是强制要求的，但是这可以避免遇到[自动插入分号](http://stackoverflow.com/q/2846283)陷阱。

###### JSX也是一个表达式

在编译之后，JSX 表达式会被转为普通 JavaScript 函数调用，并且对其取值后得到 JavaScript 对象。

也就是说，你可以在 `if` 语句和 `for` 循环的代码块中使用 JSX，将 JSX 赋值给变量，把 JSX 当作参数传入，以及从函数中返回 JSX：

```
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

###### JSX特定属性

你可以通过使用引号，来将属性值指定为字符串字面量：

```
const element = <div tabIndex="0"></div>;
```

也可以使用大括号，来在属性值中插入一个 JavaScript 表达式：

```
const element = <img src={user.avatarUrl}></img>;
```

在属性中嵌入 JavaScript 表达式时，不要在大括号外面加上引号。你应该仅使用引号（对于字符串值）或大括号（对于表达式）中的一个，对于同一属性不能同时使用这两种符号。

警告：因为JSX语法上更接近JavaScript而不是HTML，所以React DOM使用camelCase(小驼峰命名)来定义属性的名称，而不使用HTML属性名称的命名约定。

例如，JSX里的class变成了className，而tabindex则变为tabIndex。

###### 使用JSX指定子元素

假如一个标签里面没有内容，你可以使用 `/>` 来闭合标签，就像 XML 语法一样：

JSX 标签里能够包含很多子元素:

###### JSX防止注入攻击

React DOM 在渲染所有输入内容之前，默认会进行[转义](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html)。它可以确保在你的应用中，永远不会注入那些并非自己明确编写的内容。所有的内容在渲染之前都被转换成了字符串。这样可以有效地防止 [XSS（cross-site-scripting, 跨站脚本）](https://en.wikipedia.org/wiki/Cross-site_scripting)攻击。

###### JSX表示对象

Babel会把JSX转译成一个名为React.createElement()函数调用。

`React.createElement()` 会预先执行一些检查，以帮助你编写无错代码。

###### 元素渲染

元素是构成React应用的最小砖块。元素描述了你在屏幕上想看到的内容。

与浏览器的DOM元素不同，React元素是创建开销极小的普通对象。React DOM会负责更新DOM来与React元素保持一致。

组件是由元素构成的。

###### 将一个元素渲染为DOM

仅使用 React 构建的应用通常只有单一的根 DOM 节点。

想要将一个 React 元素渲染到根 DOM 节点中，只需把它们一起传入 [`ReactDOM.render()`](https://reactjs.bootcss.com/docs/react-dom.html#render)

###### 更新已渲染的元素

React 元素是[不可变对象](https://en.wikipedia.org/wiki/Immutable_object)。一旦被创建，你就无法更改它的子元素或者属性。一个元素就像电影的单帧：它代表了某个特定时刻的 UI。

根据我们已有的知识，更新 UI 唯一的方式是创建一个全新的元素，并将其传入 [`ReactDOM.render()`](https://reactjs.bootcss.com/docs/react-dom.html#render)。

###### React只更新它需要更新的部分

React DOM会将元素和它的子元素与它们之前的状态进行比较，并只会进行必要的更新来使DOM达到预期的状态。

###### 组件&Props

组件允许你将UI拆分为独立可复用的代码片段，并对每个片段进行独立构思。

组件，从概念上类似于JavaScript函数。它接受任意的入参（即“props”）,并返回用于描述页面展示内容的React元素。

###### 函数组件与class组件

定义组件最简单的方式就是编写JavaScript函数

```
function Welcome(props){
	return <h1>Hello, {props.name}</h1>;
}
```

该函数是一个有效的React组件，因为它接收唯一带有数据的“props”(代表属性)对象与并返回一个React元素。这类组件被称为“函数组件”，因为它本质上就是JavaScript函数。

同时还可以使用ES6的class来定义组件：

```
class Welcome extends React.Component{
	render(){
		return <h1>Hello, {this.props.name}</h1>;
	}
}
```

上述两个组件在React里是等效的。

###### 渲染组件

之前，我们遇到的React元素都只是DOM标签：

```
const element = <div />
```

不过，React元素也可以是用户自定义的组件：

```
const element = <Welcome name='Sara'>;
```

当 React 元素为用户自定义组件时，它会将 JSX 所接收的属性（attributes）以及子组件（children）转换为单个对象传递给组件，这个对象被称之为 “props”。

注意：组件名称必须以大写字母开头。React会将以小写字母开头的组件视为原生DOM标签。

###### 组合组件

组件可以在其输出中引用其他组件。这就可以让我们用同一组件来抽象出任意层次的细节。按钮、表单、对话框、甚至整个屏幕的内容：在React应用程序中，这些通常都会以组件的形式表示。

###### 提取组件

将组件拆分为更小的组件。

###### Props的只读性

组件无论是使用函数声明还是通过class声明，都决不能修改自身的props。

所有React组件都必须像纯函数一样保护它们的props不被更改。

###### State & 生命周期

在元素渲染章节中，我们只了解了一种更新UI界面的方法。通过调用ReactDOM.render()来修改我们想要渲染的元素。

State与props类似，但是state是私有的，并且完全受控于当前组件。

###### 将函数组件转换成class组件

通过以下五步将Clock的函数组件转成class组件：

1. 创建一个同名的ES6 class，并且继承于React.Component。
2. 添加一个空的render()方法。
3. 将函数体移动到render()方法之中。
4. 在render()方法中使用this.props替换props。
5. 删除剩余的空函数声明。

###### 向class组件中添加局部的state

###### 将生命周期方法添加到Class中

在具有许多组件的应用程序中，当组件被销毁时释放所占用的资源是非常重要的。

###### 正确地使用State

###### 不要直接修改State 

而是应该使用setState()

###### State的更新可能是异步的

出于性能考虑，React可能会把多个setState()调用合并成一个调用。

因为this.props和this.state可能会异步更新，所以不要依赖他们的值来更新下一个状态。

###### State的更新会被合并

当你调用setState()的时候，React会把你提供的对象合并到当前的state。

###### 数据是向下流动的

不管是父组件或是子组件都无法知道某个组件是有状态的还是无状态的，并且它们也并不关心它是函数组件还是 class 组件。

这就是为什么称 state 为局部的或是封装的的原因。除了拥有并设置了它的组件，其他组件都无法访问。

组件可以选择把它的 state 作为 props 向下传递到它的子组件中。

这通常会被叫做“自上而下”或是“单向”的数据流。任何的 state 总是所属于特定的组件，而且从该 state 派生的任何数据或 UI 只能影响树中“低于”它们的组件。

如果你把一个以组件构成的树想象成一个 props 的数据瀑布的话，那么每一个组件的 state 就像是在任意一点上给瀑布增加额外的水源，但是它只能向下流动。

在 React 应用中，组件是有状态组件还是无状态组件属于组件实现的细节，它可能会随着时间的推移而改变。你可以在有状态的组件中使用无状态的组件，反之亦然。

###### 事件处理

React元素的事件处理和DOM元素的很相似，但是有一点语法上的不同：

- React事件的命名采用小驼峰式（camelCase）,而不是纯小写。
- 使用JSX语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。

例如，传统的HTML:

```
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

在React中略微不同：

```
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

在 React 中另一个不同点是你不能通过返回 `false` 的方式阻止默认行为。你必须显式的使用 `preventDefault` 。例如，传统的 HTML 中阻止链接默认打开一个新页面，你可以这样写：

```
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

在React中，可能是这样的：

```
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

在这里，`e` 是一个合成事件。React 根据 [W3C 规范](https://www.w3.org/TR/DOM-Level-3-Events/)来定义这些合成事件，所以你不需要担心跨浏览器的兼容性问题。

使用 React 时，你一般不需要使用 `addEventListener` 为已创建的 DOM 元素添加监听器。事实上，你只需要在该元素初始渲染的时候添加监听器即可。

当你使用 [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 语法定义一个组件的时候，通常的做法是将事件处理函数声明为 class 中的方法。

你必须谨慎对待 JSX 回调函数中的 `this`，在 JavaScript 中，class 的方法默认不会[绑定](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) `this`。如果你忘记绑定 `this.handleClick` 并把它传入了 `onClick`，当你调用这个函数的时候 `this` 的值为 `undefined`。

这并不是 React 特有的行为；这其实与 [JavaScript 函数工作原理](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/)有关。通常情况下，如果你没有在方法后面添加 `()`，例如 `onClick={this.handleClick}`，你应该为这个方法绑定 `this`。

如果觉得使用bind很麻烦，这里有两种方式可以解决。如果你正在使用实验性的public class fields语法，你可以使用class fields正确的绑定回调函数

```
class LogginButton extends React.Component{
	//此语法确保'handleClick'内的this已被绑定
	//注意:这是“实验性”语法
	handleClick= ()=>{
		console.log('this is:',this)
	}
	
	render(){
		return(
			<button onClick={this.handleClick}>
				click name
			</button>	
		)
	}
}
```

如果你没有使用class fields语法，你可以在回调中使用箭头函数：

```
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // 此语法确保 `handleClick` 内的 `this` 已被绑定。
    return (
      <button onClick={() => this.handleClick()}>
        Click me
      </button>
    );
  }
}
```

此语法问题在于每次渲染 `LoggingButton` 时都会创建不同的回调函数。在大多数情况下，这没什么问题，但如果该回调函数作为 prop 传入子组件时，这些组件可能会进行额外的重新渲染。我们通常建议在构造器中绑定或使用 class fields 语法来避免这类性能问题。

###### 向事件处理程序传递参数

在循环中，通常我们会为事件处理函数传递额外的参数。例如，若id是你要删除那一行的ID，以下两种方式都可以向事件处理函数传递参数

```
<button onClick={(e)=>this.deleteRow(id,e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this,id)}>Delete Row</button>
```

上述两种方式是等价的，分别通过箭头函数和Function.prototype.bind来实现。

在这两种情况下，React的事件对象e会被作为第二个参数传递。如果通过箭头函数的方式，事件对象必须显式的进行传递，而通过bind的方式，事件对象以及更多的参数将会被隐式的进行传递。

###### 条件渲染

React中的条件渲染和JavaScript中的一样，使用JavaScript运算符if或者条件运算符去创建元素来表现当前的状态，然后让React根据它们来更新UI。

###### 元素变量

你可以使用变量来储存元素。它可以帮助你有条件地渲染组件的一部分，而其他的渲染部分并不会因此而改变。

###### 与运算符&&

通过花括号包裹代码，你可以[在 JSX 中嵌入任何表达式](https://reactjs.bootcss.com/docs/introducing-jsx.html#embedding-expressions-in-jsx)。这也包括 JavaScript 中的逻辑与 (&&) 运算符。它可以很方便地进行元素的条件渲染。

之所以能这样做，是因为在 JavaScript 中，`true && expression` 总是会返回 `expression`, 而 `false && expression` 总是会返回 `false`。

因此，如果条件是 `true`，`&&` 右侧的元素就会被渲染，如果是 `false`，React 会忽略并跳过它。

###### 三目运算符

另一种内联条件渲染的方法是使用 JavaScript 中的三目运算符 [`condition ? true : false`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)。

###### 阻止组件渲染

在极少数情况下，你可能希望能隐藏组件，即使它已经被其他组件渲染。若要完成此操作，你可以让 `render` 方法直接返回 `null`，而不进行任何渲染。

在组件的 `render` 方法中返回 `null` 并不会影响组件的生命周期。例如，上面这个示例中，`componentDidUpdate` 依然会被调用。

##### 列表 & Key

###### 渲染多个组件

可以通过使用{}在JSX内构建一个元素集合。

###### Key

Key帮助React识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。

一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用数据中的 id 来作为元素的 key。

###### 用 key 提取组件

元素的key只有放在就近的数组上下文中才有意义。

比方说，如果你[提取](https://reactjs.bootcss.com/docs/components-and-props.html#extracting-components)出一个 `ListItem` 组件，你应该把 key 保留在数组中的这个 `<ListItem />` 元素上，而不是放在 `ListItem` 组件中的 `<li>` 元素上。

###### key 只是在兄弟节点之间必须唯一

数组元素中使用的 key 在其兄弟节点之间应该是独一无二的。然而，它们不需要是全局唯一的。

##### 表单

在 React 里，HTML 表单元素的工作方式和其他的 DOM 元素有些不同，这是因为表单元素通常会保持一些内部的 state。

```
<form>
  <label>
    名字:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="提交" />
</form>
```

此表单具有默认的 HTML 表单行为，即在用户提交表单后浏览到新页面。如果你在 React 中执行相同的代码，它依然有效。但大多数情况下，使用 JavaScript 函数可以很方便的处理表单的提交， 同时还可以访问用户填写的表单数据。实现这种效果的标准方式是使用“受控组件”。

###### 受控组件

在 HTML 中，表单元素（如`<input>`、 `<textarea>` 和 `<select>`）之类的表单元素通常自己维护 state，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用 [`setState()`](https://reactjs.bootcss.com/docs/react-component.html#setstate)来更新。

我们可以把两者结合起来，使 React 的 state 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。

对于受控组件来说，输入的值始终由 React 的 state 驱动。你也可以将 value 传递给其他 UI 元素，或者通过其他事件处理函数重置，但这意味着你需要编写更多的代码。

总的来说，这使得 `<input type="text">`, `<textarea>` 和 `<select>` 之类的标签都非常相似—它们都接受一个 `value` 属性，你可以使用它来实现受控组件。

###### 受控输入空值

在[受控组件](https://reactjs.bootcss.com/docs/forms.html#controlled-components)上指定 value 的 prop 会阻止用户更改输入。如果你指定了 `value`，但输入仍可编辑，则可能是你意外地将`value` 设置为 `undefined` 或 `null`。

###### 状态提升

通常，多个组件需要反映相同的变化数据，这时我们建议将共享状态提升到最近的共同父组件中去。

在 React 中，将多个组件中需要共享的 state 向上移动到它们的最近共同父组件中，便可实现共享 state。这就是所谓的“状态提升”。

在 React 应用中，任何可变数据应当只有一个相对应的唯一“数据源”。通常，state 都是首先添加到需要渲染数据的组件中去。然后，如果其他组件也需要这个 state，那么你可以将它提升至这些组件的最近共同父组件中。你应当依靠[自上而下的数据流](https://reactjs.bootcss.com/docs/state-and-lifecycle.html#the-data-flows-down)，而不是尝试在不同组件间同步 state。

##### 组合 VS 继承

React 有十分强大的组合模式。我们推荐使用组合而非继承来实现组件间的代码重用。

组合也同样适用于以 class 形式定义的组件。

###### 那么继承呢？

在Facebook，我们在成千上百个组件中使用react。我们并没有发现需要使用继承来构建组件层次的情况。

Props和组合为你提供了清晰而安全地定制组件外观和行为的灵活方式。注意：组件可以接受任意props，包括基本数据类型，React元素以及函数。

如果你想要在组件间复用非UI的功能，建议将其提取为一个单独的JavaScript模块，如函数、对象或者类。组件可以直接引入（import）而无需通过extend继承它们。

##### React哲学

我们认为，React 是用 JavaScript 构建快速响应的大型 Web 应用程序的首选方式。它在 Facebook 和 Instagram 上表现优秀。

React 最棒的部分之一是引导我们思考如何构建一个应用。

##### 无障碍辅助功能

###### 为什么我们需要无障碍辅助功能？

网络无障碍辅助功能（Accessibility，也被称为 [**a11y**](https://en.wiktionary.org/wiki/a11y)，因为以 A 开头，以 Y 结尾，中间一共 11 个字母）是一种可以帮助所有人获得服务的设计和创造。无障碍辅助功能是使得辅助技术正确解读网页的必要条件。

React 对于创建可访问网站有着全面的支持，而这通常是通过标准 HTML 技术实现的。

##### 代码分隔

###### 打包

大多数 React 应用都会使用 [Webpack](https://webpack.docschina.org/)，[Rollup](https://rollupjs.org/) 或 [Browserify](http://browserify.org/) 这类的构建工具来打包文件。 打包是一个将文件引入并合并到一个单独文件的过程，最终形成一个 “bundle”。 接着在页面上引入该 bundle，整个应用即可一次性加载。

###### 代码分隔

打包是个非常棒的技术，但随着你的应用增长，你的代码包也将随之增长。尤其是在整合了体积巨大的第三方库的情况下。你需要关注你代码包中所包含的代码，以避免因体积过大而导致加载时间过长。

为了避免搞出大体积的代码包，在前期就思考该问题并对代码包进行分割是个不错的选择。 代码分割是由诸如 [Webpack](https://webpack.docschina.org/guides/code-splitting/)，[Rollup](https://rollupjs.org/guide/en/#code-splitting) 和 Browserify（[factor-bundle](https://github.com/browserify/factor-bundle)）这类打包器支持的一项技术，能够创建多个包并在运行时动态加载。

对你的应用进行代码分割能够帮助你“懒加载”当前用户所需要的内容，能够显著地提高你的应用性能。尽管并没有减少应用整体的代码体积，但你可以避免加载用户永远不需要的代码，并在初始加载的时候减少所需加载的代码量。

###### import()

在你的应用中引入代码分隔的最佳方式是通过动态import()语法。

###### React.lazy

React.lazy和Suspense技术还不支持服务端渲染。如果你想要在使用服务端渲染的应用中使用，我们推荐Loadable Components 这个库。它是一个很棒的服务端渲染打包指南。

React.lazy函数能让你像渲染常规组件一样动态引入（的组件）。

React.lazy接受一个函数，这个函数需要动态调用import()。它必须返回一个Promise,该Promise需要resolve一个default export的React组件。

然后应在 `Suspense` 组件中渲染 lazy 组件，如此使得我们可以使用在等待加载 lazy 组件时做优雅降级（如 loading 指示器等）。

###### 异常捕获边界（Error boundaries）

如果模块加载失败（如网络问题），它会触发一个错误。你可以通过异常捕获边界（Error boundaries）技术来处理这些情况，以显示良好的用户体验并管理恢复事宜。

###### 基于路由的代码分割

决定在哪引入代码分割需要一些技巧。你需要确保选择的位置能够均匀地分割代码包而不会影响用户体验。

一个不错的选择是从路由开始。大多数网络用户习惯于页面之间能有个加载切换过程。你也可以选择重新渲染整个页面，这样您的用户就不必在渲染的同时再和页面上的其他元素进行交互。

###### 命名导出（Named Exports）

`React.lazy` 目前只支持默认导出（default exports）。如果你想被引入的模块使用命名导出（named exports），你可以创建一个中间模块，来重新导出为默认模块。这能保证 tree shaking 不会出错，并且不必引入不需要的组件。

###### Context

Context提供了一个无需为每层组件手动添加props，就能在组件树间进行数据传递的方法。

在一个典型的 React 应用中，数据是通过 props 属性自上而下（由父及子）进行传递的，但这种做法对于某些类型的属性而言是极其繁琐的（例如：地区偏好，UI 主题），这些属性是应用程序中许多组件都需要的。Context 提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递 props。

###### 何时使用 Context

Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言。

###### 使用 Context 之前的考虑

Context 主要应用场景在于*很多*不同层级的组件需要访问同样一些的数据。请谨慎使用，因为这会使得组件的复用性变差。

**如果你只是想避免层层传递一些属性，[组件组合（component composition）](https://reactjs.bootcss.com/docs/composition-vs-inheritance.html)有时候是一个比 context 更好的解决方案。**

##### 错误边界

过去，组件内的 JavaScript 错误会导致 React 的内部状态被破坏，并且在下一次渲染时 [产生](https://github.com/facebook/react/issues/4026) [可能无法追踪的](https://github.com/facebook/react/issues/6895) [错误](https://github.com/facebook/react/issues/8579)。这些错误基本上是由较早的其他代码（非 React 组件代码）错误引起的，但 React 并没有提供一种在组件中优雅处理这些错误的方式，也无法从错误中恢复。

###### 错误边界（Error Boundaries）

部分 UI 的 JavaScript 错误不应该导致整个应用崩溃，为了解决这个问题，React 16 引入了一个新的概念 —— 错误边界。

错误边界是一种 React 组件，这种组件**可以捕获并打印发生在其子组件树任何位置的 JavaScript 错误，并且，它会渲染出备用 UI**，而不是渲染那些崩溃了的子组件树。错误边界在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误。

注意

错误边界**无法**捕获以下场景中产生的错误：

- 事件处理（[了解更多](https://reactjs.bootcss.com/docs/error-boundaries.html#how-about-event-handlers)）
- 异步代码（例如 `setTimeout` 或 `requestAnimationFrame` 回调函数）
- 服务端渲染
- 它自身抛出来的错误（并非它的子组件）

###### 错误边界应该放置在哪？

错误边界的粒度由你来决定，可以将其包装在最顶层的路由组件并为用户展示一个 “Something went wrong” 的错误信息，就像服务端框架经常处理崩溃一样。你也可以将单独的部件包装在错误边界以保护应用其他部分不崩溃。

###### 未捕获错误（Uncaught Errors）的新行为

这一改变具有重要意义，**自 React 16 起，任何未被错误边界捕获的错误将会导致整个 React 组件树被卸载。**

###### 关于事件处理器

错误边界**无法**捕获事件处理器内部的错误。

React 不需要错误边界来捕获事件处理器中的错误。与 render 方法和生命周期方法不同，事件处理器不会在渲染期间触发。因此，如果它们抛出异常，React 仍然能够知道需要在屏幕上显示什么。

如果你需要在事件处理器内部捕获错误，使用普通的 JavaScript `try` / `catch` 语句：

##### Refs 转发

Ref 转发是一项将 [ref](https://reactjs.bootcss.com/docs/refs-and-the-dom.html) 自动地通过组件传递到其一子组件的技巧。对于大多数应用中的组件来说，这通常不是必需的。但其对某些组件，尤其是可重用的组件库是很有用的。

###### 转发 refs 到 DOM 组件

**Ref 转发是一个可选特性，其允许某些组件接收 `ref`，并将其向下传递（换句话说，“转发”它）给子组件。**

##### Fragments

React 中的一个常见模式是一个组件返回多个元素。Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点。

```
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  );
}
```

###### 短语法

你可以使用一种新的，且更简短的语法来声明 Fragments。它看起来像空标签：

```
class Columns extends React.Component {
  render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
```

###### 带 key 的 Fragments

使用显式 `<React.Fragment>` 语法声明的片段可能具有 key。一个使用场景是将一个集合映射到一个 Fragments 数组。

```
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // 没有`key`，React 会发出一个关键警告
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}
```

`key` 是唯一可以传递给 `Fragment` 的属性。未来我们可能会添加对其他属性的支持，例如事件。

##### 高阶组件

高阶组件（HOC）是React中用于复用组件逻辑的一种高级技巧。HOC自身不是React API的一部分，它是一种基于React的组合特性而形成的设计模式。

具体而言，高阶组件是参数为组件，返回值为新组件的函数。

```
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

组件是将props转换为UI，而高阶组件是将组件转换为另一个组件。

HOC 在 React 的第三方库中很常见，例如 Redux 的 [`connect`](https://github.com/reduxjs/react-redux/blob/master/docs/api/connect.md#connect) 和 Relay 的 [`createFragmentContainer`](http://facebook.github.io/relay/docs/en/fragment-container.html)。

##### 与第三方库协同

React 可以被用于任何 web 应用中。它可以被嵌入到其他应用，且需要注意，其他的应用也可以被嵌入到 React。

##### 深入 JSX

实际上，JSX 仅仅只是 `React.createElement(component, props, ...children)` 函数的语法糖。

###### 指定 React 元素类型

JSX 标签的第一部分指定了 React 元素的类型。

大写字母开头的 JSX 标签意味着它们是 React 组件。这些标签会被编译为对命名变量的直接引用，所以，当你使用 JSX `<Foo />` 表达式时，`Foo` 必须包含在作用域内。

###### React 必须在作用域内

由于 JSX 会编译为 `React.createElement` 调用形式，所以 `React` 库也必须包含在 JSX 代码作用域内。

###### 在 JSX 类型中使用点语法

在 JSX 中，你也可以使用点语法来引用一个 React 组件。当你在一个模块中导出许多 React 组件时，这会非常方便。例如，如果 `MyComponents.DatePicker` 是一个组件，你可以在 JSX 中直接使用：

```
import React from react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}

```

###### 用户定义的组件必须以大写字母开头

以小写字母开头的元素代表一个 HTML 内置组件，比如 `<div>` 或者 `<span>` 会生成相应的字符串 `'div'` 或者 `'span'` 传递给 `React.createElement`（作为参数）。大写字母开头的元素则对应着在 JavaScript 引入或自定义的组件，如 `<Foo />` 会编译为 `React.createElement(Foo)`。

我们建议使用大写字母开头命名自定义组件。如果你确实需要一个以小写字母开头的组件，则在 JSX 中使用它之前，必须将它赋值给一个大写字母开头的变量。

###### JSX 中的 Props

有多种方式可以在 JSX 中指定 props。

###### JavaScript 表达式作为 Props

你可以把包裹在 `{}` 中的 JavaScript 表达式作为一个 prop 传递给 JSX 元素。

`if` 语句以及 `for` 循环不是 JavaScript 表达式，所以不能在 JSX 中直接使用。但是，你可以用在 JSX 以外的代码中。

###### 字符串字面量

你可以将字符串字面量赋值给 prop. 如下两个 JSX 表达式是等价的：

```
<MyComponent message="hello world" />

<MyComponent message={'hello world'} />
```

###### Props 默认值为 “True”

如果你没给 prop 赋值，它的默认值是 `true`。以下两个 JSX 表达式是等价的：

```
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```

通常，我们不建议不传递 value 给 prop，因为这可能与 [ES6 对象简写](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#New_notations_in_ECMAScript_2015)混淆，`{foo}` 是 `{foo: foo}` 的简写，而不是 `{foo: true}`。这样实现只是为了保持和 HTML 中标签属性的行为一致。

###### 属性展开

如果你已经有了一个 props 对象，你可以使用展开运算符 `...` 来在 JSX 中传递整个 props 对象。以下两个组件是等价的：

```
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```

###### JSX 中的子元素

包含在开始和结束标签之间的 JSX 表达式内容将作为特定属性 `props.children` 传递给外层组件。有几种不同的方法来传递子元素：

###### 字符串字面量

你可以将字符串放在开始和结束标签之间，此时 `props.children` 就只是该字符串。这对于很多内置的 HTML 元素很有用。例如：

```
<MyComponent>Hello world!</MyComponent>
```

这是一个合法的 JSX，`MyComponent` 中的 `props.children` 是一个简单的未转义字符串 `"Hello world!"`。因此你可以采用编写 HTML 的方式来编写 JSX。如下所示：

```
<div>This is valid HTML &amp; JSX at the same time.</div>
```

JSX 会移除行首尾的空格以及空行。与标签相邻的空行均会被删除，文本字符串之间的新行会被压缩为一个空格。

###### JSX 子元素

子元素允许由多个 JSX 元素组成。

###### JavaScript 表达式作为子元素

JavaScript 表达式可以被包裹在 `{}` 中作为子元素。

###### 函数作为子元素

通常，JSX 中的 JavaScript 表达式将会被计算为字符串、React 元素或者是列表。不过，`props.children` 和其他 prop 一样，它可以传递任意类型的数据，而不仅仅是 React 已知的可渲染类型。

###### 布尔类型、Null 以及 Undefined 将会忽略

`false`, `null`, `undefined`, and `true` 是合法的子元素。但它们并不会被渲染。

##### 性能优化

UI 更新需要昂贵的 DOM 操作，而 React 内部使用几种巧妙的技术以便最小化 DOM 操作次数。对于大部分应用而言，使用 React 时无需专门优化就已拥有高性能的用户界面。尽管如此，你仍然有办法来加速你的 React 应用。

###### 使用生产版本

如果你不能确定你的编译过程是否设置正确，你可以通过安装 [Chrome 的 React 开发者工具](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi) 来检查。如果你浏览一个基于 React 生产版本的网站，图标背景会变成深色。如果你浏览一个基于 React 开发模式的网站，图标背景会变成红色

###### 单文件构建

###### Brunch

###### Browserify

###### Rollup

###### webpack

###### 使用 Chrome Performance 标签分析组件

###### 使用开发者工具中的分析器对组件进行分析

###### 虚拟化长列表

###### 避免调停

###### shouldComponentUpdate 的作用

###### 不可变数据的力量

##### Portals

Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。

```
ReactDOM.createPortal(child, container)
```

第一个参数（`child`）是任何[可渲染的 React 子元素](https://reactjs.bootcss.com/docs/react-component.html#render)，例如一个元素，字符串或 fragment。第二个参数（`container`）是一个 DOM 元素。

##### Profiler API

`Profiler` 测量渲染一个 React 应用多久渲染一次以及渲染一次的“代价”。 它的目的是识别出应用中渲染较慢的部分，或是可以使用[类似 memoization 优化](https://reactjs.bootcss.com/docs/hooks-faq.html#how-to-memoize-calculations)的部分，并从相关优化中获益。

###### 用法

`Profiler` 能添加在 React 树中的任何地方来测量树中这部分渲染所带来的开销。 它需要两个 prop ：一个是 `id`(string)，一个是当组件树中的组件“提交”更新的时候被React调用的回调函数 `onRender`(function)。

例如，为了分析 `Navigation` 组件和它的子代：

```
render(
  <App>
    <Profiler id="Navigation" onRender={callback}>      <Navigation {...props} />
    </Profiler>
    <Main {...props} />
  </App>
);
```

##### 不使用ES6

通常我们会用 JavaScript 的 `class` 关键字来定义 React 组件。如果你还未使用过 ES6，你可以使用 `create-react-class` 模块。

##### 不使用JSX的React

React 并不强制要求使用 JSX。当你不想在构建环境中配置有关 JSX 编译时，不在 React 中使用 JSX 会更加方便。

每个 JSX 元素只是调用 `React.createElement(component, props, ...children)` 的语法糖。因此，使用 JSX 可以完成的任何事情都可以通过纯 JavaScript 完成。

#####  协调

##### Diffing 算法

当对比两颗树时，React 首先比较两棵树的根节点。不同类型的根节点元素会有不同的形态。

###### 对比不同类型的元素

当根节点为不同类型的元素时，React 会拆卸原有的树并且建立起新的树。

###### 对比同一类型的元素

当对比两个相同类型的 React 元素时，React 会保留 DOM 节点，仅比对及更新有改变的属性。

###### 对比同类型的组件元素

当一个组件更新时，组件实例保持不变，这样 state 在跨越不同的渲染时保持一致。React 将更新该组件实例的 props 以跟最新的元素保持一致，并且调用该实例的 `componentWillReceiveProps()` 和 `componentWillUpdate()` 方法。

下一步，调用 `render()` 方法，diff 算法将在之前的结果以及新的结果中进行递归。

###### 对子节点进行递归

在默认条件下，当递归 DOM 节点的子元素时，React 会同时遍历两个子元素的列表；当产生差异时，生成一个 mutation。

在子元素列表末尾新增元素时，更新开销比较小。

##### Keys

为了解决以上问题，React 支持 `key` 属性。当子元素拥有 key 时，React 使用 key 来匹配原有树上的子元素以及最新树上的子元素。

###### 权衡

请谨记协调算法是一个实现细节。React 可以在每个 action 之后对整个应用进行重新渲染，得到的最终结果也会是一样的。在此情境下，重新渲染表示在所有组件内调用 `render` 方法，这不代表 React 会卸载或装载它们。React 只会基于以上提到的规则来决定如何进行差异的合并。

我们定期探索优化算法，让常见用例更高效地执行。在当前的实现中，可以理解为一棵子树能在其兄弟之间移动，但不能移动到其他位置。在这种情况下，算法会重新渲染整棵子树。

由于 React 依赖探索的算法，因此当以下假设没有得到满足，性能会有所损耗。

1. 该算法不会尝试匹配不同组件类型的子树。如果你发现你在两种不同类型的组件中切换，但输出非常相似的内容，建议把它们改成同一类型。在实践中，我们没有遇到这类问题。
2. Key 应该具有稳定，可预测，以及列表内唯一的特质。不稳定的 key（比如通过 `Math.random()` 生成的）会导致许多组件实例和 DOM 节点被不必要地重新创建，这可能导致性能下降和子组件中的状态丢失。

##### Refs and the DOM

Refs 提供了一种方式，允许我们访问 DOM 节点或在 render 方法中创建的 React 元素。

在典型的 React 数据流中，[props](https://reactjs.bootcss.com/docs/components-and-props.html) 是父组件与子组件交互的唯一方式。要修改一个子组件，你需要使用新的 props 来重新渲染它。但是，在某些情况下，你需要在典型数据流之外强制修改子组件。被修改的子组件可能是一个 React 组件的实例，也可能是一个 DOM 元素。对于这两种情况，React 都提供了解决办法。

###### 何时使用 Refs

下面是几个适合使用 refs 的情况：

- 管理焦点，文本选择或媒体播放。
- 触发强制动画。
- 集成第三方 DOM 库。

##### Render Props

术语 [“render prop”](https://cdb.reacttraining.com/use-a-render-prop-50de598f11ce) 是指一种在 React 组件之间使用一个值为函数的 prop 共享代码的简单技术

具有 render prop 的组件接受一个函数，该函数返回一个 React 元素并调用它而不是实现自己的渲染逻辑。

```
<DataProvider render={data => (
  <h1>Hello {data.target}</h1>
)}/>
```

使用 render prop 的库有 [React Router](https://reacttraining.com/react-router/web/api/Route/render-func)、[Downshift](https://github.com/paypal/downshift) 以及 [Formik](https://github.com/jaredpalmer/formik)。

##### 静态类型检查

像 [Flow](https://flow.org/) 和 [TypeScript](https://www.typescriptlang.org/) 等这些静态类型检查器，可以在运行前识别某些类型的问题。他们还可以通过增加自动补全等功能来改善开发者的工作流程。出于这个原因，我们建议在大型代码库中使用 Flow 或 TypeScript 来代替 `PropTypes`。

###### TypeScript

[TypeScript](https://www.typescriptlang.org/) 是一种由微软开发的编程语言。它是 JavaScript 的一个类型超集，包含独立的编译器。作为一种类型语言，TypeScript 可以在构建时发现 bug 和错误，这样程序运行时就可以避免此类错误。

###### 文件扩展名

在 React 中，你的组件文件大多数使用 `.js` 作为扩展名。在 TypeScript 中，提供两种文件扩展名：

`.ts` 是默认的文件扩展名，而 `.tsx` 是一个用于包含 `JSX` 代码的特殊扩展名。

##### 严格模式

`StrictMode` 是一个用来突出显示应用程序中潜在问题的工具。与 `Fragment` 一样，`StrictMode` 不会渲染任何可见的 UI。它为其后代元素触发额外的检查和警告。

注意：

严格模式检查仅在开发模式下运行；*它们不会影响生产构建*。

```
import React from 'react';

function ExampleApplication() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <ComponentOne />
          <ComponentTwo />
        </div>
      </React.StrictMode>
      <Footer />
    </div>
  );
}
```

`StrictMode` 目前有助于：

- [识别不安全的生命周期](https://reactjs.bootcss.com/docs/strict-mode.html#identifying-unsafe-lifecycles)
- [关于使用过时字符串 ref API 的警告](https://reactjs.bootcss.com/docs/strict-mode.html#warning-about-legacy-string-ref-api-usage)
- [关于使用废弃的 findDOMNode 方法的警告](https://reactjs.bootcss.com/docs/strict-mode.html#warning-about-deprecated-finddomnode-usage)
- [检测意外的副作用](https://reactjs.bootcss.com/docs/strict-mode.html#detecting-unexpected-side-effects)
- [检测过时的 context API](https://reactjs.bootcss.com/docs/strict-mode.html#detecting-legacy-context-api)

##### 使用 PropTypes 进行类型检查

注意：

自 React v15.5 起，`React.PropTypes` 已移入另一个包中。请使用 [`prop-types` 库](https://www.npmjs.com/package/prop-types) 代替。

随着你的应用程序不断增长，你可以通过类型检查捕获大量错误。对于某些应用程序来说，你可以使用 [Flow](https://flow.org/) 或 [TypeScript](https://www.typescriptlang.org/) 等 JavaScript 扩展来对整个应用程序做类型检查。但即使你不使用这些扩展，React 也内置了一些类型检查的功能。要在组件的 props 上进行类型检查，你只需配置特定的 `propTypes` 属性。

##### 非受控组件

在大多数情况下，我们推荐使用 [受控组件](https://reactjs.bootcss.com/docs/forms.html#controlled-components) 来处理表单数据。在一个受控组件中，表单数据是由 React 组件来管理的。另一种替代方案是使用非受控组件，这时表单数据将交由 DOM 节点来处理。

要编写一个非受控组件，而不是为每个状态更新都编写数据处理函数，你可以 [使用 ref](https://reactjs.bootcss.com/docs/refs-and-the-dom.html) 来从 DOM 节点中获取表单数据。

##### Web Components

React 和 [Web Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components) 为了解决不同的问题而生。Web Components 为可复用组件提供了强大的封装，而 React 则提供了声明式的解决方案，使 DOM 与数据保持同步。两者旨在互补。作为开发人员，可以自由选择在 Web Components 中使用 React，或者在 React 中使用 Web Components，或者两者共存。

##### React顶层API

`React` 是 React 库的入口。如果你通过使用 `<script>` 标签的方式来加载 React，则可以通过 `React` 全局变量对象来获得 React 的顶层 API。当你使用 ES6 与 npm 时，可以通过编写 `import React from 'react'` 来引入它们。当你使用 ES5 与 npm 时，则可以通过编写 `var React = require('react')` 来引入它们。

###### 组件

使用 React 组件可以将 UI 拆分为独立且复用的代码片段，每部分都可独立维护。你可以通过子类 `React.Component` 或 `React.PureComponent` 来定义 React 组件。

`React.PureComponent` 与 [`React.Component`](https://reactjs.bootcss.com/docs/react-api.html#reactcomponent) 很相似。两者的区别在于 [`React.Component`](https://reactjs.bootcss.com/docs/react-api.html#reactcomponent) 并未实现 [`shouldComponentUpdate()`](https://reactjs.bootcss.com/docs/react-component.html#shouldcomponentupdate)，而 `React.PureComponent` 中以浅层对比 prop 和 state 的方式来实现了该函数。

如果赋予 React 组件相同的 props 和 state，`render()` 函数会渲染相同的内容，那么在某些情况下使用 `React.PureComponent` 可提高性能。

注意

`React.PureComponent` 中的 `shouldComponentUpdate()` 仅作对象的浅层比较。如果对象中包含复杂的数据结构，则有可能因为无法检查深层的差别，产生错误的比对结果。仅在你的 props 和 state 较为简单时，才使用 `React.PureComponent`，或者在深层数据结构发生变化时调用 [`forceUpdate()`](https://reactjs.bootcss.com/docs/react-component.html#forceupdate) 来确保组件被正确地更新。你也可以考虑使用 [immutable 对象](https://facebook.github.io/immutable-js/)加速嵌套数据的比较。

此外，`React.PureComponent` 中的 `shouldComponentUpdate()` 将跳过所有子组件树的 prop 更新。因此，请确保所有子组件也都是“纯”的组件。

```
const MyComponent = React.memo(function MyComponent(props) {
  /* 使用 props 渲染 */
});
```

`React.memo` 为[高阶组件](https://reactjs.bootcss.com/docs/higher-order-components.html)。它与 [`React.PureComponent`](https://reactjs.bootcss.com/docs/react-api.html#reactpurecomponent) 非常相似，但只适用于函数组件，而不适用 class 组件。

如果你的函数组件在给定相同 props 的情况下渲染相同的结果，那么你可以通过将其包装在 `React.memo` 中调用，以此通过记忆组件渲染结果的方式来提高组件的性能表现。这意味着在这种情况下，React 将跳过渲染组件的操作并直接复用最近一次渲染的结果。

`React.memo` 仅检查 props 变更。如果函数组件被 `React.memo` 包裹，且其实现中拥有 [`useState`](https://reactjs.bootcss.com/docs/hooks-state.html) 或 [`useContext`](https://reactjs.bootcss.com/docs/hooks-reference.html#usecontext) 的 Hook，当 context 发生变化时，它仍会重新渲染。

###### 创建React元素

我们建议[使用 JSX](https://reactjs.bootcss.com/docs/introducing-jsx.html) 来编写你的 UI 组件。每个 JSX 元素都是调用 [`React.createElement()`](https://reactjs.bootcss.com/docs/react-api.html#createelement) 的语法糖。一般来说，如果你使用了 JSX，就不再需要调用以下方法。

- [`createElement()`](https://reactjs.bootcss.com/docs/react-api.html#createelement)  创建并返回指定类型的新 [React 元素](https://reactjs.bootcss.com/docs/rendering-elements.html)。
- [`createFactory()`](https://reactjs.bootcss.com/docs/react-api.html#createfactory) 返回用于生成指定类型 React 元素的函数。

###### 转换元素

`React` 提供了几个用于操作元素的 API：

- [`cloneElement()`](https://reactjs.bootcss.com/docs/react-api.html#cloneelement) 以 `element` 元素为样板克隆并返回新的 React 元素。返回元素的 props 是将新的 props 与原始元素的 props 浅层合并后的结果。新的子元素将取代现有的子元素，而来自原始元素的 `key` 和 `ref` 将被保留

```
<element.type {...element.props} {...props}>{children}</element.type>
```

- [`isValidElement()`](https://reactjs.bootcss.com/docs/react-api.html#isvalidelement) 验证对象是否为 React 元素，返回值为 `true` 或 `false`。
- [`React.Children`](https://reactjs.bootcss.com/docs/react-api.html#reactchildren) `React.Children` 提供了用于处理 `this.props.children` 不透明数据结构的实用方法。

###### Fragments

React还提供了用于减少不必要嵌套的组件。

React.Fragment 组件能够在不额外创建 DOM 元素的情况下，让 `render()` 方法中返回多个元素。

###### Refs

- [`React.createRef`](https://reactjs.bootcss.com/docs/react-api.html#reactcreateref)  创建一个能够通过 ref 属性附加到 React 元素的 [ref](https://reactjs.bootcss.com/docs/refs-and-the-dom.html)。
- [`React.forwardRef`](https://reactjs.bootcss.com/docs/react-api.html#reactforwardref) `React.forwardRef` 会创建一个React组件，这个组件能够将其接受的 [ref](https://reactjs.bootcss.com/docs/refs-and-the-dom.html) 属性转发到其组件树下的另一个组件中。

###### Suspense

Suspense 使得组件可以“等待”某些操作结束后，再进行渲染。目前，Suspense 仅支持的使用场景是：[通过 `React.lazy` 动态加载组件](https://reactjs.bootcss.com/docs/code-splitting.html#reactlazy)。它将在未来支持其它使用场景，如数据获取等。

- [`React.lazy`](https://reactjs.bootcss.com/docs/react-api.html#reactlazy) `React.lazy()` 允许你定义一个动态加载的组件。这有助于缩减 bundle 的体积，并延迟加载在初次渲染时未用到的组件。
- [`React.Suspense`](https://reactjs.bootcss.com/docs/react-api.html#reactsuspense)  可以指定加载指示器（loading indicator），以防其组件树中的某些子组件尚未具备渲染条件。目前，懒加载组件是 `<React.Suspense>` 支持的**唯一**用例

###### Hook

*Hook* 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。Hook 拥有[专属文档章节](https://reactjs.bootcss.com/docs/hooks-intro.html)和单独的 API 参考文档：

- [基础 Hook](https://reactjs.bootcss.com/docs/hooks-reference.html#basic-hooks)

  - [`useState`](https://reactjs.bootcss.com/docs/hooks-reference.html#usestate)
  - [`useEffect`](https://reactjs.bootcss.com/docs/hooks-reference.html#useeffect)
  - [`useContext`](https://reactjs.bootcss.com/docs/hooks-reference.html#usecontext)

- [额外的 Hook](https://reactjs.bootcss.com/docs/hooks-reference.html#additional-hooks)

  - [`useReducer`](https://reactjs.bootcss.com/docs/hooks-reference.html#usereducer)
  - [`useCallback`](https://reactjs.bootcss.com/docs/hooks-reference.html#usecallback)
  - [`useMemo`](https://reactjs.bootcss.com/docs/hooks-reference.html#usememo)
  - [`useRef`](https://reactjs.bootcss.com/docs/hooks-reference.html#useref)
  - [`useImperativeHandle`](https://reactjs.bootcss.com/docs/hooks-reference.html#useimperativehandle)
  - [`useLayoutEffect`](https://reactjs.bootcss.com/docs/hooks-reference.html#uselayouteffect)
  - [`useDebugValue`](https://reactjs.bootcss.com/docs/hooks-reference.html#usedebugvalue)

  ##### React.Component

  ###### 组件的生命周期

  ###### 挂载  当组件实例被创建并插入DOM中时，其生命周期调用顺序如下：

  1. constructor()
  2. static getDerivedStateFromProps()
  3. render()
  4. componentDidMount()

  注意：下述生命周期方法即将过时，在新代码中应该避免使用它们：

  UNSAFE_componentWillMount()

  ###### 更新 当组件的props或state发生变化时会触发更新。组件更新的生命周期调用顺序如下：

  1. static getDerivedStateFromProps()
  2. shouldComponentUpdate()
  3. render()
  4. getSnapshotBeforeUpdate()
  5. componentDidUpdate()

  注意 下述方法即将过时，在新代码中应该避免使用它们：

  UNSAFE_componentWillUpdate()

  UNSAFE_componentWillReceiveProps()

###### 卸载 当组件从DOM中移除时会调用如下方法：

componentWillUnmount()

###### 错误处理 当渲染过程，生命周期，或子组件的构造函数中抛出错误时，会调用如下方法：

static getDerivedStateFromError()

componentDidCatch()

###### 其他APIs

组件还提供了一些额外的API:

setState()

forceUpdate()

###### class属性

defaultProps

displayName

###### 实例属性

props

state

##### React 术语词汇表

###### 单页面应用

单页面应用(single-page application)，是一个应用程序，它可以加载单个 HTML 页面，以及运行应用程序所需的所有必要资源（例如 JavaScript 和 CSS）。与页面或后续页面的任何交互，都不再需要往返 server 加载资源，即页面不会重新加载。

###### ES6, ES2015, ES2016 等

这些首字母缩写都是指 ECMAScript 语言规范标准的最新版本，JavaScript 语言是此标准的一个实现。其中 ES6 版本（也称为 ES2015）包括对前面版本的许多补充，例如：箭头函数、class、模板字面量、`let` 和 `const` 语句。

###### Compiler（编译器）

JavaScript compiler 接收 JavaScript 代码，然后对其进行转换，最终返回不同格式的 JavaScript 代码。最为常见的使用示例是，接收 ES6 语法，然后将其转换为旧版本浏览器能够解释执行的语法。[Babel](https://babeljs.io/) 是 React 最常用的 compiler。

###### Bundler（打包工具）

bundler 会接收写成单独模块（通常有数百个）的 JavaScript 和 CSS 代码，然后将它们组合在一起，最终生成出一些为浏览器优化的文件。常用的打包 React 应用的工具有 [webpack](https://webpack.js.org/) 和 [Browserify](http://browserify.org/)。

###### Package 管理工具

package 管理工具，是帮助你管理项目依赖的工具。[npm](https://www.npmjs.com/) 和 [Yarn](https://yarnpkg.com/) 是两个常用的管理 React 应用依赖的 package 管理工具。它们都是使用了相同 npm package registry 的客户端。

###### CDN

CDN 代表内容分发网络（Content Delivery Network）。CDN 会通过一个遍布全球的服务器网络来分发缓存的静态内容。

###### JSX

JSX 是一个 JavaScript 语法扩展。它类似于模板语言，但它具有 JavaScript 的全部能力。JSX 最终会被编译为 `React.createElement()` 函数调用，返回称为 “React 元素” 的普通 JavaScript 对象。

React DOM 使用 camelCase（驼峰式命名）来定义属性的名称，而不使用 HTML 属性名称的命名约定。例如，HTML 的 `tabindex` 属性变成了 JSX 的 `tabIndex`。而 `class` 属性则变为 `className`，这是因为 `class` 是 JavaScript 中的保留字

###### 元素

React 元素是构成 React 应用的基础砖块。人们可能会把元素与广为人知的“组件”概念相互混淆。元素描述了你在屏幕上想看到的内容。React 元素是不可变对象。

```
const element = <h1>Hello, world</h1>;
```

通常我们不会直接使用元素，而是从组件中返回元素。

###### 组件

React 组件是可复用的小的代码片段，它们返回要在页面中渲染的 React 元素。React 组件的最简版本是，一个返回 React 元素的普通 JavaScript 函数

###### props

`props` 是 React 组件的输入。它们是从父组件向下传递给子组件的数据。

记住，`props` 是只读的。不应以任何方式修改它们

###### `props.children`

每个组件都可以获取到 `props.children`。它包含组件的开始标签和结束标签之间的内容。

###### state

当组件中的一些数据在某些时刻发生变化时，这时就需要使用 `state` 来跟踪状态。例如，`Checkbox` 组件可能需要 `isChecked` 状态，而 `NewsFeed` 组件可能需要跟踪 `fetchedPosts` 状态。

`state` 和 `props` 之间最重要的区别是：`props` 由父组件传入，而 `state` 由组件本身管理。组件不能修改 `props`，但它可以修改 `state`。

对于所有变化数据中的每个特定部分，只应该由一个组件在其 state 中“持有”它。不要试图同步来自于两个不同组件的 state。相反，应当将其[提升](https://reactjs.bootcss.com/docs/lifting-state-up.html)到最近的共同祖先组件中，并将这个 state 作为 props 传递到两个子组件。

###### 周期方法

生命周期方法，用于在组件不同阶段执行自定义功能。在组件被创建并插入到 DOM 时（即[挂载中阶段（mounting）](https://reactjs.bootcss.com/docs/react-component.html#mounting)），组件更新时，组件取消挂载或从 DOM 中删除时，都有可以使用的生命周期方法。

###### 受控组件vs 非受控组件

React 有两种不同的方式来处理表单输入。

如果一个 input 表单元素的值是由 React 控制，就其称为*受控组件*。当用户将数据输入到受控组件时，会触发修改状态的事件处理器，这时由你的代码来决定此输入是否有效（如果有效就使用更新后的值重新渲染）。如果不重新渲染，则表单元素将保持不变。

一个*非受控组件*，就像是运行在 React 体系之外的表单元素。当用户将数据输入到表单字段（例如 input，dropdown 等）时，React 不需要做任何事情就可以映射更新后的信息。然而，这也意味着，你无法强制给这个表单字段设置一个特定值。

在大多数情况下，你应该使用受控组件。

###### key

“key” 是在创建元素数组时，需要用到的一个特殊字符串属性。key 帮助 React 识别出被修改、添加或删除的 item。应当给数组内的每个元素都设定 key，以使元素具有固定身份标识。

只需要保证，在同一个数组中的兄弟元素之间的 key 是唯一的。而不需要在整个应用程序甚至单个组件中保持唯一。

不要将 `Math.random()` 之类的值传递给 key。重要的是，在前后两次渲染之间的 key 要具有“固定身份标识”的特点，以便 React 可以在添加、删除或重新排序 item 时，前后对应起来。理想情况下，key 应该从数据中获取，对应着唯一且固定的标识符，例如 `post.id`。

###### Ref

React 支持一个特殊的、可以附加到任何组件上的 `ref` 属性。此属性可以是一个由 [`React.createRef()` 函数](https://reactjs.bootcss.com/docs/react-api.html#reactcreateref)创建的对象、或者一个回调函数、或者一个字符串（遗留 API）。当 `ref` 属性是一个回调函数时，此函数会（根据元素的类型）接收底层 DOM 元素或 class 实例作为其参数。这能够让你直接访问 DOM 元素或组件实例。

谨慎使用 ref。如果你发现自己经常使用 ref 来在应用中“实现想要的功能”，你可以考虑去了解一下[自上而下的数据流](https://reactjs.bootcss.com/docs/lifting-state-up.html)。

###### 事件

使用 React 元素处理事件时，有一些语法上差异：

- React 事件处理器使用 camelCase（驼峰式命名）而不使用小写命名。
- 通过 JSX，你可以直接传入一个函数，而不是传入一个字符串，来作为事件处理器。

###### 协调

当组件的 props 或 state 发生变化时，React 通过将最新返回的元素与原先渲染的元素进行比较，来决定是否有必要进行一次实际的 DOM 更新。当它们不相等时，React 才会更新 DOM。这个过程被称为“协调”。

###### 什么是 Virtual DOM？

Virtual DOM 是一种编程概念。在这个概念里， UI 以一种理想化的，或者说“虚拟的”表现形式被保存于内存中，并通过如 ReactDOM 等类库使之与“真实的” DOM 同步。这一过程叫做[协调](https://reactjs.bootcss.com/docs/reconciliation.html)。

这种方式赋予了 React 声明式的 API：您告诉 React 希望让 UI 是什么状态，React 就确保 DOM 匹配该状态。这使您可以从属性操作、事件处理和手动 DOM 更新这些在构建应用程序时必要的操作中解放出来。

与其将 “Virtual DOM” 视为一种技术，不如说它是一种模式，人们提到它时经常是要表达不同的东西。在 React 的世界里，术语 “Virtual DOM” 通常与 [React 元素](https://reactjs.bootcss.com/docs/rendering-elements.html)关联在一起，因为它们都是代表了用户界面的对象。而 React 也使用一个名为 “fibers” 的内部对象来存放组件树的附加信息。上述二者也被认为是 React 中 “Virtual DOM” 实现的一部分。

###### Shadow DOM 和 Virtual DOM 是一回事吗？

不，他们不一样。Shadow DOM 是一种浏览器技术，主要用于在 web 组件中封装变量和 CSS。Virtual DOM 则是一种由 Javascript 类库基于浏览器 API 实现的概念。

###### 什么是 “React Fiber”？

Fiber 是 React 16 中新的协调引擎。它的主要目的是使 Virtual DOM 可以进行增量式渲染。

###### `setState` 实际做了什么？

`setState()` 会对一个组件的 `state` 对象安排一次更新。当 state 改变了，该组件就会重新渲染。

###### `state` 和 `props` 之间的区别是什么？

[`props`](https://reactjs.bootcss.com/docs/components-and-props.html)（“properties” 的缩写）和 [`state`](https://reactjs.bootcss.com/docs/state-and-lifecycle.html) 都是普通的 JavaScript 对象。它们都是用来保存信息的，这些信息可以控制组件的渲染输出，而它们的一个重要的不同点就是：`props` 是传递*给*组件的（类似于函数的形参），而 `state` 是在组件*内*被组件自己管理的（类似于在一个函数内声明的变量）。

###### 为什么 `setState` 给了我一个错误的值？

在 React 中，`this.props` 和 `this.state` 都代表着*已经被渲染了的*值，即当前屏幕上显示的值。

调用 `setState` 其实是异步的 —— 不要指望在调用 `setState` 之后，`this.state` 会立即映射为新的值。如果你需要基于当前的 state 来计算出新的值，那你应该传递一个函数，而不是一个对象。

###### 我应该如何更新那些依赖于当前的 state 的 state 呢？

给 `setState` 传递一个函数，而不是一个对象，就可以确保每次的调用都是使用最新版的 state（见下面的说明）。

###### 给 `setState` 传递一个对象与传递一个函数的区别是什么？

传递一个函数可以让你在函数内访问到当前的 state 的值。因为 `setState` 的调用是分批的，所以你可以链式地进行更新，并确保它们是一个建立在另一个之上的，这样才不会发生冲突。

###### `setState` 什么时候是异步的？

目前，在事件处理函数内部的 `setState` 是异步的。

例如，如果 `Parent` 和 `Child` 在同一个 click 事件中都调用了 `setState` ，这样就可以确保 `Child` 不会被重新渲染两次。取而代之的是，React 会将该 state “冲洗” 到浏览器事件结束的时候，再统一地进行更新。这种机制可以在大型应用中得到很好的性能提升。

这只是一个实现的细节，所以请不要直接依赖于这种机制。在以后的版本当中，React 会在更多的情况下静默地使用 state 的批更新机制。

###### 为什么 React 不同步地更新 `this.state`？

如前面章节解释的那样，在开始重新渲染之前，React 会有意地进行“等待”，直到所有在组件的事件处理函数内调用的 `setState()` 完成之后。这样可以通过避免不必要的重新渲染来提升性能。

但是，你可能还是会想，为什么 React 不能立即更新 `this.state`，而不对组件进行重新渲染呢。

主要有两个原因：

- 这样会破坏掉 `props` 和 `state` 之间的一致性，造成一些难以 debug 的问题。
- 这样会让一些我们正在实现的新功能变得无法实现。

###### 怎样阻止函数被调用太快或者太多次？

如果你有一个 `onClick` 或者 `onScroll` 这样的事件处理器，想要阻止回调被触发的太快，那么可以限制执行回调的速度，可以通过以下几种方式做到这点：

- **节流**：基于时间的频率来进行抽样更改 (例如 [`_.throttle`](https://lodash.com/docs#throttle))
- **防抖**：一段时间的不活动之后发布更改 (例如 [`_.debounce`](https://lodash.com/docs#debounce))
- **`requestAnimationFrame` 节流**：基于 requestAnimationFrame 的抽样更改 (例如 [raf-schd](https://reactjs.bootcss.com/docs/[`raf-schd`](https://github.com/alexreardon/raf-schd)))

###### 节流

节流阻止函数在给定时间窗口内被调不能超过一次。

###### 防抖

防抖确保函数不会在上一次被调用之后一定量的时间内被执行。当必须进行一些费时的计算来响应快速派发的事件时（比如鼠标滚动或键盘事件时），防抖是非常有用的。

###### `requestAnimationFrame` 节流

[`requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) 是在浏览器中排队等待执行的一种方法，它可以在呈现性能的最佳时间执行。

###### 6.

##### 从头开始打造工具链

一组JavaScript构建工具链通常由这些组成：

- 一个package管理器，比如Yarn或npm。它能让你充分利用庞大的第三方package的生态系统，并且轻松地安装或更新它们。
- 一个打包器，比如webpack或Parcel。它能让你编写模块化代码，并将它们组合在一起成为小的package，以优化加载时间。
- 一个编译器，例如Babel。它能让你编写的新版本JavaScript代码，在旧版浏览器中依然能够工作。

##### JSX简介

JSX，是一个JavaScript的语法扩展。建议在React中配合使用JSX，JSX可以很好地描述UI应该呈现出它应有交互的本质形式。JSX可能会使人联想到模板语言，但它具有JavaScript的全部功能。

###### 为什么使用JSX？

React认为渲染逻辑本质上与其他逻辑内在耦合，比如，在UI中需要绑定事件处理、在某些时刻状态发生变化时需要通知到UI，以及需要在UI中展示准备好的数据。

React并没有采用将标记与逻辑进行分离到不同文件这种人为地分离方式，而是通过将二者共同存放在称之为“组件”的松散耦合单元之中，来实现关注点分离。

React不强制要求使用JSX，但是大多数人发现，在JavaScript代码中将JSX和UI放在一起时，会在视觉上有辅助作用。它还可以使React显示更多有用的错误和警告消息。

在JSX语法中，你可以在大括号内放置任何有效的JavaScript表达式。

###### JSX防止注入攻击 

你可以安全地在JSX当中插入用户输入内容。React DOM在渲染所有输入内容分之前，默认会进行转义。它可以确保在你的应用中，永远不会注入那些并非自己明确编写的内容。所有的内容在渲染之前都被转换成了字符串。这样可以有效地防止XSS(cross-site-scripting，跨站脚本)攻击。

###### JSX表示对象 

Babel会把JSX转译成一个名为React.createElement()函数调用。

##### 元素渲染

元素是构成React应用的最小砖块。元素描述了你在屏幕上想看到的内容。

与浏览器的DOM元素不同，React元素是创建开销极小的普通对象。React DOM会负责更新DOM来与React元素保持一致。

###### 注意：组件是由元素构成的。

###### 更新已渲染的元素

React元素是不可变对象。一旦被创建，你就无法更改它的子元素或者属性。

根据我们已有的知识，更新UI唯一的方式是创建一个全新的元素，并将其传入ReactDOM.render()。

###### React只更新它需要更新的部分

React DOM会将元素和它的子元素与它们之前的状态进行比较，并只会进行必要的更新来使DOM达到预期的状态。

##### 组件&Props

组件允许你将UI拆分为独立可复用的代码片段，并对每个片段进行独立思考。

组件，从概念上类似于JavaScript函数。它接受任意的入参（即“props”），并返回用于描述页面展示内容的React元素。

###### 函数组件与class组件

定义组件最简单的方式就是编写JavaScript函数。

```
function Welcome(props){
	return <h1>Hello,{props.name}</h1>;
}
```

该函数式一个有效的React组件，因为它接收唯一带有数据的props对象与并返回一个React元素。这类组件被称为“函数组件”，因为它本质上就是JavaScript函数。

同时还可以使用ES6的class来定义组件。

```
class Welcome extends React.Component{
    render(){
    	return <h1>Hello,{this.props.name}</h1>
    }
}
```

###### 渲染组件

之前，遇到的React元素都只是DOM标签：

```
const element = <div/>
```

不过，React元素也可以是用户自定义的组件

```
const element = <Welcome name='Sara'>
```

当React元素为用户自定义组件时，它会将JSX所接收的属性（attributes）转换为单个对象传递给组件，这个对象被称之为“props”。

自定义组件名称必须以大写字母开头。React会将以小写字母开头的组件视为原生DOM标签。

###### Props的只读性

组件无论是使用函数声明还是通过class声明，都决不能修改自身的props。

```
function sum(a, b){
	return a + b;
}
```

这样的函数被称为“纯函数”，因为该函数不会尝试更改入参，且多次调用下相同的入参始终返回相同的结果。

相反，下面这个函数则不是纯函数，因为它更改了自己的入参：

```
function withdraw(account, amount){
	account.total -= amount;
}
```

React非常灵活，但它也有一个严格的规则：所有React组件都必须像纯函数一样保护它们的props不被更改。

##### State & 生命周期

State与props类似，但是state是私有的，并且完全受控于当前组件。

现在Clock组件被定义为class，而不是函数。每次组件更新时render方法都会被调用，但只要在相同的DOM节点中渲染<Clock/>，就仅有一个Clock组件的class实例被创建使用。这就使得我们可以使用如state或生命周期方法等很多其他特性。

###### 将生命周期方法添加到Class中

可以为class组件声明一些特殊的方法，当组件挂载或卸载时就会去执行这些方法。这些方法叫做“生命周期方法”。

###### 正确地使用State

1. 不要直接修改State  this.state.comment = 'Hello'; //wrong 而是应该使用setState this.setState({comment: 'hello'})   **构造函数是唯一可以给this.state赋值的地方

2. State的更新可能是异步的

   出于性能考虑，React可能会把多个setState()调用合并成一个调用。

   要解决这个问题，可以让setState()接收一个函数而不是一个对象。这个函数用上一个state作为第一个参数，将此次更新被应用时的props作为第二个参数：

   ```
   this.setState((state, props)=>({
   	counter:state.counter + props.increment
   }));
   ```

​    3.State的更新会被合并

​       当你调用setState()的时候，React会把你提供的对象合并到当前的state。

###### 数据是向下流动的

不管是父组件或是子组件都无法知道某个组件是有状态的还是无状态的，并且它们也并不关心它是函数组件还是class组件。这就是为什么称state为局部或是封装的原因。除了拥有并设置了它的组件，其他组件都无法访问。

组件可以选择把它的state作为props向下传递到它的子组件中。这对于自定义组件同样适用。

这通常会被叫做“自上而下”或是“单向”的数据流。任何的state总是所属于特定的组件，而且从该state派生的任何数据或UI只能影响树中“低于”它们的组件。

在React应用中，组件是有状态组价还是无状态组件属于组件实现的细节，它可能会随着时间的推移而改变。你可以在有状态的组件中使用无状态的组件，反之亦然。

Props和State之间最重要的区别是：props由父组件传入，而state由组件本身管理。组件不能修改props，但它可以修改state。

##### 事件处理

React元素的事件处理和DOM元素的很相似，但是有一点语法上的不同：

- React事件的命名采用小驼峰式，而不是纯小写
- 使用JSX语法时你需要传入一个函数作为事件处理函数，而不是一个字符
- 在React中另一个不同点是你不能通过返回false的方式阻止默认行为。你必须显示的使用preventDefault。

 例如，传统的HTML:

```
<button onclick='activateLasers()'>
	Active Lasers
</button>
```

在React中略微不同：

```
<button onClick={activateLasers}>
    Activate Lasers
</button>
```

传统的HTML中阻止链接默认打开一个新页面，可以这样写：

```
<a href='#' onclick="console.log('The link was clicked.'); return flase"></a>
```

在React中，可能是这样的：

```
function ActionLink(){
	funciton handleClick(e){
		e.preventDefault();
		consoel.log('The link was clicked.');
	}
	return (
		<a href='#' onClick={}handleClick>Click me</a>
	)
}
```

在这里，e是一个合成事件。React根据W3C规范来定义这些合成事件，所以你不需要担心跨刘安琪的兼容性问题。

当你使用ES6 class语法定义一个组件的时候，通常的做法是将事件处理函数声明为 class中的方法。

你必须谨慎对待JSX回调函数中的this，在JavaScript中，class的方法默认不会绑定this。如果你忘记绑定this.handleClick并把它传入onClick，当你调用这个函数的时候this的值为undefined。

这并不是React特有的行为；这其实与JavaScript函数工作原理有关。通常情况下，如果你没有在方法后面添加()，例如onClick={this.handleClick}，你应该为这个方法绑定this。

如果觉得使用bind很麻烦，这里有两种方式可以解决。如果你正在使用实验性的public class fields语法，你可以使用class fields 正确的绑定回调函数：

```
class LogginButton extends React.Component{
	//此语法确保`handleCick`内的`this`已被绑定
	//注意：这是*实验性*语法
	handleClick = () =>{
		ocnsole.log('this is :',this)
	}
	render(){
		return(
			<button onClick={this.handleClick}>Click me</button>
		)
	}
}

```

如果你没有使用class fields语法，你可以在回调中使用箭头函数。

```
class LogginButton extends React.Component{
	handleClick(){
		console.log('this is:', this);
	}
	render(){
		return(
			<button onClick={(e)=>this.handelClick(e)}>Click me</button>
		)
	}
}
```

此语法问题在于每次渲染LogginButton时都会创建不同的回调函数。在大多数情况下，这没什么问题，但如果该回调函数作为prop传入子组件时，这些组件肯会进行额外的重新渲染。我们通常建议在构造器中绑定或使用class fields语法来避免这类性能问题。

###### 向事件处理程序传递参数

在循环中，通常我们会为事件处理函数传递额外的参数。例如，若id是你要删除那一行的ID，以下两种方式都可以向事件处理函数传递参数：

```
<button onClick={(e)=> this.deleteRow(id,e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this,id)}>Delete Row</button>
```

上述两种方式是等价的，分别通过箭头函数和Function.prototype.bind来实现。

在这两种情况下，React的事件对象e会被作为第二个参数传递。如果通过箭头函数的方式，事件对象必须显式的进行传递，而通过bind的方式，事件对象以及更多的参数将会被隐式的进行传递。

##### 条件渲染

React中的条件渲染和JavaScript中的一样，使用JavaScript运算符if或者条件运算符去创建元素来表现当前的状态，然后让React根据它们来更新UI。

声明一个变量并使用if语句进行条件渲染是不错的方式，但有时你可能会想使用更为简洁的语法。

###### 介绍几种在JSX中内联条件渲染的方法

与运算符&&

通过花括号包裹代码，你可以在JSX中嵌入任何表达式。这也包括JavaScript中的逻辑与(&&)运算符。它可以很方便地进行元素的条件渲染。

之所以能这样做，是因为在JavaScrip中，true && expression 总是返回expression，而false && expression 总是返回false。

因此，如果条件是true，&&右侧的元素就会被渲染，如果是false,React会忽略并跳过它。

三目运算符

另一种内联条件渲染的方法是使用JavaScript中的三目运算符 condition ? true : false。

###### 阻止组件渲染

在极少数情况下，你可能希望能隐藏组件，即使它已经被其他组件渲染。若要完成此操作，你可以让render方法直接返回null，而不进行任何渲染。

在组件的render方法中返回null并不会影响组件的生命周期。例如，上面这个示例中，componentDidUpdate依然会被调用。

##### 列表& Key

###### 渲染多个组件

可以通过使用{}在JSX内构建一个元素集合。

使用JavaScript中的map()方法来遍历numbers数组。

###### Key

Key帮助React识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。一个元素的key最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用来自数据id来作为元素的key。

###### 用key提取组件

元素的key只有放在就近的数组上下文中才有意义。比方说，如果你提取出一个ListItem组件，你罂告把key保留在数组中的这个<ListItem />元素上，而不是放在ListItem组件中的<li>元素上。

###### key只是在兄弟节点之间必须唯一

数组元素中使用的key在其兄弟节点之间应该是独一无二的。然而，它们不需要是全局唯一的。当我们生成两个不同的数组时，我们可以使用相同的key值。

##### 表单

###### 受控组件

在HTML中，表单元素（如<input>、<textarea>和<select>）之类的表单元素通常自己维护state，并根据用户输入进行更新。而在React中，可变状态（mutable state）通常保存在组件的state属性中，并且只能通过使用setState()来更新。

我们可以把两者结合起来，使React的state成为“唯一数据源”。渲染表单的React组件还控制着用户输入过程中表单发生的操作。被React以这种方式控制取值的表单输入元素就叫做“受控组件”。

##### 状态提升

通常，多个组件需要反映相同的变化数据，这时我们建议将共享状态提升到最近的共同父组件中去。

在React中，将多个组件中需要共享的state向上移动到它们的最近共同父组件中，便可实现共享state。这就是所谓的“状态提升”。

在React应用中，任何可变数据应当只有一个相对应的唯一“数据源”。通常，state都是首先添加到需要渲染数据的组件中去。然后，如果其他组件也需要这个state，那么你可以将它提升至这些组件的最近共同父组件中。你应当依靠自上而下的数据流，而不是尝试在不同组件间同步state。

##### 组合 vs 继承

React有十分强大的组合模式。我们推荐使用组合而非继承来实现组件间的代码重用。

###### 那么继承呢？

在Facebook，我们在成百上千个组件中使用React。我们并没有发现需要使用继承来构建组件层次的情况。

Props和组合为你提供了清晰而安全地定制组件外观和行为的灵活方式。注意：组件可以接受任意props，包括基本数据类型，React元素以及函数。

如果你想要在组件间复用非UI的功能，我们建议将其提取为一个单独的JavaScript模块，如函数、对象或者类。组件可以直接引入（import）而无需通过extend继承它们。

##### React哲学

根据单一功能原则来判定组件的范围。也就是说，一个组件原则上只能负责一个功能。

##### 代码分隔

###### 打包

大多数React应用都会使用Webpack或Browserify这类的构建工具来打包文件。打包是一个将文件引入并合并到一个单独文件的过程，最终形成一个“bundle”。接着在页面上引入该bundle，整个应用即可一次性加载。

###### 代码分隔

打包是个非常棒的技术，但随着你的应用增长，你的代码包也将随之增长。尤其是在整合了体积巨大的第三方库的情况下。你需要关注你代码包中所包含的代码，以避免因体积过大而导致加载时间过长。

为了避免搞出大体积的代码包，在前期就思考该问题并对代码包进行分割是个不错的选择。代码分割是由诸如Webpack(代码分割)和Browserify(factor-bundle)这类打包器支持的一项技术，能够创建多个包并在运行时动态加载。

对你的应用进行代码分割能够帮助你“懒加载”当前用户所需要的内容，能够显著地提高你的应用性能。尽管并没有减少应用整体的代码体积，但你可以避免加载用户永远不需要的代码，并在初始加载的时候减少所需加载的代码量。

###### 基于路由的代码分割

##### Context

Context提供了一个无需为每层组件手动添加props，就能在组件树间进行数据传递的方法。

Context主要应用场景在于很多不同层级的组件需要访问同样一些的数据。请谨慎使用，因为这会使得组件的复用性变差。

如果你只是想避免层层传递一些属性，组件组合（component composition）有时候是一个比context更好的解决方案。

使用context的通用的场景包括管理当前的locale，theme，或者一些缓存数据，这比替代方案要简单的多。

##### 错误边界

过去，组件内的JavaScript错误会导致React的内部状态被破坏，并且在下一次渲染时产生可能无法追踪的错误。这些错误基本上是由较早的其他代码（非React组件代码）错误引起的，但React并没有提供一种在组件中优雅处理这些错误的方式，也无法从错误中恢复。

###### 错误边界（Error Boundaries）

部分UI的JavaScript错误不应该导致整个应用崩溃，为了解决这个问题，React16引入了一个新的概念----错误边界。

错误边界是一种React组件，这种组件可以捕获并打印发生在其子组件树任何位置的JavaScript错误，并且，它会渲染出备用UI，而不是渲染那些崩溃了的子组件树。错误边界在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误。

##### Refs转发

Ref转发是一项将ref自动地通过组件传递到其一子组件的技巧。

##### Fragments

React中的一个常见模式是一个组件返回多个元素。Fragments允许你将子列表分组，而无需向DOM添加额外节点。

```
render(){
	return(
		<React.Fragment>
			<ChildA />
			<ChildB />
			<ChildC />
		</React.Fragment>
	)
}
```

使用<></>代替<React.Fragment></React.Fragment>，不支持key或属性。

key是唯一可以传递给Fragment的属性。

##### 高阶组件

高阶组件(HOC)是React中用于复用组件逻辑的一种高级技巧。HOC自身不是React API的一部分，它是一种基于React的组合特性而形成的设计模式。具体而言，高阶组件是参数为组件，返回值为新组件的函数。

组件是将props转换为UI，而高阶组件是将组件转换为另一个组件。

请注意。HOC不会修改传入的组件，也不会使用继承来复制其行为。相反，HOC通过将组件包装在容器组件中来组成新组件。HOC是纯函数，没有副作用。

###### 不要改变原始组件。使用组合。

不要试图咋HOC中修改组价原型（或以其他方式改变它）。

HOC不应该修改传入组件，而应该使用组合的方式，通过将组件包装在容器组件中实现功能。

注意事项：不要在render方法中使用HOC、务必复制静态方法、Refs不会被传递

##### 深入JSX

实际上，JSX仅仅只是React.createElement(component, props, ...children)函数的语法糖。

if语句以及for循环不是JavaScript表达式，所以不能在JSX中直接使用。但是，你可以用在JSX以外的代码中。

###### JSX中的子元素

包含在开始和结束标签之间的JSX表达式内容将作为特定属性props.children传递给外层组件。有几种不同的方法来传递子元素

###### 布尔类型、Null以及Undefined将会忽略

false，null，undefined，true是合法的子元素。但它们并不会被渲染。

##### 性能优化

在开发应用时使用开发模式，而在为用户部署时使用生产模式。

如果你浏览一个基于React开发模式的网站，图标背景色会变成红色。如果你浏览一个基于React生产版本的网站，图标背景色会变成深色。

##### Portals

Portal提供了一种将子节点渲染到存在于父组件以外的DOM节点的优秀的方案。

```
ReactDOM.createPortal(child, container)
```

第一个参数（child）是任何可渲染的React子元素，例如一个元素，字符串或fragment。第二个参数（container）是一个DOM元素。

通常来讲，当你从组件的render方法返回一个元素时，该元素将被挂载到DOM节点中离其最近的父节点。

##### 不使用ES6

###### 通常会使用JavaScript的class关键字来定义React组件。

###### 声明默认属性

无论是函数组件还是class组件，都拥有defaultProps属性

###### 初始化State

如果使用ES6的class关键字创建组件，可以通过给this.state赋值的方式来定义组件的初始state。

###### 自动绑定

对于使用ES6的class关键字创建的React组件，组件中的方法遵循与常规ES6 class相同的语法规则。这意味着这些方法不会自动绑定this到这个组件实例。你需要在constructor中显式地调用.bind(this)。

ES6本身不包含任何mixin支持。因此，当你在React中使用ES6 class时，将不支持mixins。

##### 生命周期方法

###### React16以前的生命周期方法：

挂载：constructor()、componentWillMount()、render()、componentDidMount()

更新：props change ->componentWillReceiveProps 、 state change->shouldComponentUpdate 、componentWillUdate、render()、componentDidUpdate()

卸载：componentWillUnmount()

###### React16以后的生命周期方法：（16可以用，17是 正式的）

挂载：当组件实例被创建并插入DOM中时，其生命周期调用顺序如下：

constructor(props)、static getDerivedStateFromProps()、render()、componentDidMount()

注意：下述生命周期方法即将过时，在新代码中应该避免使用它们：

UNSAFE_componentWillMount()

更新：当组件的props或state发生变化时会触发更新。组件更新的生命周期调用顺序如下：

static getDerivedStateFromProps(props, state)、

shouldComponentUpdate(nextPorps, nextState)、render()、getSnapshotBeforeUpdate()、componentDidUpdate(prevProps, prevState, snapshot)

注意：下述方法即将过时，在新代码中应该避免使用它们：

UNSAFE_componentWillUpdate()、UNSAFE_componentWillReceiveProps()

卸载：当组件从DOM中移除时会调用如下方法：

componentWillUnmount()

错误处理：当渲染过程中，生命周期或子组件的构造函数中抛出错误时，会调用如下方法：

static getDerivedStateFromError()、componentDidCatch()

###### 常用的生命周期方法

```
render() 是class组件中唯一必须实现的方法。
```

```
constructor(props)如果不初始化state或不进行方法绑定，则不需要为React组件实现构造函数。
```

在React组件挂载之前，会调用它的构造函数。在为React.Component子类实现构造函数时，应在其他语句之前调用super(props)。否则，this.props在构造函数中可能会出现未定义的bug。

通常，在React中，构造函数仅用于以下两种情况：

- 通过给this.state赋值对象来初始化内部state。
- 为事件处理函数绑定实例。

在constructor()函数中不要调用setState()方法。如果你的组件需要使用内部state,请直接在构造函数中为this.state赋值初始state。

避免将props的值复制给state!这是一个常见的错误

```
componentDidMount() 会在组件挂载后（插入DOM树中）立即调用。依赖于DOM节点的初始化应该放在这里。如需通过网络请求获取数据，此处是实例化请求的好地方。
```

```
componentDidUpdate(prevProps, prevState, snapshot) 会在更新后会被立即调用。首次渲染不会执行此方法。
```

当组件更新后，可以在此处对DOM进行操作。如果你对更新前后的props进行了比较，也可以选择在此处进行网络请求。（例如，当props未发生变化时，则不会执行网络请求）。

如果组件实现了getSnapshotBeforeUpdate()生命周期，则它的返回值将作为componentDidUpdate()的第三个参数‘snapshot’参数传递，否则此参数将为undefined。

```
componentWillUnmount()会在组件卸载及销毁之前直接调用。在此方法中执行必要的清理操作，例如，清除timer，取消网络请求或清除在componentDidMount()中创建的订阅等。
```

componentWillUnmount()中不应调用setState()，因为该组件将永远不会重新渲染。组件实例卸载后，将永远不会再挂载它。

###### 不常用的生命周期方法

```
shouldComponentUpdate(nextProps, nextState)
```

根据shouldComponentUpdate(nextprops, nextState)的返回值，判断React组件的输出是否受当前state或props更改的影响。默认行为是state每次发生变化组件都会重新渲染。大部分情况下，你应该遵循默认行为。

当props或state发生变化时，shouldComponentUpdate()会在熏染执行之前被调用。返回值默认为true。首次渲染或使用forceUpdate()时不会调用该方法。

目前，如果shouldComponentUpdate()返回false，则不会调用UNSAFE_componentWillUpdate，render()和componentDidUpdate()。后续版本，React可能会将shouldComponentUpdate视为提示而不是严格的指令，并且，当返回false时，仍可能导致组件重新渲染。

```
static getDerivedStateFromProps(props,state)
```

getDerivedStateFromProps会在调用render方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新state，如果返回null则不更新任何内容。

此方法适用于罕见的用例，即state的值在任何时候都取决于props。例如，实现<Transition>组件可能很方便，该组件会比较当前组件与下一组件，以决定针对哪些组件进行转场动画。

请注意，不管原因是什么，都会在每次渲染前触发此方法。这与UNSAFE_componentWillReceiveProps形成对比，后者仅在父组件重新渲染时触发，而不是在内部调用setState时。

```
getSnapshotBeforeUpdate(prevProps, prevState)
```

getSnapshotBeforeUpdate()在最近一次渲染输出（提交到DOM节点）之前调用。它使得组件能在发生更改之前从DOM中捕获一些信息（例如，滚动位置）。此生命周期的任何返回值将作为参数传递给componentDidUpdate()。

此用法并不常见，但它可能出现在UI处理中，如需要以特殊方式处理滚动位置的聊天线程等。

应返回snapshot的值（或null）。

##### Error boundaries

Error boundaries 是React组件，它会在其子组件树中的任何位置捕获JavaScript错误，并记录这些错误，展示降级UI而不是崩溃的组件树。Error boundaries组件会捕获在渲染期间，在生命周期方法以及其整个树的构造函数中发生的错误。

如果class组件定义了生命周期方法 static getDerivedStateFromError()或componentDidCatch()中的任何一个（或两个），它就成为了Error boundaries()。通过生命周期更新state可让组件捕获树中未处理的JavaScript错误并展示降级UI。

仅适用Error boundaries组件来从意外异常中恢复的情况；不要将它们用于流程控制。

Error boundaries 仅捕获组件树中以下组件中的错误。但它本身的错误无法捕获。

```
static getDerivedStateFromError(error)
```

此生命周期会在后代组件抛出错误后被调用。它将抛出的错误作为参数，并返回一个值以更新state。

getDerivedStateFromError()h会在渲染阶段调用，因此不允许出现副作用。如遇此类情况，请改用componentDidCatch()。

```
componentDidCatch(error, info)此生命周期在后代组件抛出错误后被调用
```

接收2个参数： error - 抛出的错误  info-带有componentStack key的对象，其中包含有关组件引发错误的栈信息

componentDidCatch()会在‘提交’阶段被调用，因此允许执行副作用。它应该用于记录错误之类的情况。

##### 合成事件

剪贴板事件

```
onCopy onCut onPaste
```

复合事件

```
onCompositionEnd onCompositionStart onCompositionUpdate
```

键盘事件

```
onKeyDown onKeyPress onKeyUp
```

焦点事件

```
onFocus onBlur
```

表单事件

```
onChange onInput onInvalid onSubmit
```

鼠标事件

```
onClick onContextMenu onDoubleClick onDrag onDragEnd onDragEnter onDragExit
onDragLeave onDragOver onDragStart onDrop onMouseDown onMouseEnter onMouseLeave onMouseMove onMouseOut onMouseOver onMouseUp
```

指针事件

```
onPointerDown onPointerMove onPointerUp onPointerCancel onGotPointerCapture 
onLostPointerCapture onPointerEnter onPointerLeave onPointerOver onPointerOut
```

选择事件

```
onSelect
```

触摸事件

```
onTouchCancel onTouchEnd onTouchMove onTouchStart
```

UI事件

```
onScroll
```

滚轮事件

```
onWheel
```

媒体事件

```
onAbort onCanPlay onCanPlayThrough onDurationChange onEmptied onEncrypted onEnded onError onLoadedData onLoadedMetadata onLoadStart onPause onPlay onPlaying onProgress onRateChange onSeeked onSeeking onStalled onSuspend onTimeUpdate onVolumeChange onWaiting 
```

图像事件

```
onLoad onError
```

动画事件

```
onAnimationStart onAnimationEnd onAnimationIteration
```

过渡事件

```
onTransitionEnd
```

其他事件

```
onToggle
```

##### 单页面应用

单页面应用（single-page application），是一个应用程序，它可以加载单个HTML页面，以及运行应用程序所需的所有必要资源（例如JavaScript 和 CSS）。与页面或后续页面的任何交互，都不再需要往返server加载资源，即页面不会重新加载。

##### 受控组件 vs 非受控组件

React有两种不同的方式来处理表单输入。

如果有一个input表单元素的值是由React控制，就其称为受控组件。当用户将数据输入到受控组件时，会触发修改状态的事件处理器，这时由你的代码来决定此输入是否有效（如果有效就使用更新后的值重新渲染）。入股不重新渲染，则表单元素将保持不变。

一个非受控组件，就像是运行在React体系之外的表单元素。当用户将数据输入到表单字段（例如input,dropdown等）时，React不需要做任何事情就可以映射更新后的信息。然而，这也意味着，你无法强制给这个表单字段设置一个特定值。

在大多数情况下，你应该使用受控组件。

##### 协调

当组件的pros或state发生变化时，React通过将最新返回的元素与原先渲染的元素进行比较，来决定是否有必要进行一次实际的DOM更新。当它们不相等时，React才会更新DOM。这个过程被称为“协调”。

setState什么时候是异步的？目前，在事件处理函数内部的setState是异步的。

###### 7.ref

某些情况下需要直接修改元素，要修改的子代可以是 React 组件实例，也可以是 DOM 元素。这时就要用到refs来操作DOM。

React 支持给任意组件添加特殊属性ref ，ref可以是一个字符串，也可以是一个属性接受一个回调函数，它在组件被加载或卸载时会立即执行。

[注意]在组件`mount`之后再去获取`ref`。`componentWillMount`和第一次`render`时都获取不到，在`componentDidMount`才能获取到。

版本16.3 之前，React 有两种提供 `ref`的方式：字符串和回调，因为字符串的方式有些问题，所以官方建议使用回调来使用 `ref`。而现在引入的`createRef API`，据官方说是一种零缺点的使用`ref`的方式，回调方式也可以让让路了。

##### React的ref有3种用法：

1. 字符串(已废弃)
2. 回调函数
3. React.createRef() （React16.3提供）

##### 1.字符串

最早的ref用法。

1.dom节点上使用，通过this.refs[refName]来引用真实的dom节点

```
<input ref="inputRef" /> //this.refs['inputRef']来访问
```

2.类组件上使用，通过this.refs[refName]来引用组件的实例

```
<CustomInput ref="comRef" /> //this.refs['comRef']来访问
```

**2. 回调函数**

回调函数就是在dom节点或组件上挂载函数，函数的入参是dom节点或组件实例，达到的效果与字符串形式是一样的，都是获取其引用。

回调函数的触发时机：

\1. 组件渲染后，即componentDidMount后
\2. 组件卸载后，即componentWillUnMount后，此时，入参为null
\3. ref改变后

1.dom节点上使用回调函数

```
<input ref={(input) => {this.textInput = input;}} type="text" />
```

2.类组件上使用

```
<CustomInput ref={(input) => {this.textInput = input;}} />
```

**3.React.createRef()**

在React 16.3版本后，使用此方法来创建ref。将其赋值给一个变量，通过ref挂载在dom节点或组件上，该ref的current属性将能拿到dom节点或组件的实例。

```
class Child extends React.Component{
    constructor(props){
        super(props);
        this.myRef=React.createRef();
    }
    componentDidMount(){
        console.log(this.myRef.current);
    }
    render(){
        return <input ref={this.myRef}/>
    }
}
```

注意：

1. ref在函数式组件上不可使用，函数式组件无实例，但是其内部的dom节点和类组件可以使用
2. 可以通过ReactDOM.findDOMNode()，入参是一个组件或dom节点，返回值的组件对应的dom根节点或dom节点本身
   通过refs获取到组件实例后，可以通过此方法来获取其对应的dom节点
3. React的render函数返回的是vDom(虚拟dom)

##### ref传递原理

当给 HTML 元素添加`ref 属性`时，`ref` 回调接收了底层的 DOM 元素作为参数。
 React 组件在加载时将 DOM 元素传入 ref 的回调函数**，在卸载时则会传入 null。

##### 类组件

当·`ref`属性用于使用 `class`声明的自定义组件时，`ref`的回调接收的是已经加载的 React 实例。

不管ref设置值是回调函数还是字符串，都可以通过ReactDOM.findDOMNode(ref)来获取组件挂载后真正的dom节点。

但是对于html元素使用ref的情况，ref本身引用的就是该元素的实际dom节点，无需使用ReactDOM.findDOMNode(ref)来获取，该方法常用于React组件上的ref。

Q：`this.refs` 和 `ReactDOM.findDOMNode`的区别

- ref添加到Compoennt上获取的是Compoennt实例，添加到原生HTML上获取的是DOM；
- `ReactDOM.findDOMNode`，当参数是DOM，返回值就是该DOM；当参数是Component获取的是该Component render方法中的DOM
- 二者主要区别在ref绑定在组件上的时候，`this.refs`获取到的是组件实例，`ReactDOM.findDOMNode`获取到的是dom节点。

###### 子组件调用父组件通过props

###### 父组件调用子组件的方法 - onRef

```
//Parent
import React,{Component} from "react";
import FileUpload from ".Fileload";
class Parent extends Component{
	handleClick = ()=>{
		alert(this.child.state.info)//通过this.child可以拿到child所有状态和方法
	}
	onRef = ref=>{
		this.child = ref;
		console.log(ref);//获取整个Child元素
	}
	render(){
	 return(
	 	<div>
	 		<FileUpload onRef={this.onRef} />
	 		<button onClick={this.handleClick} />
	 	</div>
	 )
	}
}
export defalut Parent;
```

```
//Child
import React,{Component} from 'React';
class Child extends React.Component{
	constructor(props){
		super(props);
		this.state={
			info:'快点击子组件按钮哈哈哈'
		}
	}
	componentDidMount(){
	  this.props.onRef(this);
	  console.log(this)//将child传递给this.props.onRef()方法
	}
	handleClick=()=>{
		this.setState({info:'通过父组件按钮取到子组件信息啦啦啦'})
	}
	render(){
		return <button onClick={this.handleChildClick}></button>*
	}
}
```

当在子组件中调用onRef函数时，正在调用从父组件传递的函数。this.props.onRef(this)这里的参数指向子组件本身，父组件接收该引用作为第一个参数：onRef=ref=>{this.child=ref},然后它使用this.child保存引用。之后，可以在父组件内访问整个子组件实例，并且可以调用子组件函数。

###### 8.Redux

##### Redux

Redux是JavaScript应用程序的可预测状态容器。Redux由Flux演变而来，但受Elm的启发，避开了Flux的复杂性。

Redux源文件由ES2015编写，但是会预编译到CommonJS和UMD规范的ES6，所以它可以支持任何现代浏览器。你不必非得使用Babel或模块打包器来使用Redux。

###### 要点

应用中所有的 state 都以一个对象树的形式储存在一个单一的 *store* 中。 惟一改变 state 的办法是触发 *action*，一个描述发生什么的对象。 为了描述 action 如何改变 state 树，你需要编写 *reducers*。

###### 动机

随着 JavaScript 单页应用开发日趋复杂，**JavaScript 需要管理比任何时候都要多的 state （状态）**。 这些 state 可能包括服务器响应、缓存数据、本地生成尚未持久化到服务器的数据，也包括 UI 状态，如激活的路由，被选中的标签，是否显示加载动效或者分页器等等。

**Redux 试图让 state 的变化变得可预测**。

###### 核心概念

要想更新 state 中的数据，你需要发起一个 action。Action 就是一个普通 JavaScript 对象（注意到没，这儿没有任何魔法？）用来描述发生了什么。

强制使用 action 来描述所有变化带来的好处是可以清晰地知道应用中到底发生了什么。如果一些东西改变了，就可以知道为什么变。action 就像是描述发生了什么的指示器。最终，为了把 action 和 state 串起来，开发一些函数，这就是 reducer。再次地，没有任何魔法，reducer 只是一个接收 state 和 action，并返回新的 state 的函数。

但是主要的想法是如何根据这些 action 对象来更新 state，而且 90% 的代码都是纯 JavaScript，没用 Redux、Redux API 和其它魔法。

###### 三大原则

单一数据源

**整个应用的 [state](https://www.redux.org.cn/docs/Glossary.html#state) 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 [store](https://www.redux.org.cn/docs/Glossary.html#store) 中。**

State是只读的

**唯一改变 state 的方法就是触发 [action](https://www.redux.org.cn/docs/Glossary.html#action)，action 是一个用于描述已发生事件的普通对象。**

使用纯函数来执行修改

**为了描述 action 如何改变 state tree ，你需要编写 [reducers](https://www.redux.org.cn/docs/Glossary.html#reducer)。**

Reducer 只是一些纯函数，它接收先前的 state 和 action，并返回新的 state

##### Action

**Action** 是把数据从应用（译者注：这里之所以不叫 view 是因为这些数据有可能是服务器响应，用户输入或其它非 view 的数据 ）传到 store 的有效载荷。它是 store 数据的**唯一**来源。一般来说你会通过 [`store.dispatch()`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 将 action 传到 store。

Action 本质上是 JavaScript 普通对象。我们约定，action 内必须使用一个字符串类型的 `type` 字段来表示将要执行的动作。多数情况下，`type` 会被定义成字符串常量。

除了 `type` 字段外，action 对象的结构完全由你自己决定。

Action创建函数

**Action 创建函数** 就是生成 action 的方法。

在 Redux 中的 action 创建函数只是简单的返回一个 action:

```
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

store 里能直接通过 [`store.dispatch()`](https://cn.redux.js.org/docs/api/Store.html#dispatch) 调用 `dispatch()` 方法，但是多数情况下你会使用 [react-redux](http://github.com/gaearon/react-redux) 提供的 `connect()` 帮助器来调用。[`bindActionCreators()`](https://cn.redux.js.org/docs/api/bindActionCreators.html) 可以自动把多个 action 创建函数 绑定到 `dispatch()` 方法上。

##### Reducer

**Reducers** 指定了应用状态的变化如何响应 [actions](https://www.redux.org.cn/docs/basics/Actions.html) 并发送到 store 的，记住 actions 只是描述了*有事情发生了*这一事实，并没有描述应用如何更新 state。

reducer 就是一个纯函数，接收旧的 state 和 action，返回新的 state。(previousState, action) => newState

**永远不要**在 reducer 里做这些操作：

- 修改传入参数；
- 执行有副作用的操作，如 API 请求和路由跳转；
- 调用非纯函数，如 `Date.now()` 或 `Math.random()`。

**只要传入参数相同，返回计算得到的下一个 state 就一定相同。没有特殊情况、没有副作用，没有 API 请求、没有变量修改，单纯执行计算。**

注意：

1. **不要修改 `state`。** 使用 [`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 新建了一个副本。不能这样使用 `Object.assign(state, { visibilityFilter: action.filter })`，因为它会改变第一个参数的值。你**必须**把第一个参数设置为空对象。你也可以开启对ES7提案[对象展开运算符](https://www.redux.org.cn/docs/recipes/UsingObjectSpreadOperator.html)的支持, 从而使用 `{ ...state, ...newState }` 达到相同的目的。
2. **在 `default` 情况下返回旧的 `state`。**遇到未知的 action 时，一定要返回旧的 `state`。

[`combineReducers()`](https://www.redux.org.cn/docs/api/combineReducers.html) 所做的只是生成一个函数，这个函数来调用你的一系列 reducer，每个 reducer **根据它们的 key 来筛选出 state 中的一部分数据并处理**，然后这个生成的函数再将所有 reducer 的结果合并成一个大的对象。

**注意每个 reducer 只负责管理全局 state 中它负责的一部分。每个 reducer 的 `state` 参数都不同，分别对应它管理的那部分 state 数据。**

[`combineReducers()`](https://cn.redux.js.org/docs/api/combineReducers.html) 所做的只是生成一个函数，这个函数来调用你的一系列 reducer，每个 reducer **根据它们的 key 来筛选出 state 中的一部分数据并处理**，然后这个生成的函数再将所有 reducer 的结果合并成一个大的对象。[没有任何魔法。](https://github.com/gaearon/redux/issues/428#issuecomment-129223274)正如其他 reducers，如果 combineReducers() 中包含的所有 reducers 都没有更改 state，那么也就不会创建一个新的对象。

##### Store

在前面的章节中，我们学会了使用 [action](https://www.redux.org.cn/docs/basics/Actions.html) 来描述“发生了什么”，和使用 [reducers](https://www.redux.org.cn/docs/basics/Reducers.html) 来根据 action 更新 state 的用法。

**Store** 就是把它们联系到一起的对象。Store 有以下职责：

- 维持应用的 state；
- 提供 [`getState()`](https://www.redux.org.cn/docs/api/Store.html#getState) 方法获取 state；
- 提供 [`dispatch(action)`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 方法更新 state；
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 注册监听器;
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 返回的函数注销监听器。

再次强调一下 **Redux 应用只有一个单一的 store**。当需要拆分数据处理逻辑时，你应该使用 [reducer 组合](https://cn.redux.js.org/docs/basics/Reducers.html#splitting-reducers) 而不是创建多个 store。

##### 数据流

**严格的单向数据流**是 Redux 架构的设计核心。

这意味着应用中所有的数据都遵循相同的生命周期，这样可以让应用变得更加可预测且容易理解。同时也鼓励做数据范式化，这样可以避免使用多个且独立的无法相互引用的重复数据。

Redux 应用中数据的生命周期遵循下面 4 个步骤：

**1.调用** [`store.dispatch(action)`](https://www.redux.org.cn/docs/api/Store.html#dispatch)。

[Action](https://www.redux.org.cn/docs/basics/Actions.html) 就是一个描述“发生了什么”的普通对象。

**2.Redux store 调用传入的 reducer 函数。**

[Store](https://www.redux.org.cn/docs/basics/Store.html) 会把两个参数传入 [reducer](https://www.redux.org.cn/docs/basics/Reducers.html)： 当前的 state 树和 action。

**3.根 reducer 应该把多个子 reducer 输出合并成一个单一的 state 树。**

根 reducer 的结构完全由你决定。Redux 原生提供[`combineReducers()`](https://www.redux.org.cn/docs/api/combineReducers.html)辅助函数，来把根 reducer 拆分成多个函数，用于分别处理 state 树的一个分支。

**4.Redux store 保存了根 reducer 返回的完整 state 树。**

这个新的树就是应用的下一个 state！所有订阅 [`store.subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 的监听器都将被调用；监听器里可以调用 [`store.getState()`](https://www.redux.org.cn/docs/api/Store.html#getState) 获得当前 state。

现在，可以应用新的 state 来更新 UI。如果你使用了 [React Redux](https://github.com/gaearon/react-redux) 这类的绑定库，这时就应该调用 `component.setState(newState)` 来更新。

##### 搭配React

这里需要再强调一下：Redux 和 React 之间没有关系。Redux 支持 React、Angular、Ember、jQuery 甚至纯 JavaScript。

尽管如此，Redux 还是和 [React](http://facebook.github.io/react/) 和 [Deku](https://github.com/dekujs/deku) 这类库搭配起来用最好，因为这类库允许你以 state 函数的形式来描述界面，Redux 通过 action 的形式来发起 state 变化。

###### 容器组件（Smart/Container Components）和展示组件（Dumb/Presentational Components）

|                | 展示组件                   | 容器组件                           |
| -------------: | :------------------------- | :--------------------------------- |
|           作用 | 描述如何展现（骨架、样式） | 描述如何运行（数据获取、状态更新） |
| 直接使用 Redux | 否                         | 是                                 |
|       数据来源 | props                      | 监听 Redux state                   |
|       数据修改 | 从 props 调用回调函数      | 向 Redux 派发 actions              |
|       调用方式 | 手动                       | 通常由 React Redux 生成            |

##### 使用对象展开运算符（Object Spread Operator）

从不直接修改 state 是 Redux 的核心理念之一, 所以你会发现自己总是在使用 [`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 创建对象拷贝, 而拷贝中会包含新创建或更新过的属性值。

一个可行的替代方案是使用 ES7 提案的 [对象展开运算符](https://github.com/sebmarkbage/ecmascript-rest-spread)。该提案让你可以通过展开运算符 (`...`) , 以更加简洁的形式将一个对象的可枚举属性拷贝至另一个对象。

compose(...functions) 从右到左来组合多个函数

###### 参数

1. (*arguments*): 需要合成的多个函数。预计每个函数都接收一个参数。它的返回值将作为一个参数提供给它左边的函数，以此类推。例外是最右边的参数可以接受多个参数，因为它将为由此产生的函数提供签名。（译者注：`compose(funcA, funcB, funcC)` 形象为 `compose(funcA(funcB(funcC())))`）

###### 返回值

(*Function*): 从右到左把接收到的函数合成后的最终函数。

##### 异步action创建函数

如何把 [之前定义](https://cn.redux.js.org/docs/advanced/AsyncActions.html#synchronous-action-creators) 的同步 action 创建函数和网络请求结合起来呢？标准的做法是使用 [Redux Thunk 中间件](https://github.com/gaearon/redux-thunk)。要引入 `redux-thunk` 这个专门的库才能使用。我们 [后面](https://cn.redux.js.org/docs/advanced/Middleware.html) 会介绍 middleware 大体上是如何工作的；目前，你只需要知道一个要点：通过使用指定的 middleware，action 创建函数除了返回 action 对象外还可以返回函数。这时，这个 action 创建函数就成为了 [thunk](https://en.wikipedia.org/wiki/Thunk)。

thunk 的一个优点是它的结果可以再次被 dispatch

[Thunk middleware](https://github.com/gaearon/redux-thunk) 并不是 Redux 处理异步 action 的唯一方式：

- 你可以使用 [redux-promise](https://github.com/acdlite/redux-promise) 或者 [redux-promise-middleware](https://github.com/pburtchaell/redux-promise-middleware) 来 dispatch Promise 来替代函数。
- 你可以使用 [redux-observable](https://github.com/redux-observable/redux-observable) 来 dispatch Observable。
- 你可以使用 [redux-saga](https://github.com/yelouafi/redux-saga/) 中间件来创建更加复杂的异步 action。
- 你可以使用 [redux-pack](https://github.com/lelandrichardson/redux-pack) 中间件 dispatch 基于 Promise 的异步 Action。
- 你甚至可以写一个自定义的 middleware 来描述 API 请求，就像这个 [真实场景的案例](https://cn.redux.js.org/docs/introduction/Examples.html#real-world) 中的做法一样。

##### 异步数据流

默认情况下，[`createStore()`](https://cn.redux.js.org/docs/api/createStore.html) 所创建的 Redux store 没有使用 [middleware](https://cn.redux.js.org/docs/advanced/Middleware.html)，所以只支持 [同步数据流](https://cn.redux.js.org/docs/basics/DataFlow.html)。

你可以使用 [`applyMiddleware()`](https://cn.redux.js.org/docs/api/applyMiddleware.html) 来增强 [`createStore()`](https://cn.redux.js.org/docs/api/createStore.html)。虽然这不是必须的，但是它可以帮助你[用简便的方式来描述异步的 action](https://cn.redux.js.org/docs/advanced/AsyncActions.html)。

##### Middleware

Redux middleware 被用于解决不同的问题，但其中的概念是类似的。**它提供的是位于 action 被发起之后，到达 reducer 之前的扩展点。** 你可以利用 Redux middleware 来进行日志记录、创建崩溃报告、调用异步接口或者路由等等。

##### 应该使用什么样的异步中间件? 怎样在 thunk、saga、observable 或其他类似的库中做出选择?

这里有 [许多可用的异步/副作用中间件](https://github.com/markerikson/redux-ecosystem-links/blob/master/side-effects.md), 但最常用的有 [`redux-thunk`](https://github.com/reduxjs/redux-thunk)、 [`redux-saga`](https://github.com/redux-saga/redux-saga)和 [`redux-observable`](https://github.com/redux-observable/redux-observable)。 它们是优势、弱点及用例各不相同的工具。

一般的经验是:

- Thunk 最适合复杂的同步逻辑（特别是需要访问整个 Redux 存储状态的代码）和简单的异步逻辑（如基本的 AJAX 调用）。 通过使用`async / await`，也可将 thunk 用于更复杂的基于 promise 的逻辑。
- Saga 最适合复杂的异步逻辑和解耦的“后台线程”类型的行为，特别是如果需要监听发起的（dispatched）action（这是通过 thunk 无法完成的事情）。 saga 需要使用者熟练使用 ES6 generator 函数和`redux-saga`的“effects”运算符。
- Observable 与 saga 解决了同一类问题，但依赖于 RxJS 来实现异步行为。 Observable 需要使用者熟练使用 RxJS API。

我们建议大多数 Redux 用户应该从 thunk 开始，如果他们的应用确实需要处理更复杂的异步逻辑，再添加一个额外的副作用库，如 saga 或 observable。

因为 saga 和 observable 具有相同的用例，所以应用程序通常使用其中一个，但不能同时使用两者。 但是，请注意 **同时使用 thunk 和 saga 或者 observable 是完全没问题的**，因为它们解决的是不同的问题。

##### 为什么 Redux 需要不变性？

- Redux 和 React-Redux 都使用了

  浅比较

  。具体来说：

  - Redux 的 `combineReducers` 方法 [浅比较](https://cn.redux.js.org/docs/faq/ImmutableData.html#how-redux-uses-shallow-checking) 它调用的 reducer 的引用是否发生变化。
  - React-Redux 的 `connect` 方法生成的组件通过 [浅比较根 state 的引用变化](https://cn.redux.js.org/docs/faq/ImmutableData.html#how-react-redux-uses-shallow-checking) 与 `mapStateToProps` 函数的返回值，来判断包装的组件是否需要重新渲染。 以上[浅比较需要不变性](https://cn.redux.js.org/docs/faq/ImmutableData.html#redux-shallow-checking-requires-immutability)才能正常工作

- 不可变数据的管理极大地提升了数据处理的安全性。

- 进行时间旅行调试要求 reducer 是一个没有副作用的纯函数，以此在不同 state 之间正确的移动。

##### 浅比较和深比较有何区别？

浅比较（也被称为 **引用相等**）只检查两个不同 **变量** 是否为同一对象的引用；与之相反，深比较（也被称为 **原值相等**）必须检查两个对象所有属性的 **值** 是否相等。

所以，浅比较就是简单的（且快速的）`a === b`，而深比较需要以递归的方式遍历两个对象的所有属性，在每一个循环中对比各个属性的值。

正是因为性能考虑，Redux 使用浅比较。

##### Redux 是如何使用浅比较的？

Redux 在 `combineReducers` 函数中使用浅比较来检查根 state 对象（root state object）是否发生变化，有修改时，返回经过修改的根 state 对象的拷贝，没有修改时，返回当前的根 state 对象。

##### React-Redux 是如何使用浅比较的？

React-Redux 使用浅比较来决定它包装的组件是否需要重新渲染。

首先 React-Redux 假设包装的组件是一个“纯”（pure）组件，即[给定相同的 props 和 state，这个组件会返回相同的结果](https://github.com/reactjs/react-redux/blob/f4d55840a14601c3a5bdc0c3d741fc5753e87f66/docs/troubleshooting.md#my-views-arent-updating-when-something-changes-outside-of-redux)。

做出这样的假设后，React-Redux 就只需检查根 state 对象或 `mapStateToProps` 的返回值是否改变。如果没变，包装的组件就无需重新渲染。

为了检测改变是否发生，React-Redux 会保留一个对根 state 对象的引用，还会保留 `mapStateToProps` 返回的 props 对象的**每个值**的引用。

最后 React-Redux 会对根 state 对象的引用与传递给它的 state 对象进行浅比较，还会对每个 props 对象的每个值的引用与 `mapStateToProps` 返回的那些值进行一系列浅比较。

##### 为什么 React-Redux 对 `mapStateToProps` 返回的 props 对象的每个值进行浅比较？

对 props 对象来说，React-Redux 会对其中的每个**值**进行浅比较，而不是 props 对象本身。

它这样做的原因是：props 对象实际上是一组由属性名和其值（或用于取值或生成值的 selector 函数）的键值对组成的。

##### React-Redux 是如何使用浅比较来决定组件是否需要重新渲染的？

每次调用 React-Redux 提供的 `connect` 函数时，它储存的根 state 对象的引用，与当前传递给 store 的根 state 对象之间，会进行浅比较。如果相等，说明根 state 对象没有变化，也就无需重新渲染组件，甚至无需调用 `mapStateToProps`。

如果发现其不相等，说明根 state 对象**已经**被更新了，这时 `connect` 会调用 `mapStateToProps` 来查看传给包装的组件的 props 是否被更新。

它会对该对象的每一个值各自进行浅比较，如果发现其中有不相等的才会触发重新渲染。

##### [react-router\] hashHistory 和 browserHistory 的区别

react-router提供了三种方式来实现路由，并没有默认的路由，需要在声明路由的时候，显式指定所使用的路由。

- browserHistory
- hashHistory
- createMemoryHistory

官方推荐使用browserHistory

使用hashHistory，浏览器的url是这样的：/#/user/liuna?_k=adsels

在url的前面会有一个#  可以理解为锚点 每个路由前面都会有# 这样不至于路由出问题找不到页面

优点：不需要后端配置

使用browserHistory，它是使用浏览器内置的 [History](https://link.jianshu.com/?t=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FHistory) API 实现的。浏览器的url是这样的：/user/liuna

路由是怎么样的就会展示什么样的

这样看起来当然是browserHistory更好一些，但是它需要server端支持。同时，后端也需要特殊配置，来处理这些可能发送到后端的路由，不光是因为用户可能手动刷新页面，还有另外一个更重要的原因。在不支持 History 特性的旧式浏览器中，react-router就会直接重新加载页面（显然后端必须识别这些路由）。

使用hashHistory时，因为有#的存在，浏览器不会发送request，react-router自己根据url去render相应的模块。

使用browserHistory时，从/到user/liuna，浏览器会向server发送request，所以server要做特殊请求，比如用的express的话，你需要handle所有的路由，`app.get('*', (req, res) => { ... })`，使用了 nginx 的话，nginx也要做相应的配置。

如果只是静态页面，就不需要用browserHistory,直接hashHistory就好了。

##### 单一职责

当一个组件只有一个改变的原因时，它有一个单一的职责。

编写React组件时要考虑的基本准则是单一职责原则。单一职责原则(缩写：`SRP`)要求组件有一个且只有一个变更的原因。

为什么只有一个理由可以改变很重要？因为这样组件的修改隔离并且受控。单一职责原则控制了组件的大小，使其集中在一件事情上。集中在一件事情上的组件便于编码、修改、重用和测试。

组合（composition）是一种通过将各组件联合在一起以创建更大组件的方式。组合是 React 的核心。

一种组件的变化对另一种组件的影响很小。这就是单一职责原则的作用：修改隔离，对系统的其他组件产生影响很轻微并且可预测。

##### 封装

一个封装组件提供 `props` 控制其行为而不是暴露其内部结构。

耦合是决定组件之间依赖程度的系统特性。根据组件的依赖程度，可区分两种耦合类型：

- 当应用程序组件对其他组件知之甚少或一无所知时，就会发生松耦合。
- 当应用程序组件知道彼此的许多详细信息时，就会发生紧耦合。

松耦合是我们设计应用结构和组件之间关系的目标。

###### 松耦合应用(封装组件)

**松耦合**会带来以下好处：

- 可以在不影响应用其它部分的情况下对某一块进行修改。、
- 任何组件都可以替换为另一种实现
- 在整个应用程序中实现组件复用，从而避免重复代码
- 独立组件更容易测试，增加了测试覆盖率

相反，紧耦合的系统会失去上面描述的好处。主要缺点是很难修改高度依赖于其他组件的组件。即使是一处修改，也可能导致一系列的依赖组件需要修改。

###### 紧耦合应用(组件无封装)

**封装** 或 **信息隐藏** 是如何设计组件的基本原则，也是松耦合的关键。

###### 信息隐藏

封装良好的组件隐藏其内部结构，并提供一组属性来控制其行为。

隐藏内部结构是必要的。其他组件没必要知道或也不依赖组件的内部结构或实现细节。

`React` 组件可能是函数组件或类组件、定义实例方法、设置 `ref`、拥有 `state` 或使用生命周期方法。这些实现细节被封装在组件内部，其他组件不应该知道这些细节。

隐藏内部结构的组件彼此之间的依赖性较小，而降低依赖度会带来松耦合的好处。

###### 通信

细节隐藏是隔离组件的关键。此时，你需要一种组件通信的方法：`props`。`porps` 是组件的输入

建议 `prop` 的类型为基本数据（例如，`string` 、 `number` 、`boolean`）：

必要时，使用复杂的数据结构，如对象或数组

`prop` 可以是一个事件处理函数和异步函数

`prop` 甚至可以是一个组件构造函数。组件可以处理其他组件的实例化

给子组件设置 `props` 的父组件不应该暴露其内部结构的任何细节。例如，使用 `props` 传输整个组件实例或 `refs` 都是一个不好的做法。

访问全局变量同样也会对封装产生负面影响。

##### 组合

一个组合式组件是由更小的特定组件组合而成的。

组合（composition）是一种通过将各组件联合在一起以创建更大组件的方式。组合是 React 的核心。

幸运的是，组合易于理解。把一组小的片段，联合起来，创建一个更大个儿。

React 组件的组合是自然而然的。这个库使用了一个声明范式，从而不会抑制组合式的表现力。

[单一责任原则](https://juejin.im/post/5d4acb28e51d45620771f082)描述了如何将需求拆分为组件，[封装](https://juejin.im/post/5d4c329e51882511ed7c203f)描述了如何组织这些组件，组合描述了如何将整个系统粘合在一起。

###### 组合的好处

单一责任：组合的一个重要方面在于能够从特定的小组件组成复杂组件的能力。这种分而治之的方式帮助了被组合而成的复杂组件也能符合 SRP 原则。

可重用：组合有可重用的有点，使用组合的组件可以重用公共逻辑

灵活：在 `react` 中，一个组合式的组件通过给子组件传递 `props` 的方式，来控制其子组件。这就带来了灵活性的好处。

高效：用户界面可组合的层次结构，因此，组件的组合是一种构建用户界面的有效的方式。

##### 复用

可重用的组件，一次编写多次使用。

符合单一职责原则是必须的：复用一个组件实际上就意味着复用其职责

只有一个职责的组件是最容易复用的。

`react-router` 使用了声明式的路由来构建一个单页应用。使用 `<Route>` 将 `URL` 和组件关联起来。当用户访问匹配的 `URL` 时，路由将渲染相应的组件。

`redux` 和 `react-redux` 引入了单向和可预测的应用状态管理。可以将异步的和非纯的代码（例如 `HTTP` 请求）从组件中提取出来，从而符合单一职责原则并创建出 纯（pure）组件 或 几乎纯（almost-pure）的组件。

##### 纯组件

纯组件总是为相同的属性值渲染相同的元素。几乎纯的组件总是为相同的属性值呈现相同的元素，但是会产生副作用。

而纯函数没有副作用且不依赖于全局状态。只要输入相同，输出一定相同。因此，纯函数的结果是可预测的，确定的，可以复用，并且易于测试。

`React` 组件也应该考虑设计为纯组件，当 `prop` 的值相同时， 纯组件(注意区分`React.PureComponent`)渲染的内容相同。

非纯组件是必要的，大多数应用程序中都需要全局状态，网络请求，本地存储等。你所能做的就是将 纯组件和非纯组件隔离，也就是说将你的组件进行**提纯**。

非纯代码显式的表明了它有副作用，或者是依赖全局状态。在隔离状态下，不纯代码对系统其它部分的不可预测的影响较小。

##### 可测试和经过测试

**经过测试**的组件验证了其在给定输入的情况下，输出是否符合预期。

**可测试**组件易于测试。

为什么自动化组件验证很重要：进行单元测试。单元测试确保每次进行修改时，组件都能正常工作。

单元测试不仅涉及早期错误检测。另一个重要方面是能够验证组件架构是否合理。

组件很难测试往往是因为它有很多 `props`、依赖项、需要原型和访问全局变量，而这些都是设计糟糕的标志。

当组件的架构设计很脆弱时，就会变得难以测试，而当组件难以测试的时候，你大概会跳过编写单元测试的过程，最终的结果就是：组件未测试。

良好封装的组件，测试起来简单明了，相反，没有正确封装的组件难以测试。

可测试性是确定组件结构良好程度的实用标准。

阅读有意义的代码很容易。然而，想要编写有意义的代码，需要清晰的代码实践和不断的努力来清楚地表达自己。

##### 组件命名

###### 帕斯卡命名法

组件名是由一个或多个帕斯卡单词（主要是名词）串联起来的，比如：`<DatePicker>`、`<GridItem>`、`Application`、`<Header>`。

组件，方法和变量的有意义的名称足以使代码可读。 因此，注释大多是多余的。

##### 结论：

可靠的组件实现一个职责，隐藏其内部结构并提供有效的 `props` 来控制其行为。

单一职责和封装是 `solid` 设计的基础。(maybe你需要了解一下 solid 原则是什么。)

单一职责建议创建一个只实现一个职责的组件，并有一个改变的理由。

良好封装的组件隐藏其内部结构和实现细节，并定义 `props` 来控制行为和输出。

组合结构大的组件。只需将它们分成较小的块，然后使用组合进行整合，使复杂组件变得简单。

可复用的组件是精心设计的系统的结果。尽可能重复使用代码以避免重复。

网络请求或全局变量等副作用使组件依赖于环境。通过为相同的 `prop` 值返回相同的输出来使它们变得纯净。

有意义的组件命名和表达性代码是可读性的关键。你的代码必须易于理解和阅读。

测试不仅是一种自动检测错误的方式。如果你发现某个组件难以测试，则很可能是设计不正确。

成功的应用站在可靠的组件的肩膀上，因此编写可靠、可扩展和可维护的组件非常中重要。

