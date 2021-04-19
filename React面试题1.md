##### React面试知识点

##### 1.什么是声明式编程

声明式编程是一种编程范式，它关注的是你要做什么，而不是如何做。它表达逻辑而不显式地定义步骤。这意味着我们需要根据逻辑的计算来声明要显示的组件。它没有描述控制流步骤。声明式编程的例子有HTML、SQL等。

##### 2.声明式编程VS命令式编程

声明式编程的编写方式描述了应该做什么，而命令式编程描述了如何做。在声明式编程中，让编译器决定如何做事情。声明性程序很容易推理，因为代码本身描述了它在做什么。

下面是一个例子，数组中的每个元素都乘以 `2`，我们使用声明式`map`函数，让编译器来完成其余的工作，而使用命令式，需要编写所有的流程步骤。

const numbers = [1,2,3,4,5];

// 声明式

const doubleWithDec = numbers.map(number => number * 2);

console.log(doubleWithDec)

// 命令式

const doubleWithImp = [];

for(let i=0; i<numbers.length; i++) {

​    const numberdouble = numbers[i] * 2;

​    doubleWithImp.push(numberdouble)

}

console.log(doubleWithImp)

##### 3.什么是函数式编程

函数式编程是声明式编程的一部分。javascript中的函数是第一类公民，这意味着函数是数据，你可以像保存变量一样在应用程序中保存、检索和传递这些函数。

函数式编程有些核心的概念，如下：

不可变性（Immutability）

纯函数（Pure Functions）

数据转换（Data Transformations）

高阶函数（Higher-Order Functions）

递归

组合

不可变性（Immutability）

不可变性意味着不可改变。在函数式编程中，你无法更改数据，也不能更改。如果要改变或更改数据，则必须复制数据副本来更改。

###### 纯函数

纯函数是始终接受一个或多个参数并计算参数并返回数据或函数的函数。它没有副作用，例如设置全局状态，更改应用程序状态，它总是将参数视为不可改变数据。

使用纯函数，它接受参数，基于参数计算，返回一个新对象而不修改参数。

###### 数据转换

讲了很多关于不可变性的内容，如果数据是不可变的，我们如何改变数据。如上所述，我们总是生成原始数据的转换副本，而不是直接更改原始数据。再介绍一些Javascript内置函数，当然还有很多其他的函数。所有这些函数都不改变现有的数据，而是返回新的数组或对象。

###### 高阶函数

高阶函数是将函数作为参数或返回函数的函数，或者有时它们都有。这些高阶函数可以操纵其他函数。

Array.map , Array.filter 和 Array.reduce 是高阶函数，因为它们将函数作为参数。

###### 递归

递归是一种函数在满足一定条件之前调用自身的技术。只要可能，最好使用递归而不是循环。你必须注意这一点，浏览器不能处理太多递归和抛出错误。

###### 组合

在React中，我们将功能划分为小型可重用的纯函数，我们必须将所有这些可重用的函数放在一起，最终使其成为产品。将所有较小的函数组合成更大的函数，最终，得到一个应用程序，这称为组合。

实现组合有许多不同方法。从JavaScript中了解到的一种常见方法是链接。链接是一种使用点表示法调用前一个函数的返回值的函数的方法。

##### 4.什么是React

React是一个简单的JavaScript UI库，用于构建高效、快速的用户界面。它是一个轻量级库，因此很受欢迎。它遵循组件设计模式、声明式编程范式和函数式编程概念，以使前端应用程序更高效。它使用虚拟DOM来有效地操作DOM。它遵循从高阶组件到低阶组件的单向数据流。

##### 5.React 与 Angular 有何不同？

Angular是一个成熟的MVC框架，带有很多特定的特性，比如服务、指令、模板、模块、解析器等等。React是一个非常轻量级的库，它只关注MVC的视图部分。

Angular遵循两个方向的数据流，而React遵循从上到下的单向数据流。React在开发特性时给了开发人员很大的自由，例如，调用API的方式、路由等等。我们不需要包括路由器库，除非我们需要它在我们的项目。

##### 6.什么是Virtual DOM及其工作原理

React使用Virtual DOM来更新真正的DOM，从而提高效率和速度。

###### 什么是Virtual DOM

浏览器遵循HTML指令来构造文档对象模型（DOM）。当浏览器加载HTML并呈现用户界面时，HTML文档中的所有元素都变成DOM元素。

DOM是从根元素开始的元素层次结构。当在浏览器加载这个HTML时，所有这些HTML元素都被转换成DOM元素。当涉及到SPA应用程序时，首次加载index.html，并在index.html本身中加载更新后的数据或另一个html。当用户浏览站点时，我们使用新内容更新相同的index.html。每当DOM发生更改时，浏览器都需要重新计算CSS、进行布局并重新绘制web页面。

React使用Virtual DOM有效地重建DOM。对于我们来说，这使得DOM操作的一项非常复杂和耗时的任务变得更加容易。React从开发人员那里抽象出所有这些，以便在Virtual DOM的帮助下构建高效的UI。

###### 虚拟DOM是如何工作的

虚拟DOM只不过是真实DOM的javascript对象表示。与更新真实DOM相比，更新javascript对象更容易，更快捷。

React将整个DOM副本保存为虚拟DOM。每当有更新时，它都会维护两个虚拟DOM，以比较之前的状态和当前状态，并确定哪些对象已被更改。现在，它通过比较两个虚拟DOM差异，并将这些变化更新到实际DOM。一旦真正的DOM更新，它也会更新UI。

##### 7.什么是JSX

JSX是javascript的语法扩展。它就像一个拥有javascript全部 功能的模板语言。它生成React元素，这些元素将在DOM中呈现。React建议在组件使用JSX。在JSX中，我们结合了javascript和HTML,并生成了可以在DOM中呈现的react元素。

##### 8.组件和不同类型

React中一切都是组件。我们通常将应用程序的整个逻辑分解为小的单个部分。我们将每个单独的部分称为组件。通常，组件是一个javascript函数，它接受输入，处理它并返回在UI中呈现的React元素。

在React中有不同类型的组件。

###### 函数/无状态/展示组件

函数或无状态组件是一个纯函数，它可接受参数，并返回react元素。这些都是没有任何副作用的纯函数。这些组件没有状态或生命周期方法。

###### 类/有状态组件

类或有状态组件具有状态和生命周期方可能通过setState()方法更改组件的状态。类组件是通过扩展React创建的。它在构造函数中初始化，也可能有子组件。

###### 受控组件

受控组件是在React中处理输入表单的一种技术。表单元素通常维护它们自己的状态，而React则在组件的状态属性中维护状态。我们可以将两者结合起来控制输入表单。这称为受控组件。因此，在受控组件表单中，数据由React组件处理。

###### 非受控组件

大多数情况下，建议使用受控组件。有一种称为非受控组件的方法可以通过使用Ref来处理表单数据。在非受控组件中，Ref用于直接从DOM访问表单值，而不是事件处理程序。

###### 容器组件

容器组件是处理获取数据、订阅redux存储等的组件。它们包含展示组件和其他容器组件，但是里面从来没有html。

###### 高阶组件

高阶组件是将组件作为参数并生成另一个组件的组件。Redux connect是高阶组件的示例。这是一种用于生成可重用组件的强大技术。

##### 9.Props和State

Props是只读属性，传递给组件以呈现UI和状态。

##### 10.什么是PropTypes

PropTypes为组件提供类型检查，并为其他开发人员提供很好的文档。

##### 11.如何更新状态以及如何不更新

不应该直接修改状态。可以在构造函数中定义状态值。直接使用状态不会触发重新渲染。React使用this.setState()时合并状态。

使用this.setState()接受函数更安全，因为更新的props和状态是异步的。

##### 12.组件生命周期方法

###### **componentWillMount()**

在渲染前调用,在客户端也在服务端，它只发生一次。

###### **componentDidMount()**

在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过`this.getDOMNode()`来进行访问。 如果你想和其他JavaScript框架一起使用，可以在这个方法中调用`setTimeout`, `setInterval`或者发送AJAX请求等操作(防止异部操作阻塞UI)。

###### componentWillReceiveProps()

在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。

###### **shouldComponentUpdate()**

返回一个布尔值。在组件接收到新的`props`或者`state`时被调用。在初始化时或者使用`forceUpdate`时不被调用。 可以在你确认不需要更新组件时使用。

###### **componentWillUpdate()**

在组件接收到新的`props`或者`state`但还没有`render`时被调用。在初始化时不会被调用。

###### **componentDidUpdate()**

在组件完成更新后立即调用。在初始化时不会被调用。

###### **componentWillUnMount()**

件从 DOM 中移除的时候立刻被调用。

###### **getDerivedStateFromError()**

这个生命周期方法在**ErrorBoundary**类中使用。实际上，如果使用这个生命周期方法，任何类都会变成`ErrorBoundary`。这用于在组件树中出现错误时呈现回退UI，而不是在屏幕上显示一些奇怪的错误。

###### **componentDidCatch()**

这个生命周期方法在ErrorBoundary类中使用。实际上，如果使用这个生命周期方法，任何类都会变成ErrorBoundary。这用于在组件树中出现错误时记录错误。

##### 13.超越继承的组合

在React中，我们总是使用组合而不是继承。我们已经在函数式编程部分讨论了什么是组合。这是一种结合简单的可重用函数来生成高阶组件的技术。

##### 14.如何在React中应用样式

将样式应用于React组件有三种方法。

###### 外部样式表

在此方法中，可以将外部样式表导入组件使用类中。但是你应该使用className而不是class来为React元素应用样式。

###### 内联样式

style={{backgroundColor:'orange'}}

###### 定义样式对象并使用它

因为我们将javascript对象传递给`style`属性，所以我们可以在组件中定义一个`style`对象并使用它。

##### 15.什么是Redux及其工作原理

Redux是React的一个状态管理库，它基于flux。Redux简化了React中的单向数据流。Redux将状态管理完全从React中抽象出来。

###### 它是如何工作的

在React中，组件连接到redux,如果要访问redux,需要派出一个包含id和负载（payload）的action。action中的payload是可选的，action将其转发给reducer。

当reducer收到action时，通过switch...case语法比较action中type。匹配时，更新对应的内容返回新的state。

当Redux状态更改时，连接到Redux的组件将接收新的状态作为props。当组件接收到这些props时，它将进入更新阶段并重新渲染UI。

###### Redux循环细节

![img](https://user-gold-cdn.xitu.io/2019/5/30/16b0804b978a975e?imageslim)

###### 组件如何与redux进行连接

mapStateToProps:此函数将state映射到props上，因此只要state发生变化，新state会重新映射到props。这是订阅store的方式。

mapDispatchToProps:此函数用于将action creators 绑定到你的props。

connect和bindActionCreators来自redux。前者用于连接store,后者用于将action creators 绑定到你的props。

##### 16.什么是React Router Dom 及其工作原理

react-router-dom 是应用程序中路由的库。React库中没有路由功能，需要单独安装react-router-dom。

react-router-dom 提供两个路由器BrowserRouter和HashRouter。前者基于rul的pathname段，后者基于hash段。

###### react-router-dom组件

BrowserRouter和HashRouter 是路由器。

Route用于路由匹配。

Link组件用于在应用程序中创建链接。它将在HTML中渲染为锚标记。

NavLink是突出显示当前活动链接的特殊链接。

Switch不是必需的，但在组合路由时很有用。

Redirect用于强制路由重定向。

##### 17.什么是错误边界（ErrorBoundary）

在React中，我们通常有一个组件树。如果任何一个组件发生错误，它将破坏整个组件树。没有办法捕捉这些错误，我们可以用错误边界优雅地处理这些错误。

错误边界有两个作用：

  1)如果发生错误，显示回退UI

  2)记录错误

##### 18.什么是Fragments

在React中，我们需要有一个父元素，同时从组件返回React元素。有时在DOM中添加额外的节点会很烦人。使用Fragments，我们不需要在DOM中添加额外的节点。我们只需要用React.Fragment或简写<>来包裹内容就行了。

##### 19.什么是传送门（Portals）

默认情况下，所有子组件都在UI上呈现，具体取决于组件层次结构。Portal提供了一种将子节点渲染到存在于父组件以外的DOM节点的优秀的方案。

##### 20.什么是上下文

有时我们必须将props传递给组件树，即使所有中间组件都不需要这些props。上下文是一种传递props的方法，而不用再每一层传递组件树。

##### 21.什么是Hooks

Hooks是React版本16.8中的新功能。请记住，我们不能在函数组件中使用state,因为它们不是类组件。Hooks让我们在函数组件中可以使用state和其他功能。

下面是Hooks的基本规则：

  1）Hooks应该在外层使用，不应该在循环，条件或嵌套函数中使用

  2）Hooks应该只在函数组件中使用。

##### 22.如何提高性能

  1）适当地使用shouldComponentUpdate生命周期方法。它避免了子组件的不必要的渲染。如果树中有100个组件，则不重新渲染整个组件树来提高应用程序性能。

 2）使用create-react-app 来构建项目，这会创建整个项目结构，并进行大量优化。

  3）不可变性是提高性能的关键。不要对数据进行修改，而是始终在现有集合的基础上创建新的集合，以保持尽可能少的复制，从而提高性能。

  4）在显示列表或表格时始终使用keys,这会让React的更新速度更快。

  5）代码分离是将代码插入到单独的文件中，只加载模块或部分所需的文件的 技术。

##### 23.如何在重新加载页面时保留数据

单页应用程序首先在DOM中加载index.html，然后在用户浏览页面时加载内容，或者从同一index.html中的后端API获取任何数据。

如果通过点击浏览器中的重新加载按钮重新加载页面index.html，整个React应用程序将重新加载，我们将丢失应用程序的状态。如何保留应用状态？

每当重新加载应用程序时，我们使用浏览器locaStorage来保存应用程序的状态。我们将整个存储数据保存在localStorage中，每当有页面刷新或重新加载时，我们从localStorage加载状态。

##### 24.如何在React进行API调用

我们使用redux-thunk在React中调用API。因为reduce是纯函数，所以没有副作用。

因此，我们必须使用redux-thunk从action creators 那里进行API调用。Action creator派发一个action,将来自API的数据放入action的payload中。

redux-thunk 是一个中间件。一旦它被引入到项目中，每次派发一个action时，都会通过thunk传递。如果它是一个函数，它只是等待函数处理并返回响应。如果它不是一个函数，它只是正常处理。

##### 25.为什么选择使用框架而不是原生？

框架的好处：

1.组件化：其中以React的组件化最为彻底，甚至可以到函数级别的原子组件，高度的组件化可以使我们的工程易于维护、易于组合扩展。

2.天然分层：jQuery时代的代码大部分情况下是面条代码，耦合严重，现代框架不管是MVC、MVP还是MVVM模式都能帮助我们进行分层，代码解耦更易于读写。

3.生态：现在主流前端框架都自带生态，不管是数据流管理架构还是UI库都有成熟的解决方案。

4.开发效率：现代前端框架都默认自动更新DOM，而非我们手动操作，解放了开发者的手动DOM成本，提高开发效率，从根本上解决了UI与状态同步问题。

##### 26.虚拟DOM的优劣如何？

优点：

- 保证性能下限：虚拟DOM可以经过diff找出最小差异，然后批量进行patch，这种操作虽然比不上手动优化，但是比起粗暴的DOM操作性能要好很多，因此虚拟DOM可以保证性能下限
- 无需手动操作DOM:虚拟DOM的diff和patch都是在一次更新中自动进行的，我们无需手动操作DOM，极大提高开发效率
- 跨平台：虚拟DOM本质上是JavaScript对象，而DOM与平台强相关，相比之下虚拟DOM可以进行更方便地跨平台操作，例如服务器渲染、移动端开发等等

缺点：

- 无法进行极致优化：在一些性能要求极高的应用中虚拟DOM无法进行针对性的极致优化，比如VScode采用直接手动操作DOM的方式进行极端的性能优化

##### 27.虚拟DOM实现原理？

- 虚拟DOM本质上是JavaScript对象，是对真实DOM的抽象
- 状态变更时，记录新树和旧树的差异
- 最后把差异更新到真正的dom中

##### 28.React最新的生命周期是怎样的?

React 16之后有三个生命周期被废弃(但并未删除)

- componentWillMount
- componentWillReceiveProps
- componentWillUpdate

官方计划在17版本完全删除这三个函数，只保留UNSAVE_前缀的三个函数，目的是为了向下兼容，但是对于开发者而言应该尽量避免使用他们，而是使用新增的生命周期函数替代它们

目前React 16.8 +的生命周期分为三个阶段,分别是挂载阶段、更新阶段、卸载阶段

挂载阶段:

- constructor: 构造函数，最先被执行,我们通常在构造函数里初始化state对象或者给自定义方法绑定this
- getDerivedStateFromProps: `static getDerivedStateFromProps(nextProps, prevState)`,这是个静态方法,当我们接收到新的属性想去修改我们state，可以使用getDerivedStateFromProps
- render: render函数是纯函数，只返回需要渲染的东西，不应该包含其它的业务逻辑,可以返回原生的DOM、React组件、Fragment、Portals、字符串和数字、Boolean和null等内容
- componentDidMount: 组件装载之后调用，此时我们可以获取到DOM节点并操作，比如对canvas，svg的操作，服务器请求，订阅都可以写在这个里面，但是记得在componentWillUnmount中取消订阅

更新阶段:

- getDerivedStateFromProps: 此方法在更新个挂载阶段都可能会调用
- shouldComponentUpdate: `shouldComponentUpdate(nextProps, nextState)`,有两个参数nextProps和nextState，表示新的属性和变化之后的state，返回一个布尔值，true表示会触发重新渲染，false表示不会触发重新渲染，默认返回true,我们通常利用此生命周期来优化React程序性能
- render: 更新阶段也会触发此生命周期
- getSnapshotBeforeUpdate: `getSnapshotBeforeUpdate(prevProps, prevState)`,这个方法在render之后，componentDidUpdate之前调用，有两个参数prevProps和prevState，表示之前的属性和之前的state，这个函数有一个返回值，会作为第三个参数传给componentDidUpdate，如果你不想要返回值，可以返回null，此生命周期必须与componentDidUpdate搭配使用
- componentDidUpdate: `componentDidUpdate(prevProps, prevState, snapshot)`,该方法在getSnapshotBeforeUpdate方法之后被调用，有三个参数prevProps，prevState，snapshot，表示之前的props，之前的state，和snapshot。第三个参数是getSnapshotBeforeUpdate返回的,如果触发某些回调函数时需要用到 DOM 元素的状态，则将对比或计算的过程迁移至 getSnapshotBeforeUpdate，然后在 componentDidUpdate 中统一触发回调或更新状态。

卸载阶段:

- componentWillUnmount: 当我们的组件被卸载或者销毁了就会调用，我们可以在这个函数里去清除一些定时器，取消网络请求，清理无效的DOM元素等垃圾清理工作

![img](https://user-gold-cdn.xitu.io/2019/8/23/16cbc24e71728047?imageslim)

##### 29.React的请求应该放在哪个生命周期中?

React的异步请求到底应该放在哪个生命周期里,有人认为在`componentWillMount`中可以提前进行异步请求,避免白屏,其实这个观点是有问题的.

由于JavaScript中异步事件的性质，当您启动API调用时，浏览器会在此期间返回执行其他工作。当React渲染一个组件时，它不会等待componentWillMount它完成任何事情 - React继续前进并继续render,没有办法“暂停”渲染以等待数据到达。

而且在`componentWillMount`请求会有一系列潜在的问题,首先,在服务器渲染时,如果在 componentWillMount 里获取数据，fetch data会执行两次，一次在服务端一次在客户端，这造成了多余的请求,其次,在React 16进行React Fiber重写后,`componentWillMount`可能在一次渲染中多次调用.

目前官方推荐的异步请求是在`componentDidmount`中进行.

如果有特殊需求需要提前请求,也可以在特殊情况下在`constructor`中请求:

react 17之后`componentWillMount`会被废弃,仅仅保留`UNSAFE_componentWillMount`

##### 30.setState到底是异步还是同步?

先给出答案: 有时表现出异步,有时表现出同步

1. setState只在合成事件和钩子函数中是“异步”的，在原生事件和setTimeout 中都是同步的。
2. setState 的“异步”并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形成了所谓的“异步”，当然可以通过第二个参数 setState(partialState, callback) 中的callback拿到更新后的结果。
3. setState 的批量更新优化也是建立在“异步”（合成事件、钩子函数）之上的，在原生事件和setTimeout 中不会批量更新，在“异步”中如果对同一个值进行多次setState，setState的批量更新策略会对其进行覆盖，取最后一次的执行，如果是同时setState多个不同的值，在更新时会对其进行合并批量更新。

##### 31.React组件通信如何实现？

React组件间通信方式：

父组件向子组件通讯：父组件可以向子组件通过传props的方式，向子组件进行通讯

子组件向父组件通讯：props+回调的方式，父组件向子组件传递props进行通讯，此props为作用域为父组件自身的函数，子组件调用该函数，将子组件想要传递的信息作为参数，传递到父组件的作用域中

React中当state发生改变时，组件才会update。在父组件中设定state的初始值以及处理该state的函数，同时将函数名通过以props属性值的形式传入子组件，子组件通过调用父组件的函数，进而引起state变化，达到在父组件中展示子组件产生的变化。

跨层级通信：Context设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言，对于跨越多层的全局数据通过Context通信再适合不过

发布订阅模式：发布者发布事件，订阅者监听事件并做出反应，我们可以通过引入event模块进行通信

全局状态管理工具：借助Redux或者Mobx等全局状态管理工具进行通信，这种工具会维护一个全局状态中心Store，并根据不同的事件产生新的状态

32.React如何进行组件/逻辑复用？

抛开已经被官方弃用的Mixin，组件抽象的技术目前有三种比较主流：

- 高阶组件  属性代理、反向继承
- 渲染属性
- react-hooks

##### 33.你是如何理解fiber的？

React Fiber 是一种基于浏览器的单线程调度算法。

React16之前，reconcilation算法实际上是递归，想要中断递归是很困难的，React16开始使用了循环来代替之前的递归。

Fiber:一种将reconcilation(递归diff)，拆分成无数个小任务的算法；它随时能够停止，恢复。停止恢复的时机取决于当前的一帧（16ms）内，还有没有足够的时间允许计算。

##### 34.redux的工作流程?

首先，我们看下几个核心概念：

- Store：保存数据的地方，你可以把它看成一个容器，整个应用只能有一个Store。
- State：Store对象包含所有数据，如果想得到某个时点的数据，就要对Store生成快照，这种时点的数据集合，就叫做State。
- Action：State的变化，会导致View的变化。但是，用户接触不到State，只能接触到View。所以，State的变化必须是View导致的。Action就是View发出的通知，表示State应该要发生变化了。
- Action Creator：View要发送多少种消息，就会有多少种Action。如果都手写，会很麻烦，所以我们定义一个函数来生成Action，这个函数就叫Action Creator。
- Reducer：Store收到Action以后，必须给出一个新的State，这样View才会发生变化。这种State的计算过程就叫做Reducer。Reducer是一个函数，它接受Action和当前State作为参数，返回一个新的State。
- dispatch：是View发出Action的唯一方法。

然后我们过下整个工作流程：

1. 首先，用户（通过View）发出Action，发出方式就用到了dispatch方法。
2. 然后，Store自动调用Reducer，并且传入两个参数：当前State和收到的Action，Reducer会返回新的State
3. State一旦有变化，Store就会调用监听函数，来更新View。

到这儿为止，一次用户交互流程结束。可以看到，在整个流程中数据都是单向流动的，这种方式保证了流程的清晰。

![img](https://user-gold-cdn.xitu.io/2019/8/23/16cbc24efade2de0?imageslim)

##### 35.react-redux是如何工作的?

Provider: Provider的作用是从最外部封装了整个应用，并向connect模块传递store

connect: 负责连接React和Redux 

- 获取state: connect通过context获取Provider中的store，通过store.getState()获取整个store tree 上所有state
- 包装原组件: 将state和action通过props的方式传入到原组件内部wrapWithConnect返回一个ReactComponent对象Connect，Connect重新render外部传入的原组件WrappedComponent，并把connect中传入的mapStateToProps, mapDispatchToProps与组件上原有的props合并后，通过属性的方式传给WrappedComponent
- 监听store tree变化: connect缓存了store tree中state的状态,通过当前state状态和变更前state状态进行比较,从而确定是否调用`this.setState()`方法触发Connect及其子组件的重新渲染

![img](https://user-gold-cdn.xitu.io/2019/8/23/16cbc24efb408781?imageslim)

##### 36.redux与mobx的区别?

**两者对比:**

- redux将数据保存在单一的store中，mobx将数据保存在分散的多个store中
- redux使用plain object保存数据，需要手动处理变化后的操作；mobx适用observable保存数据，数据变化后自动处理响应的操作
- redux使用不可变状态，这意味着状态是只读的，不能直接去修改它，而是应该返回一个新的状态，同时使用纯函数；mobx中的状态是可变的，可以直接对其进行修改
- mobx相对来说比较简单，在其中有很多的抽象，mobx更多的使用面向对象的编程思维；redux会比较复杂，因为其中的函数式编程思想掌握起来不是那么容易，同时需要借助一系列的中间件来处理异步和副作用
- mobx中有更多的抽象和封装，调试会比较困难，同时结果也难以预测；而redux提供能够进行时间回溯的开发工具，同时其纯函数以及更少的抽象，让调试变得更加的容易

**场景辨析:**

基于以上区别,我们可以简单得分析一下两者的不同使用场景.

mobx更适合数据不复杂的应用: mobx难以调试,很多状态无法回溯,面对复杂度高的应用时,往往力不从心.

redux适合有回溯需求的应用: 比如一个画板应用、一个表格应用，很多时候需要撤销、重做等操作，由于redux不可变的特性，天然支持这些操作.

mobx适合短平快的项目: mobx上手简单,样板代码少,可以很大程度上提高开发效率.

当然mobx和redux也并不一定是非此即彼的关系,你也可以在项目中用redux作为全局状态管理,用mobx作为组件局部状态管理器来用.

##### 37.redux中如何进行异步操作?

当然,我们可以在`componentDidmount`中直接进行请求无须借助redux.

但是在一定规模的项目中,上述方法很难进行异步流的管理,通常情况下我们会借助redux的异步中间件进行异步处理.

redux异步流中间件其实有很多,但是当下主流的异步中间件只有两种redux-thunk、redux-saga，当然redux-observable可能也有资格占据一席之地,其余的异步中间件不管是社区活跃度还是npm下载量都比较差了.

##### 38.redux异步中间件之间的优劣？

###### redux-thunk优点：

- 体积小：redux-thunk的实现方式很简单，只有不到20行代码
- 使用简单：redux-thunk没哟引入像redux-saga或者redux-observable额外的范式，上手简单

###### redux-thunk缺点：

- 样板代码过多：与redux本身一样，通常一个请求需要大量的代码，而且很多都是重复性质的
- 耦合严重：异步操作与redux的action耦合在一起，不方便管理
- 功能孱弱：有一些实际开发中常用的功能需要自己进行封装

###### **redux-saga优点:**

- 异步解耦: 异步操作被被转移到单独 saga.js 中，不再是掺杂在 action.js 或 component.js 中
- action摆脱thunk function: dispatch 的参数依然是一个纯粹的 action (FSA)，而不是充满 “黑魔法” thunk function
- 异常处理: 受益于 generator function 的 saga 实现，代码异常/请求失败 都可以直接通过 try/catch 语法直接捕获处理
- 功能强大: redux-saga提供了大量的Saga 辅助函数和Effect 创建器供开发者使用,开发者无须封装或者简单封装即可使用
- 灵活: redux-saga可以将多个Saga可以串行/并行组合起来,形成一个非常实用的异步flow
- 易测试，提供了各种case的测试方案，包括mock task，分支覆盖等等

###### **redux-saga缺陷:**

- 额外的学习成本: redux-saga不仅在使用难以理解的 generator function,而且有数十个API,学习成本远超redux-thunk,最重要的是你的额外学习成本是只服务于这个库的,与redux-observable不同,redux-observable虽然也有额外学习成本但是背后是rxjs和一整套思想
- 体积庞大: 体积略大,代码近2000行，min版25KB左右
- 功能过剩: 实际上并发控制等功能很难用到,但是我们依然需要引入这些代码
- ts支持不友好: yield无法返回TS类型

###### **redux-observable优点:**

- 功能最强: 由于背靠rxjs这个强大的响应式编程的库,借助rxjs的操作符,你可以几乎做任何你能想到的异步处理
- 背靠rxjs: 由于有rxjs的加持,如果你已经学习了rxjs,redux-observable的学习成本并不高,而且随着rxjs的升级redux-observable也会变得更强大

###### **redux-observable缺陷:**

- 学习成本奇高: 如果你不会rxjs,则需要额外学习两个复杂的库
- 社区一般: redux-observable的下载量只有redux-saga的1/5,社区也不够活跃,在复杂异步流中间件这个层面redux-saga仍处于领导地位





