##### Vue面试点1

###### 1.

##### Vue

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。

###### 声明式渲染

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统。现在数据和 DOM 已经被建立了关联，所有东西都是**响应式的**。

`v-bind` attribute 被称为**指令**。指令带有前缀 `v-`，以表示它们是 Vue 提供的特殊 attribute。可能你已经猜到了，它们会在渲染的 DOM 上应用特殊的响应式行为。

`v-for` 指令可以绑定数组的数据来渲染一个项目列表

###### 处理用户输入

为了让用户和你的应用进行交互，我们可以用 `v-on` 指令添加一个事件监听器，通过它调用在 Vue 实例中定义的方法。

Vue 还提供了 `v-model` 指令，它能轻松实现表单输入和应用状态之间的双向绑定。

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。

###### 数据与方法

当一个 Vue 实例被创建时，它将 `data` 对象中的所有的 property 加入到 Vue 的**响应式系统**中。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

###### 实例生命周期钩子

![Vue 实例生命周期](https://cn.vuejs.org/images/lifecycle.png)

###### 模板语法

Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值

```
<span>Message: {{ msg }}</span>
```

Mustache 语法不能作用在 HTML attribute 上，遇到这种情况应该使用 [`v-bind` 指令](https://cn.vuejs.org/v2/api/#v-bind)

```
<div v-bind:id="dynamicId"></div>
```

###### 指令

指令 (Directives) 是带有 `v-` 前缀的特殊 attribute。指令 attribute 的值预期是**单个 JavaScript 表达式** (`v-for` 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，`v-bind` 指令可以用于响应式地更新 HTML attribute：

```
<a v-bind:href="url">...</a>
```

在这里href是参数，告知v-bind指令将该元素的href attribute与表达式url的值绑定

###### 修饰符

修饰符 (modifier) 是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：

```
<form v-on:submit.prevent='onSubmit'>...</form>
```

###### 缩写

v-bind缩写

```
<a v-bind:href='url'>...</a>
<a :href='url'>...</a>
动态参数的缩写
<a :[key]='url'>...</a>
```

v-on缩写

```
<a v-on:click='dosomething'></a>
<a @click='dosomething'></a>
动态参数的缩写
<a @[event]='dosomething'></a>
```

###### 计算属性

对于任何复杂逻辑，你都应当使用计算属性。computed

###### 计算属性缓存VS方法

可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。

###### 条件渲染

`v-if` 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 truthy 值的时候被渲染。

`v-else` 元素必须紧跟在带 `v-if` 或者 `v-else-if` 的元素的后面，否则它将不会被识别。

`v-else-if`，顾名思义，充当 `v-if` 的“else-if 块”，可以连续使用

另一个用于根据条件展示元素的选项是 `v-show` 指令。

不同的是带有 `v-show` 的元素始终会被渲染并保留在 DOM 中。`v-show` 只是简单地切换元素的 CSS property `display`。

###### v-if vs v-show

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。

###### 用key管理可复用的元素

###### v-if 与v-for一起使用

不推荐同时使用v-if 和v-for。当v-if与v-for一起使用时，v-for具有比v-if更高的优先级。这意味着 `v-if` 将分别重复运行于每个 `v-for` 循环中。

###### v-for

```
<li v-for="(item,index) in items" v-bind:key='index'></li>
```

为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 `key` attribute

###### 事件处理

可以用 `v-on` 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。在 `methods` 对象中定义方法

###### 为什么在HTML中监听事件？

你可能注意到这种事件监听的方式违背了关注点分离 (separation of concern) 这个长期以来的优良传统。但不必担心，因为所有的 Vue.js 事件处理方法和表达式都严格绑定在当前视图的 ViewModel 上，它不会导致任何维护上的困难。实际上，使用 `v-on` 有几个好处：

1. 扫一眼 HTML 模板便能轻松定位在 JavaScript 代码里对应的方法。
2. 因为你无须在 JavaScript 里手动绑定事件，你的 ViewModel 代码可以是非常纯粹的逻辑，和 DOM 完全解耦，更易于测试。
3. 当一个 ViewModel 被销毁时，所有的事件处理器都会自动被删除。你无须担心如何清理它们。

###### 表单输入绑定

你可以用 `v-model` 指令在表单 < input>、< textarea> 及 < select>元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 `v-model` 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 `data` 选项中声明初始值。

##### 组件基础

###### data必须是一个函数

一个组件的data选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝。

```
data:function(){
	return {
		count: 0
	}
}
```

###### 组件的组织

组件的注册类型：**全局注册**和**局部注册**。至此，我们的组件都只是通过 `Vue.component` 全局注册的。

全局注册的组件可以用在其被注册之后的任何 (通过 `new Vue`) 新创建的 Vue 根实例，也包括其组件树中的所有子组件的模板中。

###### 通过prop向子组件传递数据

###### 单个根元素 每个组件必须只有一个根元素

###### 单向数据流

所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定**：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外变更父级组件的状态，从而导致你的应用的数据流向难以理解。

额外的，每次父级组件发生变更时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你**不**应该在一个子组件内部改变 prop。如果你这样做了，Vue 会在浏览器的控制台中发出警告。

父组件向子组件传递数据，可以通过prop属性向下传递数据，子组件向父组件传递数据是通过事件给父组件发送消息。

非父子组件之间传值，需要定义个公共的实列文件来作为中间仓库来传值的。比如定义一个叫 bus.js 文件。
所谓中间仓库就是创建一个事件中心，相当于中转站，可以使用它来传递事件和接收事件的。

```
import Vue from 'vue';
import VueRouter from 'vue-router';

// 告诉 vue 使用 vueRouter
Vue.use(VueRouter);
```

###### VUE字符串模板和非字符串模板的区别

字符串模板：指的是在组件选项里用template:''指定的模板，换句话说，写在js中的template:''中的就是字符串模板。比如下面这个：

```
var tmp= new Vue({
	template:''
})
```

非字符串模板：在单文件里用指定的模板，换句话说，写在html中的就是非字符串模板。

HTML 模板：应该指的是原生HTML，通过 `el` 挂载到 Vue 实例上。

首先，Vue 会将 template 中的内容插到 DOM 中，以方便解析标签。由于 HTML 标签不区分大小写，所以在生成的标签名都会转换为小写。例如，当你的 template 为 < MyComponent>< /MyComponent> 时，插入 DOM 后会被转换为 < mycomponent>< /mycomponent>。

然后，通过标签名寻找对应的自定义组件。**匹配的优先顺序从高到低为：原标签名、camelCase化的标签名、PascalCase化的标签名。**例如 < my-component> 会依次匹配 my-component、myComponent、MyComponent。



###### 注意：组件名大小写

注意：当直接在DOM中使用一个组件（而不是字符串模板或单文件组件）的时候，我们强烈推荐遵循W3C规范中的自定义组件名（字母全小写且必须包含一个连字符）。这会帮助你避免和当前以及未来的HTML元素相冲突。

（1）使用kebab-case:

```
Vue.component('my-component-name',{});
```

当使用kebab-case(短横线分隔命名)定义一个组件时，你也必须在引用这个自定义元素时使用kebad-case，例如<my-component-case>。

（2）使用PascalCase:

```
Vue.component('MyComponentName',{})
```

当使用PascalCase(驼峰式命名)定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说< my-component-name>和< MyComponentName >都是可以接受的。注意，尽管如此，直接在DOM（即非字符串的模板，如在单个组件的< template></ template>中或者index.html中直接CDN引入vue.js的< div id='app'>< /div>，使用驼峰式命名或短横线分隔命名都可以）使用时只有kebad-case是有效的，使用驼峰式，是不会渲染的。



**1. 什么是vuex？**

Vuex官网的解释：Vuex是一个专为vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

与其他模式不同的是，Vuex 是专门为 Vue.js 设计的状态管理库，以利用 Vue.js 的细粒度数据响应机制来进行高效的状态更新。

每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。Vuex 和单纯的全局对象有以下两点不同：

1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
2. 你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地**提交 (commit) mutation**。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

##### state 单一状态树

Vuex 使用**单一状态树**——是的，用一个对象就包含了全部的应用层级状态。至此它便作为一个“唯一数据源 ([SSOT](https://en.wikipedia.org/wiki/Single_source_of_truth))”而存在。这也意味着，每个应用将仅仅包含一个 store 实例。单一状态树让我们能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照。

######  在 Vue 组件中获得 Vuex 状态

由于 Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在[计算属性](https://cn.vuejs.org/guide/computed.html)中返回某个状态。

Vuex通过store选项，提供了一种机制将状态从根组件“注入”到每一个子组件中（需要调用Vue.use(vuex)）

```
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```

通过在根实例中注册 `store` 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过 `this.$store` 访问到。

```
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

###### `mapState` 辅助函数

当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性，让你少按几次键：

######  对象展开运算符

`mapState` 函数返回的是一个对象。我们如何将它与局部计算属性混合使用呢？通常，我们需要使用一个工具函数将多个对象合并为一个，以使我们可以将最终对象传给 `computed` 属性。但是自从有了[对象展开运算符](https://github.com/tc39/proposal-object-rest-spread)，我们可以极大地简化写法

```
computed: {
  localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}
```

###### 组件仍然保有局部状态

使用 Vuex 并不意味着你需要将**所有的**状态放入 Vuex。虽然将所有的状态放到 Vuex 会使状态变化更显式和易调试，但也会使代码变得冗长和不直观。如果有些状态严格属于单个组件，最好还是作为组件的局部状态。你应该根据你的应用开发需要进行权衡和确定。

##### Getter

有时候我们需要从 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数：

```
computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

如果有多个组件需要用到此属性，我们要么复制这个函数，或者抽取到一个共享函数然后在多处导入它——无论哪种方式都不是很理想。

Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

Getter 接受 state 作为其第一个参数：

```
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
}
```

###### `mapGetters` 辅助函数

`mapGetters` 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性

##### Mutation

更改Vuex的store中的状态的唯一方法是提交mutation。Vuex中的mutation非常类似于事件：每个mutation都有一个字符串的事件类型（type）和一个回调函数（handler）。这个回调函数就是我们实际进行状态更改的地方，并且它会接受state作为第一个参数。

```
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

###### Mutation必须是同步函数

###### Mutation需遵守Vue的响应规则

既然 Vuex 的 store 中的状态是响应式的，那么当我们变更状态时，监视状态的 Vue 组件也会自动更新。这也意味着 Vuex 中的 mutation 也需要与使用 Vue 一样遵守一些注意事项：

1. 最好提前在你的 store 中初始化好所有所需属性。
2. 当需要在对象上添加新属性时，你应该

- 使用 `Vue.set(obj, 'newProp', 123)`, 或者

- 以新对象替换老对象。例如，利用[对象展开运算符](https://github.com/tc39/proposal-object-rest-spread)我们可以这样写：

  ```js
  state.obj = { ...state.obj, newProp: 123 }
  ```

在 Vuex 中，**mutation 都是同步事务**

##### Action

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters。当我们在之后介绍到 [Modules](https://vuex.vuejs.org/zh/guide/modules.html) 时，你就知道 context 对象为什么不是 store 实例本身了。

实践中，我们会经常用到 ES2015 的 [参数解构](https://github.com/lukehoban/es6features#destructuring) 来简化代码（特别是我们需要调用 `commit` 很多次的时候）：

```
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

###### 分发Action

Action通过store.dispatch方法触发：

```
store.dispatch('increment');
```

Actions支持同样的载荷方式和对象方式进行分发：

```
//以载荷形式分发
store.dispatch('incrementAsync',{
	amount:10
})
//以对象形式分发
store.dispatch({
	type:'incrementAsync',
	amount:10
})
```

###### 在组件中分发Action

你在组件中使用this.$store.dispatch('xxx')分发action，或者使用mapActions辅助函数将组件的methods映射为store.dispatch调用（需要先在根节点注入store）

```
import {mapActions} from 'vuex'

export default{
//...
 methods: {
 	...mapActions([
 		'increment', //将this.increment()映射为this.$store.dispatch('increment')
 		//mapActions也支持载荷
 		'incrementBy' 
 		//将this.incrementBy(amount)映射为this.$store.dispatch('incrementBy',amount)
 	]),
 	...mapActions({
 		add: 'increment' //将this.add()映射为this.$store.dispatch('increment')
 	})
 }
}
```

###### 组合Action

首先，你需要明白 store.dispatch 可以处理被触发的action的处理函数返回的Promise，并且store.dispatch仍旧返回Promise。

最后，利用async/await ，可以如下组合action

```
actions: {
	async actionA({commit}){
		commit('getData', await getData())
	},
	async actionB({dispatch, commit}){
		await dispatch(actionA) //等待actionA完成
		commit('getOtherData', await getOtherData())
	}
}
```

一个store.dispatch在不同模块中可以触发多个action函数。在这种情况下，只有当所有触发函数完成后，返回的Promise才会执行。

##### Module

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割

###### 命名空间

默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的——这样使得多个模块能够对同一 mutation 或 action 作出响应。

如果希望你的模块具有更高的封装度和复用性，你可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名。

启用了命名空间的 getter 和 action 会收到局部化的 `getter`，`dispatch` 和 `commit`。换言之，你在使用模块内容（module assets）时不需要在同一模块内额外添加空间名前缀。更改 `namespaced` 属性后不需要修改模块内的代码。

Vuex 并不限制你的代码结构。但是，它规定了一些需要遵守的规则：

1. 应用层级的状态应该集中到单个 store 对象中。
2. 提交 **mutation** 是更改状态的唯一方法，并且这个过程是同步的。
3. 异步逻辑都应该封装到 **action** 里面。

##### 严格模式

开启严格模式，仅需在创建 store 的时候传入 `strict: true`

在严格模式下，无论何时发生了状态变更且不是由 mutation 函数引起的，将会抛出错误。这能保证所有的状态变更都能被调试工具跟踪到。

**不要在发布环境下启用严格模式**！



官网的解释很模糊。其实在vue组件开发中，经常会需要将当前的组件的数据传递给其他的组件，父子组件通信的话，我们可以采用props+emit 这种方式，如上面的demo方式来传递数据，但是当通信双方不是父子组件甚至根本不存在任何关系的时候，或者说一个状态需要共享给多个组件的时候，那么就会非常麻烦，数据维护也相当的不好维护，因此就出现了vuex。它能帮助我们把公用的状态抽出来放在vuex的容器中，然后根据一定的规则进行管理。vuex采用了集中式存储管理所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

```
<script type="text/javascript">
    Vue.use(Vuex); // 使用vuex
    var myStore = new Vuex.Store({
      // state是存储状态 定义应用状态全局的数据结构
      state: {
        name: 'kongzhi',
        todoLists: []
      },
      /*
        mutations是提交状态修改，也就是set、get中的set，这是vuex中唯一修改state的方式，但是不支持异步操作。
        每个mutation都有一个字符串的事件类型(type)和一个回调函数(handler)
        第一个参数默认是state，外部调用方式为：store.commit('SET_AGE', 30).
      */
      mutations: {
        // 新增list
        ADDLIST(state, item) {
          state.todoLists.push(item);
        },
        // 删除list中的项
        DELLIST(state, index) {
          state.todoLists.splice(index, 1);
        },
        // 设置 错误提示信息
        SETERROR(state, msg) {
          state.message = msg;
        }
      },
      /*
        getters是从state中派生出状态的。也就是set、get中的get，它有两个可选的参数，state和getters，
        分别可以获取state中的变量和其他的getters。外部调用的方式：store.getters.todoCount()
      */
      getters: {
        todoCount(state) {
          return state.todoLists.length;
        }
      },
      /*
       和上面的mutations类似，但是actions支持异步操作的，外部调用方式为：store.dispatch('nameAction')
       常见的使用是：从服务器端获取数据，在数据获取完成后会调用 store.commit()来更改Store中的状态。
       Action函数接收一个与store实列具有相同方法和属性的context对象，因此我们可以使用 context.commit 提交一个
       mutation，或者通过 context.state 和 context.getters来获取state和getters
      */
      actions: {
        addList(context, item) {
          if (item) {
            context.commit('ADDLIST', item);
            context.commit('SETERROR', '');
          } else {
            context.commit('SETERROR', '添加失败');
          }
        },
        delList(context, index) {
          context.commit('DELLIST', index);
          context.commit('SETERROR', '删除成功');
        }
      },
      /*
       modules 对象允许将单一的Store拆分为多个Store的同时保存在单一的状态树中。
      */
      modules: {

      }
    });
    new Vue({
      el: '#app',
      data: {
        name: 'init name'
      },
      store: myStore,
      mounted: function() {
        console.log(this);
      }
    })
  </script>
</body>
</html>
```

new Vuex.store({}) 含义是创建一个Vuex实列。store是vuex的一个核心方法，字面含义为 '仓库'的意思。实列化完成后，需要注入到vue实列中，它有五个核心的选项，state、mutations、getters、actions和modules。

 **vuex中如何获取state的数据呢？**

在组件内部中的computed来获取state的数据(computed是实时响应的)。

**如何对state的数据进行筛选和过滤**

有时候，我们需要对state的数据进行刷选和过滤操作，比如后台请求回来的数据，我们需要进行数据过滤操作，getters就可以了。

 **mutations操作来改变state数据**

如上是如何获取state的数据了，那么现在我们使用mutations来改变state的数据了，在Vuex中，改变状态state的唯一方式是通过提交commit的一个mutations，mutations下的对应函数接收第一个参数state，第二个参数为payload(载荷)，payload一般是一个对象，用来记录开发时使用该函数的一些信息。mutations是处理同步的请求。不能处理异步请求的。

**actions操作mutations异步来改变state数据**
actions可以异步操作，actions提交mutations，通过mutations来提交数据的变更。它是异步修改state的状态的。
外部调用方式是 this.$store.dispatch('nameAsyn');

**理解context:** context是和 this.$store 具有相同的方法和属性的对象。我们可以通过 context.state 和 context.getters来获取state和getters。

**理解dispatch:** 它含有异步操作，含义可以理解为 '派发'，比如向后台提交数据，可以为 this.$store.dispatch('actions方法名', 值);

![image-20200616150049837](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20200616150049837.png)

组件派发任务到actions，actions触发mutations中的方法，然后mutations来改变state中的数据，数据变更后响应推送给组件，组件重新渲染。

```
export default {
     namespaced:true,//用于在全局引用此文件里的方法时标识这一个的文件名
     state,
     getters,
     mutations,
     actions
}
```

```
computed:{
    ...mapState({  //这里的...是超引用，ES6的语法，意思是state里有多少属性值我可以在这里放多少属性值
         isShow:state=>state.footerStatus.showFooter 
         //注意这些与上面的区别就是state.footerStatus,
         //里面定义的showFooter是指footerStatus.js里state的showFooter
      }),
methods:{
      ...mapActions('collection',[ //collection是指modules文件夹下的collection.js
          'invokePushItems'  
          //collection.js文件中的actions里的方法，在上面的@click中执行并传入实参
      ])
  }
```

//mand-mobile 、vue-router 、vuex

##### Vue Router

Vue Router 是 [Vue.js](http://cn.vuejs.org/) 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。包含的功能有：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于 Vue.js 过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的 CSS class 的链接
- HTML5 历史模式或 hash 模式，在 IE9 中自动降级
- 自定义的滚动条行为

当你要把 Vue Router 添加进来，我们需要做的是，将组件 (components) 映射到路由 (routes)，然后告诉 Vue Router 在哪里渲染它们。

##### 动态路由匹配

动态路径参数

一个“路径参数”使用冒号 `:` 标记。当匹配到一个路由时，参数值会被设置到 `this.$route.params`，可以在每个组件内使用。

##### 嵌套路由

需要在 `VueRouter` 的参数中使用 `children` 配置

**要注意，以 `/` 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。**

##### 编程式的导航

**注意：在 Vue 实例内部，你可以通过 `$router` 访问路由实例。因此你可以调用 `this.$router.push`。**

想要导航到不同的 URL，则使用 `router.push` 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

当你点击 <route-link>`` 时，这个方法会在内部调用，所以说，点击 `` <router-link :to="...">等同于调用 `router.push(...)`。

| 声明式                  | 编程式           |
| ----------------------- | ---------------- |
| <router-link :to="..."> | router.push(...) |

###### `router.replace(location, onComplete?, onAbort?)`

跟 `router.push` 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录。

| 声明式                          | 编程式                |
| ------------------------------- | --------------------- |
| <router-link :to="..." replace> | `router.replace(...)` |

###### [#](https://router.vuejs.org/zh/guide/essentials/navigation.html#router-go-n)`router.go(n)`

这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 `window.history.go(n)`。

例子

```js
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
router.go(100)
```

###### 操作 History

你也许注意到 `router.push`、 `router.replace` 和 `router.go` 跟 [`window.history.pushState`、 `window.history.replaceState` 和 `window.history.go`](https://developer.mozilla.org/en-US/docs/Web/API/History)好像， 实际上它们确实是效仿 `window.history` API 的。

##### 命名路由

有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。你可以在创建 Router 实例的时候，在 `routes` 配置中给某个路由设置名称。

```
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```

要链接到一个命名路由，可以给 `router-link` 的 `to` 属性传一个对象

```
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```

##### 命名视图

有时候想同时 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有 `sidebar` (侧导航) 和 `main` (主内容) 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 `router-view` 没有设置名字，那么默认为 `default`。

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 `components` 配置 (带上 s)：

```
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

##### 重定向和别名

重定向也是通过routes配置来完成。

```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```

重定向的目标也可以是一个命名的路由

```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})
```

甚至 是一个方法，动态返回重定向目标

```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]
})
```

###### 别名

“重定向”的意思是，当用户访问 `/a`时，URL 将会被替换成 `/b`，然后匹配路由为 `/b`，那么“别名”又是什么呢？

**`/a` 的别名是 `/b`，意味着，当用户访问 `/b` 时，URL 会保持为 `/b`，但是路由匹配则为 `/a`，就像用户访问 `/a` 一样。**

```
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

“别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。

##### 路由组件传参

在组件中使用 `$route` 会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性。

使用 `props` 将组件和路由解耦。

##### HTML5 History 模式

`vue-router` 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

如果不想要很丑的 hash，我们可以用路由的 **history 模式**，这种模式充分利用 `history.pushState` API 来完成 URL 跳转而无须重新加载页面。

当你使用 history 模式时，URL 就像正常的 url，例如 `http://yoursite.com/user/id`，也好看！

不过这种模式要玩好，还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 `http://oursite.com/user/id` 就会返回 404，这就不好看了。

所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 `index.html` 页面，这个页面就是你 app 依赖的页面。

###### 区别：

最直观的区别就是在url中 hash 带了一个很丑的 # 而history是没有#的

hash —— 即地址栏 URL 中的 # 符号（此 hash 不是密码学里的散列运算）。比如这个 URL：http://www.abc.com/#/hello hash 的值为 #/hello。它的特点在于：hash 虽然出现在 URL 中，但不会被包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。
history —— 利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。（需要特定浏览器支持）这两个方法应用于浏览器的历史记录栈，在当前已有的 back、forward、go 的基础之上，它们提供了对历史记录进行修改的功能。只是当它们执行修改时，虽然改变了当前的 URL，但浏览器不会立即向后端发送请求。
因此可以说，hash 模式和 history 模式都属于浏览器自身的特性，Vue-Router 只是利用了这两个特性（通过调用浏览器提供的接口）来实现前端路由.

##### 导航守卫

正如其名，`vue-router` 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。有多种机会植入路由导航过程中：全局的, 单个路由独享的, 或者组件级的。

记住**参数或查询的改变并不会触发进入/离开的导航守卫**。你可以通过[观察 `$route` 对象](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#响应路由参数的变化)来应对这些变化，或使用 `beforeRouteUpdate` 的组件内守卫。

###### 全局前置守卫

使用 `router.beforeEach` 注册一个全局前置守卫。

###### 全局解析守卫

在 2.5.0+ 你可以用 `router.beforeResolve` 注册一个全局守卫。这和 `router.beforeEach` 类似，区别是在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后**，解析守卫就被调用。

###### 全局后置钩子

你也可以注册全局后置钩子，然而和守卫不同的是，这些钩子不会接受 `next` 函数也不会改变导航本身

###### 路由独享的守卫

你可以在路由配置上直接定义 `beforeEnter` 守卫

###### 组件内的守卫

###### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。

###### 路由元信息

定义路由的时候可以配置 `meta` 字段

```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})
```

那么如何访问这个 `meta` 字段呢？

首先，我们称呼 `routes` 配置中的每个路由对象为 **路由记录**。路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录

例如，根据上面的路由配置，`/foo/bar` 这个 URL 将会匹配父路由记录以及子路由记录。

一个路由匹配到的所有路由记录会暴露为 `$route` 对象 (还有在导航守卫中的路由对象) 的 `$route.matched` 数组。因此，我们需要遍历 `$route.matched` 来检查路由记录中的 `meta` 字段。

##### 过渡动效

`` < router-view>是基本的动态组件，所以我们可以用 `` < transition>组件给它添加一些过渡效果。

```
<transition>
  <router-view></router-view>
</transition>
```

###### 单个路由的过渡

上面的用法会给所有路由设置一样的过渡效果，如果你想让每个路由组件有各自的过渡效果，可以在各路由组件内使用 <transition> 并设置不同的 name。

```
const Foo = {
  template: `
    <transition name="slide">
      <div class="foo">...</div>
    </transition>
  `
}

const Bar = {
  template: `
    <transition name="fade">
      <div class="bar">...</div>
    </transition>
  `
}
```

##### 数据获取

有时候，进入某个路由后，需要从服务器获取数据。例如，在渲染用户信息时，你需要从服务器获取用户的数据。我们可以通过两种方式来实现：

- **导航完成之后获取**：先完成导航，然后在接下来的组件生命周期钩子中获取数据。在数据获取期间显示“加载中”之类的指示。
- **导航完成之前获取**：导航完成前，在路由进入的守卫中获取数据，在数据获取成功后执行导航。

从技术角度讲，两种方式都不错 —— 就看你想要的用户体验是哪种。

##### 滚动行为

使用前端路由，当切换到新路由时，想要页面滚到顶部，或者是保持原先的滚动位置，就像重新加载页面那样。 `vue-router` 能做到，而且更好，它让你可以自定义路由切换时页面如何滚动。

**注意: 这个功能只在支持 `history.pushState` 的浏览器中可用。**

当创建一个 Router 实例，你可以提供一个 `scrollBehavior` 方法：

```
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
```

`scrollBehavior` 方法接收 `to` 和 `from` 路由对象。第三个参数 `savedPosition` 当且仅当 `popstate` 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。

这个方法返回滚动位置的对象信息，长这样：

- `{ x: number, y: number }`
- `{ selector: string, offset? : { x: number, y: number }}` (offset 只在 2.6.0+ 支持)

如果返回一个 falsy (译者注：falsy 不是 `false`，[参考这里](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy))的值，或者是一个空对象，那么不会发生滚动。

##### 路由懒加载

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

结合 Vue 的[异步组件](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#异步组件)和 Webpack 的[代码分割功能](https://doc.webpack-china.org/guides/code-splitting-async/#require-ensure-/)，轻松实现路由组件的懒加载。

首先，可以将异步组件定义为返回一个 Promise 的工厂函数 (该函数返回的 Promise 应该 resolve 组件本身)：

```
const Foo = () => Promise.resolve({ /* 组件定义对象 */ })
```

第二，在 Webpack 2 中，我们可以使用[动态 import](https://github.com/tc39/proposal-dynamic-import)语法来定义代码分块点 (split point)：

```
import('./Foo.vue') // 返回 Promise
```

结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。

```
const Foo = () => import('./Foo.vue')
```

在路由配置中什么都不需要改变，只需要像往常一样使用 `Foo`：

```
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```

###### 把组件按组分块

有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用 [命名 chunk](https://webpack.js.org/guides/code-splitting-require/#chunkname)，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)。

```
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```

Webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。

###### 2.

vue记录

template的作用是模板占位符，可帮助我们包裹元素，但在循环过程中，template不会被渲染到页面上。

template标签是不会渲染成标签元素的，因此里面必须要有一个父级标签，通常直接放个div即可

一个空vue.js的单文件组件是这样的：

```
<template>
	<div>
	</div>
</template>

<script>
export default{
	data(){
		return {}
	}
}

</script>
```

##### vue组件使用分三步：

1.引用组件 

```
import facePop from './components/facePop'
```

2.注册组件

```
components = {facePop}
```

3.使用组件

```
<facePop></facePop>
```

新建一个components文件存放组件

src/components/facePop.vue

```
<template>
	<div>
		<h2>我是一个facePop组件</h2>
	</div>
</template>
```

src/index.vue

```
<template>
	<div>
		<facePop></facePop>  //3.在模板中使用kebab-case、PascalCase两种都可以
		<face-pop></face-pop>
	</div>
</template>

<script>
	import facePop from './components/facePop';//1.引入组件 import后的名字一般与组件名称相同，也可不一样
	export default{
		data(){},
		components:{
			facePop  //2.注册组件 一般直接取一个名字 即 facePop: facePop
		}
	}
</script>
```

##### 组件名大小写

定义组件名的方式有两种：

（一）使用kebab-case 

```
Vue.component('my-component-name',{...})
```

当使用kebab-case(短横线分隔命名)定义一个组件时，你也必须在引用这个自定义元素时使用kebab-case，例如< my-component-name>。

（二）使用PascalCase

```
Vue.component('MyComponentName',{...})
```

当使用PascalCase(首字母大写命名)定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说< my-component-name>和< MyComponentName>都是可接受的。注意，尽管如此，直接在DOM（非字符串模板）中使用时只有kebab-case是有效的。

我们使用字符串模板，所以这个限制就不存在了。

##### 组件之间传值

在Vue中，父子组件的关系可以总结为**prop**向下传递，**事件**向上传递。父组件通过**prop**给子组件下发数据，子组件通过**事件**给父组件发送消息。父组件可以使用 props 把数据传给子组件、子组件可以使用 $emit 触发父组件的自定义事件。

子组件直接通过this.$emit("自定义事件")，然后父组件在组件中添加@自定义事件=‘event’就可以实现了。

vm.$emit( event, arg ) //触发当前实例上的事件

vm.$on( event, fn );//监听event事件后运行 fn； 

##### 单向数据流

prop是单向绑定的：当父组件的属性变化时，将传给子组件，但是反过来不会。这是为了防止子组件无意间修改了父组件的状态，来避免应用的数据流变得难以理解。另外，每次父组件更新时，子组件的所有prop都会更新为最新值。这意味着你不应该在子组件内部改变prop。如果你这么做了，Vue会在控制台给出警告。

注意在JavaScript中对象和数组是引用类型，指向同一个内存空间，如果prop是一个对象或数组，在子组件内部改变它会影响父组件的状态。

##### $refs的使用场景

父组件调用子组件的方法，可以传递数据
**注意**：子组件标签中的时间也不区分大小写要用“-”隔开

父组件：

```
<template>
  <div id="app">
    <child-a ref="child"></child-a>
    <!--用ref给子组件起个名字-->
    <button @click="getMyEvent">点击父组件</button>
  </div>
</template>
<script>
  import ChildA from './components/child.vue'
  export default {
    components: {
      ChildA
    },
    data() {
      return {
        msg: "我是父组件中的数据"
      }
    },
    methods: {
      getMyEvent(){
          this.$refs.child.emitEvent(this.msg);
          //调用子组件的方法，child是上边ref起的名字，emitEvent是子组件的方法。
      }
    }
  }
</script>
```

子组件：

```
<template>
  <button>点击我</button>
</template>
<script>
  export default {
    methods: {
      emitEvent(msg){
        console.log('接收的数据--------->'+msg)//接收的数据--------->我是父组件中的数据
      }
    }
  }
</script>
```

##### v-bind缩写

```
<!--完整语法-->
<a v-bind:href='url'>测试</a>
<!--缩写-->
<a :href='url'>测试</a>
```

##### v-on缩写

```
<!--完整语法-->
<a v-on:click='doSomething'>修改</a>
<!--缩写-->
<a @click='doSomething'>修改</a>
```

v-bind指令用于设置HTML属性。v-on指令用于绑定HTML事件。

Vue.js的指令是指v-开头，作用于html标签，提供一些特殊的特性，当指令被绑定到html元素的时候，指令会为被绑定的元素添加一些特殊的行为，可以将指令看成html的一种属性。

内置指令：

v-text  数据绑定标签，将vue对象data中的属性绑定给对应的标签作为内容显示出来，类似js的text属性;

v-html 类似v-text标签，他是将data的属性作为html语法输出，类似js中的innerHtml属性;

v-show  根据表达式的真假来切换所绑定的dom的display属性  v-show用于控制dom的显隐，实际是控制了dom的css的display属性

v-if 可以实现条件渲染，Vue会根据表达式的值的真假条件来渲染元素。

v-if和v-show功能差不多，都是用来控制dom的显隐，用法也一样，只是原理不同，当v-if=‘false’时，dom被直接删除掉；为true时，又重新渲染此dom;

注意：v-if有更高的切换开销。v-show有更高的初始渲染开销。因此，如果要非常频繁的切换，则使用v-show较好；如果在运行时条件不太可能改变，则v-if较好。

v-for 根据遍历数组来进行渲染

注意：当v-for和v-if同处于一个节点时，v-for的优先级比v-if更高。这意味着v-if将运行在每个v-for循环中。

v-on 缩写 @ 参数： event  主要用来监听dom事件，以便执行一些代码块。表达式可以是一个方法名。

```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用`v-on:click.prevent.self`会**阻止所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。

v-bind 缩写： 参数：attr/Prop 用于将vue的值绑定给对应dom的属性值。用来**动态的绑定一个或者多个特性**。没有参数时，可以绑定到一个包含键值对的对象。常用于动态绑定class和style。以及href等。

v-model 这个指令用于在表单上创建双向数据绑定。v-model会忽略所有表单元素的value、checked、selected特性的初始值。因为它选择Vue实例数据作为具体的值。

v-model修饰符：

v-mode.lazy: 一般v-model绑定的数据都是实时同步的，加上这个修饰符可以等到change事件再同步另一个的值；

v-model.number：自动将输入的值转为数值类型

v-model.trim:自动trim

v-slot 缩写 # 参数：插槽名（默认default）

##### data必须是函数

```
data(){
	return {};
}
```

一个组件的data选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝。

注意当点击按钮时，每个组件都会各自独立维护它的 `count`。因为你每用一次组件，就会有一个它的新**实例**被创建。

##### 模板语法

Vue.js使用了基于HTML的模板语法，允许开发者声明式地将DOM绑定至底层Vue实例的数据。所有vue.js的模板都是合法的HTML，所以能被遵循规范的浏览器和HTML解析器解析。

数据绑定最常见的形式就是使用“Mustache”语法（双大括号）的文本插值。

```
<span>Message:{{msg}}</span>
```

##### 计算属性 computed

```
computed: {
	...mapState({
		myCardMysInfo: state => state.mine.myCardMysInfo
	})
}
```

//computed 先通过... 之后通过mapState这个方法，把state中的数据取出来。这个state指store中初始化的state,当进行server请求后，把数据放到state中存起来。

##### 计算属性缓存VS方法

可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是计算属性是基于它们的响应式依赖进行缓存的。只在相关响应依赖发生改变时它们才会重新求值。这就意味着只要message还没有发生改变，多次访问reversedMessage计算属性会立即返回之前的计算结果，而不必再次执行函数。

##### methods

methods 对象定义当前页面操作的方法

```
methods:{
	//获取卡密弹框
    getCardMysInfo(){
    	this.getCardMystery({
    		body:{applicationId: this.applicationInfoId}, //参数
    		callback: data=>{ //回调
    			
    		}
    	})
    },
    hidePopUpCenter(){
    
    },
    ,,,
    ...mapActions("mine",['getCardMystery']); 
    // 此方法指明，this.getCardMystery是定义在mine(store)下的方法
}
```

##### 生命周期钩子函数

new Vue() -> beforeCreate() -> created() -> beforeMount() -> mounted() -> beforeUpdate -> updated() -> 

beforeDestory() -> destoryed() 

其余的方法：created(){} 创建的时候执行、mounted(){}挂载完成的时候执行、destroyed(){}销毁的时候执行、就是生命周期钩子函数。beforeRouteEnter(to, from, next){} 在路由进入后执行（路由守卫）

##### Vuex

Vuex官网的解释：Vuex是一个专为vue.js应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生改变。

与其他模式不同的是，Vuex是专门为Vue.js设计的状态管理库，以利用Vue.js的细粒度数据响应机制来进行高效的状态更新。

每一个Vuex应用的核心就是store(仓库)。“store”基本上就是一个容器，它包含着你的应用中大部分的状态（state）。Vuex和单纯的全局对象有以下两点不同：

1.Vuex的状态存储是响应式的。当Vue组件从store中读取状态的时候，若store中的状态发生变化，那么相应的组件也会相应地得到高效更新。

2.你不能直接改变store中的状态。改变store中的状态的唯一途径就是显式地提交(commit)mutation。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

##### Mutation

更改Vuex的store中的状态的唯一方法是提交mutation。Vuex中的mutation非常类似于事件：每个mutation都有一个字符串的事件类型(type)和一个回调函数(handler)。这个回调函数就是我们实际进行状态更改的地方，并且它会接受state作为第一个参数。

```
const mutations = {
	MY_GAME_URL:(state, payload)=>{
		state.myGameUrl = payload;
	}
};
```

##### Mutation必须是同步函数

##### Mutation需遵守Vue的响应规则

既然Vuex的store中的状态是响应式的，那么当我们变更状态时，监视状态的Vue组件也会自动更新。这也意味着Vuex中的mutation也需要与使用Vue一样遵守一些注意事项：

1.最好提前在你的store中初始化好所有所需属性。

2.当需要在对象上添加新属性时，你应该使用Vue.set(obj, 'newProp', 123)，或者以新对象替换老对象。例如，利用对象展开运算符我们可以这样写：state.obj = {...state.obj, newProp: 123}

在Vuex中，mutation都是同步事务

##### Action

Action类似于mutation，不同在于：

- Action提交的是mutation，而不是直接变更状态
- Action可以包含任意异步操作

Action函数接受一个与store实例具有相同方法和属性的context对象，因此你可以调用context.commit提交一个mutation，或者通过context.state和context.getters来获取state和getters。

```
const actions = {
	getMyGameUrlData: async({commit},payload)=>{
		try{
			const {body, callback}=payload;
			const {data=''} = await getMyGameUrl(body);
			commit('MY_GAME_URL', data);
			callback && callback(data);
		}catch(error){
		
		}
	}
};
```

##### 分发Action

Action通过store.dispatch方法触发：

```
store.dispatch('increment');
```

Actions支持同样的载荷方式和对象方式进行分发：

```
//以载荷形式分发
store.dispatch('incrementAsync',{
	amount:10
})
//以对象形式分发
store.dispatch({
	type:'incrementAsync',
	amount:10
})
```

##### 在组件中分发Action

在组件中使用this.$store.dispatch('xxx')分发action，或者使用mapActions辅助函数将组件的methods映射为store.dispatch调用（需要先在根节点注入store）

##### 组合Action

首先，你需要明白store.dispatch可以处理被触发的action的处理函数返回的Promise，并且store.dispatch仍旧返回Promise。

最后，利用async/await，可以组合action

```
actions: {
	async actionA({commit}){
		commit('getData', await getData())
	},
	async actionB({dispatch, commit}){
		await dispatch(actionA) //等待actionA完成
		commit('getOtherData', await getOtherData())
	}
}
```

一个store.dispatch在不同模块中可以触发多个action函数。在这种情况下，只有当所有触发函数完成后，返回的Promise才会执行。

Vuex并不限制你的代码结构。但是，它规定了一些需要遵守的规则：

1.应用层级的状态应该集中到单个store对象中。

2.提交mutation是更改状态的唯一方法，并且这个过程是同步的。

3.异步逻辑都应该封装到action里面。

外部调用mutations: store.commit('SET_AGE', 30);

外部调用actions:store.dispatch('nameAction');

###### vuex中如何获取state的数据呢？

在组件内部中的computed来获取state的数据（computed是实时响应的）。

###### mutations操作来改变state数据

在vuex中，改变状态state的唯一方式是通过提交commit的一个mutations,mutations下的对应函数接收第一个参数state，第二个参数为payload(载荷),payload一般是一个对象，用来记录开发时使用该函数的一些信息。mutations是处理同步的请求。不能处理异步请求的。

###### actions操作mutations异步来改变state数据

actions可以异步操作，actions提交mutations，通过mutations来提交数据的变更。它是异步修改state的状态的。

外部调用方式是 this.$store.dispatch('nameAsyn');

**理解context:**context是和this.$store具有相同的方法和属性的对象。我们可以通过context.state和context.getters来获取state和getters。

**理解dispatch:**它含有异步操作，含义可以理解为“派发”，比如向后台提交数据，可以为this.$store.dispatch('actions方法名', 值);

![image-20200616150049837](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20200616150049837.png)

组件派发任务到actions，actions触发mutations中的方法，然后mutations来改变state中的数据，数据变更后响应推送给组件，组件重新渲染。

vue-router

重定向的意思是，当用户访问/a时，URL将会被替换成/b，然后匹配路由为/b，

/a的别名是/b，意味着，当用户访问/b时，URL会保持为/b，但是路由匹配则为/a，就像用户访问/a一样。

###### Vue中slot的作用？

https://www.cnblogs.com/l-y-c/p/13413359.html

什么是插槽？

我们知道Vue中Child组件的标签的中间是不可以包着什么的。

![img](https://img2020.cnblogs.com/blog/1776897/202007/1776897-20200731155201091-374681511.png)

可是往往在很多时候我们在使用组件的时候总是想在组件间外面自定义一些标签，vue新增了一种插槽机制，叫做作用域插槽。要求的版本是2.1.0+；

插槽，其实就相当于占位符。它在组件中给你的HTML模板占了一个位置，让你来传入一些东西。插槽又分为匿名插槽、具名插槽、作用域插槽。

在2.6.0中，我们为具名插槽和作用域插槽引入了一个新的统一的语法（即v-slot指令）。它取代了slot和slot-scope

###### 匿名插槽

匿名插槽，我们也可以叫它单个插槽或者默认插槽。和具名插槽相对，它是不需要设置name属性的，它隐藏的name属性为default。

father.vue

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814210103243-2117774987.png)

child.vue

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814205652604-841900382.png)

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814210342412-1605968979.png)

匿名插槽，**name**的属性对应的是***default***也可以不写就是默认的意思啦；

在使用的时候还有一个问题要注意的 如果是有2个以上的匿名插槽是会child标签里的内容全部都替换到每个slot；

###### 具名插槽（vue2.6.0+被废弃的slot='name'）

顾名思议就是slot是带有name的，定义：<slot name='header'/> 或者使用简单缩写的定义#header 使用：要用一个template标签包裹

father.vue

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814215614912-2134577339.png)

child.vue![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814215811845-354693148.png)

这里说一下多个具名插槽的使用：多个具名插槽，插槽的位置不是使用插槽的位置而定的，是在定义的时候的位置来替换的

father.vue

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814222032415-869631960.png)

child.vue

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200814222212360-1766206425.png)

###### 作用域插槽

就是用来传递数据的插槽

当你想在一个插槽中使用数据时，要注意一个问题作用域的问题，Vue官方文档中说了父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

为了让子组件中的数据在父级的插槽内容中可用，我们可以将数据作为元素的一个特性绑定上去：v-bind:text=‘text’

注意：匿名的作用域插槽和具名的作用域插槽，区别在v-slot:default='接受的名称'（default[匿名的可以不写，具名的相反要写的是对应的name]）

v-slot可以解构接收：解构接收的字段要和传的字段一样才可以 例如：one 对应 v-slot="{one}"

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200815001613374-777519014.png)

效果图：

![img](https://img2020.cnblogs.com/blog/1776897/202008/1776897-20200815001754867-1631510469.png)



##### computed、watch、methods的区别：

##### watch属性可以用来监听data属性中数据的变化

同时还可以直接在监听的function中使用参数来获取新值与旧值。其中第一个参数是新值，第二个参数是旧值。同时Watch还可以被用来监听路由router的变化，只是这里的监听的元素是固定的。

Watch 监听 firstName 数据，并根据改变得到的新值，进行某些操作。

![image-20200907145808921](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20200907145808921.png)

注意：上例中，初始 fullName 是没有值的，只有当数据改变时，才会显示。因为 watch 的方法默认是不会执行的，只有当监听数据变化，才会执行。

###### immerdiate 属性

通过声明 immediate 选项为 true，可以立即执行一次 handler。

watch: {

  firstName: {

   handler (newName, oldName) {

​    this.fullName = newName + ' ' + this.lastName

   },

   immediate: true

  }

 },

watch 并不适用于显示某一个数据以及数据的拼装等。watch 用在监听数据变化，做某些指令操作（给后台发数据请求）

###### **deep属性**

不使用 deep 时，当我们改变 obj.a 的值时，watch不能监听到数据变化，默认情况下，handler 只监听属性引用的变化，也就是只监听了一层，但改对象内部的属性是监听不到的。

通过使用 deep: true 进行深入观察，这时，我们监听 obj，会把 obj 下面的属性层层遍历，都加上监听事件，这样做，性能开销也会变大，只要修改 obj 中任意属性值，都会触发 handler。

##### Computed:

computed属性的作用与watch类似，也可以监听属性的变化。只是他会根据他依赖的属性，生成一个属性，让vm对象可以使用这个属性。

如果 computed **所依赖的数据发生改变时，计算属性才会重新计算**，并进行缓存；当改变其他数据时，computed 属性 并不会重新计算，从而提升性能。

computed一般是改变data或者props里面的值为己用。

computed的值不可以在data中定义和赋值

computed是在dom加载后马上执行的。

computed是计算属性，实时响应的，比如你要根据data里一个值随时变化做出一些处理，就用computed。

computed中定义的函数，调用的时候后面不能有小括号，类似属性的调用。

**Methods:**

computed是计算属性，methods是方法。

我们可以将同一函数定义为一个 method 或者一个计算属性。对于最终的结果，两种方式确实是相同的。

不同的是computed计算属性是基于它们的依赖进行缓存的。计算属性computed只有在它的相关依赖发生改变时才会重新求值。这就意味着只要message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。而对于method ，只要发生重新渲染，method 调用总会执行该函数。

当有一个性能开销比较大的的计算属性 A ，它需要遍历一个极大的数组和做大量的计算。然后我们可能有其他的计算属性依赖于 A ，这时候，我们就需要缓存！

总之：数据量大，需要缓存的时候用computed；每次确实需要重新加载，不需要缓存时用methods

###### methods,watch,computed的区别：

1.computed 属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用；

2.methods 方法表示一个具体的操作，主要书写业务逻辑；

3.watch 一个对象，键是需要观察的表达式，值是对应回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作；可以看作是 computed 和 methods 的结合体；

Vue.js在模板表达式中限制了，绑定表达式最多只能有一条表达式，但某些数据需要一条以上的表达式运算实现，此时就可以将此数据放在计算属性（computed）当中。

###### Vuejs中关于computed、methods、watch的区别:

computed：计算属性将被混入到 Vue 实例中。所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例。

methods：methods 将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 this 自动绑定为 Vue 实例。

watch：是一种更通用的方式来观察和响应 Vue

实例上的数据变动。一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。Vue 实例将会在实例化时调用 $watch()，遍历 watch 对象的每一个属性。

通俗来讲：

computed是在HTML DOM加载后马上执行的，如赋值；

而methods则必须要有一定的触发条件才能执行，如点击事件；

watch呢？它用于观察Vue实例上的数据变动。对应一个对象，键是观察表达式，值是对应回调。值也可以是方法名，或者是对象，包含选项。

所以他们的执行顺序为：**默认加载的时候先computed再watch，不执行methods；等触发某一事件后，则是：先methods再watch。**

**computed、watch、methods中定义的方法，在引用时直接使用名字，按属性调用。**

##### 生命周期

![img](https://upload-images.jianshu.io/upload_images/12602393-5310c1449192165f.jpg)

![img](https://upload-images.jianshu.io/upload_images/12602393-1bd8cb609ed7521a.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

- `beforeCreate`:在实例初始化之后，数据观测data observer(`props、data、computed`) 和 `event/watcher` 事件配置之前被调用。
- `created`:实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， `watch/event` 事件回调。然而，挂载阶段还没开始，`$el` 属性目前不可见。
- `beforeMount`:在挂载开始之前被调用：相关的 `render` 函数首次被调用。
- `mounted`: `el` 被新创建的 `vm.$el` 替换，并挂载到实例上去之后调用该钩子。
- `beforeUpdate`:数据更新时调用，发生在虚拟 `DOM` 重新渲染和打补丁之前。 你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。
- `updated`：无论是组件本身的数据变更，还是从父组件接收到的 `props` 或者从`vuex`里面拿到的数据有变更，都会触发虚拟 `DOM` 重新渲染和打补丁，并在之后调用 `updated`。
- `beforeDestroy`:实例销毁之前调用。在这一步，实例仍然完全可用。
- `destroyed`:`Vue` 实例销毁后调用。调用后，`Vue` 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。

注意:
 `created`阶段的`ajax`请求与`mounted`请求的区别：前者页面视图未出现，如果请求信息过多，页面会长时间处于白屏状态。

**单个组件的生命周期**

1. 初始化组件时，仅执行了`beforeCreate/Created/beforeMount/mounted`四个钩子函数
2. 当改变data中定义的变量（响应式变量）时，会执行`beforeUpdate/updated`钩子函数
3. 当切换组件（当前组件未缓存）时，会执行`beforeDestory/destroyed`钩子函数
4. 初始化和销毁时的生命钩子函数均只会执行一次，`beforeUpdate/updated`可多次执行

**Vue.nextTick()**

在下次 `DOM` 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 `DOM`。

获取更新后的`DOM`言外之意就是什么操作需要用到了更新后的`DOM`而不能使用之前的`DOM`或者使用更新前的`DOM`会出问题，所以就衍生出了这个获取更新后的    `DOM`的`Vue`方法。所以放在`Vue.nextTick()`回调函数中的执行的应该是会对`DOM`进行操作的 `js`代码。

this.$nextTick()将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 Vue.nextTick 一样，不同的是回调的 this 自动绑定到调用它的实例上。

this.$nextTick()在页面交互，尤其是从后台获取数据后重新生成dom对象之后的操作有很大的优势。

.prevent的作用：阻止默认程序的运行，只执行我们分配给它的方法

###### Vue过滤器 filter

简单介绍一下过滤器，顾名思义，过滤就是一个数据经过了这个过滤之后出来另一样东西，可以是从中取得你想要的，或者给那个数据添加点什么装饰，那么过滤器则是过滤的工具。例如，从['abc','abd','ade']数组中取得包含‘ab’的值，那么可通过过滤器筛选出来‘abc’和‘abd’；把‘Hello’变成‘Hello World’，那么可用过滤器给值‘Hello’后面添加上‘ World’；或者把时间节点改为时间戳等等都可以使用过滤器。

首先，过滤器可在new Vue实例前注册全局的，也可以在组件上写局部。

全局过滤器：

```
Vue.filter('globalFilter',function(value){
	return value+'!!!'
});
```

组件过滤器（局部）：

```
filters:{
	componentFilter:function(value){
		return value + '!!!';
	}
}
```

##### 全局注册时是filter,没有s的。而组件过滤器是filters，是有s的。

###### 使用方法：

``` 
{{'ok' | globalFilter}}
```

###### v-slot的用法：

一句话概括就是v-slot ：后边是插槽名称，=后边是组件内部绑定作用域值的映射。

###### mapGetters

mapGetters(namespace?: string, map:Array<string> | Object<string>: Object)

为组件创建计算属性以返回getter的返回值。第一个参数是可选的，可以是一个命名空间字符串。

###### Vuex Getter

有时候我们需要从store中的state中派生出一些状态，例如对列表进行过滤并计数：

```
computed:{
	doneTodosCount(){
		return this.$store.state.todos.filter(todo=>todo.done).length;
	}
}
```

如果有多个组件需要用到此属性，我们要么复制这个函数，或者抽取到一个共享函数然后在多处导入它——无论哪种方式都不是很理想。

Vuex允许我们在store中定义"getter"(可以认为是store的计算属性)。就像计算属性一样，getter的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

Getter接受state作为其第一个参数：

```
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

通过属性访问：

Getter会暴露为store.getters对象，你可以以属性的形式访问这些值：

```
store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
```

Getter也可以接受其他getter作为第二个参数：

```
getters: {
  // ...
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length
  }
}
```

```
store.getters.doneTodosCount // -> 1
```

我们可以很容易地在任何组件中使用它：

```
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```

注意，getter在通过属性访问时是作为Vue的响应式系统的一部分缓存其中的。

通过方法访问：

你也可以通过让getter返回一个函数，来实现给getter传参。在你对store里的数组进行查询时非常有用。

```
getters:{
	getTodoById:(state)=>(id)=>{
		return state.todos.find(todo=>todo.id===id);
	}
}
```

```
store.getters.getTodoById(2) //->{ id: 2, text: '...', done: false }
```

注意，getter在通过方法访问时，每次都会去进行调用，而不会缓存结果。

mapGetters辅助函数

mapGetters辅助甘薯仅仅是将store中的getter映射到局部计算属性：

```
import {mapGetters} from 'vuex';
expoer default{
	computed:{
		//使用对象展开运算符将getter混入computed对象中
		...mapGetters([
			'doneTodosCount',
			'anotherGetter'
		])
	}
}
```

如果你想将一个getter属性另取一个名字，使用对象形式：

```
...mapGetters({
	//把`this.doneCount`映射为`this.$store.getters.doneTodosCount`
	doneConut: 'doneTodosCount'
})
```

###### 在Vue组件中获得Vuex状态

那么我们如何在Vue组件中展示状态呢？由于Vuex的状态存储是响应式的，从store实例中读取状态最简单的方法就是在计算属性中返回某个状态：

```
//创建一个Counter组件
const Counter={
	template:`<div>{{count}}</div>`,
	computed:{
		count(){
			return store.state.count
		}
	}
}
```

每当store.state.count变化的时候，都会重新求取计算属性，并且触发更新相关联的DOM。

mapState辅助函数

mapState(namespace?: string, map:Array<string> | Object<string | function>) ： Object

为组件创建计算属性以返回Vuex store中的状态。

第一个参数是可选的，可以是一个命名空间字符串。

对象形式的第二个参数的成员是一个函数。function(state:any)

```
import {mapState} from 'vuex';
export default{
	computed: {
		mapState({
			conut: state=>state.count,
			//传字符串参数'count'等同于`state=>state.count`
			countAlias:'count',
			//为了能够使用this获取局部状态，必须使用常规函数
			conutPlusLocalState(state){
				return state.count + this.localCount
			}
		})
	}
}
```

当映射的计算属性的名称与state的子节点名称相同时，也可以给mapState传一个字符串数组

```
computed:mapState([
	'count'
	//映射this.count为store.state.count
])
```

###### Action

Action类似于mutation，不同在于：Action提交的是mutation，而不是直接变更状态。Action可以包含任意异步操作。

Action函数接受一个与store实例具有相同方法和属性的context对象，因此你可以调用context.commit提交一个mutation，或者通过context.state和context.getters来获取state和getters。

```
const store= new Vuex.Store({
	state:{
		count: 0
	},
	mutation:{
		increment(state){
			state.count++;
		}
	},
	actions:{
		increment(context){
			context.commit('increment');
		}
	}
})
```

//为什么context对象不是store实例本身？

实践中，我们会经常用到ES2015的参数解构来简化代码（特别是我们需要调用commit很多次的时候）：

```
actions:{
	increment({commit}){
		commit('increment')
	}
}
```

分发Action

Action通过store.dispatch方法触发：

```
store.dispatch('increment');
```

Actions支持同样的载荷方式和对象方式进行分发：

```
//以载荷形式分发
store.dispatch('incrementAsync',{
	amount:10
})
//以对象形式分发
store.dispatch({
	type:'incrementAsync',
	amount:10
})
```

在组件中分发Action 

在组件中使用this.$store.dispatch('xxx')分发action，或者使用mapActions辅助函数将组件的methods映射为store.dispatch调用（需要先在跟节点注入store）

```
import {mapActions} from 'vuex'
export default{
	methods:{
		...mapActions([
			'increment',
			//将this.increment()映射为this.$store.dispatch('increment')
			'incrementBy',
			//将this.incrementBy(amount)映射为this.$store.dispatch('incrementBy',amount)
		])
		...mapActions({
			add: 'increment'
			//将this.add()映射为this.$store.dispatch('increment')
		})
	}
}
```

mapActions

mapActions(namespace?: string, map: Array<string>|Object<string|function>): Object

创建组件方法分发action

第一个数是可选的，可以是一个命名空间字符串。

对象形式的第二个参数的成员可以是一个函数。function(dispatch:funciton,...args: any[])

###### Vuex

专为Vue.js应用程序开发的状态管理模式，采用集中式存储管理应用的所有组件的状态，以相应的规则保证状态以一种可预测的方式发生变化。

 1.Vuex的状态存储是响应式的。当Vue组件从store中读取状态的时候，若store中的状态发生变化，那么相应的组件也会相应地得到高效更新。

2.你不能直接改变store中的状态。改变store中的状态的唯一途径就是显式地提交commit mutation。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

- **store** : 类似容器，包含应用的大部分状态

1. 一个页面只能有一个store
2. 状态存储是响应式的
3. 不能直接改变store中的状态，唯一途径是显示的提交mutations

- **State**:包含所有应用级别的状态的对象```
- **Getters**:在组件内部获取store中状态的函数
- **Mutations**:唯一修改状态的事件回调函数
- **Actions**:包含异步操作、提交mutation改变状态

- Action 类似于 mutation，**不同在于**：

1. Action 提交的是 mutation，而不是直接变更状态。
2. Action 可以包含任意异步操作。

- **Modules**:将store分割成不同的模块
- **我的总结**：数据统一放在state中，然后对于数据的处理，都统一在mutations中，接着在组件内部，绑定函数，触发mutations中的函数。

vue使用vue-shortkey 插件，来使用快捷键

#### Running functions

The code below ensures that the key combination ctrl + alt + o will perform the 'theAction' method.

```
<button v-shortkey="['ctrl', 'alt', 'o']" @shortkey="theAction()">Open</button>
```

The function in the modifier **@shortkey** will be called repeatedly while the key is pressed. To call the function only once, use the **once** modifier

```
<button v-shortkey.once="['ctrl', 'alt', 'o']" @shortkey="theAction()">Open</button>
```

###### vue-事件修饰符（.prevent .stop  .once .capture .self）

(1) .prevent --- 等于javascript的 event.preventDefault()

作用：阻止默认程序的运行

```
<form @submit.prevent='SomeFunction'></form>
```

单独submit点击后会自动进行提交等一系列操作，prevent就可以阻止这些操作，让上面这段代码乖乖执行我们分配给它的SomeFunction。

(2)  .stop 作用：阻止冒泡

什么叫冒泡？冒泡就是子元素的事件传递到父元素。用以下代码为例，点击button后，触发inner-click，但是因为这个button在中间的这个middle里，同时也相当于我们点击了middle这个元素，也就是button的父元素。同理，也相当于点击了outer元素，也就是说执行了inner-click后再执行middle-click紧接着outer-click。**这就叫做冒泡，一个从子元素向父元素传递事件的行为。**

```
<div id="app"> 
　　<div class="outer" @click="outer-click"> 
　　　　<div class="middle" @click="middle-click"> 
　　　　　　<button @click="inner-click">点击我(^_^)</button>
    　　</div>
    </div> 
</div>
```

```
methods: { 
　　　　inner: function () {
　　　　　　 console.log('inner： 这是最里面的Button' )
　　　　}, 
　　　　middle: function () { 
　　　　　　console.log('middle: 这是中间的Div' )
　　　　}, 
　　　　outer: function () { 
　　　　　　console.log('outer: 这是外面的Div' )
　　　　} 
　　} 
```

运行结果：

```
inner： 这是最里面的Button
middle: 这是中间的Div
outer: 这是外面的Div
```

.stop的存在就相当于阻止事件向父元素传递，保证只执行inner-cli

```
<div id="app"> 
　　<div class="outer" @click="outer-cli"> 
　　　　<div class="middle" @click="middle-cli"> 
　　　　　　<button @click.stop="inner-cli">点击我(^_^)</button>
    　　</div>
    </div> 
</div>
```

运行结果：

```
inner： 这是最里面的Button
```

同理.stop如果放在middle里面，那么输出结果为：

```
inner： 这是最里面的Button
middle: 这是中间的Div
```

(3) .capture

作用：打乱冒泡顺序

用以下代码为例，**发生click事件时会优先去找你可以传递到的所有父元素中最后一个有.capture的元素**（这里可以传递到middle和outer，最后一个有.capture的元素是outer），然后优先执行这个元素的事件，**紧接着执行倒数第二个有.capture的事件**（middle）,最后再按照正常的冒泡顺序**从自己开始往上执行未经执行的父元素**的click事件。

```
<div id="app"> 
　　<div class="outer" @click.capture="outer"> 
　　　　<div class="middle" @click.capture="middle"> 
　　　　　　<button @click="inner">点击我(^_^)</button>
    　　</div>
    </div> 
</div>
```

运行结果：

```
outer: 这是外面的Div
middle: 这是中间的Div
inner： 这是最里面的Button
```

其余例子：

```
<div id="app"> 
　　<div class="outer" @click="outer"> 
　　　　<div class="middle" @click.capture="middle"> 
　　　　　　<button @click="inner">点击我(^_^)</button>
    　　</div>
    </div> 
</div>
```

运行结果：

```
middle: 这是中间的Div
inner： 这是最里面的Button
outer: 这是外面的Div
```

(4) .self 

作用：不让子元素的事件触发自己绑定的事件，**但是不会阻止冒泡**

```
<div id="app"> 
　　<div class="outer" @click="outer"> 
　　　　<div class="middle" @click.self="middle"> 
　　　　　　<button @click="inner">点击我(^_^)</button>
    　　</div>
    </div> 
</div>
```

这里middle这有一个.self，当我们点击button的时候，先执行inner，传递到middle,但是.self阻止了middle的click事件，继续冒泡到outer，执行outer的click事件。

运行结果：

```
	inner： 这是最里面的Button
	outer: 这是外面的Div
```

(5) .once 事件只会触发一次

```
<button @click.once='inner'>点击我</button>
```

Vue中组件有的时候，属性带:,有的时候不带，什么区别？

冒号后面为变量，会动态变化的值；一般属性后面为常量。

this.$nextTick()将回调延迟到下次DOM更新循环之后执行。在修改数据之后立即使用它，然后等待DOM更新。它跟全局方法vue.nextTick一样，不同的是回调的this自动绑定到调用它的实例上。

this.$nextTick()在页面交互，尤其是从后台获取数据后重新生成dom对象之后的操作有很大的优势。

###### 父组件向子组件传值props

1.定义父组件，父组件传递inputText这个数值给子组件：

```
//父组件
//引入的add-widget组件
//使用 v-bind 的缩写语法通常更简单：
<add-widget :msg-val="msg"> //这里必须要用 - 代替驼峰
// HTML 特性是不区分大小写的。所以，当使用的不是字符串模板，camelCased (驼峰式) 命名的 prop 需要转换为相对应的 kebab-case (短横线隔开式) 命名，当你使用的是字符串模板的时候，则没有这些限制 
</add-widget>
data(){
    return {
        msg: [1,2,3]
    };
}
```

2.定义子组件，子组件通过props方法获取父组件传递过来的值。props中可以定义能接收的数据类型，如果不符合会报错。

```
//子组件通过props来接收数据
//方式1
props: ['msgVal']
//方式2
props: {
	msgVal: Array
}
//方式3
props: {
	msgVal: {
		type: Array, //指定传入的类型
		//type 也可以使一个自定义构造器函数，使用instanceof检测
		default: [0,0,0] //这样可以指定默认的值
	}
}
//注意props会在组件实例创建之前进行校验，所以在default或validator函数里，诸如data、computed或methods等实例属性还无法使用
```

注意：父子组件传值，数据是异步请求，有可能数据渲染时报错

原因：异步请求时，数据还没有获取到但是此时已经渲染节点了

解决方案：可以在父组件需要传递数据的节点加上v-If=isReady(isReady默认为false),异步请求获取数据后（isReady赋值为true）,v-if=isReady

###### 子组件向父组件传递数据

子组件通过$emit方法传递数据

子组件： 子组件绑定个方法，$emit的方法名是父组件的方法名

![img](https://img-blog.csdn.net/20180208172821122)

父组件：触发父组件的方法，然后执行相应的操作

![img](https://img-blog.csdn.net/20180208172849795)

###### 子组件向子组件传递数据

Vue没有直接子对子传参的方法，建议将需要传递数据的子组件，都合并为一个组件。如果一定需要子对子传参，可以先从传到父组件，再传到子组件。或者通过eventBus或vuex(小项目少页面用eventBus,大项目多页面使用vuex)传值。

###### 画面迁移的组件之间传递数据

1）通过路由带参数进行传值，例：两个组件A和B，A组件通过query把orderId传递给B组件

A组件传值写法：

```
this.$router.push({ path: '/componentB', query: {orderId: 123}}) //跳转到B
```

B组件取值的写法

```
this.$route.query.orderId
```

设置路由导航的两种方法：

声明式的导航 <router-link :to='...'>

编程式的导航 router.push(...)

传参的方式又分为查询参数query(+path)和命名路由params(+name)两种方式：

- 命名路由搭配params，刷新页面参数会丢失
- 查询参数搭配query,刷新页面数据不会丢失
- 接受参数使用this.$router后面就是搭配路由的名称就能获取到参数的值

2）通过设置Session Storage缓存的形式进行传递

两个组件A和B，在A组件中设置缓存orderData

```
const orderData = {'orderId': 123, 'price': 88};
sessionStorage.setItem('缓存名称', JSON.stringify(orderData));
```

B组件就可以获取在A中设置的缓存了

```
const dataB= JSON.parse(sessionStorage.getItem('缓存名称'));
```

3）通过provide/inject传值

详情见：https://www.cnblogs.com/vickylinj/p/13368745.html

4）通过$attrs、$listeners传值

​    详情见：https://www.cnblogs.com/vickylinj/p/13376391.html

src/router

![image-20201117140134368](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201117140134368.png)

路由匹配时，props:true的作用：

当在routes中设置props:true时，我们在组件中可以通过props:['id']获取路由中的参数（id参数）值，当props:false是无法获取的。

如果我们不使用props属性，那么我们只能通过传统的方式在组件中获取参数数据，那么传统的方式为{{$route.params.id}}，那么传统的方式就是在组件中用到了路由对象，那么组件就和路由耦合了。

变量作为对象的key的使用方法： [] 即变量外加[]

```
var lastWord = 'last word';
    var a = {
      'first word': 'hello',
      [lastWord]: 'world'
    };

    a['first word'] // "hello"
    a[lastWord] // "world"
    a['last word'] // "world"
```

!!value  !!的作用-》类型转换，最终输出的是true或false

数组：使用,声明一个空元素

console.log([,2])

```
<style lang="scss" scoped>
```

lang属性，普通的style标签支持普通的样式，如果想要启用scss或less ,需要为style元素设置lang属性。

scoped 属性，是用来专门用于标签元素内部的，它是通过CSS的属性选择器实现的。scoped只让样式渲染本组件，不会全局渲染。

for (int m = 0; m < lines?.Length; m++)

就问你装不装。?.的意义是，如果前面为空，则返回void.如果不为空则继续下去。是不是同时想起了?: 这种符号。

这叫语法糖，减少你的代码量。

js问号点(可选链)操作符【?.】【??】

相信大家应该都写过类似的代码：

 

let arr = res && res.data && res.data.list

是不是非常不美观，今天介绍的新语法就是为了解决这种问题的

可选链操作符?.

来，用新语法再写一次

let arr = res?.data?.list

是不是很简洁了。

还有，要是想设置默认值怎么办

以前我们是这么写的

let arr = res && res.data || []

现在可以这样

let arr = res?.res?.data ?? []

这个??的意思是当左边的值为null或undefined的时候 就取??右边的值 

###### 全局

当你注册完之后，可以在任何组件中直接使用标签，而不需要在各个组件中引入并局部注册

通常公共组件放在src文件夹下的components文件夹中，这里的组件进行全局注册。

###### 局部

页面中私有的组件放在各自的页面文件夹中并使用下面代码局部注册

```
import ComponentA from './ComponentA'
import ComponentB from './ComponentB'
export default {
  name: "part",
  components: { ComponentA, ComponentB },
}
```

Vue.use()是全局注册插件 ，

Vue.component()是全局注册组件。

![image-20201117142515364](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201117142515364.png)

注册组件：

![image-20201117142534691](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201117142534691.png)

![image-20201117142552885](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201117142552885.png)

![image-20201117142602844](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201117142602844.png)

通常我们在vue里面使用别人开发的组件，第一步就是install,第二步在main.js里面引入，第三步Vue.use这个组件。

 

Vue.use()是全局注册插件 ，Vue.component()是全局注册组件。但是为什么会出现进行全局注册组件也使用到了use,而且成功了，自己写的组件会报错呢？

后来看到一篇文章Vue.use自定义自己的全局组件，里面提到了自定义好组件后，对组件又进行了封装，封装成了插件，在方法中通过Vue.component对自定义的组件进行了全局注册。

封装插件

import MyLoading from './Loading.vue'

// 这里是重点 在这个文件中使用install方法来全局注册该组件

const Loading = {

  install: function(Vue){

​    Vue.component('Loading',MyLoading)

  }

}

只要在index.js里规定了install方法，就可以向其他ui组件库那样，使用Vue.use()来全局使用

相信很多人在用Vue使用别人的组件时，会用到 Vue.use() 。例如：Vue.use(VueRouter)、Vue.use(MintUI)。但是用 axios时，就不需要用 Vue.use(axios)，就能直接使用。那这是为什么呐？

因为 axios 没有 install。 https://www.jianshu.com/p/89a05706917a

https://blog.csdn.net/wang729506596/article/details/81018270

当我们封装的插件是这样的：

export const testObj = {

install(Vue, arg) {

​    }

  }

有install方法，那么就要使用Vue.use去初始化这个插件。这样写的好处就是插件需要一开始调用的方法都封装在install里面，更加精简和可拓展性更高。

如果封装的插件是靠这个对象去调用方法，比如axios，那么直接用的就是export default暴露出一个对象，那么就不需要使用Vue.use。

 

两者刚好让我们知道，如果一个插件是必须全部引入，那么使用暴露一整个对象，使用exportdefault或者是暴露一个用install的对象使用Vue.use。而像UI库那么庞大的插件，我们一般按需引入，那么就使用一个一个export的方法，使用花括号{}按需引入。

 

***\*插件\****

插件通常用来为 Vue 添加全局功能。插件的功能范围没有严格的限制——一般有下面几种：

1.添加全局方法或者 property。如：[***\*vue-custom-element\****](https://github.com/karol-f/vue-custom-element)

2.添加全局资源：指令/过滤器/过渡等。如 [***\*vue-touch\****](https://github.com/vuejs/vue-touch)

3.通过全局混入来添加一些组件选项。如 [***\*vue-router\****](https://github.com/vuejs/vue-router)

4.添加 Vue 实例方法，通过把它们添加到 Vue.prototype 上实现。

5.一个库，提供自己的 API，同时提供上面提到的一个或多个功能。如 [***\*vue-router\****](https://github.com/vuejs/vue-router)

***\*使用插件\****

通过全局方法 Vue.use() 使用插件。它需要在你调用 new Vue() 启动应用之前完成：

// 调用 `MyPlugin.install(Vue)` 

Vue.use(MyPlugin) 

new Vue({ // ...组件选项 })

Vue.use 会自动阻止多次注册相同插件，届时即使多次调用也只会注册一次该插件。

***\*i18n(其来源是英文单词internationalization的首末字符i和n，18为中间的字符数)是“国际化”的简称。在咨讯领域，国际化(i18n)指让产品（出版物，软件，硬件等）无需做大的改变就能够适应不同的语言和地区的需要。对程序来说，在不修改内部代码的情况下，能根据不同语言及地区显示相应的界面。在全球化的时代，国际化尤为重要，因为产品的潜在用户可能来自世界的各个角落。通常与i18n相关的还有L10n(\*******\*“\*******\*本地化\*******\*”\*******\*的简称)。\****

***\*export default {\****

***\*mixins：[form],\****

***\*}\****

***\*mixins:[form]的作用：\****

vue中提供了一种混合机制--mixins，用来更高效的实现组件内容的复用。

混合 (mixins) 是一种分发 Vue 组件中可复用功能的非常灵活的方式。
混合对象可以包含任意组件选项。
当组件使用混合对象时，所有混合对象的选项将被混入该组件本身的选项。

组件在引用之后相当于在父组件内开辟了一块单独的空间，来根据父组件props过来的值进行相应的操作，单本质上两者还是泾渭分明，相对独立。

而mixins则是在引入组件之后，则是将组件内部的内容如data等方法、method等属性与父组件相应内容进行合并。相当于在引入后，父组件的各种属性方法都被扩充了。

· 单纯组件引用：
***\*父组件 + 子组件 >>> 父组件 + 子组件\****

· mixins：
***\*父组件 + 子组件 >>> new父组件\****
有点像注册了一个vue的公共方法，可以绑定在多个组件或者多个Vue对象实例中使用。另一点，类似于在原型对象中注册方法，实例对象即组件或者Vue实例对象中，仍然可以定义相同函数名的方法进行覆盖，有点像子类和父类的感觉。

如果在引用mixins的同时，在组件中重复定义相同的方法，则mixins中的方法会被覆盖。

***\*mixins的特点\****

1 方法和参数在各组件中不共享

2 值为对象的选项，如methods,components等，选项会被合并，键冲突的组件会覆盖混入对象的

3 值为函数的选项，如created,mounted等，就会被合并调用，混合对象里的钩子函数在组件里的钩子函数之前调用

与vuex的区别

经过上面的例子之后，他们之间的区别应该很明显了哈~

vuex：用来做状态管理的，里面定义的变量在每个组件中均可以使用和修改，在任一组件中修改此变量的值之后，其他组件中此变量的值也会随之修改。

Mixins：可以定义共用的变量，在每个组件中使用，引入组件中之后，各个变量是相互独立的，值的修改在组件中不会相互影响。

与公共组件的区别

同样明显的区别来再列一遍哈~

组件：在父组件中引入组件，相当于在父组件中给出一片独立的空间供子组件使用，然后根据props来传值，但本质上两者是相对独立的。

Mixins：则是在引入组件之后与组件中的对象和方法进行合并，相当于扩展了父组件的对象与方法，可以理解为形成了一个新的组件。

**顺序很重要. 默认情况下, mixins将首先被调用, 然后是组件, 所以我们可以根据需要来覆盖它(override). 就是说, 组件有最后的发言权. 这只有当冲突发生时才变得非常重要, 在这个时候, 组件必须决定哪一个胜出, 否则一切将被放置在一个数组中执行, mixins相关的放在前面, 然后是组件相关的.**

###### vue v-for循环的用法

1.v-for 循环普通数组

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181205201733535-216659452.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181205201914380-1474410470.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181205202014422-1731723030.png)

2.v-for循环对象数组

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206105740055-1278485321.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206105926902-1376545124.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110006145-1458418636.png)

3.v-for循环对象

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110237124-571883828.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110349160-724955596.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110437789-908913742.png)

4.v-for循环数字

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110638613-739529289.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110712436-7051402.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206110755346-747301693.png)

5.v-for中key的使用方式

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206113322631-1352293864.png)

注意：

- v-for循环的时候，key属性只能使用number或string
- key在使用的时候，必须使用v-bind属性绑定的形式，指定key的值
- 在组件中使用v-for循环的时候，或者在一些特殊情况中，如果v-for有问题，必须在使用v-for的同事，指定唯一的字符串/数字类型 :key值。

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206113612245-270150968.png)

![img](https://img2018.cnblogs.com/blog/1513086/201812/1513086-20181206113743400-603555001.png)



###### Ajax请求放在Vue哪个生命周期中？

mounted。axios是一个基于Promise的HTTP请求客户端，用来发生请求。

为什么不在created里去发ajax ? created可是比mounted更早调用，更早调用意味着更早返回结果，那样性能不是更高？

首先，一个组件的created比mounted也早调用不了几微秒，性能没啥提高；而且，等到异步渲染开启的时候，created就可能被中途打断，中断之后渲染又要重做一遍，想一想，在created中做ajax调用，代码里看到只有调用一次，但是实际上可能调用N多次，这明显不合适。相反，若把ajax放在mounted，因为mounted在第二阶段，所以绝对不会多次重复调用，这才是ajax合适的位置。

在created的时候，视图中的dom并没有被渲染出来，所以此时如果直接去操作dom节点，无法找到相关元素。

在mounted中，由于此时的dom元素已经渲染出来了，所以可以直接使用dom节点。

一般情况下，都放在mounted中，保证逻辑的统一性。因为生命周期是同步执行的，ajax是异步执行的。

###### computed与method的区别？

computed是计算属性，method是方法。

使用computed性能会很好，但是如果你不希望缓存，你可以使用methods方法。可以使用methods来替代computed，效果上两个都是一样的，但是computed是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用methods,在重新渲染的时候，函数总会重新调用执行。

computed必须返回一个值，页面绑定的才能取得值。而methods中可以只执行逻辑代码，可以有返回值，也可以没有。

注意：computed不能计算data()中的属性。

##### Vue生命周期

**beforeCreate（创建前）:** 在数据观测和初始化事件还未开始,data、watcher、methods都还不存在，但是$route已存在，可以根据路由信息进行重定向等操作。

**created(创建后)：**在实例创建之后被调用，该阶段可以访问data，使用watcher、events、methods，也就是说 数据观测(data observer) 和event/watcher 事件配置 已完成。但是此时dom还没有被挂载。该阶段允许执行http请求操作。

**beforeMount （挂载前）：**将HTML解析生成AST节点，再根据AST节点动态生成渲染函数。相关render函数首次被调用(划重点)。

**mounted (挂载后)：**在挂载完成之后被调用，执行render函数生成虚拟dom，创建真实dom替换虚拟dom，并挂载到实例。可以操作dom，比如事件监听

**beforeUpdate：**vm.data更新之后，虚拟dom重新渲染之前被调用。在这个钩子可以修改vm.data更新之后，虚拟dom重新渲染之前被调用。在这个钩子可以修改*vm*.*data*更新之后，虚拟*dom*重新渲染之前被调用。在这个钩子可以修改vm.data，并不会触发附加的冲渲染过程。

**updated：**虚拟dom重新渲染后调用，若再次修改$vm.data，会再次触发beforeUpdate、updated，进入死循环。

**beforeDestroy：**实例被销毁前调用，也就是说在这个阶段还是可以调用实例的。

**destroyed：**实例被销毁后调用，所有的事件监听器已被移除，子实例被销毁

###### 3.

```
<b-modal v-model="modalShow"
 title="Select Excel File"
 size="lg"
 header-bg-variant="light"
 :hideHeaderClose="true"
 hide-footer>
</b-modal>
```

:hideHeaderClose="true" 或者 hideHeaderClose 直接写一个属性，就是默认设置为true .删除这个属性，默认设置为false 或者 :hideHeaderClose="false"。建议直接写 hideHeaderClose

加: =后面的是个表达式，不加:后面的是字符串。

###### vue数据与方法

当一个Vue实例被创建时，它将data对象中的所有的property加入到Vue的响应式系统中。当这些property的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

注意的是只有当实例被创建时就已经存在于data中的property才是响应式的。

除了数据property，Vue实例还暴露了一些有用的实例property与方法。它们都有前缀$，以便与用户定义的property区分开来。

生命周期钩子的this上下文指向调用它的Vue实例。

###### 模板语法

**文本：**数据绑定最常见的形式就是使用“Mustache”语法(双大括号)的文本插值

```
<span>Message: {{message}}</span>
```

Mustache标签将会被替代为对应数据对象上msg property的值。无论何时，绑定的数据对象上msg property发生了改变，插值处的内容都会更新。

**原始HTML**:双大括号会将数据解释为普通文本，而非HTML代码。为了输出真正的HTML，你需要使用v-html指令。

**Attribute：**Mustache语法不能作用在HTML attribute上，遇到这种情况应该使用v-bind指令。

指令：（Directives）是带有v-前缀的特殊attribute。指令attribute的值预期是单个JavaScript表达式(v-for是例外情况)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于DOM。

###### 计算属性缓存vs方法

可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是**计算属性是基于它们的响应式依赖进行缓存的。**只在相关响应式依赖发生改变时它们才会重新求值。相比之下，每当触发重新渲染时，调用方法将**总会**再次执行函数。

###### 计算属性vs侦听属性

Vue提供了一种更通用的方式来观察和响应Vue实例上的数据变动：侦听属性。

虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么Vue通过watch选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

###### Class与Style绑定

操作元素的class列表和内联样式是数据绑定的一个常见需求。因为它们都是attribute，所以我们可以用v-bind处理它们：只需要通过表达式计算出字符串结果即可。不过，字符串拼接麻烦且易错。因此，在将v-bind用于class和style时，Vue.js做了专门的增强。**表达式结果的类型除了字符串**之外，还可以是对象或数组。

绑定HTML Class

对象语法 ：可以传给v-bind:class 一个对象，以动态地切换class

```
<div v-bind:class="{active: isActive}"></div>
```

上面的语法表示active这个class存在与否将取决于数据property isActive的 truthiness。

可以在对象中传入更多字段来动态切换多个class。此外，v-bind:class指令也可以与普通的class attribute共存。

数组语法：可以把一个数组传给v-bind:class，以应用一个class列表

```
<div v-bind:class="[activeClass, errorClass]"></div>
```

当在一个自定义组件上使用class property时，这些class将被添加到该组件的根元素上面。这个元素上已经存在的class不会被覆盖。

###### 绑定内联样式

对象语法 ：v-bind:style 的对象语法十分直观--看着非常像CSS，但其实是一个JavaScript对象。CSS property 名可以用驼峰式(camelCase)或短横线分隔(kebab-case,记得用引号括起来)来命名：

```
<div v-bind:style="{color: activeColor, fontSize: fontSize + 'px'}"></div>
```

数组语法： v-bind:style 的数组语法可以将多个样式对象应用到同一个元素上

```
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

###### 条件渲染

v-if 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回truthy值得时候被渲染。

也可以用 v-else 添加一个“else 块”

在< template>元素上使用v-if条件渲染分组

因为v-if是一个指令，所以必须将它添加到一个元素上。但是如果想切换多个元素呢？此时可以把一个< template>元素当做不可见的包裹元素，并在上面使用v-if。最终的渲染结果将不包括<template>元素。

v-else-if，顾名思义，充当v-if的else-if块，可以连续使用

###### 用key管理可复用的元素

Vue会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做除了使Vue变得非常快之外，还有其它一些好处。

**<u>Vue提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的key attribute即可。</u>**

v-show 另一个用于根据条件展示元素的选项是v-show指令。用法大致一样：

```
<h1 v-show="ok">Hello!</h1>
```

不同的是带有v-show的元素始终会被渲染并保留在DOM中。v-show只是简单地切换元素的CSS property display。

注意，v-show不支持< template>元素，也不支持v-else。

###### v-if vs v-show

v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做--- 一直到条件第一次变为真时，才会开始渲染条件块。

相比之下， v-show就简单得多-- 不管初始条件是什么，元素总是会被渲染，并且只是简单地基于CSS进行切换。

一般来说，v-if有更高的切换开销，而v-show有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用v-show较好；如果在运行时条件很少改变，则使用v-if较好。

###### v-if 与 v-for一起使用

不推荐同时使用v-if和v-for。

当v-if与v-for一起使用时，v-for具有比v-if更高的优先级。

###### 列表渲染

用v-for把一个数组对应为一组元素

可以用v-for指令基于一个数组来渲染一个列表。v-for指令需要使用item in items形式的特殊语法，其中items是源数据数组，而item则是被迭代的数组元素的别名。

在v-for块中，我们可以访问所有父作用域的property。v-for还支持一个可选的第二个参数，即当前项的索引。

```
<ul id='example-2'>
	<li v-for="(item, index) in items">
		{{parentMessage}} - {{index}} - {{item.message}}
	</li>
</ul>
```

也可以使用of替代in作为分隔符，因为它更接近JavaScript迭代器的语法

```
<div v-for="item of items"></div>
```

在v-for里使用对象：也可以用v-for来遍历一个对象的property.

在遍历对象时，会按Object.keys()的结果遍历，但是不能保证它的结果在不同的JavaScript引擎下都一致。

为了给Vue一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一的key attribute。

建议尽可能在使用v-for时提供key attribute，除非遍历输出的DOM内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。

因为它是Vue识别节点的一个通用机制，key并不仅与v-for特别关联。

不要使用对象或数组之类的非基本类型值作为v-for的key。请用字符串或数值类型的值。

在<template>上使用v-for

类似于v-if，你也可以利用带有v-for的<template>来循环渲染一段包含多个元素的内容。

###### v-for与v-if一同使用

当它们处于同一节点，v-for的优先级比v-if更高，这意味着v-if将分别重复运行于每个V-for循环中。

###### 事件修饰符

Vue.js为v-on提供了事件修饰符。之前提过，修饰符是由点开头的指令后缀来表示的。

.stop  阻止单击事件继续传播

.prevent  提交事件不再重载页面

.capture  添加事件监听器时使用事件捕获模式，即内部元素触发的事件先在此处理，然后才交由内部元素进行处理

.self  只当在event.target是当前元素自身时触发函数，即事件不是从内部元素触发的

.once  点击事件只会触发一次

.passive 会告诉浏览器你不想阻止事件的默认行为

.exact 2.5.0新增 修饰符允许你控制由精确的系统修饰符组合触发的事件

###### 为什么在HTML中监听事件？

你可能注意到这种事件监听的方式违背了关注点分离(separation of concern)这个长期以来的优良传统。但不必担心，因为所有的Vue.js事件处理方法和表达式都严格绑定在当前视图的ViewModel上，它不会导致任何维护上的困难。实际上，使用v-on有几个好处：

1. 扫一眼HTML模板便能轻松定位在JavaScript代码里对应的方法。
2. 因为你无须在JavaScript里手动绑定事件，你的ViewModel代码可以是非常纯粹的逻辑，和DOM完全解耦，更易于测试。
3. 当一个ViewModel被销毁时，所有的事件处理器都会自动被删除。你无须担心如何处理它们。

##### 表单输入绑定

###### 基础用法

你可以用v-model指令在表单< input >、< textarea >及< select >元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。但v-model本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

v-model会忽略所有表单元素的value、checked、selected attribute的初始值而总是将Vue实例的数据作为数据来源。你应该通过JavaScript在组件的data选项中声明初始值。

v-model在内部为不同的输入元素使用不同的property并抛出不同的事件：

- text和textarea元素使用value property 和input事件；
- checkbox和radio使用checked property 和change事件；
- select字段将value作为prop并将change作为事件。

###### 修饰符

.lazy 默认情况下，v-model在每次input事件触发后将输入框的值与数据进行同步。你可以添加.lazy修饰符，从而转为在change事件之后进行同步。

```
<!-- 在change时而非input时更新 -->
<input v-model.lazy='msg'>
```

.number 如果想自动将用户的输入值转为数值类型，可以给v-model添加number修饰符

.trim 如果要自动过滤用户输入的首尾空白字符，可以给v-model添加trim修饰符

##### 组件基础

组件是可复用的Vue实例，且带有一个名字。

因为组件是可复用的Vue实例，所以它们与new Vue接收相同的选项，例如 data、computed、watch、methods以及生命周期钩子等。

可以将组件进行任意次数的复用。因为你每用一次组件，就会有一个它的新实例被创建。

###### data必须是一个函数

一个组件的data选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝。

有两种组件的注册类型：全局注册和局部注册。通过Vue.component全局注册的。

全局注册的组件可以用在其被注册之后的任何(通过new Vue)新创建的Vue根实例，也包括其组件树中的所有子组件的模板中。

###### 通过Prop向子组件传递数据

一个组件默认可以拥有任意数量的prop，任何值都可以传递给任何prop。

###### 通过插槽分发内容

只要在需要的地方加入插槽就行 < slot>< /slot>

###### 模板来源：

- 字符串(例如： template:'...')

- 单文件组件(.vue)

- ```
  <script type="text/x-template"></script>
  ```

组件注册

###### 组件名大小写

定义组件名的方式有两种：

1)使用kebab-case

```
Vue.component('my-component-name', {/*...*/})
```

当使用kebab-case(短横线分隔命名)定义一个组件时，你也必须在引用这个自定义元素时使用kebab-case,例如< my-component-name>

2)使用PascalCase

```
Vue.component('MyComponentName', {/*...*/})
```

当使用PascalCase(首字母大写命名)定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说< my-component-name>和< MyComponentName>都是可接受的。注意，尽管如此，直接在DOM(即非字符串的模板)中使用时只有kebab-case是有效的。

###### 全局注册

到目前为止，我们只用过Vue.component来创建组件：

```
Vue.component('my-component-name', {
	// ...选项
})
```

这些组件是全局注册的。也就是说它们在注册之后可以用在任何新创建的Vue根实例(new Vue)的模版中。

###### 局部注册

全局注册往往是不够理想的。比如，如果你使用一个像webpack这样的构建系统，全局注册所有的组件意味着即便你已经不再使用一个组件了，它仍然会被包含在你最终的构建结果中。这造成了用户下载的JavaScript的无畏的增加。

注意局部注册的组件在其子组件中**不可用**。

记住全局注册的行为必须在根Vue实例(通过new Vue)创建之前发生。

##### Prop

###### Prop的大小写(camelCase vs kebab-case)

HTML中的attribute名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。这意味着当你使用DOM中的模板时，camelCase(驼峰命名法)的prop名需要使用其等价的kebab-case(短横线分隔命名)命名。

```
Vue.component('blog-post', {
	props: ['postTitle'],//在JavaScript中是camelCase的
	template: '<h3>{{postTitle}}</h3>'
})
```

```
//在HTML中是kebab-case 的
<blog-post post-title='hello'></blog-post> 
```

重申一次，如果你使用字符串模板，那么这个限制就不存在了。

###### Prop类型

到这里，我们只看到了以字符串数组形式列出的prop:

```
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

但是，通常

###### CSS3文字阴影效果

text-shadow属性的语法：

```
text-shadow: h-shadow v-shadow blur color;
```

text-shadow属性可以设置4个参数，每个参数用空格隔开。

h-shadow: 设置水平阴影的位置（x轴方向），必需要设置的参数；允许负值。如果为正值，则阴影在对象的右边，反之其值为负值时，阴影在对象的左边。

v-shadow:设置垂直阴影的位置（y轴方向），必需要设置的参数，允许负值。如果为正值，阴影在对象的底部，反之其值为负值时，阴影在对象的顶部。

blur:阴影模糊的距离（半径大小），可选择设置的参数。只能设置为正值，如果值越大，阴影越模糊，反之阴影越清晰。如果其值为0时，表示阴影不具有模糊效果。

color:阴影的颜色，可选择设置的参数。当没有指定颜色值的时候，浏览器会取默认色，但各浏览器默认色不一样，特别是在webkit内核下的safari和chrome浏览器将无色，也就是透明，建议不要省略此参数。

注：Internet Explorer 9 以及更早版本的浏览器不支持 text-shadow 属性。 

###### 4.vue-roouter

###### vue-router路由信息meta

在全局守卫beforeEach((to, from, next) => {...})中判断当前路由是否允许登录、是否需要身份认证、权限认证等，虽然可以采用路由匹配的方式 if(to.path === '/url')，很显然当需要验证的路由较多时，会增加太多的if判断，这不利于代码维护，此时可在定义路由的时候可以配置meta字段，通过设置一些属性值，可以便于我们对当前的路由做一些处理，也可以使用next()重定向到其他路由。

使用，在定义路由时定义一个需要验证的meta信息 meta: {requiresAuth: true}

路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录，例如，根据下面的路由配置，/foo/bar这个URL将会匹配父路由记录以及子路由记录。

```
const router = new VueRouter({
	routes: [
		{
		    path: '/foo',
		    component: Foo,
		    children: [
		    	{
		    		path: 'bar',
		    		component: Bar,
		    		meta: {requiresAuth: true}
		    	}
		    ]
		}
	]
})
```

一个路由匹配到的所有路由记录会暴露为$route对象的$route.matched数组。因此，我们需要遍历$route.matched来检查路由记录中的meta字段。

然后再全局导航守卫中检查元字段是否需要验证，

```
router.beforeEach((to, from, next)=>{
	if(to.matched.some(record=>record.meta.requiresAuth)){
		// this route requires auth, check if logged in
    	// if not, redirect to login page.
		if(!auth.loggedIn()){
			next({
				path: '/login',
				query: {redirect: to.fullPath}
			})
		}else{
			next()
		}
	}else{
		next() //确保一定要调用next()
	}
})
```



###### 路由懒加载

为什么要使用路由懒加载呢?

用vue.js写单页面应用时，会出现打包后的JavaScript包非常大，影响页面加载，我们可以利用路由的懒加载去优化这个问题，当我们用到某个路由后，才去加载对应的组件，这样就会更加高效。

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

结合 Vue 的[异步组件](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#异步组件)和 Webpack 的[代码分割功能](https://doc.webpack-china.org/guides/code-splitting-async/#require-ensure-/)，轻松实现路由组件的懒加载。

首先，可以将异步组件定义为返回一个 Promise 的工厂函数 (该函数返回的 Promise 应该 resolve 组件本身)：

```
const Foo = () => Promise.resolve({ /* 组件定义对象 */ })
```

第二，在 Webpack 2 中，我们可以使用[动态 import](https://github.com/tc39/proposal-dynamic-import)语法来定义代码分块点 (split point)

```
import('./Foo.vue') // 返回 Promise
```

结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。

```
const Foo = () => import('./Foo.vue')
```

在路由配置中什么都不需要改变，只需要像往常一样使用 `Foo`

```
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```

用法：

```
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router)
export default new Router({
  routes: [
    {
      path: '/',
	  meta: {
       requiresAuth: true
      },
      component: resolve => require(['components/Hello.vue'], resolve)
    },
    {
        path: '/about',
        component: resolve => require(['components/About.vue'], resolve)
    }
  ]
})
```

###### 把组件按组划分

有时候我们想把某个路由下的所有组件都打包在同个异步块（chunk）中。只需要使用命名chunk，一个特殊的注释语法来提供chunk name(需要 Webpack > 2.4).

```
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```

Webpack会将任何一个异步模块与相同的块名称组合到相同的异步块中。

###### redirect 重定向

“重定向”的意思是，当用户访问/a时，URL将会被替换成/b，然后匹配路由为/b。

```
{
path: '/gohome', //路由路径
redirect: '/'    //重定向的路径就是要跳转的那个页面的路径
}
```

redirect的路径就是我们要跳转到的组件的路径

重定向也是通过routes配置来完成，下面例子是从/a 重定向到 /b；

```
const router = new VueRouter({
	routes:[
		{ path: '/a', redirect: '/b'}
	]
})
```

重定向的目标也可以是一个命名的路由：

```
const router = new VueRouter({
	routes: [
		{ path: '/a', redirect: { name: 'foo'}}
	]
})
```

甚至是一个方法，动态返回重定向目标：

```
const router = new VueRouter({
	routes: [
		{
			path: '/a', redirect: to => {
				//方法接收 目标路由 作为参数
				// return 重定向的 字符串路径/路径对象
			}
		}
	]
})
```

注意导航守卫并没有应用在跳转路由上，而仅仅应用在其目标上。在下面这个例子中，为/a路由添加一个 beforeEnter 守卫并不会有任何效果。

###### 别名

/a的别名是/b，意味着，当用户访问/b时，URL会保持为/b，但是路由匹配则为/a，就想用户访问/a一样。

上面对应的路由配置为：

```
const router = new VueRouter({
	routes: [
		{ path: '/a', component: A, alias: '/b'}
	]
})
```

“别名”的功能让你可以自由地将UI结构映射到任意的URL，而不是受限于配置的嵌套路由结构。

###### path与name的区别？

```
this.$router.replace({
	name: 'securityRequirements',
	query: this.getQuery(),
})
与
this.$router.replace({
    path: 'sources',
    query: {
         query: this.query || undefined,
 
    },
})
有什么区别？

name 指的是对应路由下的路由名称，
path 指的是对应路由的路径
```

可参考路由router/index.js下的配置

![image-20201020112330432](C:\Users\Grace.Liu1\AppData\Roaming\Typora\typora-user-images\image-20201020112330432.png)

用js实现跳转时有两种对应组合：

```javascript
	this.$router.push({'name':one; params:{'id':id}});
	this.$router.push({'path':'/home'; query:{'id':id}});
```

也就是name和params组合使用，path和query组合使用。

区别：其实name和query也可以组合实现页面跳转，但是参数无法正常传递接收，
params传递参数在地址栏是看不到的，就跟post请求很像。

query参数会显示在地址栏，就跟get请求很像。

query类似 get, 跳转之后页面 url后面会拼接参数,类似?id=1, 非重要性的可以这样传, 密码之类还是用params，刷新页面id还在。

params类似 post, 跳转之后页面 url后面不会拼接参数 , 但是刷新页面id 会消失

###### vue路由跳转的带参与不带参,路由跳转传参方式：name 、 path

方法一：用**path**跳转加参数，相当于get请求，页面可以看到你传参

方法二：用name跳转，params传参，相当于post请求，页面不可以看到你传参值

###### 路由传递参数和传统传递参数是一样的，命名路由类似表单提交而查询就是url传递：

1.命名路由搭配params，刷新页面参数会丢失

2.查询参数搭配query，刷新页面数据不会丢失

3.接收参数使用this.$router后面就是搭配路由的名称就能获取到参数的值

######  path:'name'和path:'/name'的区别

```
//比如当前路径是home
this.$router.push({path:'name'})//==>path为/home/name
this.$router.push({path:'/name'})//==>path为/name
```

###### path 和 Name路由跳转方式，都可以用query传参

###### replace 与push 的区别

push方法会向history栈添加一个新的记录，而replace方法是替换当前的页面，不会向history栈添加一个新的记录

this.$router.push跳转到指定url路径，并想history栈中添加一个记录，点击后退会返回到上一个页面。

this.$router.replace 跳转到指定url路径，但是history栈中不会有记录，点击返回会跳转到上上个页面 (就是直接替换了当前页面)。

this.$router.go(n) 向前或者向后跳转n个页面，n可为正整数或负整数。

配置路由时path有时会加'/'有时不加，以‘/’开头的会被当作根路径，就不会一直嵌套之前的路径。

###### this.$router和this.$route的区别

`this.$router`是VueRouter的实例方法，当导航到不同url，可以使用`this.$router.push`方法，这个方法则会向history里面添加一条记录，当点击浏览器回退按钮或者`this.$router.back()`就会回退之前的url。
`this.$route`相当于当前激活的路由对象，包含当前url解析得到的数据，可以从对象里获取一些数据，如name,path等。

###### 路由跳转分为编程式和声明式

声明式

```
//路由入口
<router-link :to="...">
//视图出口
<router-view></router-view>
```

编程式

```
// 字符串
router.push('home')
// 对象
router.push({ path: 'home' })
// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})
// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

注意：如果提供了path, params会被忽略，上述例子中的query并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的name或手写完整的带有参数的path:

```
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

同样的规则也适用于router-link组件的to属性。

###### $route对象

![2019070516324236.png](https://img.jbzj.com/file_images/article/201907/2019070516324236.png)

$route对象表示当前的路由信息，包含了当前 URL 解析得到的信息。包含当前的路径，参数，query对象等。
**1.$route.path**
   字符串，对应当前路由的路径，总是解析为绝对路径，如 "/foo/bar"。
**2.$route.params**
   一个 key/value 对象，包含了 动态片段 和 全匹配片段，
   如果没有路由参数，就是一个空对象。
**3.$route.query**
   一个 key/value 对象，表示 URL 查询参数。
   例如，对于路径 /foo?user=1，则有 $route.query.user == 1，
   如果没有查询参数，则是个空对象。
**4.$route.hash**
   当前路由的 hash 值 (不带 #) ，如果没有 hash 值，则为空字符串。锚点
**5.$route.fullPath**
   完成解析后的 URL，包含查询参数和 hash 的完整路径。
**6.$route.matched**
   数组，包含当前匹配的路径中所包含的所有片段所对应的配置参数对象。
**7.$route.name  当前路径名字**
**8.$route.meta 路由元信息**

###### $router对象

![2019070516324337.png](https://img.jbzj.com/file_images/article/201907/2019070516324337.png)

$router对象是全局路由的实例，是router构造方法的实例。

路由实例方法： 

push 、 go 、replace 、

###### vue路由守卫beforeEach、钩子

总体来讲vue里面提供了三大类钩子

1.全局钩子函数

2.单个路由钩子钩子函数（路由独享的守卫）

3.组件内钩子函数

###### vue-router全局钩子函数

beforeEach和afterEach是vue-router实例对象的属性。特别提醒：每次路由跳转，都会执行beforeEach和afterEach

router.beforeEach有三个参数：to/from/next

- **`to: Route`**: 即将要进入的目标 [路由对象](https://router.vuejs.org/zh/api/#路由对象)
- **`from: Route`**: 当前导航正要离开的路由
- **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。

```
router.beforeEach(function (to,from,next) {
// to: Route: 即将要进入的目标 路由对象
// from: Route: 当前导航正要离开的路由
// next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。
  next();
})
```

router.afterEach有两个参数：to/from，这些钩子不会接受 `next` 函数也不会改变导航本身

```
router.afterEach(function (to,from) {
  console.log(to);//到达的路由
  console.log(from);//离开的路由
})
```

这类钩子主要作用于全局,一般用来判断权限,以及以及页面丢失时候需要执行的操作。

###### 单个路由钩子函数

给某个路由单独使用的，本质上和后面的组件内钩子是一样的。都是特指的某个路由。不同的是，这里的一般定义在router当中，而不是在组件内。

```
{
path: '/dashboard',
component: resolve => require(['../components/page/Dashboard.vue'], resolve),
meta: { title: '系统首页' },
beforeEnter: (to, from, next) => {

},
},
```

###### 组件路由钩子函数

主要包括 beforeRouteEnter和beforeRouteUpdate ,beforeRouteLeave,这几个钩子都是写在组件里面也可以传三个参数(to,from,next),作用与前面类似.

```
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

记住**参数或查询的改变并不会触发进入/离开的导航守卫**。

`beforeRouteEnter` 守卫 **不能** 访问 `this`，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。

不过，你可以通过传一个回调给 `next`来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。

```
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫。对于 `beforeRouteUpdate` 和 `beforeRouteLeave` 来说，`this` 已经可用了，所以**不支持**传递回调，因为没有必要了。

###### [#](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#完整的导航解析流程)完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

###### 过渡动效

<router-view>是基本的动态组件，所以我们可以用<transition>组件给它添加一些过渡效果。

```
<transition>
	<router-view></router-view>
</transition>
```

单个路由的过渡

上面的用法会给所有路由设置一样的过渡效果，如果你想让每个路由组件有各自的过渡效果，可以在各路由组件内使用 `<transition>` 并设置不同的 name。

```
const Foo = {
  template: `
    <transition name="slide">
      <div class="foo">...</div>
    </transition>
  `
}

const Bar = {
  template: `
    <transition name="fade">
      <div class="bar">...</div>
    </transition>
  `
}
```

基于路由的动态过渡

还可以基于当前路由与目标路由的变化关系，动态设置过渡效果

```
<!-- 使用动态的 transition name -->
<transition :name="transitionName">
  <router-view></router-view>
</transition>
```

```
// 接着在父组件内
// watch $route 决定使用哪种过渡
watch: {
  '$route' (to, from) {
    const toDepth = to.path.split('/').length
    const fromDepth = from.path.split('/').length
    this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
  }
}
```

###### 数据获取

有时候，进入某个路由后，需要从服务器获取数据。例如，在渲染用户信息时，你需要从服务器获取用户的数据。我们可以通过两种方式来实现：

- 导航完成之后获取： 先完成导航，然后再接下来的组件生命周期钩子中获取数据。在数据获取期间显示“加载中”之类的指示。
- 导航完成之前获取：导航完成前，在路由进入的守卫中获取数据，在数据获取成功后执行导航。

从技术角度讲，两种方式都不错---就看你响应的用户体验是哪种。

###### 滚动行为

使用前端路由，当切换到新路由时，想要页面滚到底部，或者是保持原先的滚到位置，就像重新加载页面那样。vue-router能做到，而且更好，它让你可以自定义路由切换时页面如何滚动。

注意：这个功能只在支持 history.pushState的浏览器中可用。

当创建一个Router实例，你可以提供一个scrollBehavior方法：

```
const router = new VueRouter({
	routes: [...],
	scrollBehavior(to, from, savedPosition){
		//return 期望滚动到哪个的位置
	}
})
```

`scrollBehavior` 方法接收 `to` 和 `from` 路由对象。第三个参数 `savedPosition` 当且仅当 `popstate` 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。

这个方法返回滚动位置的对象信息，长这样：

- `{ x: number, y: number }`
- `{ selector: string, offset? : { x: number, y: number }}` (offset 只在 2.6.0+ 支持)

如果返回一个 falsy (译者注：falsy 不是 `false`，[参考这里](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy))的值，或者是一个空对象，那么不会发生滚动。

留意一下 `this.$router` 和 `router` 使用起来完全一样。我们使用 `this.$router` 的原因是我们并不想在每个独立需要封装路由的组件中都导入路由。

动态路由匹配：使用:标记。

###### 响应路由参数的变化

提醒一下，当使用路由参数时，例如从/user/foo导航到/user/bar，原来的组件实例会被复用。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，这也意味着组件的生命周期钩子不会再被调用。

复用组件时，想对路由参数的变化作出响应的话，你可以简单地watch（监测变化）$route对象。

###### 捕获所有路由或404 Not Found 路由

常规参数只会匹配被/分隔的URL片断中的字符。如果想匹配任意路径，可以使用通配符(*)。

当使用通配符路由时，请确保路由的顺序是正确的，也就是说含有通配符的路由应该放在最后。

当使用一个*通配符*时，`$route.params` 内会自动添加一个名为 `pathMatch` 参数。它包含了 URL 通过*通配符*被匹配的部分

###### 匹配优先级

有时候，同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：谁先定义的，谁的优先级就最高。

注意：在Vue实例内部，你可以通过$router访问路由实例。因此你可以调用this.$router.push。

###### 命名视图

有时候想同时 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有 `sidebar` (侧导航) 和 `main` (主内容) 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 `router-view` 没有设置名字，那么默认为 `default`。

```
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 `components` 配置 (带上 s)：

```
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

###### HTML5 History 模式

vue-router默认hash模式---使用URL的hash来模拟一个完整的URL，于是当URL改变时，页面不会重新加载。

如果不想要很丑的hash，我们可以用路由的history模式，这种模式充分利用history.pushStateAPI来完成URL跳转而无须重新加载页面。

```
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

当你使用 history 模式时，URL 就像正常的 url，例如 `http://yoursite.com/user/id`，也好看！

不过这种模式要玩好，还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 `http://oursite.com/user/id` 就会返回 404，这就不好看了。

所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 `index.html` 页面，这个页面就是你 app 依赖的页面。



<router-link>

<router-view>  显示的是当前路由地址所对应的内容

在vue-router中, 我们看到它定义了两个标签<router-link> 和<router-view>来对应点击和显示部分。<router-link> 就是定义页面中点击的部分，<router-view> 定义显示部分，就是点击后，区配的内容显示在什么地方。所以 <router-link> 还有一个非常重要的属性 to，定义点击之后，要到哪里去， 如：<router-link to="/home">Home</router-link>

执行过程：当用户点击 router-link 标签时，会去寻找它的 to 属性， 它的 to 属性和 js中配置的路径{ path: '/home', component: Home} path 一一对应，从而找到了匹配的组件， 最后把组件渲染到
<router-view> 标签所在的地方。所有的这些实现才是基于hash 实现的。

###### 路由嵌套总结

任何子路由都是在其父路由的组件中切换显示，不管是多少层的路由嵌套，都是这样的理解，所以父路由需要有以下两点，二者缺一不可

- 有组件引用
- 组价中有router-view组件

##### 5.

##### vue-router路由传参

用params传参，F5强制刷新参数会被清空，用query,由于参数适用路径传参的，所以F5强制刷新也不会被清空。也可以选用sessionStorage/localStorage/cookie存储

###### 1.params(URL不显示参数)、F5刷新后参数消失

（1）路由配置(index.js)

```
routes: [
	{
		path: '/',
		component: A
	},{
		path: '/b',
		name: 'b', //这句特别重要，params必须用name来识别路径
		component: B
	}
]
```

（2）按钮事件

```
<button @click='goToB'>路由跳转</button>
```

采用编程式跳转。把要传递的参数放在params里，data可以是具体值，也可以是本组件的变量。**特别要注意**，必须给要跳转的组件在配置路由时就要添加一个name，尝试在index.js里不带name,在b组件接收参数时就找不到。

```
goToB() {
	this.$router.push({
		path: "/b",
		name: 'b',
		params: {
			data: 1,
		}
	})
}
```

在b组件里接收参数。使用this.$route.params,其中data

```
created(){
	console.log(this.$route.params.data)
}
```

**注意：**刷新b组件后参数会消失。

###### sessionStorage:容量大、安全、临时存储、跨页面

###### localStorage:容量大、安全、永久存储、跨页面

###### 2.params(URL不显示参数)、F5刷新后参数不消失

```
<!-- 存储页面 test-local -->
<template>
  <div>
    <a @click="toAnother()">点击</a>
  </div>
</template>
<script>
export default {
  methods: {
    toAnother() {
      // session 添加session
      sessionStorage.code = 90088
      sessionStorage.setItem('page', 1)
      // local 添加local
      localStorage.age = 27
      localStorage.setItem('passcode', 12345)
      this.$router.push(`/testLocalTo`)
    }
  }
}
</script>
```

```
<!-- 跳转页面 test-local-to -->
<template>
  <div></div>
</template>
<template>
  <div></div>
</template>
<script>
export default{
  created() {
    this._getLocalData()
  },
  methods: {
    _getLocalData() {
      // session 获取session
      let code = sessionStorage.code
      console.log('code =', code)
      let page = sessionStorage.getItem('page')
      console.log('page =', page)
      // local 获取local
      let age = localStorage.age
      console.log('age =', age)
      let passcode = localStorage.getItem('passcode')
      console.log('passcode =', passcode)
    }
  }
}
</script>
```

页面刷新后，参数依然在

关闭当前页面，再次打开后，sessionStorage消失，localStorage依旧有。

###### cookie:容量小、不安全、有时间期限、跨页面

###### 3.params（URL显示参数）

(1)路由配置

```
export default new Router({
	routes: [
	{
		path: '/',
		component: A
	},{
		path: '/b/:data', //本行是为了路由的模式匹配
		name: 'b',
		component: B
	}]
})
```

(2)按钮事件

```
<button @click='goToB(data)'>路由跳转</button>
```

(3)编程跳转

```
methods: {
	goToB(data) {
		this.$router.push({
			path: `/b/${data}`, //本行对应路由格式配置，data可以是实值，可以是本地变量
		})
	}
}
```

获取参数的方式：

```
created() {
	console.log(this.$route.params.data)
}
```

**注意：**此种传参方式会把参数显示在URL中，通常用于路径跳转的需求。

###### 4.使用query

(1)路由

```
routes: [
    {
      path: '/',
      component: A
    },
    {
      path: '/b',
      component: B
    }
  ]
```

(2)按钮

```
<button @click="goToB(data)">路由跳转</button>
```

(3)编程跳转

可以看到只是params变成了query，其他的和第一种很相似。只不过这种会在地址栏显示参数而已

```
 goToB(data) {
      this.$router.push({
        path: `/b`,
        query:{
          data
        }
      });
    },
```

获取参数的方法：console.log(this.$route.query.data)

结果：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200917113125535.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNjg2NTI5,size_16,color_FFFFFF,t_70#pic_center)

由于query与params传参机制不一样，造成的差异，如果要隐藏参数用params，如果强制刷新不被清除用query

////

先有如下场景，点击当前页的某个按钮跳转到另外一个页面去，并将某个值带过去

```
<el-button type='primary' @click="handleClick(2)">查看详情</el-button>
```

###### 1.第一种方法：拼接方式

```
methods: {
	handleClick(id){
		//直接调用$router.push 实现带参数的跳转
		this.$router.push({
			path: `/detail/${id}`
		})
	}
}
```

对应路由配置：

```
{
	path: '/detail/:id',
	name: 'detail',
	component: detail
},

```

获取参数方式：

```
this.$route.params.id
```

这种方式传参，页面刷新数据不会丢失。

###### 2.第二种方法：params传参

通过路由属性中的name来确定匹配的路由，通过params来传递参数。

```
methods: {
	handleClick(id){
		this.$router.push({
			name: 'detail',//根据name确定匹配路由
			params: {
				id: id
			}
		})
	}
}
//或者采用router-link
<router-link :to="{name: 'detail', params: {id: 1}}">前往detail页面<router-link>
```

对应路由配置：

```
{
	path: '/detail/:id',
	name: 'detail',
	component: detail
}
```

获取参数方式：

```
this.$route.params.id
```

**需要注意的是，params动态路由传参，一定要在路由中定义参数，然后再路由跳转的时候必须要加上参数，否则就是空白页面。例如：**

```
//定义的路由中，只定义一个id参数
{
	path: 'detail/:id',
	name: 'detail',
	component: detail
}

//template中的路由传参，传了一个id参数和一个token参数，id是在路由中已经定义的参数，而token没有定义
<router-link :to="{name: 'detail', params: {id: 1, token: '123456'}}">前往detail页面</router-link>

//在详情页接收
created(){
	//以下都可以正常获取到
	//但是页面刷新后，id依然可以获取，而token此时就不存在了
	const id = this.$route.params.id;
	const token = this.$route.params.token;
}
```

###### 3.第三种方法：query传参

使用path来匹配路由，然后通过query来传递参数，这种情况下query传递的参数会显示在url后面?id=?

```
methods: {
	handleClick(id){
		this.$router.push({
			path: '/detail',
			query: {
				id: id
			}
		})
	}
}
```

对应路由配置：

```
{
	path: '/detail',
	name: 'detail',
	component: detail
}
```

获取参数：

```
this.$route.query.id
```

###### 4.总结：params和query中的区别

1.接收方式

query传参：this.$route.query.id

params传参：this.$route.params.id

2.路由展现方式

query传参：/detail?id=1&user=123&identity=1&更多参数

params传参： /detail/123
