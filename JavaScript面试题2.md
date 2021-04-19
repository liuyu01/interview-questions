##### JavaScript面试题2

###### 1.

使用`var`关键字声明的变量在JavaScript中会被提升，并在内存中分配值`undefined。 

let`和`const`声明可以让变量在其作用域上受限于它所使用的块、语句或表达式。与`var`不同的是，这些变量没有被提升，并且有一个所谓的**暂时死区(TDZ)**。试图访问**TDZ**中的这些变量将引发`ReferenceError`，因为只有在执行到达声明时才能访问它们。

###### 2.

ES全称ECMAScript,ECMAScript是ECMA制定的标准化脚本语言。目前JavaScript使用的ECMAScript版本为ECMA-417。

ECMA规范最终由TC39敲定。TC39由包括浏览器厂商在内的各方组成，他们开会推动JavaScript提案沿着一条严格的发展道路前进。从提案到入选ECMA规范主要有以下几个阶段：

Stage 0: strawman——最初想法的提交。

Stage 1: proposal（提案）——由TC39至少一名成员倡导的正式提案文件，该文件包括API事例。

Stage 2: draft（草案）——功能规范的初始版本，该版本包含功能规范的两个实验实现。

Stage 3: candidate（候选）——提案规范通过审查并从厂商那里收集反馈

Stage 4: finished（完成）——提案准备加入ECMAScript，但是到浏览器或者Nodejs中可能需要更长的时间。

##### ES6新特性（2015）

常用的：

- 类(class)
- 模块化(Module) export 导出 import 导入
- 箭头函数
- 函数参数默认值
- 模板字符串
- 解构赋值
- 扩展操作符
- 对象属性简写
- Promise
- let const const与let都是块级作用域

ES7新特性（2016）

- 数组includes()方法，用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回true，否则返回false。
- a ** b指数运算法，它与Math.pow(a,b)相同。

##### ES6中对象新增的方法

Object.is() 目的是为了保证在所有环境中，只要两个值是一样的，它们就应该相等，其行为与===基本一致，用来比较两个值是否严格相等。有两个不同之处：一是+0不等于-0而是NaN等于自身。

Object.assign()用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）上。

Object.getOwnPropertyDescriptors() 返回指定对象所有自身属性(非继承属性)的描述对象。

Object.getPrototypeOf()用于读取一个对象的原型对象

Object.setPrototypeOf()用来设置一个对象的prototype对象，返回参数对象本身。它是ES6正式推荐的设置原型对象的方法。

Object.keys() 返回一个成员是参数对象自身的（不含继承的）所有可遍历(enumerable)属性的键名的数组

Object.values()返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历(enumerable)属性的键值。

Object.entries()返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历(enumerable)属性的键值对数组。

Object.fromEntries()是Object.entries()的逆操作，用于将一个键值对数组转为对象。

###### 3.

call、apply、bind的介绍

##### 语法：

fun.call(thisArg, param1,  param2,...)

fun.apply(thisArg, [param1, param2,...])

fun.bind(thisArg, param1, param2,...)

##### 返回值：

call/apply：fun执行的结果

bind：返回fun的拷贝，并拥有指定的this值和初始参数。不调用函数，但生成一个函数并指定函数内的this。返回值是一个函数。

##### 参数：

thisArg(可选)：

1. fun的this指向thisArg对象
2. 非严格模式下：thisArg为空、null、undefined，fun中的this指向window对象
3. 严格模式下：fun的this为undefined
4. 值为原始值（数字、字符串、布尔值）的this会指向该原始值的自动包装对象，如String、Number、Boolean

param1,param2(可选)：传给fun的参数

1. 如果param不传或为null/undefined，则表示不需要传入任何参数
2. apply第二个参数为数组，数组内的值为传给fun的参数

##### 调用call/ apply/ bind的必须是个函数

call 、apply和bind是挂在Function对象上的三个方法，只有函数才有这些方法

只要是函数就可以，比如：Object.prototype.toString就是个函数，我们经常看到这样的用法：Object.prototype.toString.call(data)

##### 作用：

1. 改变函数执行时的this指向
2. 借用其他对象身上的方法
3. 继承

##### 如何不弄混call和apply

apply是以a开头，它传给fun的参数是Array,也是以a开头的

##### 区别：

###### call与apply的唯一区别：

传给fun的参数写法不同：

- apply是第2个参数，这个参数是一个数组：传给fun参数都写在数组中。
- call从第2~n的参数都是传给fun的。

###### call/apply与bind的区别

执行：

- call/apply改变了函数的this上下文后马上执行该函数
- bind则是返回改变了上下文后的函数，不执行该函数

返回值：

- call/apply返回fun的执行结果
- bind返回fun的拷贝，并指定了fun的this指向，保存了fun的参数

性能：

apply的性能比call的性能低，低5到6倍左右

##### call/apply/bind的核心理念：借用方法

借助已实现的方法，改变方法中数据的this指向，减少重复代码，节省内存。

```
const result = Math.max.apply(null,[3,5,9]);
```

Math.max就是借用的方法。

##### call、apply，该用哪个？

call，apply的效果完全一样，它们的区别在于：

- 参数数量/顺序确定就用call，参数数量/顺序不确定的话就用apply。
- 考虑可读性：参数数量不多就用call，参数数量比较多的话，把参数整合成数组，使用apply。
- 参数集合已经是一个数组的情况，用apply,

###### 举例借用其他对象身上的方法

const str='hehehaha';

let arr = Array.prototype.slice.call(str);

console.log(arr) // ["h", "e", "h", "e", "h", "a", "h", "a"]

不是所有的方法都可以借用：

const str='hehehaha';

console.log([].push.call(str)) //VM211:2 Uncaught TypeError: Cannot assign to read only property 'length' of object '[object String]'

报错的原因：字符串的length属性不可写。是只读的。

###### 4.

##### JS 的5个不良编码习惯

###### 1.不要使用隐式类型转换

###### **最佳实践列表：**

- 始终使用严格的相等运算符`===`进行比较
- 不要使用松散等式运算符`==`
- 加法运算符 `operand1 + operand2`：两个操作数应该是数字或字符串
- 算术运算符 `- * /％**`：两个操作数都应该是数字
- `if（condition）{...}`，`while（condition）{...}`等语句：`condition` 必须是一个布尔类型值

###### 2. 不要使用早期的JavaScript技巧

ES6 中可以使用 `array.includes(item)` 来代替 `array.indexOf(item) !== -1`

###### 3. 不要污染函数作用域

在ES2015之前，你可能会养成了将所有变量声明在函数作用域里面。

通过引入具有块作用域 `let`和`const`，应该尽可能地限制变量的生命周期。

###### 4.尽量避免 undefined 和 null

未赋值的变量默认被赋值为`undefined`

访问不存在的属性`hero.city`时，也会返回`undefined`。

为什么直接使用`undefined`是一个不好习惯？ 因为与`undefined`进行比较时，你正在处理未初始化状态的变量。

变量、对象属性和数组在使用前必须用值初始化

`null`是一个缺失对象的指示符。应该尽量避免从函数返回 `null`，特别是使用`null`作为参数调用函数。

###### 5. 不要使用随意的编码风格，执行一个标准

建议使用 eslint 来规范编码风格

###### 总结：

编写高质量和干净的代码需要纪律，克服不好的编码习惯。

JavaScript是一种宽容的语言，具有很大的灵活性。但是你必须注意你所使用的特性。这里建议是避免使用隐式类型转换，`undefined` 和 `null` 。

现在这种语言发展得相当快。找出复杂的代码，并使用最新 JS 特性来重构。

整个代码库的一致编码风格有益于可读性。良好的编程技能总是一个双赢的解决方案。

###### 6.

##### JS运算符&&和||及其优先级

###### &&(逻辑与)

```
var a = 1 && 2 && 3; //3
var b = 0 && 1 && 2; //0
var c = 1 && 0 && 2; //0
alert(a)
alert(b)
alert(c)
```

a && b，如果a为true，直接返回b，而不管b为true或者false。如果a为false那么直接返回a。

var a = 1 && 2 && 3; 因为1 && 2,1为真，返回2，2&&3，2为真，返回3。

###### ||(逻辑或)

```
var a = 0 || 1 || 2; //1
var b = 1 || 0 ||3; //1
alert(a)
alert(b)
```

||运算遇到true就返回。例如 a||b，如果a为false，直接返回b，而不管b为true或者false。如果a为true，而不会继续往下执行。

###### &&(逻辑与)和||(逻辑或)混合使用的时候要注意他们的优先级：

###### &&优先级高于||

return a && b || c ,
根据a来判断返回值，a 是 false 则肯定返回 c；如果 b , c 都是 true ，那么我们就可以根据 a 来决定b 还是 c ，如果 a 是 false 则返回 c，如果a是true 则返回 b。
return a || b && c
根据优先级相当于先算 b && c ,然后和a 相 或；如果a是true，则返回a，不论是b或c，如果a是false，则如果b是false，返回b，如果b是true，返回c；

```
var a = 3 && 0 || 2; //2
var b = 3 || 0 && 2; //3
var c = 0 || 2 && 3; //3
alert(a)
alert(b)
alert(c)
```

###### 7.

设计模式的定义是：在面向对象软件设计过程中针对特定问题的简洁而优雅的解决方案。

通俗一点说，设计模式是在某种场合下对某个问题的一种解决方案。如果再通俗一点说，设计模式就是给面向对象软件开发中的一些好的设计取个名字。

所有设计模式的实现都遵循一条原则，即“找出程序中变化的地方，并将变化封装起来”。一个程序的设计总是可以分为可变的部分和不变的部分。当我们找出可变的部分，并且把这些部分封装起来，那么剩下的就是不变和稳定的部分。这些不变和稳定的部分是非常容易复用的。这也是设计模式为什么描写的是可复用面向对象软件基础的原因。

###### 分辨模式的关键是意图而不是结构

辨别模式的关键是这个模式出现的场景，以及为我们解决了什么问题。

某种解决方案要成为一种模式，还是有几个原则要遵守的。这几个原则即是“再现” “教学” 和“能够以一个名字来描述这种模式”。

###### 面向对象的JavaScript

编程语言按照数据类型大体可以分为两类，一类是静态类型语言，另一类是动态类型语言。

静态类型语言在编译时便已确定变量的类型，而动态类型语言的变量类型要到程序运行的时候，待变量被赋予某个值之后，才会具有某种类型。

静态类型语言的优点首先是在编译时就能发现类型不匹配的错误，编辑器可以帮助我们提前避免程序在运行期间有可能发生的一些错误。其次，如果在程序中明确地规定了数据类型，编译器还可以针对这些信息对程序进行一些优化工作，提高程序执行速度。

静态类型语言的缺点首先是迫使程序员依照强契约来编写程序，为每个变量规定数据类型，归根结底只是辅助我们编写可靠性高程序的一种手段，而不是编写程序的目的，毕竟大部分人编写程序的目的是为了完成需求交付生产。其次，类型的声明也会增加更多的代码，在程序编写过程中，这些细节会让程序员的精力从思考业务逻辑上分散开来。
动态类型语言的优点是编写的代码数量更少，看起来也更加简洁，程序员可以把精力更多地放在业务逻辑上面。虽然不区分类型在某些情况下会让程序变得难以理解，但整体而言，代码量越少，越专注于逻辑表达，对阅读程序是越有帮助的。
动态类型语言的缺点是无法保证变量的类型，从而在程序的运行期有可能发生跟类型相关的错误。

在JavaScript中，当我们对一个变量赋值时，显然不需要考虑它的类型，因此，JavaScript是一门典型的动态类型语言。

动态类型语言对变量类型的宽容给实际编码带来了很大的灵活性。由于无需进行类型检测，我们可以尝试调用任何对象的任意方法，而无需去考虑它原本是否被设计为拥有该方法。

这一切都建立在鸭子类型（duck typing）的概念上，鸭子类型的通俗说法是：“如果它走起路来像鸭子，叫起来也是鸭子，那么它就是鸭子。”

鸭子类型指导我们只关注对象的行为，而不关注对象本身，也就是关注HAS-A，而不是IS-A。

###### 多态

‘多态’一词源于希腊文polymorphism，拆开来看是poly(复数) + morph(形态) + ism，从字面上我们可以理解为复数形态。

多态的实际含义是：同一操作作用于不同的对象上面，可以产生不同的解释和不同的执行结果。换句话说，给不同的对象发送同一个消息的时候，这些对象会根据这个消息分别给出不同的反馈。

多态背后的思想是将“做什么” 和 “谁去做以及怎样去做” 分离开来，也就是将“ 不变的事物” 与 “可能改变的事物”分离开来。

使用继承来得到多态效果，是让对象表现出多态性的最常用手段。继承通常包括实现继承和接口继承。

多态的思想实际上是把“做什么”和“谁去做”分离开来，要实现这一点，归根结底先要消除类型之间的耦合关系。

###### 封装

封装的目的是将信息隐藏。一般而言，我们讨论的封装是封装数据和封装实现。这一节将讨论更广义的封装，不仅包括封装数据和封装实现，还包括封装类型和封装变化。

封装的目的是将信息隐藏，封装应该被视为“任何形式的封装”，也就是说，封装不仅仅是隐藏数据，还包括隐藏实现细节、设计细节以及隐藏对象的类型等。

从设计模式的角度出发，封装在更重要的层面体现为封装变化。

《设计模式》一书中共归纳总结了23种设计模式。从意图上区分，这23 种设计模式分别被划分为创建型模式、结构型模式和行为型模式。

###### 8.

##### 什么是闭包

闭包就是指有权访问另一个函数作用域中的变量的函数。

##### 什么是作用域

作用域是一个变量和函数的作用范围，JS中函数内声明的所有变量在函数体内始终是可见的，在ES6前有全局作用域和局部作用域，但是没有块级作用域（catch只在其内部生效），局部变量的优先级高于全局变量。

##### 作用域链

JavaScrit中有一个执行上下文（execution context）的概念,它定义了变量或函数有权访问的其它数据，决定了他们各自的行为。每个执行环境都有一个与之关联的变量对象，环境中定义的所有变量和函数都保存在这个对象中。

当访问一个变量时，解释器会首先在当前作用域查找标示符，如果没有找到，就去父作用域找，直到找到该变量的标示符或者不在父作用域中，这就是作用域链。

作用域链和原型继承查找是的区别：如果去查找一个普通对象的属性，但是在当前对象和其原型中都找不到时，会返回undefined；但查找的属性在作用域链中不存在的话就会抛出ReferenceError。

作用域链的顶端是全局对象，在全局环境中定义的变量就会绑定到全局对象中。

闭包的作用域链包含着它自己的作用域，以及包含它的函数的作用域和全局作用域。

##### 闭包的特性

1. 函数嵌套函数
2. 内部函数可以访问外部作用域（或外部函数）的变量和参数
3. 参数和变量不会被回收机制回收，一直存在于内存中，除非手动清除

##### 为什么要用闭包

1. 希望变量长期存在内存中
2. 避免全局变量污染

###### JavaScript中的函数运行在它们被定义的作用域里，而不是它们被执行的作用域里。--<<JavaScript权威指南>>

var name = 'Jane';

function getName(){

  console.log(name);

}

function myName(){

   var name = 'haha';

   getName();

}

myName();//Jane

##### 闭包的缺陷

闭包的缺点就是常驻内存会增大内存使用量，并且使用不当很容易造成内存泄漏。

如果不是因为某些特殊任务而需要闭包，在没有必要的情况下，在其他函数中创建函数式不明智的，因为闭包对脚本性能具有负面影响，包括处理速度和内存消耗。

https://juejin.im/post/5d7864e46fb9a06b051818e8

总结：

闭包是函数在特定情况下执行产生的一种现象，这种现象产生的条件为：当一个函数（outer）运行时它的形参或者它的局部变量被其他函数所引用。满足这个条件时，那么这个outer就会形成闭包，这个闭包所包含的变量为outer本身运行时被其他函数引用到的所有形参和局部变量。形参和变量被引用的方式可以是内部直接运行（包括自执行匿名函数）、被赋值给外部变量、或者是被return。

##### 闭包的应用

1）在循环中找到正确的索引

2）模块化开发

3）柯里化

###### 9.

This指向

1.在全局环境中的this指向全局对象本身（在浏览器环境下指向window）

2.函数环境中的this，由调用函数的方式决定

​         a.如果函数是独立调用，那this指向undefined。但是在非严格模式中，当this指向undefined时，它会被自动指向全局对象。

​         b.如果函数是被某一个对象调用，那this指向这个对象。

​         注意：箭头函数根本没有自己的this，它的this就是外层代码块的this。它的this始终是固定的。

3.构造函数与原型里的this

​          a.构造函数里的this指向实例

​           b.原型里的this指向实例

const obj = {
    a:10,
    c:this.a + 20,
    fn:function(){
	    return this.a;
    }
};
obj.c; //NaN
obj.fn(); //10



obj = {
	a: 1,
	getA() {
	console.log("getA: ", **this**.a);
	}
};
obj.getA(); //1
x = obj.getA;
x();//undefined
setTimeout(obj.getA, 100);//2   undefined 首先定时器会返回一个执行的ID。其次返回this.a的值。

如果没有特殊指向，setInterval和setTimeout的回调函数中this的指向都是window。这是因为JS的定时器方法是定义在window下的。

##### []语法中的this关键字

```
function fn(){console.log(this)}
function fn2(){alert(1)}
var arr = [fn,fn2]
arr[0]()//这里的this又是什么呢？
```

愉快的转换

```
arr[0]()
假想为 arr.0()
然后转换为 arr.0.call(arr)
那么里面的this就是arr了
```

##### 默认绑定的另一种情况

在函数中以函数作为参数传递，例如setTimeout和setInterval等，这些函数中传递的函数中的this指向，在非严格模式指向的是全局对象。

```
var name = 'koala';
var person = {
	name : '程序员成长指北',
	sayHi : sayHi
};
function sayHi(){
	setTimeout(function(){
		console.log('Hello',this.name);
	})
}
person.sayHi();
setTimeout(function(){
	person.sayHi();
},200);
```

##### 隐式绑定的另一种情况

当有多层对象嵌套调用某个函数的时候，例如对象.对象.函数，this指向的是最后一层对象。

```
function sayHi(){
	console.log('hello,',this.name);
}
var person2 = {
	name : "程序员成长指北",
	sayHi : sayHi
}
var person1 = {
	name : 'koala',
	friend : person2
}
person1.friend.sayHi();//hello, 程序员成长指北
```

##### 显示绑定

显示绑定，通过函数call apply bind 可以修改函数this的指向。call与apply方法都是挂载在Function原型下的方法，所有的函数都能使用。

call 和 apply的第一个参数会绑定到函数体的this上，如果不传参数，非严格模式，this默认绑定到全局对象。

###### call和apply的注意点

这两个方法在调用的时候，如果我们传入数字或者字符串，这两个方法会把传入的参数转成对象类型。

```
var number = 1,string = '程序员成长指北';
function getThisType(){
	var number = 3;
	console.log('this指向内容',this);
	console.log(typeof this);
}
getThisType.call(number);//this指向内容 Number{1} //object
getThisType.apply(string);//this指向内容 String{'程序员成长指北'} //object
```

###### bind函数

bind方法会创建一个新函数。当这个新函数被调用时，bind()的第一个参数将作为它运行时的this，之后的一序列参数将会在传递的实参前传入作为它的参数。

##### this绑定优先级

new绑定 > 显示绑定 > 隐式绑定 > 默认绑定

##### 箭头函数中的this

###### 箭头函数

- 箭头函数中没有arguments
- 箭头函数没有构造函数
- 箭头函数没有原型
- 箭头函数中没有super
- 箭头函数中没有自己的this
- 无论在严格模式还是非严格模式下，箭头函数都不能具有重复的命名参数
- 在函数的整个生命周期中，箭头函数内部的值保持不变，并且总是与接近的非箭头函数中的值绑定。

箭头函数中的this直接指向的是调用函数的上一层运行时。

##### 自执行函数

什么是自执行函数？自执行函数式在代码定以后，无需调用，会自动执行。

```
(function(){
	console.log('程序员成长指北')
})()
```

或者

```
(function(){
	console.log('程序员成长指北')
}())
```

但是如果使用了箭头函数简化一下就只能使用第一种情况了。使用第二种情况简化会报错。

```
(()=>{
	console.log('程序员成长指北');
})()
```

###### 10.

##### JS对象属性

JS有三种不同的属性：数据属性、访问器属性和内部属性。

###### 数据属性（properties）

对象的普通属性将字符串名称映射到值。例如，下面对象obj有一个数据属性，名称为prop，对应的值为123

```
var obj = {
	prop : 123
};
```

可以用以下方式读取属性的值：

```
consoe.log(obj.prop);//123
console.log(obj['prop']);//123
```

当然也可以用以下方式来设置属性的值：

```
obj.prop = 'abc';
obj['prop'] = 'abc';
```

###### 访问器属性

l另外，可以通过函数处理获取和设置属性值。这些函数称为访问器函数。处理获取的函数称为getter。处理设置的函数称为setter。

```
var obj = {
	get prop (){
		return 'Getter';
	},
	set prop (value){
		console.log('Setter: ' + value);
	}
}
```

访问obj属性

```
obj.prop  //'Geter'
obj.prop 123; //Setter: 123
```

###### 内部属性

一些属性只是用于规范，这些属于‘内部’的内部，因为它们不能直接访问，但是它们确实影响对象的行为。内部属性有特殊的名称都写在两个方括号，如：

- 内部属性[[Prototype]]指向对象的原型。它可以通过Object.getPrototypeof()读取。它的值只能通过创建具有给定原型的新对象来设置，例如通过object.create()或__ proto __ 。
- 内部属性[[Extensible]]决定是否可以向对象添加属性。可以通过Object.isExtensibe()方法判断一个对象是否是可扩展的（是否可以在它上面添加新的属性）。可以通过Object.preventExtensions()方法让一个对象变的不可扩展，也就是永远不能再添加新的属性。

##### 属性特性

属性的所有状态，包括数据和元数据，都存储在特性（attribute）中。它们是属性具有的字段，就像对象具有属性一样。特性（attribute）键通常用双括号编写：

###### 以下特性是属于数据属性：

[[Value]]：该属性的属性值，默认为undefined

[[Writable]]：是一个布尔值，表示属性值（value）是否可改变（即是否可写），默认为true。

###### 以下特性是属于访问器属性：

[[Get]]：是一个函数，表示该属性的取值函数（getter），默认为undefined

[[Set]] ：是一个函数，表示该属性的存值函数（setter）,默认为udefined

###### 所有的属性都具有以下特性：

[[Enumerable]]：是一个布尔值，表示该属性是否可遍历，默认为true。如果设为false，会使得某些操作（比如for...in循环、Object.keys()）跳过该属性。

[[Configurable]]：是一个布尔值，表示可配置性，默认为true。如果设为false，将阻止某些操作改写该属性，比如无法删除该属性，也不得改变该属性的属性描述对象（value属性除外）。也就是说，configurable属性控制了属性描述对象的可写性。

###### 11.

JS并不是多线程，只是提供了事件循环机制来模拟多线程。

单线程 -》同步模式

多线程-》异步模式

异步任务放在任务队列（消息队列）里。

任务队列  宏任务： DOM操作、用户交互（事件）、ajax、定时器

​                  微任务：  promise 、process.nextTick 、MutationObserver

先走微任务（都执行完）再走宏任务。

执行顺序：先是全局之后是微任务再是宏任务。

执行上下文：函数代码在函数执行上下文中执行，全局代码在全局执行上下文中执行。每个函数都有自己的执行上下文。

调用栈（也称为执行堆栈）：调用堆栈顾名思义是一个具有LIFO(后进先出)结构的堆栈，用于存储在代码执行期间创建的所有执行上下文。

JS只有一个调用栈，因为它是一种单线程编程语言。调用堆栈具有LIFO结构，这意味着项目只能从堆栈顶部添加或删除。

事件轮询、web api 和消息队列不是JavaScript引擎的一部分，而是浏览器的JavaScript运行时环境的一部分。

事件轮询

事件轮询的工作是监听调用堆栈，并确定调用堆栈是否为空。如果调用堆栈是空的，它将检查消息队列，看看是否有任何挂起的回调等待执行。

在这种情况下，消息队列包含一个回调，此时调用堆栈为空。因此，事件轮询将回调推到堆栈的顶部。

###### 12.

JS实现异步编程的方法

##### 1）回调函数（Callback）

###### 回调是一个函数被作为一个参数传递到另一个函数里，在那个函数执行完后再执行。

回调函数是异步操作最基本的方法。以下代码就是一个回调函数的例子：

```
ajax(url,()=>{
	//处理逻辑
})
```

但是回调函数有一个致命的弱点，就是容易写出回调地狱（Callback hell）。假设锁哥请求存在依赖性，你可能就会写出如下代码：

```
ajax(url,()=>{
	//处理逻辑
	ajax(url1,()=>{
		//处理逻辑
		ajax(url2,()=>{
			//处理逻辑
		})
	})
})
```

回调函数的优点是简单、容易理解和实现，缺点是不利于代码的阅读和维护，各个部分之间高度耦合，使得程序结构混乱、流程难以追踪（尤其是多个回调函数嵌套的情况），而且每个人任务只能指定一个回调函数。此外它不能使用try catch捕获错误，不能直接return。

##### 2）事件监听

这种方式下，异步任务的执行不取决于代码的顺序，而取决于某个事件是否发生。

下面是两个函数f1和f2，编程的意图是f2必须等到f1执行完成，才能执行。首先，为f1绑定一个事件（这里采用的jQuery的写法）

```
f1.on('done',f2);
```

上面这行代码的意思是，当f1发生done事件，就执行f2。然后，对f1进行改写:

```
function f1(){
	setTimeout(function(){
		//...
		f1.trigger('done');
	},1000);
}
```

上面代码中，f1.trigger('done')表示，执行完成后，立即出发done事件，从而开始执行f2。

这种方法的优点是比较容易理解，可以绑定多个事件，每个事件可以指定多个回调函数，而且可以“去耦合”，有利于实现模块化。缺点是整个程序都要变成事件驱动型，运行流程会变得很不清晰。阅读代码的时候，很难看出主流程。

##### 3）发布订阅

我们假定，存在一个“信号中心”，某个任务执行完成，就向信号中心“发布”（publish）一个信号，其他任务可以向信号中心“订阅”（subscribe）这个信号，从而知道什么时候自己可以开始执行。这就叫做“发布/订阅模式”（publish-subscribe pattern）,又称“观察者模式”（observer pattern）。

首先，f2向信号中心jQuery订阅done信号。

```
jQuery.subscribe('done',f2);
```

然后，f1进行如下改写：

```
function f1(){
	setTimeout(function(){
		//...
		jQuery.publish('done');
	},1000)
}
```

上面代码中，jQuery.publish('done')的意思是，f1执行完成后，向信号中心jQuery发布done信号，从而引发f2的执行。

f2完成执行后，可以取消订阅（unsubscribe）

```
jQuery.unsubscribe('done',f2);
```

这种方法的性质与“事件监听”类似，但是明显优于后者。因为可以通过查看“消息中心”，了解存在多少信号、每个信息有多少订阅者，从而监控程序的运行。

##### 4）Promise/A+

Promise本意是承诺，在程序中的意思就是承诺我过一段时间后会给你一个结果。什么时候会用到过一段时间？答案是异步操作，异步是指可能较长时间才有结果的才做，例如网络请求、读取本地文件等。

Promise是一种更强大的异步处理方式，而且它有统一的API和规范。

```
var promise = loginByPromise('http://www.r9it.com/login.php');
promise.then(function(result){
	//登录成功时处理
}).catch(function(error){
	//登录失败时处理
})
```

有了Promise对象，就可以将异步操作以同步操作的流程表达出来。这样在处理多个异步操作的时候还可以避免了层层嵌套的回调函数。此外，Promise对象提供统一的接口，必须通过调用Promise#then和Promise#catch这两个方法来结果，除此之外其他的方法都是不可用的，这样使得异步处理操作更加容易。

在ES6中，可以使用三种办法创建Promise实例（对象）

（1）构造方法

```
let promises = new Promise((resolve,reject)=>{
	resolve();//异步处理
});
```

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由JavaScript引擎提供，不用自己部署。

（2）通过Promise实例的方法，Promise#then方法返回的也是一个Promise对象

```
promise.then(onFulfilled, onRejected);
```

(3)通过Promise的静态方法，Promise.resolve()，Promise.reject()

```
var p = Promise.resolve();
p.then(function(value){
	console.log(value);
});
```

###### Promise的执行流程

- new Promise构造器之后，会返回一个promise对象
- 为promise注册一个事件处理结果的回调函数（resolved）和一个异常处理函数（rejected）

###### Promise的状态

实例化的Promise有三个状态：

Fulfilled: has-resolved，表示成功解决，这时会调用onFulfilled

Rejected:has-rejected,表示解决失败，此时会调用onRejected

Pending:unresolve，表示待解决，既不是resolve也不是reject的状态。也就是promise对象刚被创建后的初始化状态。

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。resolve函数的作用是，将Promise对象的状态从未处理变成处理成功（unresolved=>resolve）,在异步操作成功时调用，并将异步操作的结果作为参数传递出去。reject函数的作用是，将Promise对象的状态从未处理变成处理失败（unresolved=>rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

Promise实例生成以后，可以用then方法和catch方法分别指定resolved状态和rejected状态的回调函数。

只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。 

一旦状态改变，就不会再变，任何时候都可以得到这个结果 Promise对象的状态改变，只有两种可能：从 pending 变为 fulfilled 和从 pending 变为 rejected。 只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。 如果改变已经发生了，你再对 Promise 对象添加回调函数，也会立即得到这个结果。 这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的

```
var promise = new Promise((fuck, reject) => {
 resolve("xxxxx");
 //下面这行代码无效，因为前面 resolve 方法已经将 Promise 的状态改为 resolved 了
 reject(new Error()); 
});
 
promise.then((value) => { 
 console.log(value);
})

```

###### Promise 的执行顺序

```
var p = new Promise((resolve, reject) => {
 console.log("start Promise");
 resolve("resolved"); 
});
 
p.then((value) => {
 console.log(value);
}) 
 
console.log("end Promise");

```

当我们在构造 Promise 的时候，构造函数内部的代码是立即执行的

###### promise的链式调用

- 每次调用返回的都是一个新的Promise实例(这就是then可用链式调用的原因)
- 如果then中返回的是一个结果的话会把这个结果传递下一次then中的成功回调
- 如果then中出现异常,会走下一个then的失败回调
- 在 then中使用了return，那么 return 的值会被Promise.resolve() 包装(见例1，2)
- then中可以不传递参数，如果不传递会透到下一个then中(见例3)
- catch 会捕获到没有捕获的异常

它也是存在一些缺点的，比如无法取消 Promise，错误需要通过回调函数捕获。

![img](https://upload-images.jianshu.io/upload_images/3174701-460247a0a17248ca?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

promise是ECMAScript6管理异步代码的标准方式，javascript库使用promise管理ajax，动画和其他典型的异步交互。简单的说，它的思想是：每一个异步任务返回一个promise对象，该对象有一个then方法，允许指定回调函数。比如f1的回调函数f2，可以写成：

```
f1.then(f2);
```

回调函数写成了链式写法，程序的流程可以看得很清楚，而且有一整套的配套方法，可以实现很多强大的功能。

##### 5）生成器Generators/yield

Generator函数是ES6提供的一种异步编程解决方案，语法行为与传统函数完全不同，Generator最大的特点就是可以控制函数的执行。

- 语法上，首先可以把它理解成，Generator函数是一个状态机，封装了多个内部状态。
- Generator函数除了状态机，还是一个遍历器对象生成函数。
- 可暂停函数，yield可暂停，next方法可启动，每次返回的是yield后的表达式结果。
- yield表达式本身没有返回值，或者说总是返回undefined。next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值。

##### 6）async/await

使用async/await，你可以轻松地达到之前使用生成器和CO函数所做到的工作，它有如下特点：

- async/await是基于Promise实现的，它不能用于普通函数的回调函数
- async/await 与Promise一样，是非阻塞的
- async/await使得异步代码看起来像同步代码，这正是它的魔力所在

###### 一个函数如果加上async ,那么该函数就会返回一个Promise

```
async function async1(){
	return '1';
}
console.log(async1())//Promise {<resolved>: "1"}
```

await关键字只能用在async定义的函数内。async函数会隐式地返回一个promise,该promise的resolve值就是函数return的值。

JS异步编程进化史：callback->（事件监听->发布订阅）->Promise->Generator->async+await

async/await函数的实现，就是将Generator函数和自动执行器，包装在一个函数里。

async/await可以说是异步终极解决方案了。

（1）async/await函数相对于Promise，优势体现在：

- 处理then的调用链，能够更清晰准确的写出代码
- 并且也能优雅地解决回调地狱问题

当然async/await函数也存在一些缺点，因为await将异步代码改造成了同步代码，如果多个异步代码没有依赖性却使用了await会导致性能上的降低，代码没有依赖性的话，完全可以使用Promise.all的方式。

（2）async/await函数对Generator函数的改进，体现在以下三点：

- 内置执行器 ---Generator函数的执行必须靠执行器，所以才有了co函数库，而async函数自带执行器。也就是说，async函数的执行，与普通函数一模一样，只要一行。

```
var result = asyncReadFile();
```

- 更广泛的使用性。---co函数库约定，yield命令后面只能是Thunk函数或者Promise对象，而async函数的await命令后面，可以跟Promise对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作）
- 更好的语义。---async和await,比起星号和yield，语义更清楚了。async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果。

###### 为什么async/await 更好？

1.简洁  使用async/await 明显节约了不少代码。不需要写.then，不需要写匿名函数处理Promise的resolve值，也不需要定义多余的data变量，还避免了嵌套代码。

2. 错误处理  async/await 让try/catch可以同时处理同步和异步错误。
3. 条件语句
4. 中间值
5. 错误栈  Promise链中返回的错误栈没有给出错误发生位置的线索。更糟糕的是，它会误导我们。然而，async/await中的错误栈会指向错误所在的函数。
6. 调试 最后一点，也是非常重要的一点在于，async/await能够使得代码调试更简单。2个理由使得调试Promise变得非常痛苦：1）不能在返回表达式的箭头函数中设置断点 2）如果你在.then代码块中设置断点，使用Step Over快捷键，调试器不会跳到下一个.then，因为它只会跳过异步代码。

###### 	异步编程的最高境界，就是根本不用关心它是不是异步。

###### async函数的实现

async函数的实现，就是将Generator函数和自动执行器，包装在一个函数里。

```
async function fn(args){
	//...
}
//等同于
function fn(args){
	return spawn(function *(){
		//...
	})
}
```

所有的async函数都可以写成上面的第二种形式，其中的spawn函数就是自动执行器。

注意点：await命令后面的Promise对象，运行结果可能是rejected，所以最好把await命令放在try...catch代码块中。

```
async funciton myFunciton(){
	try{
	  await somethingThatReturnsAPromise();
	}catch(err){
	  console.log(err);
	}
}
//另一种写法
async function myFunction(){
	await somethingThatReturnsAPromise().catch(function(err){
		console.log(err);
	})
}
```

await命令只能用在async函数之中，如果用在普通函数，就会报错。

如果确实希望多个请求并发执行，可以使用Promise.all方法。

async函数会返回一个promise，并且Promise对象的状态值是resolve（成功的）。

如果你没有在async函数中写return，那么Promise对象resolve的值就是undefined。如果你写了return，那么return的值就会作为你成功的时候传入的值。

await 等到之后，做了一件什么事情？

那么右侧表达式的结果，就是await要等的东西。等到之后，对于await来说，分2个情况

不是promise对象、是promise对象。如果不是promise，await会阻塞后面的代码，先执行async外面的同步代码，同步代码执行完，再回到async内部，把这个非promise的东西，作为await表达式的结果。如果它等到的是一个promise对象，await也会暂停async后面的代码，先执行async外面的同步代码，等着Promise对象fulfilled，然后把resolve的参数作为await表达式的运算结果。

如果async里的代码都是同步的，那么这个函数被调用就会同步执行。

如果在await后面接的这个promise都是同步的，后面的promise会同步执行，但是拿到这个指还是得等待（特别注意：如果promise没有一个成功的值传入，对await来说就算是失败了，下面的代码就不会执行），所以不管await后面的代码是同步还是异步，await总是需要时间，从右向左执行，先执行右侧的代码，执行完后，发现有await关键字，于是让出线程，阻塞代码。

###### 13.

#### JS常用正则表达式备忘录

##### 匹配正则

使用.test()方法

/string/.test('My test string')  //true

##### 匹配多个模式

使用操作符号|

const regex = /yes|no|maybe/;

##### 忽略大小写

使用i标志表示忽略大小写

/ignore case/i.test('We use the i flag to iGnOrE cASe');  //true

##### 提取变量的第一个匹配项

使用.match()方法

const match = "Hello World!".match(/hello/i);

##### 提取数组中的所有匹配项

使用g标志

"Repeat repeat rePeAT".match(/Repeat/gi)

##### 匹配任意字符

使用通配符.作为任何字符的占位符

##### 用多种可能性匹配单个字符

使用字符类，你可以使用它来定义要匹配的一组字符

把它们放在方括号里[]

 "cat fat bat mat".match(/[cfm]at/g)

##### 匹配字母表中的字母

使用字符集内的范围[a-z]

##### 匹配单个未知字符

要匹配不想拥有的一组字符，使用否定字符集^

##### 匹配一行中出现一次或多次的字符

使用+标志

##### 匹配连续出现零次或多次的字符

使用星号*

##### 惰性匹配

字符串中与给定要求匹配的最小部分

默认情况下，正则表达式是贪婪的（匹配满足给定要求的字符串的最长部分）

使用？阻止贪婪模式（惰性匹配）

##### 匹配起始字符串模式

插入符号^,但要放大开头，不要放到字符集中

##### 匹配结束字符串模式

使用$来判断字符串是否是以规定的字符串结尾。

匹配所有字母和数字

\w

##### 除了字母和数字，其他的都要匹配

\W

##### 匹配所有数字

\d

##### 匹配所有非数字

\D

##### 匹配空格

\s匹配空格和回车符

##### 匹配非空格

\S

##### 匹配的字符数

可以使用{下界，上界}指定一行中的特定字符数。

##### 匹配最低个数的字符数

使用`{下界, }`定义最少数量的字符要求,下面示例表示字母 `i` 至少要出现2次

##### 匹配精确的字符数

使用`{requiredCount}`指定字符要求的确切数量

##### 匹配0次或1次

使用 `?` 匹配字符 0 次或1次

###### 14.

1.new Set() ES6 利用set数据结构可以实现数组去重

```
let arr = [1, 2, 3, 3];
let set = new Set(arr);
let newArr = Array.from(set);
console.log(newArr); // [1, 2, 3]
```

2.Object.assign() ES6 可以用于对象的合并拷贝

```
let obj1 = {a: 1};
let obj2 = {b: 2};
let obj3 = Object.assign({}, obj1, obj2);
console.log(obj3); //{a: 1, b: 2}
```

3.map()

map方法用于遍历数组，有返回值，可以对数组的每一项进行操作并生成一个新的数组，有些时候可以代替for和foEach循环，简化代码，比如：

```
let arr3 = [1, 2, 3, 4, 5];
let newArr3 = arr3.map((e, i)=> e * 10);
console.log(newArr3); //[10, 20, 30, 40, 50]
```

4.filter()

filter方法同样用于遍历数组，顾名思义，就是过滤数组，在每一项元素后面触发一个回调函数，通过判断，保留或移除当前项，最后返回一个新的数组，比如：

```
let arr4 = [1, 2, 3, 4, 5];
let newArr4 = arr4.filter((e,i) => e % 2 === 0);
console.log(newArr4);//[2, 4]
```

5.some()

some方法用于遍历数组，在每一项元素后面触发一个回调函数，只要一个满足条件就返回true，否则返回false，类似于||比较，比如：

```
let arr5 = [{result: true},{result: false}];
let newArr5 = arr5.some((e, i)=> e.result);
console.log(newArr5); // true
```

6.every()

every方法用于遍历数组，在每一项元素后面触发一个回调函数，只要一个不满足条件就返回false，否则返回true，类似于&&比较，比如：

```
let arr6 = [{result: true},{result: false}];
let newArr6 = arr6.every((e, i)=> e.result);
console.log(newArr6); // false
```

7.~~运算符

~符号用在JavaScript中有按位取反的作用，~~即是取反两次，而位运算的造作值要求是整数，其结果也是整数，所以经过位运算的都会自动变成整数，可以巧妙的去掉小数部分，类似于parseInt，比如：

```
let a = 1.23;
let b = -1.23;
console.log(~~a); //1
console.log(~~b); //-1
```

8.||运算符

巧妙的使用||运算符我们可以给变量设置默认值，比如：

```
let c = 1;
let d = c || 2;
console.log(d); //1
```

9 ...运算符

...运算符是ES6中用于解构数组的方法，可以用于快速获取数组的参数，比如：

```
let [num1, ...num2] = [1, 2,3];
console.log(num1); //1
console.log(num2); //[2,3]
```

10.三元运算符

该运算符应该大家都比较熟悉，在默写情况下可以简化if else的写法，比如：

```
let e = true, f= '';
if(e){
	f = 'man';
}else{
	f = 'woman';
}
//等同于
 f = e ? 'man' : 'woman';JavaScript巧学巧用
```

##### 为什么 typeof null === 'object'

在定义中，null意为no object value ，而undefined 意为no value .

事实上，这是当时受到了 Java 的影响。在 Java 中，`null`从来就不是一个单独的类型，**它代表的是所有引用类型的默认值**。这就是为什么尽管规范中规定了`null`有自己单独的`Null`类型，而`typeof null`仍旧返回`'object'`的原因。

ECMAScript标准约定number数字需要被当成64位双精度浮点数处理，但事实上，一直使用64位去存储任何数字实际是非常低效的，所以JavaScript引擎并不总会使用64位去存储数字，引擎在内部可以采用其他内存表示方式（如32位），只要保证数字外部所有能被监测到的特性对齐64位的表现就行。

Float64的整数安全范围是53位，超过这个范围数值会失去精度

Smi、HeapNumber

针对31位有符号位范围内的整型数字，V8为其定义了一种特殊的表示法Smi，其他任何不属于Smi的数据都被定义为HeapObject，HeapObject代表着内存的实体地址。

对于数字而言，非Smi范围内的数字被定义为HeapNumber，HeapNumber是一种特殊的HeapObject。

当一个数字内存区域拥有一个非Smi范围内的数值时，V8会将这块区域标志为Double区域，并会为其分配一个用64位浮点表示的MutableHeapNumber实例。

我们深入讨论了以下知识点：

- JavaScript 底层对`primitives`和`objects`的区分，以及`typeof`的不准确原因。
- 即使变量的值拥有相同的类型，引擎底层也可以使用不同的内存表示方式去存储。
- V8 会尝试找一个最优的内存表示方式去存储你 JavaScript 程序中的每一个属性。
- 我们讨论了 V8 针对 Shape 初始化、弃用与迁移的处理方案。

基于这些知识，我们可以得出一些能帮助提高性能的 JavaScript 编码最佳实践：

- **尽量用相同的数据结构去初始化你的对象，这样对 Shape 的利用是最高效的。**
- **为你的变量选择合理的初始值，让 JavaScript 引擎可以直接使用对应的内存表示方式。**
- **write readable code, and performance will follow**

##### Chrome控制台打印的默认行为

如果最后执行语句或者表达式没有return返回值，默认undefined。

```
var a = 1;
//undefined
------------------------------------------
console.log(a)
//1
//undefined
-------------------------------------------
function a(){console.log(1)}
a();
//1
//undefined
---------------------------------------------
function b(){return console.log(1)}
b();
//1
//undefined
----------------------------------------------
function c(){return 1}
c();
//1
```

arguments是一个对应于传递给函数的参数的类数组对象。

有一种特殊情况：箭头函数中没有arguments。但这不是问题，可以使用剩余参数访问箭头函数内的所有参数。

剩余参数语法允许咱们将一个不定数量的参数表示为一个数组。

##### 剩余参数和arguments对象的区别

- 剩余参数只包含那些没有对应形参的实参，而arguments对象包含了传给函数的所有实参。
- arguments对象不是一个真正的数组，而剩余参数是真正的Array实例，也就是说你能够在它上面直接使用所有的数组方法，比如，sort, map, forEach, 或pop。
- arguments对象还有一些附加的属性（如callee属性）

##### 箭头函数 VS bind

箭头函数 => 和正常函数通过.bind(this)调用有一个微妙的区别：

- .bind(this)创建该函数的‘绑定版本’
- 箭头函数=>不会创建任何绑定。该函数根本没有this。在外部上下文中，this的查找与普通变量搜索完全相同。

##### GET和POST的区别

###### GET

GET请求可被缓存

GET请求保留在浏览器历史记录中

GET请求可被收藏为书签

GET请求不应在处理敏感数据时使用

GET请求有长度限制（2048字符），IE和Safari浏览器限制2k;Opera限制4k;Firefox,Chrome限制8k

GET请求只应当用于取回数据

###### POST

POST请求不会被缓存

POST请求不会保留在浏览器历史记录中

POST不能被收藏为书签

POST请求对数据长度没有要求

##### This对象的理解

特殊的是，IE中的attachEvent中的this总是指向全局对象window。

##### eval是做什么的？

它的功能是把对应的字符串解析成JS代码并运行；应该避免使用eval，不安全，非常耗性能（2次，一次解析成js语句，一次执行）。由JSON字符串转换为JSON对象的时候可以用eval,

```
var obj = eval('('+ str +')');
```

##### 什么是window对象？什么是document对象？

window对象是指浏览器打开的窗口。document对象是Document对象（HTML文档对象）的一个只读引用，window对象的一个属性。

##### 什么是闭包（closure），为什么要用它？

闭包是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量，利用闭包可以突破作用链域，将函数内部的变量和方法传递到外部。

##### javascript代码中的‘use strict’是什么意思?使用它区别是什么？

使JS编码更加规范化的模式，消除javascript语法的一些不合理、不严谨之处，减少一些怪异行为。默认支持的槽糕特性都会被禁用，比如不能用with，也不能在意外的情况下给全局变量赋值；全局变量的显示声明，函数必须声明在顶层，不允许在非函数代码块内声明函数，arguments.callee也不允许使用；保证代码允许的安全，限制函数中的arguments修改；提高编译器效率，增加运行速度。

##### 如何解决跨域问题？

```
jsonp、iframe、window.name、window.postMessage、服务器上设置代理页面
```

##### 模块化开发怎么做？

立即执行函数，不暴露私有成员

##### 事件的节流（throttle）与防抖(debounce)

防抖和节流函数来提升页面速度和性能。这两兄弟的本质都是以闭包的形式存在。通过对事件对应的回调函数进行包裹、以自由变量的形式缓存时间信息，最后用setTimeout来控制事件的触发频率。

##### Throttle:第一个人说了算

throttle的主要思想在于：在某段时间内，不管你触发了多少次回调，都只认第一次，并在计时结束时给予响应。

##### Debounce:最后一个参赛者说了算

防抖的主要思想在于：我会等你到底。在某段时间内，不管你触发了多少次回调，我都只认最后一次。

###### 15.

JavaScript的最初版本是这样区分的：**null是一个表示"无"的对象，转为数值时为0；undefined是一个表示"无"的原始值，转为数值时为NaN。**

> ```javascript
> Number(undefined)
> // NaN
> 
> 5 + undefined
> // NaN
> ```

##### 目前的用法

但是，上面这样的区分，在实践中很快就被证明不可行。目前，null和undefined基本是同义的，只有一些细微的差别。

**null表示"没有对象"，即该处不应该有值。**典型用法是：

> （1） 作为函数的参数，表示该函数的参数不是对象。
>
> （2） 作为对象原型链的终点。

> ```javascript
> Object.getPrototypeOf(Object.prototype)
> // null
> ```

**undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。**典型用法是：

> （1）变量被声明了，但没有赋值时，就等于undefined。
>
> （2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
>
> （3）对象没有赋值的属性，该属性的值为undefined。
>
> （4）函数没有返回值时，默认返回undefined。

##### 精确判断 JavaScript 值的类型

```
var a = ['b','c'];Object.prototype.toString.call(a);
```

之后会返回 `"[object Array]"` 这个值是绝对准确的，

- [Array.prototype.forEach()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) ： 把数组每个元素丢到一个处理 function 里面，相当于 for 循环。
- [Array.prototype.every() ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every)： 所有数组元素处理后全部 return true，那么就 return true，有点像 &&。
- [Array.prototype.some()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some) ： 只要一个数组元素处理后 return true，那么就 return true，有点像 ||。
- [Array.prototype.filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) ： 将处理时 return true 的数组元素拿出来组成一个新数组。
- [Array.prototype.map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) ： 对每个数组元素进行处理，并组成一个新数组。
- [Array.prototype.reduce()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) ： 像一个累加器一样，把数组元素全部加起来（从左向右）。
- [Array.prototype.reduceRight()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight) ： 同上，顺序从右向左。

最新的 ECMAScript 标准定义了 8 种数据类型:

- 7 种原始类型:
  - [Boolean](https://developer.mozilla.org/zh-CN/docs/Glossary/Boolean)
  - [Null](https://developer.mozilla.org/zh-CN/docs/Glossary/Null)
  - [Undefined](https://developer.mozilla.org/zh-CN/docs/Glossary/undefined)
  - [Number](https://developer.mozilla.org/zh-CN/docs/Glossary/Number)
  - [BigInt](https://developer.mozilla.org/zh-CN/docs/Glossary/BigInt)
  - [String](https://developer.mozilla.org/zh-CN/docs/Glossary/字符串)
  - [Symbol](https://developer.mozilla.org/zh-CN/docs/Glossary/Symbol) 
- 和 [Object](https://developer.mozilla.org/zh-CN/docs/Glossary/Object)

符号(Symbols)是ECMAScript 第6版新定义的。符号类型是唯一的并且是不可修改的, 并且也可以用来作为Object的key的值

[`BigInt`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)类型是 JavaScript 中的一个基础的数值类型，可以用任意精度表示整数。使用 BigInt，您可以安全地存储和操作大整数，甚至可以超过数字的安全整数限制。BigInt是通过在整数末尾附加 `n `或调用构造函数来创建的。

###### 16.

**热部署：**直接重新加载整个应用（生产环境），清空内存重新打包，重新解压war包

**热加载：**在运行时重新加载class（开发环境），基于字节码的更改，不释放内存开发可用,上线不可用，热加载不重启tomcat,不重新打包

**懒加载：**延迟加载，

**热更新：**热更新就是当你在开发环境修改代码后，不用刷新整个页面即可看到修改后的效果

为什么会需要event-loop?

因为 JavaScript 是单线程的。单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。为了协调事件（event），用户交互（user interaction），脚本（script），渲染（rendering），网络（networking）等，用户代理（user agent）必须使用事件循环（event loops）。

**单点登录(SingleSignOn**，SSO)，就是通过用户的一次性鉴别登录。当用户在身份认证服务器上登录一次以后，即可获得访问单点登录系统中其他联邦系统和应用软件的权限，同时这种实现是不需要管理员对用户的登录状态或其他信息进行修改的，这意味着在多个应用系统中，用户只需一次登录就可以访问所有相互信任的应用系统。这种方式减少了由登录产生的时间消耗，辅助了用户管理，是目前比较流行的。

**SDK** 就是 Software Development Kit 的缩写，中文意思就是“软件开发工具包”。这是一个覆盖面相当广泛的名词，可以这么说：辅助开发某一类软件的相关文档、范例和工具的集合都可以叫做“SDK”。

**API**，也就是 Application Programming Interface，其实就是操作系统留给应用程序的一个调用接口，应用程序通过调用操作系统的 API 而使操作系统去执行应用程序的命令（动作）。

**js**遵循同源策略，即同协议，同域名，同端口号，否则都算跨域。

**iframe**的优缺点及改进方法

**iframe**的缺点

1、页面样式调试麻烦，出现多个滚动条；

2、浏览器的后退按钮失效；

3、过多会增加服务器的HTTP请求；

4、小型的移动设备无法完全显示框架；

5、产生多个页面，不易管理；

6、不容易打印；

7、代码复杂，无法被一些搜索引擎解读。

**iframe**的优点:

1.iframe能够原封不动的把嵌入的网页展现出来。

2.如果有多个网页引用iframe，那么你只需要修改iframe的内容，就可以实现调用的每一个页面内容的更改，方便快捷。

3.网页如果为了统一风格，头部和版本都是一样的，就可以写成一个页面，用iframe来嵌套，可以增加代码的可重用。

4.如果遇到加载缓慢的第三方内容如图标和广告，这些问题可以由iframe来解决。

5.重载页面时不需要重载整个页面，只需要重载页面中的一个框架页(减少了数据的传输，增加了网页下载速度)

**SEO**（Search Engine Optimization）：汉译为搜索引擎优化。是一种方式：利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名。目的是：为网站提供生态式的自我营销解决方案，让其在行业内占据领先地位，获得品牌收益；SEO包含站外SEO和站内SEO两方面；为了从搜索引擎中获得更多的免费流量，从网站结构、内容建设方案、用户互动传播、页面等角度进行合理规划，还会使搜索引擎中显示的网站相关信息对用户来说更具有吸引力。

**#JavaScript#** **插件、 组件、类库、框架的区别**

**类库：**提供了一些真实项目开发中常用的方法，这些方法做了一些完善处理，比如兼容处理、细节优化等，方便我们开发和维护。常用的类库有：JQuery、Zepto

**插件：**把项目中某一部分进行插件分装，具备具体的业务逻辑，有针对性。如果项目中有类似需求，直接导入插件代码即可，相关逻辑代码不需要自己在写一遍。常用插件：jquery.drag.js、jquery.validate.min.js、jquery.dialog.js、datepicker日历插件、echarts统计图插件、iscroll、swiper插件

**组件：**类似于插件，但是插件一般只是把JS部分封装，组件不仅分装了JS部分，还有CSS部分，以后再使用的时候，我们直接按照文档使用说明引入CSS/JS文件，搭建对应的结构即可。常用的组件有：Bootstrap、swiper组件

**框架：**比上面的三个都要庞大。它不仅提供了很多常用的方法，而且也可以支持一些插件的扩展（可以把一些插件集成到框架中运行），提供了非常优秀的代码管理设计思想。框架有：react、vue、react-native

**1.** **什么是**BFC？

BFC全称为Block Formatting Context，即“块级格式化上下文”，它是页面中相对独立的一块渲染区域，它决定了内部的子元素如何进行摆放和定位，以及区域内部元素和区域外部元素之间的相互作用关系。

**2. BFC**有什么特点？

当一个元素容器创建BFC后，主要有以下表现特点：

BFC可以包含浮动元素（闭合浮动）

BFC所确定的区域不会与外部浮动元素发生重叠

位于同一BFC下的相邻块级子元素在垂直方向上会发生margin重叠

位于不同BFC下的相邻元素之间不会发生margin重叠

将以上特点一言以蔽之，即BFC在页面上是一个封闭的区域，如同“结界”一般。即便是内部的浮动元素也无法脱离该区域。该区域内部的子元素无法影响区域外部，同时也不受外部影响。

**3.** **如何触发创建**BFC？

若某个元素满足以下任一条件，则会对其创建BFC：

<html>根元素

float的值不为none

overflow的值为auto、scroll或hidden

display的值为table-cell、table-caption或inline-block

position的值为fixed或absolute

**4. BFC**的常见用途

闭合浮动

阻止margin重叠

自适应流体布局:BFC最强大的用途其实是用于自适应流体布局，这是基于BFC所确定的区域不会与外部浮动元素发生重叠的特性实现的

