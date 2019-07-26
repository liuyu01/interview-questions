1.
function sayHi(){
      console.log(name)
      console.log(age)
      var name = 'Lydia';
      let age = 21;
    }
    sayHi();
undefinded  ReferenceError

var 声明 存在变量提升

let 和 const 也会存在变量提升，但初始化没有被提升。 在声明之前，不可访问。被称为‘暂时性死区’。
变量的赋值可以分为三个阶段：
创建变量，在内存中开辟空间；初始化变量，将变量初始化为undefined；真正赋值。

关于let 、var  、function：
let的创建过程被提升，但是初始化没有提升。
var的创建和初始化都被提升了。
function 的创建、初始化和赋值都被提升。

2.
for(var i=0;i<3;i++){
      setTimeout(()=>console.log(i),1)
    }

    for(let i=0;i<3;i++){
      setTimeout(()=>console.log(i),1)
    }
3  3  3 和 0  1  2
由于JavaScript中的事件执行机制，setTimeout函数真正被执行时，循环已经走完。由于第一个循环中的变量i是使用var关键字声明的，因此该值是全局的。在循环期间，我们每次使用一元运算符++都会将i的值增加1。因此在第一个例子中，当调用setTimeout函数时，i已经被赋值为3。

使用let和const关键字声明的变量具有块级作用域的。在每次迭代期间，i将会被创建为一个人新值，并且每个值都会存在于循环内的块级作用域。

3．
const shape = {
    radius:10,
    diameter(){
      return this.radius * 2;
    },
    perimeter:()=>2 * Math.PI * this.radius
   };

20 NAN

diameter 普通函数而perimeter 箭头函数。由于箭头函数，this关键字指向是它所在上下文（定义时位置）的环境。意味着调用perimeter时它不是指向shape,而是指定义时的环境（window）。没有radius属性，返回undefined。

4.
 +true;
!"Lydia";
1 and false
一元加号会将Boolean 转换为数字类型。true转换为1，false转换为0。

5.
const bird = {
    size: "small"
  };

  const mouse = {
    name : "Mickey",
    small : true
  };

  A:mouse.bird.size
  B:mouse[bird.size]
  C:mouse[bird['size']]
  D:All of them are valid
A

在JavaScript 中，所有对象键都是字符串（除了Symbol）。尽管有时候我们可能不会给定字符串类型，但它总是被转换为字符串。
在使用方括号表示法时，它会看到第一个左括号[,然后继续，直到找到右括号]。只有在那个时候，它才会对这个语句求值。
但是点表示法不会。点表示法从左到右进行，点之后就取值。

6.
let c={greeting : 'Hey'};
  let d;
  d=c;
  c.greeting='Hello';
  console.log(d.greeting)
Hello

在JavaScript中，当设置他们彼此相等时，所有对象都通过引用进行交互。更改一个对象，可以更改所有对象。

7.
let a = 3;
  let b = new Number(3);
  let c = 3;
  console.log(a == b)
  console.log(a === b)
  console.log(b === c)
true false false
new Number()是一个内置的构造函数。虽然它看起来像一个数字，但是它并不是一个真正的数字；它有一堆额外的功能，是一个对象。
== 隐士类型转换，只比较值
=== 类型和值都需要比较

8.
  class Chameleon{
    static colorChange(newColor){
      this.newColor = newColor;
      return this.newColor;
    }
    constructor({newColor='green'}={}){
      this.newColor = newColor;
    }
  }
  const freddie = new Chameleon({newColor:"purple"});
  freddie.colorChange('orange');
TypeError: freddie.colorChange is not a function

colorChange方法是静态方法。静态方法仅在创建它们的构造函数中存在并且不能传递给任何子级。由于freddie是一个子级对象，函数不会传递，所以在freddie实例上不存在colorChange方法。类（class）通过 static 关键字定义静态方法。不能在类的实例上调用静态方法，而应该通过类本身调用

9.
  let greeting;
  greetign = {};
  console.log(greetign)
{}
当我们错误地将greeting输入为greetign时，JS解释器实际上在浏览器中将其视为globa.greetign={}或window.greetign={};
为了避免这种情况，可以使用’use strict’。可以确保在将变量赋值之前必须声明变量。

10.
  function bark(){
    console.log('Woof!');
  }
  bark.animal = 'dog';
Nothing, this is totally fine! 没什么，这完全没有问题
函数时一种特殊类型的对象。该函数是具有属性的对象。此属性是可调用的。

11.
  function Person(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
  }
  const member = new Person('Lydia','Hallie');
  Person.getFullName = function(){
    return `${this.firstName} ${this.lastName}`;
  };
console.log(member.getFullName())

不能像使用常规对象那样向构造函数添加属性。如果要一次向所有对象添加功能，则必须使用原型。
假设将此方法添加到构造函数本身，也许不是每个Person实例都需要这种方法。这会浪费大量内存空间，这占用了每个实例的内存空间。相反，如果我们只将它添加到原型中，我们只需要将它放在内存中的一个位置，但他们都可以访问它。

12.
  function Person(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
  }
  const lydia = new Person('Lydia','Hallie');
  const sarah = Person('Sarah','Smith');
  console.log(lydia);
  console.log(sarah);
Person {firstName: "Lydia", lastName: "Hallie"} and undefined
https://www.cnblogs.com/yzhihao/p/9306511.html

13.
事件传播的三个阶段： Capturing捕获 -> Target目标 ->Bubbling冒泡
DOM事件流（event  flow ）存在三个阶段：事件捕获阶段、处于目标阶段、事件冒泡阶段。

14.所有对象都有原型。 错的
除基础对象外，所有对象都有原型。基础对象可以访问某些方法和属性。例如 .toString。这就是可以使用内置JavaScript方法的原因。基础对象指原型链终点的对象。基础对象的原型是null。

15.
  function sum(a,b){
    return a+b;
  }
  sum(1,'2')

‘12’ 隐式类型转换先会先将1转换为string类型，进行字符串的拼接运算。
javascript中string与number相加得到字符串，相减却是数字

16.
  let number = 0;
  console.log(number++);
  console.log(++number);
  console.log(number);
0	2  2
后置型递增（递减）先赋值再计算
前置型递增（递减）先计算再赋值

17.
  function getPersonInfo(one,two,three){
    console.log(one);
    console.log(two);
    console.log(three);
  }
  const person = 'Lydia';
  const age = 21;
  getPersonInfo`${person} is ${age} years old`;
["", " is ", " years old"] "Lydia" 21
如果使用标记的模板字符串，则第一个参数的值始终是字符串的数组。其余参数获取传递到模板字符串中的表达式的值。
模板字符串和标签模板是两个东西。
标签模板不是模板，而是函数调用的一种特殊形式。“标签”指的就是函数，紧跟在后面的模板字符串就是它的参数。
但是，如果模板字符串中有变量，就不再是简单的调用了，而是要将模板字符串先处理成多个参数，再调用函数。
模板字符串由变量和非变量组成，什么是变量，${}就是变量。模板标签函数调用的第一个参数是数组，是由模板字符串中那些非变量部分组成，包括空格。
模板字符串中变量必须是由非变量包含着的，如果变量在开始位置或者结束位置且没有非变量包含，那么该位置就是空字符串。

18.
  function checkAge(data){
    if(data === {age:18}){
      console.log('You are an audlt!')
    }else if(data=={age:18}){
      console.log('You are still an audlt.')
    }else{
      console.log(`Hmm.You don't have an age I guess`)
    }
  }
  checkAge({age:18})
Hmm.. You don't have an age I guess
在比较相等性，基本数据类型通过它们的值进行比较，而对象通过它们的引用进行比较。JavaScript检查是否具有对内存中相同位置的引用。

19.
  function getAge(...args){
    console.log(typeof args)
  }
  getAge(21)
"object"
扩展运算符（…args）返回一个带参数的数组。数组是一个对象。

20.
  function getAge(){
    'use strict';
    age = 21;
    console.log(21)
  }
  getAge()
ReferenceError:age is not defined
使用’use strict’,可以确保不会意外地声明全局变量。

21.
const sum = eval("10*10+5");
105
eval 会为字符串传递的代码求值。如果它是一个表达式，就像在这种情况下一样，它会计算表达式。

22.
cool_secret 可以访问多长时间？
sessionStorage.setItem(‘cool_secret’,123);
用户关闭选项卡时

23.
  var num = 8;
  var num = 10;
  console.log(num)
10
使用var关键字，可以用相同的名称声明多个变量，然后变量将保存最新的值。

24.
const obj = {1 : 'a', 2 : 'b', 3 : 'c'};
 const set = new Set([1,2,3,4,5]);
 obj.hasOwnProperty('1');
 obj.hasOwnProperty(1);
 set.has('1');
 set.has(1);
true true false true
所有对象键（不包括Symbols）都会被存储为字符串，即使你没有给定字符串类型的键。上面的说法不适用与Set。在Set中没有’1’。有数字型1。

25.
const obj = {a:'one',b:'two',a:'three'};
console.log(obj)
 { a: "three", b: "two" }
如果对象有两个具有相同名称的键，则将替前面的键。它扔将处于第一个位置，但具有最后指定的值。

26.
JavaScript全局执行上下文为你创建了两个东西：全局对象和this关键字。

27.
for(let i=1;i<5;i++){
   if(i===3)continue;
   console.log(i);
 }
1	2  4
如果某个条件返回true，则continue语句跳过迭代。Continue 跳过当前循环，继续执行

28.
String.prototype.giveLydiaPizza=()=>{
  return 'Just give Lydia pizza already!';
 };
 const name='Lydia';
 name.giveLydiaPizza();
"Just give Lydia pizza already!"
String是一个内置的构造函数，可以为它添加属性。刚给它的原型添加了一个方法。基本数据类型的字符串自动转换为字符串对象，由字符串原型函数生成。因此，所有字符串（字符串对象）都可以访问该方法。

29.
const a = {};
 const b = {key : 'b'};
 const c = {key : 'c'}
 a[b]=123;
 a[c]=456;
 console.log(a[b])
456
对象键自动转换为字符串。我们试图将一个对象设置为对象a的键，其值为123。
但是，当对象自动转换为字符串化时，它变成了[Object object]。 所以我们在这里说的是a["Object object"] = 123。 然后，我们可以尝试再次做同样的事情。 c对象同样会发生隐式类型转换。那么，a["Object object"] = 456。
然后，我们打印a[b]，它实际上是a["Object object"]。 我们将其设置为456，因此返回456。

30.
const foo = () => console.log('First');
 const bar = () => setTimeout(()=>console.log('Second'));
 const baz = () => console.log('Third');
 bar();
 foo();
 baz();
First Third Second
我们有一个setTimeout函数并首先调用它。 然而却最后打印了它。
这是因为在浏览器中，我们不只有运行时引擎，我们还有一个叫做WebAPI的东西。WebAPI为我们提供了setTimeout函数，例如DOM。
将callback推送到WebAPI后，setTimeout函数本身（但不是回调！）从堆栈中弹出。

 
现在，调用foo，并打印First。
 
foo从堆栈弹出，baz被调用，并打印Third。
 
WebAPI不能只是在准备就绪时将内容添加到堆栈中。 相反，它将回调函数推送到一个称为任务队列的东西。
 
这是事件循环开始工作的地方。 事件循环查看堆栈和任务队列。 如果堆栈为空，则会占用队列中的第一个内容并将其推送到堆栈中。
 
bar被调用，Second被打印，它从栈中弹出。

31.单击按钮时event.target是什么？
<div onClick="console.log('first div')">
    <div onClicl="console.log('second div')">
        <button onClick="console.log('button')">
            Click!
        </button>
    </div>
 </div>
button
导致事件的最深嵌套元素是事件的目标。可以通过event.stopPropagation停止冒泡。

32.
单击下面的html片段，打印的内容是什么？
<div onClick="console.log('div')">
    <p onClick="console.log('p')">
        Click here!
    </p>
 </div>
P div
在事件传播期间，有三个阶段：捕获、目标、冒泡。默认情况下，事件处理程序在冒泡阶段执行。它从最深的嵌套元素向外延伸。

33.下面代码的输出是什么？
const person = {name : 'Lydia'};
 function sayHi(age){
   console.log(`${this.name} is ${age}`)
 }
 sayHi.call(person,21);
 sayHi.bind(person,21);
Lydia is 21 function

使用两者，可以传递我们想要的this关键字引用的对象。但是.call方法会立即执行。.bind方法会返回函数的拷贝值，但带有绑定的上下文！它不会立即执行。

1.this对象的涵义就是指向当前对象中的属性和方法。

2.this指向的可变性。当在全局作用域时，this指向全局；当在某个对象中使用this时，this指向该对象；当把某个对象的方法赋值给另外一个对象时，this会指向后一个对象。
3.this的使用场合有：在全局环境中使用；在构造函数中使用，在对象的方法中使用。
4.this的使用注意点，最重要的一点就是要避免多层嵌套使用this对象。
总结一下call，apply，bind方法：
a：第一个参数都是指定函数内部中this的指向（函数执行时所在的作用域），然后根据指定的作用域，调用该函数。
b：都可以在函数调用时传递参数。call，bind方法需要直接传入，而apply方法需要以数组的形式传入。
c：call，apply方法是在调用之后立即执行函数，而bind方法没有立即执行，需要将函数再执行一遍。有点闭包的味道。
d：改变this对象的指向问题不仅有call，apply，bind方法，也可以使用that变量来固定this的指向。

34.下面代码的输出是什么？
function sayHi(){
   return (()=>0)();
 }

 typeof sayHi();
‘number’
只有7种内置类型。null undefined Boolean number string object symbol.function不是一个类型，因为函数是对象，它的类型是object。

35.下面这些值哪些是假值？
0;
new Number(0);
("");
(" ");
new Boolean(false);
undefined;
0, '', undefined
JavaScript 只有6个假值：
0 false undefined null NaN ‘’(empty string)

函数构造函数，如new Number new Boolean 都是真值。


36.下面代码的输出是什么？
console.log(typeof typeof 1);
‘string’
Typeof 1 返回’number’  typeof ‘number’ 返回string

37.下面代码的输出是什么？
const numbers = [1, 2, 3];
numbers[10] = 11;
console.log(numbers);
[1, 2, 3, empty × 7, 11]
当你为数组中的元素设置一个超过数组长度的值时，JavaScript会创建一个名为‘空插槽’的东西。这些位置的值实际上是undefined，但你会看到类似的东西[1, 2, 3, empty × 7, 11]，这取决于你运行它的位置（每个浏览器有可能不同）
Chrome  

firefox 
IE 

38.下面代码的输出是什么?
(()=>{
  let x,y;
  try{
    throw new Error();
  }catch(x){
    (x=1),(y=2);
    console.log(x);
  }
  console.log(x);
  console.log(y);
})();
1 undefined 2  
Catch块接受参数x。当我们传递参数时，这与变量的x不同。这个变量x是属于catch作用域的。之后，将这个块级作用域的变量设置为1，并设置变量y的值。现在，我们打印块级作用域的变量x,它等于1。在catch块外，x是undefined,而y是2。

39.JavaScript中的所有内容都是基本数据类型和对象。
基本数据类型(简单数据类型)：包括布尔值，数字，字符串，null和undefined，symbol
引用类型：object


40.下面代码的输出是什么？
[[0,1],[2,3]].reduce(
  (acc,cur)=>{
    return acc.concat(cur);
  },
  [1,2]
);
[1, 2, 0, 1, 2, 3]

arr.reduce(function(prev,cur,index,arr){…},init)
arr表示元数组
prev 表示上一次调用回调时的返回值，或者初始值init;
cur 表示当前正在处理的数组元素；
index 表示当前正在处理的数组元素的索引，若提供init值，则索引为0，否则索引为1
init 表示初始值

41.下面代码的输出是什么？
!!null;
!!"";
!!1;
false  false  true

42.setInterval方法的返回值是什么？
setInterval(() => console.log("Hi"), 1000);
一个唯一的id.此id可用于使用clearInterval()函数清除定时器。

43.返回什么？
[..."Lydia"];
["L", "y", "d", "i", "a"]
字符串是可迭代的。扩展运算符将迭代的每个字符映射到一个元素上。

一道经典的面试题，如何让：a == 1 && a == 2 && a == 3。
根据上面的拆箱转换，以及==的隐式转换，我们可以轻松写出答案：
const a = {
   value:[3,2,1],
   valueOf: function () {return this.value.pop(); },
}

在JavaScript中，每一个变量在内存中都需要一个空间来存储。

内存空间又被分为两种，栈内存与堆内存。

栈内存：
存储的值大小固定
空间较小
可以直接操作其保存的变量，运行效率高
由系统自动分配存储空间
JavaScript中的原始类型的值被直接存储在栈中，在变量定义时，栈就为其分配好了内存空间。
由于栈中的内存空间的大小是固定的，那么注定了存储在栈中的变量就是不可变的。

堆内存：
存储的值大小不定，可动态调整
空间较大，运行效率低
无法直接操作其内部存储，使用引用地址读取
通过代码进行分配空间

内存泄漏的识别方法
经验法则是，如果连续五次垃圾回收之后，内存占用一次比一次大，就有内存泄漏。
这就要求实时查看内存的占用情况。
在 Chrome 浏览器中，我们可以这样查看内存占用情况

打开开发者工具，选择 Performance 面板
在顶部勾选 Memory
点击左上角的 record 按钮
在页面上进行各种操作，模拟用户的使用情况
一段时间后，点击对话框的 stop 按钮，面板上就会显示这段时间的内存占用情况

在服务器环境中使用 Node 提供的 process.memoryUsage 方法查看内存情况
console.log(process.memoryUsage());
// { 
//     rss: 27709440,
//     heapTotal: 5685248,
//     heapUsed: 3449392,
//     external: 8772 
// }
复制代码process.memoryUsage返回一个对象，包含了 Node 进程的内存占用信息。
该对象包含四个字段，单位是字节，含义如下:

rss（resident set size）：所有内存占用，包括指令区和堆栈。
heapTotal："堆"占用的内存，包括用到的和没用到的。
heapUsed：用到的堆的部分。
external： V8 引擎内部的 C++ 对象占用的内存。

判断内存泄漏，以heapUsed字段为准。

如何避免内存泄漏
记住一个原则：不用的东西，及时归还。

减少不必要的全局变量，使用严格模式避免意外创建全局变量。
在你使用完数据后，及时解除引用（闭包中的变量，dom引用，定时器清除）。
组织好你的逻辑，避免死循环等造成浏览器卡顿，崩溃的问题。

内存生命周期
JS 环境中分配的内存有如下声明周期：

内存分配：当我们申明变量、函数、对象的时候，系统会自动为他们分配内存
内存使用：即读写内存，也就是使用变量、函数等
内存回收：使用完毕，由垃圾回收机制自动回收不再使用的内存

不再需要使用的变量也就是生命周期结束的变量，是局部变量，局部变量只在函数的执行过程中存在， 当函数运行结束，没有其他引用(闭包)，那么该变量会被标记回收。

全局变量的生命周期直至浏览器卸载页面才会结束，也就是说全局变量不会被当成垃圾回收。
1.如何正确判断this的指向？
谁调用它，this 就指向谁。

1)全局环境中的 this
浏览器环境：无论是否在严格模式下，在全局执行环境中（在任何函数体外部）this 都指向全局对象 window;

node 环境：无论是否在严格模式下，在全局执行环境中（在任何函数体外部），this 都是空对象 {};

2)是否是 new 绑定
如果是 new 绑定，并且构造函数中没有返回 function 或者是 object，那么 this 指向这个新对象。如下:构造函数返回值不是 function 或 object。
function Super(age){
  this.age = age;
}
let instance = new Super('26');
console.log(instance.age);  //26

构造函数返回值是 function 或 object，这种情况下 this 指向的是返回的对象。
function Super(age){
  this.age = age;
  let obj = {a: '2'};
  return obj;
}
let instance = new Super('hello');
console.log(instance.age); //undefined

你可以想知道为什么会这样？我们来看一下 new 的实现原理:

1)创建一个新对象。
2)这个新对象会被执行 [[原型]] 连接。
3)属性和方法被加入到 this 引用的对象中。并执行了构造函数中的方法.
4)如果函数没有返回其他对象，那么 this 指向这个新对象，否则 this 指向构造函数中返回的对象。
function new(func) {
  let target = {};
  target.__proto__ = func.prototype;
  let res = func.call(target);
  //排除 null 的情况
  if (res && typeof(res) == "object" || typeof(res) == "function") {
    return res;
  }
  return target;
}

3) 函数是否通过 call,apply 调用，或者使用了 bind 绑定，如果是，那么this绑定的就是指定的对象【归结为显式绑定】。
这里同样需要注意一种特殊情况，如果 call,apply 或者 bind 传入的第一个参数值是 undefined 或者 null，严格模式下 this 的值为传入的值 null /undefined。非严格模式下，实际应用的默认绑定规则，this 指向全局对象(node环境为global，浏览器环境为window)

隐式绑定，函数的调用是在某个对象上触发的，即调用位置上存在上下文对象。典型的隐式调用为: xxx.fn()
function info(){
  console.log(this.age);
}
var person = {
  age: 20,
  info
};
person.info();//20;执行的是隐式绑定

默认绑定，在不能应用其它绑定规则时使用的默认规则，通常是独立函数调用。
非严格模式： node环境，执行全局对象 global，浏览器环境，执行全局对象 window。

严格模式：执行 undefined

4)箭头函数的this
箭头函数没有自己的this，继承外层上下文绑定的this。
由于箭头函数不绑定this， 它会捕获其所在（即定义的位置）上下文的this值， 作为自己的this值

2.JS中原始类型有哪几种？null 是对象吗？原始数据类型和复杂数据类型有什么区别？
目前，JS原始类型有六种，分别为:

Boolean
String
Number
Undefined
Null
Symbol(ES6新增)

ES10新增了一种基本数据类型：BigInt
复杂数据类型只有一种: Object
null 不是一个对象，尽管 typeof null 输出的是 object，这是一个历史遗留问题，JS 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，000 开头代表是对象，null 表示为全零，所以将它错误的判断为 object 。
基本数据类型和复杂数据类型的区别为:

1)内存的分配不同
基本数据类型存储在栈中。
复杂数据类型存储在堆中，栈中存储的变量，是指向堆中的引用地址。

2)访问机制不同
基本数据类型是按值访问
复杂数据类型按引用访问，JS不允许直接访问保存在堆内存中的对象，在访问一个对象时，首先得到的是这个对象在栈内存中的地址，然后再按照这个地址去获得这个对象中的值。

3)复制变量时不同(a=b)

基本数据类型：a=b;是将b中保存的原始值的副本赋值给新变量a，a和b完全独立，互不影响
复杂数据类型：a=b;将b保存的对象内存的引用地址赋值给了新变量a;a和b指向了同一个堆内存地址，其中一个值发生了改变，另一个也会改变。

4)参数传递的不同(实参/形参)
函数传参都是按值传递(栈中的存储的内容)：基本数据类型，拷贝的是值；复杂数据类型，拷贝的是引用地址

3.说一说你对HTML5语义化的理解
语义化意味着顾名思义，HTML5的语义化指的是合理正确的使用语义化的标签来创建页面结构，如header,footer,nav，从标签上即可以直观的知道这个标签的作用，而不是滥用div。

语义化的优点有:
代码结构清晰，易于阅读，利于开发和维护
方便其他设备解析（如屏幕阅读器）根据语义渲染网页。
有利于搜索引擎优化（SEO），搜索引擎爬虫会根据不同的标签来赋予不同的权重
  搜索引擎爬虫 （又被称为网页蜘蛛，网络机器人），是一种按照一定的规则，自动的抓取万维网信息的程序或者脚本。
  SEO（Search Engine Optimization）：汉译为搜索引擎优化。是一种方式：利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名。目的是：为网站提供生态式的自我营销解决方案，让其在行业内占据领先地位，获得品牌收益；SEO包含站外SEO和站内SEO两方面；为了从搜索引擎中获得更多的免费流量，从网站结构、内容建设方案、用户互动传播、页面等角度进行合理规划，还会使搜索引擎中显示的网站相关信息对用户来说更具有吸引力。

语义化标签主要有：

title: 主要用于页面的头部的信息介绍
header: 定义文档的页眉
nav: 主要用于页面导航
main: 规定文档的主要内容。对于文档来说应当是唯一的。它不应包含在文档中重复出现的内容，比如侧栏、导航栏、版权信息、站点标志或搜索菜单。
article: 独立的自包含内容
h1~h6: 定义标题
ul: 用来定义无序列表
ol: 用来定义有序列表
address: 定义文档或者文章的作者/拥有者的联系信息
canvas: 用于绘制图像
dialog: 定义一个对象框、确认框或窗口
aside: 定义其所处内容之外的内容。<aside>的内容可用作文章的侧栏。
section: 定义文档中的节（section、区段）。比如章节、页眉、页脚或文档中的其他部分。
figure: 规定独立的流内容（图像、图表、照片、代码等等）。figure元素的内容应该与主内容相关，但如果被删除，则不应对文档流产生影响。
details: 描述文档或文档某一部分细节
mark: 带有记号的文本

4.如何让 (a == 1 && a == 2 && a == 3) 的值为true？
1）
let a = {
    valueOf: (function() {
        let i = 1;
        //闭包的特性之一：i 不会被回收
        return function() {
            return i++;
        }
    })()
}
console.log(a == 1 && a == 2 && a == 3);
2）
let a = {
    reg: /\d/g,
    valueOf () {
        return this.reg.exec(123)[0]
    }
}
console.log(a == 1 && a == 2 && a == 3);

3）
let a = [1, 2, 3];
a.join = a.shift;
console.log(a == 1 && a == 2 && a == 3); //true

const a = {
   value:[1,2,3],
   valueOf: function () {return this.value.shift(); },
}
console.log(a == 1 && a == 2 && a == 3)

4）
const a = {
   value:[3,2,1],
   valueOf: function () {return this.value.pop(); },
}
pop() 方法用于删除数组的最后一个元素并返回删除的元素。
shift() 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值。

5.防抖(debounce)函数的作用是什么？有哪些应用场景
函数防抖（debounce）
函数防抖，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。
简单的说，当一个动作连续触发，则只执行最后一次。

防抖函数的作用就是控制函数在一定时间内的执行次数。防抖意味着N秒内函数只会被执行一次，如果N秒内再次被触发，则重新计算延迟时间。

举例说明：小思最近在减肥，但是她非常贪吃。为此，与其男朋友约定好，如果10天不吃零食，就可以购买一个包(不要问为什么是包，因为包治百病)。但是如果中间吃了一次零食，那么就要重新计算时间，直到小思坚持10天没有吃零食，才能购买一个包。所以，管不住嘴的小思，没有机会买包(悲伤的故事)...这就是防抖。

函数节流（throttle）
节流函数的作用是规定一个单位时间，在这个单位时间内最多只能触发一次函数执行，如果这个单位时间内多次触发函数，只能有一次生效。
限制一个函数在一定时间内只能执行一次。

常见应用场景
函数防抖的应用场景
连续的事件，只需触发一次回调的场景有：
1.搜索框输入查询，如果用户一直在输入中，没有必要不停地调用去请求服务端接口，等用户停止输入的时候，再调用，设置一个合适的时间间隔，有效减轻服务端压力。
2.表单验证
3.按钮提交事件。
4.浏览器窗口缩放，resize事件等。需窗口调整完成后，计算窗口大小。防止重复渲染。

函数节流的应用场景
间隔一段时间执行一次回调的场景有：
1.按钮点击事件
2.拖拽事件
3.onScoll
4.计算鼠标移动的距离(mousemove)

函数防抖，将多次执行的事件合并成一次。函数节流，保持一段时间执行一次。

6. 深拷贝和浅拷贝的区别是什么？ 
深拷贝
深拷贝复制变量值，对于非基本类型的变量，则递归至基本类型变量后，再复制。 深拷贝后的对象与原来的对象是完全隔离的，互不影响，对一个对象的修改并不会影响另一个对象。

浅拷贝
浅拷贝是会将对象的每个属性进行依次复制，但是当对象的属性值是引用类型时，实质复制的是其引用，当引用指向的值改变时也会跟着变化。

可以使用 for in、 Object.assign、 扩展运算符 ... 、Array.prototype.slice()、Array.prototype.concat() 等

可以看出浅拷贝只最第一层属性进行了拷贝，当第一层的属性值是基本数据类型时，新的对象和原对象互不影响，但是如果第一层的属性值是复杂数据类型，那么新对象和原对象的属性值其指向的是同一块内存地址。

7. let、const、var 的区别有哪些？
声明方式  变量提升	暂时性死区   重复声明	块作用域有效   初始值	重新赋值
var        会        不存在       允许       不是           非必须   允许     
let         不会      存在         不允许     是             非必须   允许     
const       不会      存在         不允许     是             必须     不允许

8.说一说你对JS执行上下文栈和作用域链的理解？
JS执行上下文：
执行上下文就是当前 JavaScript 代码被解析和执行时所在环境的抽象概念， JavaScript 中运行任何的代码都是在执行上下文中运行。

执行上下文类型分为：

全局执行上下文
函数执行上下文
eval函数执行上下文(不被推荐)

执行上下文创建过程中，需要做以下几件事:

创建变量对象：首先初始化函数的参数arguments，提升函数声明和变量声明。
创建作用域链（Scope Chain）：在执行期上下文的创建阶段，作用域链是在变量对象之后创建的。
确定this的值，即 ResolveThisBinding

作用域
作用域负责收集和维护由所有声明的标识符（变量）组成的一系列查询，并实施一套非常严格的规则，确定当前执行的代码对这些标识符的访问权限。—— 摘录自《你不知道的JavaScript》(上卷)
作用域有两种工作模型：词法作用域和动态作用域，JS采用的是词法作用域工作模型，词法作用域意味着作用域是由书写代码时变量和函数声明的位置决定的。(with 和 eval 能够修改词法作用域，但是不推荐使用，对此不做特别说明)

作用域分为：

全局作用域
函数作用域
块级作用域

作用域链
作用域链就是从当前作用域开始一层一层向上寻找某个变量，直到找到全局作用域还是没找到，就宣布放弃。这种一层一层的关系，就是作用域链。

9.什么是XSS攻击，XSS攻击可以分为哪几类？我们如何防范XSS攻击？
XSS(Cross-Site Scripting，跨站脚本攻击)是一种代码注入攻击。攻击者在目标网站上注入恶意代码，当被攻击者登陆网站时就会执行这些恶意代码，这些脚本可以读取 cookie，session tokens，或者其它敏感的网站信息，对用户进行钓鱼欺诈，甚至发起蠕虫攻击等。
XSS 的本质是：恶意代码未经过滤，与网站正常的代码混在一起；浏览器无法分辨哪些脚本是可信的，导致恶意脚本被执行。由于直接在用户的终端执行，恶意代码能够直接获取用户的信息，利用这些信息冒充用户向网站发起攻击者定义的请求。

防范XSS攻击:

1.Content Security Policy
在服务端使用 HTTP的 Content-Security-Policy 头部来指定策略，或者在前端设置 meta 标签。
2.输入内容长度控制
对于不受信任的输入，都应该限定一个合理的长度。虽然无法完全防止 XSS 发生，但可以增加 XSS 攻击的难度。

3.输入内容限制
对于部分输入，可以限定不能包含特殊字符或者仅能输入数字等。

4.其他安全措施
HTTP-only Cookie: 禁止 JavaScript 读取某些敏感 Cookie，攻击者完成 XSS 注入后也无法窃取此 Cookie。
验证码：防止脚本冒充用户提交危险操作。

10. 如何隐藏页面中的某个元素？
屏幕并不是唯一的输出机制，比如说屏幕上看不见的元素（隐藏的元素），其中一些依然能够被读屏软件阅读出来（因为读屏软件依赖于可访问性树来阐述）。为了消除它们之间的歧义，我们将其归为三大类：

完全隐藏：元素从渲染树中消失，不占据空间。
视觉上的隐藏：屏幕中不可见，占据空间。
语义上的隐藏：读屏软件不可读，但正常占据空。

完全隐藏
1.display 属性(不占据空间)
display: none;

2.hidden 属性 (不占据空间)
HTML5 新增属性，相当于 display: none
<div hidden>
</div>

视觉上的隐藏
1.	利用 position 和 盒模型 将元素移出可视区范围
2.	利用 transform
3.	设置其大小为0
4.	设置透明度为0 (占据空间)
5.	visibility属性 (占据空间)
6.	层级覆盖，z-index 属性 (占据空间)
7.	clip-path 裁剪 (占据空间)

语义上的隐藏
aria-hidden 属性 (占据空间)
读屏软件不可读，占据空间，可见。
<div aria-hidden="true">
</div>
11. 浏览器事件代理机制的原理是什么？
事件流
在说浏览器事件代理机制原理之前，我们首先了解一下事件流的概念，早期浏览器，IE采用的是事件捕获事件流，而Netscape采用的则是事件冒泡。"DOM2级事件"把事件流分为三个阶段，捕获阶段、目标阶段、冒泡阶段。现代浏览器也都遵循此规范。

事件代理机制的原理
事件代理又称为事件委托，在祖先级 DOM 元素绑定一个事件，当触发子孙级DOM元素的事件时，利用事件冒泡的原理来触发绑定在祖先级 DOM 的事件。因为事件会从目标元素一层层冒泡至 document 对象。

12. setTimeout 倒计时为什么会出现误差？
setTimeout 只能保证延时或间隔不小于设定的时间。因为它实际上只是将回调添加到了宏任务队列中，但是如果主线程上有任务还没有执行完成，它必须要等待。

JS的运行机制
（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
（2）主线程之外，还存在"任务队列"(task queue)。
（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
（4）主线程不断重复上面的第三步。

12. 什么是闭包？闭包的作用是什么？
闭包是指有权访问另一个函数作用域中的变量的函数，创建闭包最常用的方式就是在一个函数内部创建另一个函数。在本质上，闭包是将函数内部和函数外部连接起来的桥梁。

javascript语言的特别之处就在于：函数内部可以直接读取全局变量，但是在函数外部无法读取函数内部的局部变量。
注意点：在函数内部声明变量的时候，一定要使用var命令。如果不用的话，你实际上声明的是一个全局变量！

如何从外部读取函数内部的局部变量？
那就是在函数内部，再定义一个函数。
子对象会一级一级地向上寻找所有父对象的变量。所以，父对象的所有变量，对子对象都是可见的，反之则不成立。
既然f2可以读取f1中的局部变量，那么只要把f2作为返回值，我们不就可以在f1外部读取它的内部变量了吗！
它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中，不会在f1调用后被自动清除。

JavaScript中匿名函数this指向问题
this对象是在运行时基于函数执行环境绑定的，在全局函数中，this=window，在函数被作为某个对象的方法调用时，this等于这个对象。
但是匿名函数的执行环境是全局性的。
var name = 'window';
var person = {
  name : 'Alan',
  sayName : function(){
    return function(){
      console.log(this.name);
    }
  }
};
person.sayName()();  // window

sayName方法return了一个匿名函数，这个匿名函数中this指向window

解决方法还是有的，我们可以把外部作用域的this传递给匿名函数
var name = 'window';
var person = {
  name : 'Alan',
  sayName : function(){
    var that = this;
    return function(){
      console.log(that.name);
    }
  }
};
person.sayName()();  //Alan

13. 异步加载 js 脚本的方法有哪些？

同步加载
我们平时最常使用的就是这种同步加载形式：
<script src="http://yourdomain.com/script.js"></script> 
同步模式，又称阻塞模式，会阻止浏览器的后续处理，停止了后续的解析，因此停止了后续的文件加载（如图像）、渲染、代码执行。
 js 之所以要同步执行，是因为 js 中可能有输出 document 内容、修改dom、重定向等行为，所以默认同步执行才是安全的。
以前的一般建议是把<script>放在页面末尾</body>之前，这样尽可能减少这种阻塞行为，而先让页面展示出来。
 
简单说：加载的网络 timeline 是瀑布模型，而异步加载的 timeline 是并发模型。

异步加载又叫非阻塞，浏览器在下载执行 js 同时，还会继续进行后续页面的处理。
1)	<script> 标签中增加 async(html5) 或者 defer(html4) 属性,脚本就会异步加载。
•	defer 和 async 的区别在于：英 [əˈsɪŋk]

async属性是HTML5新增属性，需要Chrome、FireFox、IE9+浏览器支持
async属性规定一旦脚本可用，则会异步执行
async属性仅适用于外部脚本
此方法不能保证脚本按顺序执行

defer属性规定是否对脚本执行进行延迟，直到页面加载为止
如果脚本不会改变文档的内容，可将defer属性加入到<script>标签中，以便加快处理文档的速度
兼容所有浏览器
此方法可以确保所有设置了defer属性的脚本按顺序执行

defer 要等到整个页面在内存中正常渲染结束（DOM 结构完全生成，以及其他脚本执行完成），在window.onload 之前执行；
async 一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。
如果有多个 defer 脚本，会按照它们在页面出现的顺序加载
多个 async 脚本不能保证加载顺序

2) 动态创建 script 标签
动态创建的 script ，设置 src 并不会开始下载，而是要添加到文档中，JS文件才会开始下载。
let script = document.createElement('script');
script.src = 'XXX.js';
//添加到html文件中才会开始下载
document.body.append(script);

3)$(document).ready()
需要引入jquery
兼容所有浏览器
$(document).ready(function(){
  alert("加载完成！");
})

4) XHR 异步加载JS
let xhr = new XMLHttpRequest();
xhr.open("get",'js/xxx.js',true);
xhr.send();
xhr.onreadystatechange = function(){
  if(xhr.readyState == 4 && xhr.status == 200){
    eval(xhr.responseText);
  }
};

14. 可迭代对象有什么特点
ES6 规定，默认的 Iterator 接口部署在数据结构的 Symbol.iterator 属性，换个角度，也可以认为，一个数据结构只要具有 Symbol.iterator 属性(Symbol.iterator 方法对应的是遍历器生成函数，返回的是一个遍历器对象)，那么就可以其认为是可迭代的。

可迭代对象的特点
具有 Symbol.iterator 属性，Symbol.iterator() 返回的是一个遍历器对象
可以使用 for ... of 进行循环

原生具有 Iterator 接口的数据结构：
Array
Map   Map是一组键值对的结构，具有极快的查找速度。
Set Set和Map类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在Set中，没有重复的key。
String
TypedArray
函数的 arguments 对象
NodeList 对象

15.CSS学习笔记-内联样式、内部样式、外部样式表
内联样式就是把css代码直接写在现有的HTML标签中，如下面代码：
<p style="color:red">这里文字是红色。</p>
在标签中添加内联样式。添加style属性 。在style中属性和值得表示方法为：style=“属性:值;属性:值”;

内部样式写在style标签中。
格式为：
	<style type="text/css">
			div(标签名){
			   属性：值;
			   属性：值;
			}
	</style>

外联样式：<link href="" rel="stylesheet" type="text/css" />（属于外部样式表）
样式。
总结来说，就是--就近原则（离被设置元素越近优先级别越高）。

16.箭头函数
箭头函数的基本语法如下：
(参数1, 参数2, …, 参数N) => { 函数声明 }
(参数1, 参数2, …, 参数N) => 表达式（单一）
// 当只有一个参数时，圆括号是可选的：
(单一参数) => {函数声明}
单一参数 => {函数声明}
// 没有参数的函数应该写成一对圆括号。
() => {函数声明}
如果返回一个对象，需要特别注意，如果是单表达式要返回自定义对象，不写括号会报错，因为和函数体的{ ... }有语法冲突。

注意，用小括号包含大括号则是对象的定义，而非函数主体
x => {key: x} // 报错
x => ({key: x}) // 正确
箭头函数内部的this是词法作用域，由上下文确定。（词法作用域就是定义在词法阶段的作用域。换句话说，词法作用域是由你在写代码时将变量和块作用域写在哪里来决定的，因此当词法分析器处理代码时会保持作用域不变 。）

17. JSONP原理及简单实现
尽管浏览器有同源策略，但是 <script> 标签的 src 属性不会被同源策略所约束，可以获取任意服务器上的脚本并执行。jsonp 通过插入 script 标签的方式来实现跨域，参数只能通过 url 传入，仅能支持 get 请求。
实现原理:

Step1: 创建 callback 方法
Step2: 插入 script 标签
Step3: 后台接受到请求，解析前端传过去的 callback 方法，返回该方法的调用，并且数据作为参数传入该方法
Step4: 前端执行服务端返回的方法调用

18. 实现一个数组去重的方法

法1: 利用ES6新增数据类型 Set
Set类似于数组，但是成员的值都是唯一的，没有重复的值。
function uniq(arry) {
    return [...new Set(arry)];
}

复制代码法2: 利用 indexOf
function uniq(arry) {
    var result = [];
    for (var i = 0; i < arry.length; i++) {
        if (result.indexOf(arry[i]) === -1) {
            //如 result 中没有 arry[i],则添加到数组中
            result.push(arry[i])
        }
    }
    return result;
}

复制代码法3: 利用 includes


function uniq(arry) {
    var result = [];
    for (var i = 0; i < arry.length; i++) {
        if (!result.includes(arry[i])) {
            //如 result 中没有 arry[i],则添加到数组中
            result.push(arry[i])
        }
    }
    return result;
}

复制代码法4：利用 reduce

function uniq(arry) {
    return arry.reduce((prev, cur) => prev.includes(cur) ? prev : [...prev, cur], []);
}

复制代码法5：利用 Map

function uniq(arry) {
    let map = new Map();
    let result = new Array();
    for (let i = 0; i < arry.length; i++) {
        if (map.has(arry[i])) {
            map.set(arry[i], true);
        } else {
            map.set(arry[i], false);
            result.push(arry[i]);
        }
    }
    return result;
}

19. 清除浮动的方法有哪些？
当容器的高度为auto，且容器的内容中有浮动（float为left或right）的元素，在这种情况下，容器的高度不能自动伸长以适应内容的高度，使得内容溢出到容器外面而影响（甚至破坏）布局的现象。这个现象叫浮动溢出，为了防止这个现象的出现而进行的CSS处理，就叫CSS清除浮动。

1. 利用 clear 属性
在 <div class='outer'> 内创建一个空元素，对其设置 clear: both; 的样式。

优点：简单，代码少，浏览器兼容性好。
缺点：需要添加大量无语义的html元素，代码不够优雅，后期不容易维护。

2. 利用 clear 属性 + 伪元素
.outer:after{
    content: '';
    display: block;
    clear: both;
    visibility: hidden;
    height: 0;
}
复制代码IE8以上和非IE浏览器才支持:after，如果想要支持IE6、7，需要给 outer 元素，设置样式 zoom: 1;
3. 利用 BFC 布局规则
根据 BFC 的规则，计算 BFC 的高度时，浮动元素也参与计算。因此清除浮动，只需要触发一个BFC即可。

可以使用以下方法来触发BFC


position 为 absolute 或 fixed
overflow 不为 visible 的块元素
display 为 inline-block, table-cell, table-caption

如：
.outer {
    overflow: hidden;
}
复制代码注意使用 display: inline-block 会产生间隙。

20. 编写一个通用的柯里化函数 currying
函数柯里化是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。
const currying = (fn, ...args) =>
    args.length < fn.length
        //参数长度不足时，重新柯里化该函数，等待接受新参数
        ? (...arguments) => currying(fn, ...args, ...arguments)
        //参数长度满足时，执行函数
        : fn(...args);

function sumFn(a, b, c) {
    return a + b + c;
}
var sum = currying(sumFn);
console.log(sum(2)(3)(5));//10
console.log(sum(2, 3, 5));//10
console.log(sum(2)(3, 5));//10
console.log(sum(2, 3)(5));//10

函数柯里化的主要作用：
参数复用
提前返回 – 返回接受余下的参数且返回结果的新函数
延迟执行 – 返回新函数，等待执行

1.	new的实现原理是什么？
new 的实现原理:

1）创建一个空对象，构造函数中的this指向这个空对象
2）这个新对象被执行 [[原型]] 连接
3）执行构造函数方法，属性和方法被添加到this引用的对象中
4）如果构造函数中没有返回其它对象，那么返回this，即创建的这个的新对象，否则，返回构造函数中返回的对象。
 

2.	如何正确判断this的指向？
如果用一句话说明 this 的指向，那么即是: 谁调用它，this 就指向谁。
但是仅通过这句话，我们很多时候并不能准确判断 this 的指向。因此我们需要借助一些规则去帮助自己：
this 的指向可以按照以下顺序判断:
全局环境中的 this
浏览器环境：无论是否在严格模式下，在全局执行环境中（在任何函数体外部）this 都指向全局对象 window;
node 环境：无论是否在严格模式下，在全局执行环境中（在任何函数体外部），this 都是空对象 {};

是否是 new 绑定
如果是 new 绑定，并且构造函数中没有返回 function 或者是 object，那么 this 指向这个新对象。如下:
构造函数返回值不是 function 或 object。new Super() 返回的是 this 对象。
 
构造函数返回值是 function 或 object，new Super()是返回的是Super中返回的对象。
 
函数是否通过 call,apply 调用，或者使用了 bind 绑定，如果是，那么this绑定的就是指定的对象【归结为显式绑定】。

这里同样需要注意一种特殊情况，如果 call,apply 或者 bind 传入的第一个参数值是 undefined 或者 null，严格模式下 this 的值为传入的值 null /undefined。非严格模式下，实际应用的默认绑定规则，this 指向全局对象(node环境为global，浏览器环境为window)

 
隐式绑定，函数的调用是在某个对象上触发的，即调用位置上存在上下文对象。典型的隐式调用为: xxx.fn()

默认绑定，在不能应用其它绑定规则时使用的默认规则，通常是独立函数调用。
非严格模式： node环境，执行全局对象 global，浏览器环境，执行全局对象 window。
严格模式：执行 undefined

箭头函数的情况：
箭头函数没有自己的this，继承外层上下文绑定的this。

3.	深拷贝和浅拷贝的区别是什么？实现一个深拷贝
深拷贝和浅拷贝是针对复杂数据类型来说的，浅拷贝只拷贝一层，而深拷贝是层层拷贝。
深拷贝
深拷贝复制变量值，对于非基本类型的变量，则递归至基本类型变量后，再复制。 深拷贝后的对象与原来的对象是完全隔离的，互不影响，对一个对象的修改并不会影响另一个对象。

浅拷贝
浅拷贝是会将对象的每个属性进行依次复制，但是当对象的属性值是引用类型时，实质复制的是其引用，当引用指向的值改变时也会跟着变化。

浅拷贝实现：
可以使用 for in、 Object.assign、 扩展运算符 ... 、Array.prototype.slice()、Array.prototype.concat() 等，例如:
 
可以看出浅拷贝只最第一层属性进行了拷贝，当第一层的属性值是基本数据类型时，新的对象和原对象互不影响，但是如果第一层的属性值是复杂数据类型，那么新对象和原对象的属性值其指向的是同一块内存地址。

深拷贝实现
1.	深拷贝最简单的实现是: JSON.parse(JSON.stringify(obj))
JSON.parse(JSON.stringify(obj)) 是最简单的实现方式，但是有一些缺陷：

1）对象的属性值是函数时，无法拷贝。
2）原型链上的属性无法拷贝
3）不能正确的处理 Date 类型的数据
4）不能处理 RegExp
5）会忽略 symbol
6）会忽略 undefined

2.实现一个 deepClone 函数

1）如果是基本数据类型，直接返回
2）如果是 RegExp 或者 Date 类型，返回对应类型
3）如果是复杂数据类型，递归。
4）考虑循环引用的问题
 

4.	call/apply 的实现原理是什么？
call 和 apply 的功能相同，都是改变 this 的执行，并立即执行函数。区别在于传参方式不同。
func.call(thisArg, arg1, arg2, ...)：第一个参数是 this 指向的对象，其它参数依次传入。
func.apply(thisArg, [argsArray])：第一个参数是 this 指向的对象，第二个参数是数组或类数组。

一起思考一下，如何模拟实现 call ？
首先，我们知道，函数都可以调用 call，说明 call 是函数原型上的方法，所有的实例都可以调用。即: Function.prototype.call。

1）在 call 方法中获取调用call()函数
2）如果第一个参数没有传入，那么默认指向 window / global(非严格模式)
3）传入 call 的第一个参数是 this 指向的对象，根据隐式绑定的规则，我们知道 obj.foo(), foo() 中的 this 指向 obj;因此我们可以这样调用函数 thisArgs.func(...args)
4）返回执行结果
 
apply 的实现思路和 call 一致，仅参数处理略有差别。如下：
 

5.	柯里化函数实现
在开始之前，我们首先需要搞清楚函数柯里化的概念。
函数柯里化是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。
 
 
函数柯里化的主要作用：
1）参数复用
2）提前返回 – 返回接受余下的参数且返回结果的新函数
3）延迟执行 – 返回新函数，等待执行

6.	什么是BFC？BFC的布局规则是什么？如何创建BFC？
Box 是 CSS 布局的对象和基本单位，页面是由若干个Box组成的。
元素的类型 和 display 属性，决定了这个 Box 的类型。不同类型的 Box 会参与不同的 Formatting Context。
Formatting Context 是页面的一块渲染区域，并且有一套渲染规则，决定了其子元素将如何定位，以及和其它元素的关系和相互作用。
Formatting Context 有 BFC (Block formatting context)，IFC (Inline formatting context)，FFC (Flex formatting context) 和 GFC (Grid formatting context)。FFC 和 GFC 为 CC3 中新增。

BFC布局规则
BFC内，盒子依次垂直排列。
BFC内，两个盒子的垂直距离由 margin 属性决定。属于同一个BFC的两个相邻Box的margin会发生重叠【符合合并原则的margin合并后是使用大的margin】
BFC内，每个盒子的左外边缘接触内部盒子的左边缘（对于从右到左的格式，右边缘接触）。即使在存在浮动的情况下也是如此。除非创建新的BFC。
BFC的区域不会与float box重叠。
BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
计算BFC的高度时，浮动元素也参与计算。

如何创建BFC
根元素
浮动元素（float 属性不为 none）
position 为 absolute 或 fixed
overflow 不为 visible 的块元素
display 为 inline-block, table-cell, table-caption

BFC 的应用
1)防止 margin  重叠 (同一个BFC内的两个两个相邻Box的 margin 会发生重叠，触发生成两个BFC，即不会重叠)
2)清除内部浮动 (创建一个新的 BFC，因为根据 BFC 的规则，计算 BFC 的高度时，浮动元素也参与计算)
3)自适应多栏布局 (BFC的区域不会与float box重叠。因此，可以触发生成一个新的BFC)

7.	请实现一个 flattenDeep 函数，把嵌套的数组扁平化
什么是数组扁平化
扁平化，顾名思义就是减少复杂性装饰，使其事物本身更简洁、简单，突出主题。
数组扁平化，对着上面意思套也知道了，就是将一个复杂的嵌套多层的数组，一层一层的转化为层级较少或者只有一层的数组。
1. 利用 Array.prototype.flat
ES6 为数组实例新增了 flat 方法，用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数组没有影响。
flat 默认只会 “拉平” 一层，如果想要 “拉平” 多层的嵌套数组，需要给 flat 传递一个整数，表示想要拉平的层数。

 
当传递的整数大于数组嵌套的层数时，会将数组拉平为一维数组，JS能表示的最大数字为 Math.pow(2, 53) - 1，因此我们可以这样定义 flattenDeep 函数
 
2. 利用 reduce 和 concat
 

3.使用 stack 无限反嵌套多层嵌套数组
 

4. 普通递归
/* ES6 */
const flatten = (arr) => {
  let result = [];
  arr.forEach((item, i, arr) => {
    if (Array.isArray(item)) {
      result = result.concat(flatten(item));
    } else {
      result.push(arr[i])
    }
  })
  return result;
};
 
const arr = [1, [2, [3, 4]]];
console.log(flatten(arr));

5. toString() 该方法是利用 toString 把数组变成以逗号分隔的字符串，然后遍历数组把每一项再变回原来的类型。
const flatten = (arr) => arr.toString().split(',').map((item)=>+item);
console.log(flatten(arr))


8.	ES5有几种方式可以实现继承？分别有哪些优缺点？
ES5 有 6 种方式可以实现继承，分别为：
1. 原型链继承
原型链继承的基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。
缺点：
1）通过原型来实现继承时，原型会变成另一个类型的实例，原先的实例属性变成了现在的原型属性，该原型的引用类型属性会被所有的实例共享。
2）在创建子类型的实例时，没有办法在不影响所有对象实例的情况下给超类型的构造函数中传递参数。
如 SubType.prototype = new SuperType();
 
2. 借用构造函数
借用构造函数的技术，其基本思想为:在子类型的构造函数中调用超类型构造函数。
 
优点:
可以向超类传递参数
解决了原型中包含引用类型值被所有实例共享的问题

缺点:
方法都在构造函数中定义，函数复用无从谈起，另外超类型原型中定义的方法对于子类型而言都是不可见的。
3. 组合继承(原型链 + 借用构造函数)
组合继承指的是将原型链和借用构造函数技术组合到一块，从而发挥二者之长的一种继承模式。
基本思路：
使用原型链实现对原型属性和方法的继承，通过借用构造函数来实现对实例属性的继承，既通过在原型上定义方法来实现了函数复用，又保证了每个实例都有自己的属性。
 
缺点:
无论什么情况下，都会调用两次超类型构造函数：一次是在创建子类型原型的时候，另一次是在子类型构造函数内部。
 
优点:
可以向超类传递参数
每个实例都有自己的属性
实现了函数复用

4. 原型式继承
原型继承的基本思想：借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型。
 
在 object() 函数内部，先穿甲一个临时性的构造函数，然后将传入的对象作为这个构造函数的原型，最后返回了这个临时类型的一个新实例，从本质上讲，object() 对传入的对象执行了一次浅拷贝。
ECMAScript5通过新增 Object.create()方法规范了原型式继承。这个方法接收两个参数：一个用作新对象原型的对象和（可选的）一个为新对象定义额外属性的对象(可以覆盖原型对象上的同名属性)，在传入一个参数的情况下，Object.create() 和 object() 方法的行为相同。
 
在没有必要创建构造函数，仅让一个对象与另一个对象保持相似的情况下，原型式继承是可以胜任的。
缺点:
同原型链实现继承一样，包含引用类型值的属性会被所有实例共享。

5. 寄生式继承
寄生式继承是与原型式继承紧密相关的一种思路。寄生式继承的思路与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数在内部已某种方式来增强对象，最后再像真地是它做了所有工作一样返回对象。
 
基于 person 返回了一个新对象 -—— person2，新对象不仅具有 person 的所有属性和方法，而且还有自己的 sayHi() 方法。在考虑对象而不是自定义类型和构造函数的情况下，寄生式继承也是一种有用的模式。
缺点：
使用寄生式继承来为对象添加函数，会由于不能做到函数复用而效率低下。
同原型链实现继承一样，包含引用类型值的属性会被所有实例共享。

6. 寄生组合式继承
所谓寄生组合式继承，即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法，基本思路：
不必为了指定子类型的原型而调用超类型的构造函数，我们需要的仅是超类型原型的一个副本，本质上就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型。寄生组合式继承的基本模式如下所示：
 
第一步：创建超类型原型的一个副本
第二步：为创建的副本添加 constructor 属性
第三步：将新创建的对象赋值给子类型的原型
至此，我们就可以通过调用 inheritPrototype 来替换为子类型原型赋值的语句
 
优点:
只调用了一次超类构造函数，效率更高。避免在SuberType.prototype上面创建不必要的、多余的属性，与其同时，原型链还能保持不变。
因此寄生组合继承是引用类型最理性的继承范式。

7.实现 ES6 的 class 语法
 
ES6 的 class 内部是基于寄生组合式继承，它是目前最理想的继承方式，通过 Object.create 方法创造一个空对象，并将这个空对象继承 Object.create 方法的参数，再让子类（subType）的原型对象等于这个空对象，就可以实现子类实例的原型等于这个空对象，而这个空对象的原型又等于父类原型对象（superType.prototype）的继承关系

而 Object.create 支持第二个参数，即给生成的空对象定义属性和属性描述符/访问器描述符，我们可以给这个空对象定义一个 constructor 属性更加符合默认的继承行为，同时它是不可枚举的内部属性（enumerable:false）

而 ES6 的 class 允许子类继承父类的静态方法和静态属性，而普通的寄生组合式继承只能做到实例与实例之间的继承，对于类与类之间的继承需要额外定义方法，这里使用 Object.setPrototypeOf 将 superType 设置为 subType 的原型，从而能够从父类中继承静态方法和静态属性

Class 可以通过extends关键字实现继承，如:
 
 

使用展开运算符合并对象和对象数组

8. 解释一下变量的提升
变量的提升是JavaScript的默认行为，这意味着将所有变量声明移动到当前作用域的顶部，并且可以在声明之前使用变量。初始化不会被提升，提升仅作用于变量的声明。

9.	JavaScript如何处理同步和异步情况
尽管JavaScript是一种只有一个调用堆栈的单线程编程语言，但它也可以使用一个称为**事件循环(event loop)**的机制来处理一些异步函数。从基本级别了解JavaScript如何工作是理解JS如何处理异步的关键部分。
 
 
如图所示，调用堆栈是定位函数的位置。一旦函数被调用，函数将被推入堆栈。然而，异步函数不会立即被推入调用堆栈，而是会被推入任务队列(Task Queue)，并在调用堆栈为空后执行。将事件从任务队列传输到调用堆栈称为事件循环。

10.	如何理解事件委托
在DOM树上绑定事件监听器并使用JS事件处理程序是处理客户端事件响应的典型方法。 从理论上讲，我们可以将监听器附加到HTML中的任何DOM元素，但由于事件委派，这样做是浪费而且没必要的。
** 什么是事件委托？**
这是一种让父元素上的事件监听器也影响子元素的技巧。 通常，事件传播（捕获和冒泡）允许我们实现事件委托。 冒泡意味着当触发子元素（目标）时，也可以逐层触发该子元素的父元素，直到它碰到DOM绑定的原始监听器（当前目标）。 捕获属性将事件阶段转换为捕获阶段，让事件下移到元素; 因此，触发方向与冒泡阶段相反。 捕获的默认值为false。

11. 如何理解高阶函数
JavaScript中的一切都是对象，包括函数。我们可以将变量作为参数传递给函数，函数也是如此。我们调用接受和或返回另一个函数称为高阶函数的函数。

12. 如何区分声明函数和表达式函数
// 声明函数
function hello() {
  return "HELLO"
}    
// 表达式函数  
var h1 = function hello() {
  return "HELLO"
}

13. 解释一下严格模式(strict mode)
严格模式用于标准化正常的JavaScript语义。严格模式可以嵌入到非严格模式中，关键字 ‘use strict’。使用严格模式后的代码应遵循JS严格的语法规则。例如，分号在每个语句声明之后使用。

14. ES6模块和 CommonJS 模块有哪些差异？
1. CommonJS 模块是运行时加载，ES6模块是编译时输出接口。

ES6模块在编译时，就能确定模块的依赖关系，以及输入和输出的变量。ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

CommonJS 加载的是一个对象，该对象只有在脚本运行完才会生成。

2.CommonJS 模块输出的是一个值的拷贝，ES6模块输出的是值的引用。

- `CommonJS` 输出的是一个值的拷贝(注意基本数据类型/复杂数据类型)
    
- ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。
3. ES6 模块自动采用严格模式，无论模块头部是否写了 "use strict";
4. require 可以做动态加载，import 语句做不到，import 语句必须位于顶层作用域中。
5. ES6 模块的输入变量是只读的，不能对其进行重新赋值
import name from './name';
name = 'Star'; //抛错
6. 当使用require命令加载某个模块时，就会运行整个模块的代码。
7. 当使用require命令加载同一个模块时，不会再执行该模块，而是取到缓存之中的值。也就是说，CommonJS模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回第一次运行的结果，除非手动清除系统缓存。
1.圣杯布局和双飞翼布局的理解和区别，并用代码实现
圣杯布局和双飞翼布局基本上是一致的，都是两边固定宽度，中间自适应的三栏布局，其中，中间栏放到文档流前面，保证先行渲染。解决方案大体相同，都是三栏全部float:left浮动，区别在于解决中间栏div的内容不被遮挡上，圣杯布局是中间栏在添加相对定位，并配合left和right属性，效果上表现为三栏是单独分开的（如果可以看到空隙的话），而双飞翼布局是在中间栏的div中嵌套一个div，内容写在嵌套的div里，然后对嵌套的div设置margin-left和margin-right，效果上表现为左右两栏在中间栏的上面，中间栏还是100%宽度，只不过中间栏的内容通过margin的值显示在中间。
对比图：
 
 圣杯布局：
<body>
        <div id='header'>header</div>
        <div id='bd'>
            <div id='middle'>middle</div>
            <div id='left'>left</div>
            <div id='right'>right</div>
        </div>
        <div id='footer'>footer</div>
    </body>
css:
<style>
            #header{
                height:50px;
                background:#666;
                text-align: center;
            }
            #bd{
                height:200px;
                padding:0 200px 0 180px;

            }
            #footer{
                height:50px;
                background:#666;
                text-align:center;
            }
            #middle{
                float:left;
                width:100%;
                height:200px;
                background:blue;
            }
            #left{
                float:left;
                width:180px;
                height:200px;
                background:yellow;
                position:relative;/*给left推到左边*/
                left:-180px;/*给left推到左边*/
                margin-left:-100%;/*让left上去，靠左*/
            }
            #right{
                float:left;
                width:200px;
                height:200px;
                background:pink;
                position:relative;/*给right推到右边*/
                right:-200px;/*给right推到右边*/
                margin-left:-200px;/*让right上去，靠右*/
            }
        </style>

双飞翼布局：
<body>
        <div id='header'>header</div>
        <div id='middle'>
            <div id='inside'>middle</div>
        </div>
        <div id='left'>left</div>
        <div id='right'>right</div>
        <div id='footer'>footer</div>
    </body>

css:
<style>
            #header{
                height:50px;
                text-align:center;
                background:#666;
            }
            
            #middle{
                width:100%;
                height:100px;
                background:yellow;
                float:left;
            }
            #left{
                float:left;
                width:180px;
                height:100px;
                background:blue;
                margin-left:-100%;
            }
            #right{
                float:left;
                width:200px;
                height:100px;
                background:pink;
                margin-left:-200px;
            }
            #inside{
                margin:0 200px 0 180px;
                height:100px;
            }
            #footer{
                clear:both;
                height:50px;
                text-align:center;
                background:#666;
            }
        </style>
圣杯布局和双飞翼布局解决的问题是一样的，就是两边顶宽，中间自适应的三栏布局，中间栏要在放在文档流前面以优先渲染。
圣杯布局和双飞翼布局解决问题的方案在前一半是相同的，也就是三栏全部float浮动，但左右两栏加上负margin让其跟中间栏div并排，以形成三栏布局。
不同在于解决”中间栏div内容不被遮挡“问题的思路不一样：
圣杯布局，为了中间div内容不被遮挡，将中间div设置了左右padding-left和padding-right后，将左右两个div用相对布局position: relative并分别配合right和left属性，以便左右两栏div移动后不遮挡中间div。
双飞翼布局，为了中间div内容不被遮挡，直接在中间div内部创建子div用于放置内容，在该子div里用margin-left和margin-right为左右两栏div留出位置。
多了1个div，少用大致4个css属性（圣杯布局中间divpadding-left和padding-right这2个属性，加上左右两个div用相对布局position: relative及对应的right和left共4个属性，一共6个；而双飞翼布局子div里用margin-left和margin-right共2个属性，6-2=4）

typeof 对于原始类型来说，除了 null 都可以显示正确的类型
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol() // 'symbol'
typeof 对于对象来说，除了函数都会显示 object，所以说 typeof 并不能准确判断变量到底是什么类型
typeof [] // 'object'
typeof {} // 'object'
typeof console.log // 'function'
如果我们想判断一个对象的正确类型，这时候可以考虑使用 instanceof，因为内部机制是通过原型链来判断的
首先我们要知道，在 JS 中类型转换只有三种情况，分别是：

转换为布尔值
转换为数字
转换为字符串

在条件判断时，除了 undefined， null， false， NaN， ''， 0， -0，其他所有值都转为 true，包括所有对象。
加法运算符不同于其他几个运算符，它有以下几个特点：
1)运算中其中一方为字符串，那么就会把另一方也转换为字符串
2)如果一方不是字符串或者数字，那么会将它转换为数字或者字符串

首先，new 的方式优先级最高，接下来是 bind 这些函数，然后是 obj.foo() 这种调用方式，最后是 foo 这种调用方式，同时，箭头函数的 this 一旦被绑定，就不会再被任何方式所改变。

在 JS 中，闭包存在的意义就是让我们可以间接访问函数内部的变量。

for (var i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i)
  }, i * 1000)
}
解决办法：
1）
for(let i=1;i<=5;i++){
  setTimeout(function timer(){
    console.log(i)
  },i*1000)
}
2）使用 setTimeout 的第三个参数，这个参数会被当成 timer 函数的参数传入。
for(var i=1;i<=5;i++){
  setTimeout(function timer(j){
    console.log(j)
  },i*1000,i)
}
3）使用闭包的方式
for(var i=1;i<=5;i+=){
  (function(j){
    setTimeout(function timer(){
      console.log(j)
    },j*1000)
  })(i)
}
首先使用了立即执行函数将 i 传入函数内部，这个时候值就被固定在了参数 j 上面不会改变，当下次执行 timer 这个闭包的时候，就可以使用外部函数的变量 j，从而达到目的。

原型的 constructor 属性指向构造函数，构造函数又通过 prototype 属性指回原型，但是并不是所有函数都具有这个属性，Function.prototype.bind() 就没有这个属性。

[html] 第1天 页面导入样式时，使用link和@import有什么区别？
1）从属关系的区别：link属于XHTML标签，而@import是CSS提供的语法规则，link除了加载CSS，还可以定义RSS，定义rel连接属性等，@import就只能加载CSS。
2）加载顺序的区别：页面加载时，link会同时被加载，而@import引用的CSS会等页面被加载完后再加载。
3）兼容性的区别：@import只有IE5以上才能被识别，而link是XHTML标签，无兼容问题。
4）DOM可控性区别：通过js操作DOM,可以插入link标签来改变样式；由于DOM方法是基于文档的，无法使用@import方式插入样式

写一个方法去掉字符串中的空格
var str = '  abc d e f  g ';
str = str.replace(/\s/g,'');
str = str.split(' ').join(''); 
console.log(str)

HTML 全局属性：可用于任何 HTML 元素。
属性	描述
accesskey
规定激活元素的快捷键。
class
规定元素的一个或多个类名（引用样式表中的类）。
contenteditable
规定元素内容是否可编辑。
contextmenu
规定元素的上下文菜单。上下文菜单在用户点击元素时显示。
data-*
用于存储页面或应用程序的私有定制数据。
dir
规定元素中内容的文本方向。
draggable
规定元素是否可拖动。
dropzone
规定在拖动被拖动数据时是否进行复制、移动或链接。
hidden
规定元素仍未或不再相关。
id
规定元素的唯一 id。
lang
规定元素内容的语言。
spellcheck
规定是否对元素进行拼写和语法检查。
style
规定元素的行内 CSS 样式。
tabindex
规定元素的 tab 键次序。
title
规定有关元素的额外信息。
translate
规定是否应该翻译元素内容。

JavaScript中label语句的使用
break和continue语句都可以与label联合使用，从而达到代码中特定的位置，这种联合使用的情况多发生在循环嵌套的情况下。
var num = 0;
    outPoint://这里为label，标签名为outPoint
    for (var i = 0; i < 10; i++) {
        for (var j = 0; j < 10; j++) {
            if (i == 5 && j == 5) {
                break outPoint;
            }else{
                console.log(i,j,88);
            }
            num++;
        }
    }
    console.log(num); //55

内部循环中break语句带了一个参数，要返回到的标签。添加这个标签的结果将导致break语句不仅会退出内部for循环，而且也会退出外部的for循环

var num = 0;
    outermost:
    for (var i = 0; i < 10; i++) {
        for (var j = 0; j < 10; j++) {
            if (i == 5 && j == 5) {
                continue outermost;
            }else{
                console.log(i,j,88);
            }
            num++;
        }
    }
    console.log(num); //95
在这种情况下，continue语句会强制继续执行循环（退出内部循环，执行外部循环）。当变量i和j都等于5时，num的值正好是95.

HTML <label> 标签
所有主流浏览器都支持 <label> 标签。
定义和用法
<label> 标签为 input 元素定义标注（标记）。

label 元素不会向用户呈现任何特殊效果。不过，它为鼠标用户改进了可用性。如果您在 label 元素内点击文本，就会触发此控件。就是说，当用户选择该标签时，浏览器就会自动将焦点转到和标签相关的表单控件上。

<label> 标签的 for 属性应当与相关元素的 id 属性相同。

属性	值	描述
for
id	规定 label 绑定到哪个表单元素。
form
formid	规定 label 字段所属的一个或多个表单。


JavaScript DOM操作：


DOM 是 W3C（万维网联盟）的标准。 

DOM 定义了访问 HTML 和 XML 文档的标准：

“W3C 文档对象模型 （DOM） 是中立于平台和语言的接口，它允许程序和脚本动态地访问和更新文档的内容、结构和样式。”

W3C DOM 标准被分为 3 个不同的部分：

核心 DOM - 针对任何结构化文档的标准模型

XML DOM - 针对 XML 文档的标准模型

HTML DOM - 针对 HTML 文档的标准模型

浏览器对象模型（BOM）以 window 对象为依托，表示浏览器窗口以及页面可见区域。同时， window对象还是 ECMAScript 中的 Global 对象，

因而所有全局变量和函数都是它的属性，且所有原生的构造函数及其他函数也都存在于它的命名空间下。

BOM 和 DOM 的关系图解
 

DOM 是为了操作文档出现的 API，document 是其的一个对象；

BOM 是为了操作浏览器出现的 API，window 是其的一个对象。

正常模式和严格模式
ECMAScript 5 的严格模式是可选择的一个限制JavaScript的变体一种方式 。
严格模式不仅仅是一个子集：它故意地具有与正常代码不同的语义。
不支持严格模式的浏览器将会执行与支持严格模式的浏览器不同行为的严格模式代码。

进入"严格模式"的标志，是下面这行语句："use strict";

一、引用方式
严格模式标志
1
2
3	  
    "use strict";
  

针对整个脚本文件
1
2
3
4
5
6
7
8
9	  
<script>
    "use strict";
    console.log("这是严格模式。");
</script>
<script>
    console.log("这是正常模式。");
</script>

针对单个函数的严格模式
1
2
3
4
5
6
7
8	  
    function strict(){
　　  "use strict";
　　　　return "这是严格模式。";
　　}
　　function notStrict() {
　　　　return "这是正常模式。";
　　}

针对模块的严格模式
1
2
3
4
5	   
  require(['moduleA', 'moduleB', 'moduleC'], function (moduleA, moduleB, moduleC){
      "use strict";
　　// some code here
});

声明匿名函数
1
2
3
4	"use strict";
function (){
   console.info(123)
}

二、语法和行为改变
2.1 全局变量显式声明

变量声明
1
2
3
4	  "use strict";
v = 1; // 报错，v未声明
for(i = 0; i < 2; i++) { // 报错，i未声明
}
在正常模式中，如果一个变量没有声明就赋值，默认是全局变量。严格模式禁止这种用法，全局变量必须显式声明。

2.2 静态绑定
Javascript 语言的一个特点，就是允许"动态绑定"，即某些属性和方法到底属于哪一个对象，不是在编译时确定的，而是在运行时（runtime）确定的。

禁止使用with语句
1
2
3
4
5
6
7
8
9	"use strict";
function Company(name) { 
    this.name = name;  
} 
var HomeCredit = new Company("HOME CREDIT"); 
with(HomeCredit){     // 语法错误
    var welcome= "Hello  " + name + " !";        
    console.info(welcome); 
}
严格模式对动态绑定做了一些限制。某些情况下，只允许静态绑定。也就是说，属性和方法到底归属哪个对象，在编译阶段就确定。
这样做有利于编译效率的提高，也使得代码更容易阅读，更少出现意外。
 
创建eval作用域
1
2
3
4	"use strict";
var x = 2;
console.info(eval("var x = 5; x")); // 5
console.info(x); // 2


正常模式下，JavaScript语言有两种变量作用域（scope）：全局作用域和函数作用域。严格模式创设了第三种作用域：eval作用域。
正常模式下，eval语句的作用域，取决于它处于全局作用域，还是处于函数作用域。严格模式下，eval语句本身就是一个作用域，不再能够生成全局变量了，它所生成的变量只能用于eval内部。

2.3 增强的安全措施
禁止 this 关键字指向全局对象
示例一
1
2
3
4
5
6
7
8
9
10	    function f(){
　　　　return !this;
　　}
　　// 返回false，因为"this"指向全局对象，"!this"就是false
　　function f(){
　　　　"use strict";
　　　　return !this;
　　}
　　// 返回true，因为严格模式下，this的值为undefined，所以"!this"为true。
  
示例二
1
2
3
4
5	function f(){
　　　　"use strict";
　　　　this.a = 1;
　　};
　　f();// 报错，this未定义
禁止在函数内部遍历调用栈
1
2
3
4
5
6
7
8	
function f1(){
　　　　"use strict";
　　　　f1.caller; // 报错
　　　　f1.arguments; // 报错
　　}
　　f1();

为什么使用严格模式:

消除JavaScript语法的一些不合理、不严谨之处，减少一些怪异行为;
消除代码运行的一些不安全之处，保证代码运行的安全；
提高编译器效率，增加运行速度；
为未来新版本的JavaScript做好铺垫。

语言类型
JS 属于动态弱类型语言。（Java是静态强类型）。        
弱类型相对于强类型来说类型检查更不严格，比如说允许变量类型的隐式转换，允许强制类型转换等等。
强类型语言一般不允许这么做。

二.引用类型转值类型:
  （1）toString() 方法。
  （2）valueOf() 方法。
通常情况下我们认为，将一个对象转换为字符串要调用 toString() 方法，转换为数字要调用 valueOf() 方法，但是真正应用的时候并没有这么简单

总结如下:

（1）有些对象只是简单继承了 toString() 或者 valueOf() 方法，比如第一个例子。
（2）有些对象则不但是继承了两个方法，而且还进行了重写。
所以有些对象的方法能够达成转换成字符串或者数字的目标，有些则不能。

调用 toString() 或者 valueOf() 将对象转换成字符串或者数字的规则如下:

调用 toString() 时，如果对象具有这个方法，则调用此方法；如果此方法返回一个值类型数据，那么就返回这个值类型数据，然后再根据所处的上下文环境进行相关数据类型转换。

如果没有 toString()，或者此方法返回值并不是一个值类型数据，那么就会调用valueOf()（如果此方法存在的话），如果valueOf()返回一个值类型数据，那么再根据所处的上下文环境进行相关的数据类型转换。

进一步说明:
（1）上面介绍了通常默认情况下 valueOf() 和 toString() 方法的作用（将对象转换为数字或者字符串），但是需要注意的是，这并不是硬性规定，也就是说并不是 valueOf() 方法必须

要返回数字或者 toString() 方法必须要转换为字符串，比如简单继承的这两个方法就无法进行实现转换为数字和字符串的功能，再比如，我们可以自己重写这两个方法，返回值也没有必要是数字或者字符串。

（2）还有需要特别注意的一点就是，很多朋友认为，转换为字符串首先要调用 toString() 方法， 其实这是错误的认识，我们应该这么理解，调用 toString() 方法可以转换为字符串，但不一定转换字符串就是首先调用 toString() 方法。

总结如下:
大多数对象隐式转换为值类型都是首先尝试调用 valueOf() 方法。但是 Date对象 是个例外，此对象的 valueOf() 和 toString() 方法都经过精心重写，默认是调用 toString() 方法，比如使用+运算符，如果在其他算数运算环境中，则会转而调用 valueOf() 方法。
JS中所有数值不管是整数还是浮点数都是以浮点数的形式保存的。

机制

网页生成过程
 
大致可以分成五步：

1)HTML 代码转化成 DOM。
2)CSS 代码转化成 CSSOM（CSS Object Model）
3)结合 DOM和 CSSOM，生成一棵渲染树（包含每个节点的视觉信息）。
4)生成布局（layout），即将所有渲染树的所有节点进行平面合成。
5)将布局绘制（paint）在屏幕上。

重排和重绘
 "布局"（layout）和"绘制"（paint）这两步，合称为"渲染"（render）。
重新渲染，就需要重新生成布局和重新绘制。前者叫做"重排"（reflow），后者叫做"重绘"（repaint）。

需要注意的是，"重绘"不一定需要"重排"，比如改变某个网页元素的颜色，就只会触发"重绘"，不会触发"重排"，因为布局没有改变。但是，"重排"必然导致"重绘"，比如改变一个网页元素的位置，就会同时触发"重排"和"重绘"，因为布局改变了。

一般的规则是：
样式表越简单，重排和重绘就越快。
重排和重绘的DOM元素层级越高，成本就越高。
table元素的重排和重绘成本，要高于div元素。

执行机制

单线程
JavaScript语言的一大特点就是单线程，也就是说，同一个时间只能做一件事。那么，为什么JavaScript不能有多个线程呢？这样能提高效率啊。

 JavaScript的单线程，与它的用途有关。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。

比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？

所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变。

为了利用多核CPU的计算能力，HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程，但是子线程完全受主线程控制，且不得操作DOM。所以，这个新标准并没有改变JavaScript单线程的本质。

任务队列

单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。

如果排队是因为计算量大，CPU忙不过来，倒也算了，但是很多时候CPU是闲着的，因为IO设备（输入输出设备）很慢（比如Ajax操作从网络读取数据），不得不等着结果出来，再往下执行。

JavaScript语言的设计者意识到，这时主线程完全可以不管IO设备，挂起处于等待中的任务，先运行排在后面的任务。等到IO设备返回了结果，再回过头，把挂起的任务继续执行下去。

于是，所有任务可以分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous）。同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；

异步任务指的是，不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

具体来说，异步执行的运行机制如下。（同步执行也是如此，因为它可以被视为没有异步任务的异步执行。）
（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
（4）主线程不断重复上面的第三步。

定义函数的方式：1）函数声明
                2）函数表达式
                3）new Function()
                4)箭头函数
函数声明的调用方式:1)fn()
                   2)fn.call(fn,’aaa’)
                   3)fn.apply(fn,[‘aaa’])
                   4)new fn()
                   5)setTimeout(fn(),0)
                   6)fn`aaa`
                   7)(function fn(){})()
标签模板
模板字符串的功能，不仅是上面那些，它还可以紧跟在一个函数后面，该函数将被调用来处理这个模板字符串，这种称为“标签模板”功能(Tagged template)。
标签模板函数第一个参数是字符串模板的常量数组，后面的每一个参数为表达式的计算结果，函数名称可以任意指定。
标签模板其它是一种特殊的函数调用形式，“标签”指的就是函数，紧跟在后面的模板字符串就是它的参数。

函数表达式：

const fn = function(text){
  console.log(text)
}('函数表达式')

声明函数的第三种方法new Function – 不推荐使用
new Function (arg1, arg2, ... , function_body) 
Function的最后一个参数是函数体，之前的参数是函数的参数。
Function的参数必须是字符串。
function add(a, b) {
  retrun a + b;
}
//等价于
var add = new Function ('a', 'b', 'return a + b');
()中只能放一个表达式
function add(){
    var a=1;
    a+=1;
    alert(a);
  }()
//函数声明后面加()不会立即执行，报错
  var add = function(){
    var a = 1;
    a+=1;
    alert(a);
  }()
//函数表达式后面加()会立即执行

函数声明转为函数表达式的方式：
1）
(function fn(){})()

2）
(function fn(){}())

自执行函数的作用：1)不必为函数命名，避免全局变量的使用
函数变成表达式的方式：
0 + function(text){
    console.log(text);
  }('aaa');

  0,function(text){
    console.log(text);
  }('bbb')
  
  true && function(text){
    console.log(text)
  }('ccc')
  
  false || function(text){
    console.log(text)
  }('ddd')

  ~function(text){
    console.log(text)
  }('eee')

  +function(text){
    console.log(text)
  }('fff')

  -function(text){
    console.log(text)
  }('ggg')

  typeof function(text){
    console.log(text)
  }('hhh')

  new function(text){
    console.log(text)
  }('iii')

逗号操作符  对它的每个操作数求值（从左到右），并返回最后一个操作数的值。

;function aa(){}
分号是为了和前面的代码隔开，js可以用换行分隔代码，但是合并压缩多个js文件之后，换行符一般会被删掉，所以连在一起可能会出错，加上分号就保险了。

自执行函数的作用：2）模块化开发
3）惰性函数（可以提高性能）
https://www.cnblogs.com/xiaohuochai/p/8028192.html
惰性函数表示函数执行的分支只会在函数第一次调用的时候执行，在第一次调用过程中，该函数会被覆盖为另一个按照合适方式执行的函数，这样任何对原函数的调用就不用再经过执行的分支了。
函数重写
在介绍惰性函数之前，首先介绍函数重写技术。由于一个函数可以返回另一个函数，因此可以用新的函数来覆盖旧的函数
惰性函数
惰性函数的本质就是函数重写。所谓惰性载入，指函数执行的分支只会发生一次，有两种实现惰性载入的方式
1、	第一种是在函数被调用时，再处理函数。函数在第一次调用时，该函数会被覆盖为另外一个按合适方式执行的函数，这样任何对原函数的调用都不用再经过执行的分支了。代码重写如下

function addEvent(type, element, fun) {
if (element.addEventListener) {
          addEvent = function (type, element, fun) {
              element.addEventListener(type, fun, false);
          }
      }
      else if(element.attachEvent){
          addEvent = function (type, element, fun) {
              element.attachEvent('on' + type, fun);
          }
      }
      else{
          addEvent = function (type, element, fun) {
              element['on' + type] = fun;
          }
      }
      return addEvent(type, element, fun);
    }
在这个惰性载入的addEvent()中，if语句的每个分支都会为addEvent变量赋值，有效覆盖了原函数。最后一步便是调用了新赋函数。下一次调用addEvent()时，便会直接调用新赋值的函数，这样就不用再执行if语句了
但是，这种方法有个缺点，如果函数名称有所改变，修改起来比较麻烦

2、	第二种是声明函数时就指定适当的函数。把嗅探浏览器的操作提前到代码加载的时候，在代码加载的时候就立刻进行一次判断，以便让addEvent返回一个包裹了正确逻辑的函数

var addEvent = (function () {
      if (document.addEventListener) {
          return function (type, element, fun) {
              element.addEventListener(type, fun, false);
          }
      }
      else if (document.attachEvent) {
          return function (type, element, fun) {
              element.attachEvent('on' + type, fun);
          }
      }
      else {
          return function (type, element, fun) {
              element['on' + type] = fun;
          }
      }
  })();

ES6代码，很多时候都会看到三个点(...)的存在,它在ES6语法中，有两种应用形式，分别为函数中的rest参数，以及扩展运算符

ES6 rest参数
rest参数（形式为“...变量名”）可以称为不定参数，rest参数和一个变量名搭配使用，生成一个数组，用于获取函数多余的参数
rest参数作用： 将多余的逗号分隔的参数序列转换为数组参数
注意： rest参数必须是最后一个参数，否则报错
函数的 length 属性，不包括rest参数
如果传入的参数连正常定义的参数都没填满，也不要紧，rest参数会接收一个空数组（注意不是undefined）。
Rest 参数接受函数的多余参数，组成一个数组，放在形参的最后，形式如下：
function func(a, b, ...theArgs){
    // ...
}
(2) Rest参数和arguments对象的区别：
rest参数只包括那些没有给出名称的参数，arguments包含所有参数
arguments 对象不是真正的数组，而rest 参数是数组实例，可以直接应用sort, map, forEach, pop等方法
arguments 对象拥有一些自己额外的功能
扩展运算符
扩展运算符可以理解为rest参数的逆运算，将数组转换为逗号分隔的参数序列
扩展运算符的应用(常用)
a、合并数组
扩展运算符提供了数组合并的新写法。//es6
arr1 = [1,2,3]
arr2 = [4,5,6]
arr3 = [7,8,9]
arr4 = [...arr1, ...arr2, ...arr3] //[1,2,3,4,5,6,7,8,9]  用于数组合并

arr1.concat(arr2, arr3);  //es5
// [1,2,3,4,5,6,7,8,9]  

b、与解构赋值结合
扩展运算符可以与解构赋值结合起来，用于生成数组。
const [first, ...rest] = [1, 2, 3, 4, 5];  
first // 1  

rest // [2, 3, 4, 5]  
const [first, ...rest] = [];  
first // undefined  
rest // []:  

const [first, ...rest] = ["foo"];  
first // "foo"  
rest // []  

如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。
const [...butLast, last] = [1, 2, 3, 4, 5];  
//  报错  
const [first, ...middle, last] = [1, 2, 3, 4, 5];  
//  报错  

什么是 Function Expression（函数表达式）？  
Function Expression 将函数定义为表达式语句（通常是变量赋值）的一部分。
通过 Function Expression 定义的函数可以是命名的，也可以是匿名的。
Function Expression 不能以“function”开头（下面自调用的例子要用括号将其括起来）。

那 Function Declaration（函数声明）是什么？
Function Declaration 可以定义命名的函数变量，而无需给变量赋值。
Function Declaration 是一种独立的结构，不能嵌套在非功能模块中。可以将它类比为 Variable Declaration（变量声明）。
就像 Variable Declaration 必须以“var”开头一样，Function Declaration 必须以“function”开头。

变量的声明

JS不同于JAVA，在变量声明时并不需要声明变量的存储空间。

变量中所存储的数据可以分为两类：基本类型和引用类型。其中数值、布尔值、null和undefined属于基本类型，对象、数组和函数属于引用类型。 

基本类型在内存中具有固定的内存大小。例如：数值型在内存中占有八个字节，布尔值只占有一个字节。

对于引用型数据，他们可以具有任意长度，因此他们的内存大小是不定的，

因此变量中存储的实际上是对此数据的引用，通常是内存地址或者指针，通过它们我们可以找到这个数据。

JS中变量声明分显式声明和隐式声明。
在函数中使用var关键字进行显式申明的变量是做为局部变量，而没有用var关键字，使用直接赋值方式声明的是全局变量。
用 var 定义的变量还有一个特点，就是不能被主动 delete.

我们从最基本的开始，在面向对象的强类型语言中（Java），其作用域都是基于块的（即：{}），块内可以对块外的变量进行操作，但是块外却对块内的变量是无法操作的。
但是JS呢？一门弱类型语言，其并没有实现基于块的作用域，而是基于function的。

JavaScript运行三部曲

脚本执行的整个过程中，js 引擎都做了什么呢？
语法分析
预编译
解释执行

在执行代码前，还有两个步骤 
语法分析很简单，就是引擎检查你的代码有没有什么低级的语法错误 
预编译简单理解就是在内存中开辟一些空间，存放一些变量与函数
解释执行顾名思义便是执行代码了 

js的代码在首次被加载完成后进行编译时，会将所有的function和var提前进行声明，但是并不会对其进行赋值，赋值则都是在该代码块进行执行时才会对其进行赋值，

如果不对变量进行var的话，它是不会存在于function执行的时创建的新Scope的。
   console.log(varscope);  
   var varscope='test' //undefined

   console.log(notVarScope); 
   notVarScope='test1'; //notVarScope is not defined

   var a = 1;
   var b = 2;
   function doit(){
      var b = 3;
      console.log(a);
   }
   doit(4);

 
在js中，var和function在预编译中，作用都是一样的，都是提前声明了变量，但是并没有对其进行赋值，变量及表达式没有对其赋值，函数会被赋值 ，所以这段代码完整的Scope应该是拥有这三个变量的，只是doit是一个指向堆的引用。而在doit执行时，才会创建新的Scope，由于js语言的特殊性，虽然doit在这个Scope里定义的，但是其执行环境可以通过引用改变到任何地方，但是doit这个函数的定义环境永远都是确定的，即这个Scope内。
作用域最大的用处就是隔离变量，不同作用域下同名变量不会有冲突。
概括几点：
第一点：js没有块级作用域（你可以自己闭包或其他方法实现），只有函数级作用域，函数外面的变量函数里面可以找到，函数里面的变量外面找不到。
var a = 10;
function aaa(){    //step-4
    alert(a);//step-5->执行alert，此时只能找到外面的a=10故弹框10
}
function bbb(){//step-2
    var a = 20;
    aaa();//step-3
}
//定义了函数没啥用，调用才是真格的所以这里是step-1
bbb();//step-1
第二点：变量的查找是就近原则，去寻找var定义的变量，当就近没有找到的时候就去查找外层。
var a=10;
function aaa(){
    alert(a);//undefined，查找a的时候会现在函数内查找，由于预解析的作用，此时的a是undefined,因此永远不会去查找外面的10了
    var a = 20;
    /*预解析
    var a
    alert(a);
    var a = 20;*/
}
aaa();

第三点：当参数跟局部变量重名时，优先级是等同的。
var a=10;
function aaa(a){
     console.info(a);
     var a = 20;
}
aaa(a); //10
还有：传参时，基本类型传值，引用类型传引用。（但是重新赋值之后就不是这样了喔）
var a=[1, 2, 3];
var b=a;
b=[1, 2, 3, 4];
console.info(a); //[1, 2, 3]

无论函数是在哪里调用，也无论函数是如何调用的，其确定的词法作用域永远都是在函数被声明的时候确定下来的

总结：
js 没有块级作用域，除了全局作用域之外，只有函数可以创建作用域，所以，我们在声明变量时，全局代码要在代码前端声明，函数中药在函数体一开始就声明好，除了这两个地方，其他地方都不要出现变量声明，而且建议使用“单var” 模式。

二.JS堆栈研究
1、栈（stack）和堆（heap）
stack为自动分配的内存空间，它由系统自动释放；而heap则是动态分配的内存，大小不定也不会自动释放。

2、基本类型和引用类型
（1）基本类型：存放在栈内存中的简单数据段，数据大小确定，内存空间大小可以分配。
5种基本数据类型有Undefined、Null、Boolean、Number 和 String，它们是直接按值存放的，所以可以直接访问。
（2）引用类型：存放在堆内存中的对象，变量实际保存的是一个指针，这个指针指向另一个位置。每个空间大小不一样，要根据情况开进行特定的分配。
当我们需要访问引用类型（如对象，数组，函数等）的值时，首先从栈中获得该对象的地址指针，然后再从堆内存中取得所需的数据。

3、传值与传址
前面之所以要说明什么是内存中的堆、栈以及变量类型，实际上是为了更好的理解什么是“浅拷贝”和“深拷贝”。
基本类型与引用类型最大的区别实际就是传值与传址的区别。

深度拷贝: 既然属性值类型是数组和或象时只会传址，那么我们就用递归来解决这个问题，把父对象中所有属于对象的属性类型都遍历赋给子对象即可。

判断一个变量是不是对象非常简单。值类型的类型判断用 typeof，引用类型的类型判断用 instanceof。

js 是弱类型语言 , 一切（引用类型）都是对象，对象是属性的集合。

函数和对象的关系
对象都是通过函数创建的
//语法糖 var obj = { a: 10, b: 20 }; 
    var obj = new Object();
    obj.a = 10;
    obj.b = 20;
 
    //语法糖 var arr = [5, 'x', true];
    var arr = new Array();
    arr[0] = 5;
    arr[1] = 'x';
    arr[2] = true;
  
    //语法糖 function functionName(arg1, arg2, ..., argN){}
    var functionName = new Function(arg1, arg2, ..., argN,function_body);
    // example
    var test= new Function('a','b','c','alert(a);alert(b);alert(c)');

对象是函数创建的，而函数却又是一种对象
先别迷糊，想知道他们到底什么关系我们得先去了解一下 prototype 原型。

function Fn() { }
    Fn.prototype.name = 'Home Credit';
    Fn.prototype.getYear = function () {
        return 2004;
    };
 
    var fn = new Fn();
    console.log(fn.name);
    console.log(fn.getYear());
即，Fn是一个函数，fn对象是从Fn函数new出来的，这样fn对象就可以调用Fn.prototype中的属性。
因为每个对象都有一个隐藏的属性 “ __proto__ ”，这个属性引用了创建这个对象的函数的prototype。即：fn.__proto__  ===  Fn.prototype
这里的 " __proto__ "  他的官方名字叫 “non-standard __proto__ pseudo-property”、“非标准的__proto__魔法属性”。业内通常称之为 “隐式原型”、“非标准选择器”

new 
特别注意，如果要对prototype进行修改，尽可能的不要使用对象字面量方式进行修改
建议使用 A.prototype.aProp={}；
非常不建议 A.prototype = { aProp : XXX }

执行上下文
什么是“执行上下文”（也叫“执行上下文环境”）？
在一段js代码拿过来真正一句一句运行之前，浏览器已经做了一些“准备工作”，其中就包括对变量的声明，而不是赋值。变量赋值是在赋值语句执行的时候进行的。可用下图模拟：
 
与第一种情况不同的是：第一种情况只是对变量进行声明（并没有赋值），而此种情况直接给this赋值。

在第三种情况中，需要注意代码注释中的两个名词——“函数表达式”和“函数声明”。虽然两者都很常用，但是这两者在“准备工作”时，却是两种待遇

 

我们总结一下，在“准备工作”中完成了哪些工作：

变量、函数表达式——变量声明，默认赋值为undefined；
this——赋值；
函数声明——赋值；

这三种数据的准备情况我们称之为“执行上下文”或者“执行上下文环境”。

函数每被调用一次，都会产生一个新的执行上下文环境。因为不同的调用可能就会有不同的参数。

另外一点不同在于，函数在定义的时候（不是调用的时候），就已经确定了函数体内部自由变量的作用域。

JavaScript 在执行一个代码段之前，都会进行这些“准备工作”来生成执行上下文。这个“代码段”其实分为三种情况（全局代码、函数体、eval 代码）。

eval不常用，也不推荐大家用。

总结一下上下文环境的数据内容。
全局代码的上下文环境数据内容为：

	
普通变量（包括函数表达式），
如： var a = 10;
	声明（默认赋值为undefined）
函数声明，
如： function fn() { }	赋值
this	赋值

如果代码段是函数体，那么在此基础上需要附加
	 
参数	赋值  
arguments	赋值
自由变量的取值作用域	赋值

Context ： 函数在被调用时的上下文环境，被调用函数中的 this 指向该上下文环境。
Scope ： 函数定义时的作用域，和变量的可见性相关。

在函数中 this 到底取何值，是在函数真正被调用执行的时候确定的，函数定义的时候确定不了。因为 this 的取值是执行上下文环境的一部分，每次调用函数，都会产生一个新的执行上下文环境。

当函数调用完成时，这个上下文环境以及其中的数据都会被消除，再重新回到全局上下文环境。处于活动状态的执行上下文环境只有一个。
有一种情况，而且是很常用的一种情况，无法做到这样干净利落的说销毁就销毁。这种情况就是伟大的——闭包。

闭包有两大特点——函数作为返回值，函数作为参数传递。

访问一个对象的属性时，先在基本属性中查找，如果没有，再沿着 __proto__ 这条链向上找，这就是原型链。
对象的原型链是沿着 __proto__ 这条线走的，因此在查找 f1.hasOwnProperty 属性时，就会顺着原型链一直查找到 Object.prototype。
由于所有的对象的原型链都会找到Object.prototype，因此所有的对象都会有Object.prototype的方法。这就是所谓的“继承”。

1.ES6 是什么
ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的下一代标准，因为当前版本的 ES6 是在 2015 年发布的，所以又称 ECMAScript 2015。

也就是说，ES6 就是 ES2015 。

虽然目前并不是所有浏览器都能兼容ES6全部特性，但越来越多的程序员在实际项目当中已经开始使用ES6了。所以就算你现在不打算使用ES6，但为了看懂别人的你也该懂点ES6的语法了。

ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。第一种场景就是你现在看到的内层变量覆盖外层变量。而 let 则实际上为JavaScript 新增了块级作用域。用它所声明的变量，只在let命令所在的代码块内有效。

另外一个var带来的不合理场景就是用来计数的循环变量泄露为全局变量

ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。
ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。

函数声明是函数的声明和实现都被提升了。
函数表达式和变量表达式只是其声明被提升了。

ES6 明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

总之，在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。
