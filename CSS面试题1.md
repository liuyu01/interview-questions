1.CSS 中类 (classes) 和 ID 的区别
书写上的差别：class名用“.”号开头来定义，id名用“#”号开头来定义；
优先级不同（权重不同）
调用上的区别：在同一个html网页页面中class是可以被多次调用的（在不同的地方）。而id名作
为标签的身份则是唯一的，id在页面中只能出现一次。在js脚本中经常会用到id来修改一个标签的
属性
id作为元素的标签，用于区分不同结构和内容，而class作为一个样式，它可以应用到任何结构和
内容上。
在布局思路上，一般坚持这样的原则：id是先确定页面的结构和内容，然后再为它定义样式：而
class相反，它先定义好一类样式，然后再页面中根据需要把类样式应用到不同的元素和内容上
面。
在实际应用时，class更多的被应用到文字版块以及页面修饰等方面，而id更多地被用来实现宏伟
布局和设计包含块，或包含框的样式。
一般原则： 类应该应用于概念上相似的元素，这些元素可以出现在同一页面上的多个位置，而ID 应该
应用于不同的唯一的元素
2.“resetting” 和 “normalizing” CSS 之间的区别？你会如何选择，为什么？
1）
重置（Resetting）： 重置意味着除去所有的浏览器默认样式。对于页面所有的元素，像
margin 、padding 、font-size 这些样式全部置成一样。你将必须重新定义各种元素的样式。
标准化（Normalizing）： 标准化没有去掉所有的默认样式，而是保留了有用的一部分，同时还
纠正了一些常见错误。
当需要实现非常个性化的网页设计时，我会选择重置的方式，因为我要写很多自定义的样式以满足设计
需求，这时候就不再需要标准化的默认样式了。
2）Normalize 相对「平和」，注重通用的方案，重置掉该重置的样式，保留有用的 user agent 样式，
同时进行一些 bug 的修复，这点是 reset 所缺乏的。 Reset 相对「暴力」，不管你有没有用，统统重置
成一样的效果，且影响的范围很大，讲求跨浏览器的一致性。
Normalize.css是一种CSS reset的替代方案。它们的区别有：
Normalize.css 保护了有价值的默认值，Reset通过为几乎所有的元素施加默认样式，强行使得元
素有相同的视觉效果。相比之下，Normalize.css保持了许多默认的浏览器样式。这就意味着你不
用再为所有公共的排版元素重新设置样式。当一个元素在不同的浏览器中有不同的默认值时，
Normalize.css会力求让这些样式保持一致并尽可能与现代标准相符合。
Normalize.css 修复了浏览器的bug，它修复了常见的桌面端和移动端浏览器的bug。这往往超出
了Reset所能做到的范畴。关于这一点，Normalize.css修复的问题包含了HTML5元素的显示设
置、预格式化文字的font-size问题、在IE9中SVG的溢出、许多出现在各浏览器和操作系统中的与
表单相关的bug。
Normalize.css 不会让你的调试工具变的杂乱
Normalize.css 是模块化的
Normalize.css 拥有详细的文档
选择Normalize.css ，主要是reset.css为几乎所有的元素施加默认样式，所以需要对所有公共的排版元
素重新设置样式，这是一件很麻烦的工作。
3.请解释浮动 (Floats) 及其工作原理
浮动（float）是 CSS 定位属性。浮动元素从网页的正常流动中移出，但是保持了部分的流动性，会影
响其他元素的定位（比如文字会围绕着浮动元素）。这一点与绝对定位不同，绝对定位的元素完全从文
档流中脱离。
CSS 的clear 属性通过使用left 、right 、both ，让该元素向下移动（清除浮动）到浮动元素下
面。
如果父元素只包含浮动元素，那么该父元素的高度将塌缩为 0。我们可以通过清除（clear）从浮动元素
后到父元素关闭前之间的浮动来修复这个问题。
float会导致父容器高度塌陷。float为什么会导致高度塌陷：元素含有浮动属性 –> 破坏inline box –> 破
坏line box高度 –> 没有高度 –> 塌陷。什么时候会塌陷：当标签里面的元素只要样子没有实际高度时会
塌陷。
浮动会脱离文档流。产生自己的块级格式化上下文。
值得一提的是，把父元素属性设置为overflow: auto 或overflow: hidden ，会使其内部的子元素形
成块格式化上下文（Block Formatting Context），并且父元素会扩张自己，使其能够包围它的子元
素。
4.描述z-index和叠加上下文是如何形成的。
首先来看在CSS中叠加上下文形成的原因：
负边距:margin为负值时元素会依参考线向外偏移。margin-left/margin-top的参考线为左边的元
素/上面的元素（如无兄弟元素则为父元素的左内侧/上内侧）,margin-right和margin-bottom的参
考线为元素本身的border右侧/border下侧。一般可以利用负边距来就行布局，但没有计算好的话
就可能造成元素重叠。堆叠顺序由元素在文档中的先后位置决定，后出现的会在上面。
position的relative/absolute/fixed定位:当为元素设置position值为relative/absolute/fixed后，元
素发生的偏移可能产生重叠，且z-index属性被激活。z-index值可以控制定位元素在垂直于显示屏
方向（Z 轴）上的堆叠顺序（stack order），值大的元素发生重叠时会在值小的元素上面。
z-index属性 ：z-index只能在position属性值为relative或absolute或fixed的元素上有效。
基本原理：z-index值可以控制定位元素在垂直于显示屏方向（Z 轴）上的堆叠顺序（stack order），
值大的元素发生重叠时会在值小的元素上面。
使用相对性：z-index值只决定同一父元素中的同级子元素的堆叠顺序。父元素的z-index值（如果有）
为子元素定义了堆叠顺序（css版堆叠“拼爹”）。向上追溯找不到含有z-index值的父元素的情况下，则
可以视为自由的z-index元素，它可以与父元素的同级兄弟定位元素或其他自由的定位元素来比较zindex
的值，决定其堆叠顺序。同级元素的z-index值如果相同，则堆叠顺序由元素在文档中的先后位置
决定，后出现的会在上面。所以如果当你发现一个z-index值较大的元素被值较小的元素遮挡了，请先
检查它们之间的dom结点关系，多半是因为其父结点含有激活并设置了z-index值的position定位元素
5.请描述 BFC(Block Formatting Context) 及其如何工作？
块格式上下文（BFC）是 Web 页面的可视化 CSS 渲染的部分，是块级盒布局发生的区域，也是浮动元
素与其他元素交互的区域。
BFC原理
1.内部的box会在垂直方向，一个接一个地放置
2.box垂直方向的距离由margin决定。属于同一个BFC的两个相邻box的margin，边距垂直方向会发生
重叠
3.BFC区域不会与浮动元素的box重叠
4.BFC在页面上是一个独立的容器，内外元素互不影响
5.每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相
反)。即使存在浮动也是如此。
6.计算BFC高度的时候，浮动元素也会参与计算
一个 HTML 盒（Box）满足以下任意一条，会创建块格式化上下文：
float 的值不是none .
position 的值不是static 或relative .可设置为absolute 或fixed
display 的值是table-cell 、table-caption 、inline-block 、flex 、或inline-flex 。
overflow 的值不是visible 。可为hidden , scroll , auto , inherit
以上四个条件都是针对父元素设置的，满足其一就可以生成BFC，即可清除浮动。
使用场景：
1. 清除浮动：BFC子元素即使是float也会参与高度计算
2. 解决垂直方向边距重叠
3. 内容增高导致侵染
6.列举不同的清除浮动的技巧，并指出它们各自适用的使用场景
空div 方法： <div style="clear:both;"></div> 。
Clearfix 方法：上文使用.clearfix 类已经提到。
父级div定义overflow：auto或者hidden
7.请解释什么是雪碧图（css sprites），以及如何实现？
雪碧图是把多张图片整合到一张上的图片。它被运用在众多使用了很多小图标的网站上（Gmail 在使
用）。实现方法：
1. 使用生成器将多张图片打包成一张雪碧图，并为其生成合适的 CSS。
2. 每张图片都有相应的 CSS 类，该类定义了background-image 、background-position 和
background-size 属性。
3. 使用图片时，将相应的类添加到你的元素中。
好处：
减少加载多张图片的 HTTP 请求数（一张雪碧图只需要一个请求）。但是对于 HTTP2 而言，加载
多张图片不再是问题。
提前加载资源，防止在需要时才在开始下载引发的问题，比如只出现在:hover 伪类中的图片，不
会出现闪烁。
8.你会如何解决特定浏览器的样式问题？
解决方案：
主张向前兼容，不考虑向后兼容，
根据产品的用户群中各大浏览器，来考虑需要兼容的浏览器
把浏览器分两类，一类历史遗留浏览器，一类是现代浏览器，然后根据这个分类开发两个版本的网
站，然后自己定义哪些浏览器是历史遗留版本，历史遗留版本浏览器，是用历史遗留界面，通过通
.clearfix::after {
content: '';
display: block;
clear: both;
}
告栏告知用户使用现代浏览器，功能更全面，提供好的用户体验
直接在用户的浏览器不能兼容的时候，提示用户至少什么版本的IE和火狐谷歌浏览器才能支持（以
上方案都失效）
项目开始前就得需要确认兼容支持的最低按本是什么，设计一个对应的兼容方案
9.有什么不同的方式可以隐藏内容（使其仅适用于屏幕阅读器）？
visibility: hidden ：元素仍然在页面流中，并占用空间。
width: 0; height: 0 ：使元素不占用屏幕上的任何空间，导致不显示它。
position: absolute; left: -99999px ： 将它置于屏幕之外。
text-indent: -9999px ：这只适用于block 元素中的文本。
Metadata： 例如通过使用 Schema.org，RDF 和 JSON-LD。
WAI-ARIA：如何增加网页可访问性的 W3C 技术规范。
即使 WAI-ARIA 是理想的解决方案，我也会采用绝对定位方法，因为它具有最少的注意事项，适用于大
多数元素，而且使用起来非常简单。
10.你用过栅格系统 (grid system) 吗？如果使用过，你最喜欢哪种？
Bootstrap提供了两种布局方式，固定式布局和流式布局（用em表示的叫做弹性布局，用百分比表示的
叫做流体布局）方式，Bootstrap的布局实际上是在栅格外加个容器 (Container)
因此两种布局方式的唯一区别是：
固定布局加的是固定宽度(width)的容器，
流式布局加的是自适应(或叫可变)宽度的容器。
11.你用过媒体查询，或针对移动端的布局/CSS 吗？
媒体查询规则是开发者能够在相同的样式中，针对不同的媒介来使用不同的样式规则。在CSS2的时候有
media Type的规则，通过不同的媒介来切换不同的CSS样式。通过媒体查询的技术可以实现响应式布
局，适应不同终端的开发。
12.你熟悉制作 SVG 吗？
是的，你可以使用内联CSS、嵌入式CSS部分或外部CSS文件对形状进行着色（包括指定对象上的属
性）。在网上大部分SVG使用的是内联CSS，不过每个类型都有优点和缺点。
通过设置fill 和stroke 属性，可以完成基本着色操作。fill 可以设置内部的颜色， stroke 可以设
置周围绘制的线条的颜色。你可以使用与HTML 中使用的CSS颜色命名方案相同的CSS颜色命名方案：颜
色名称（即red ）、RGB值（即rgb(255,0,0) ）、十六进制值、RGBA值等等。
13.除了screen ，你还能说出一个 @media 属性的例子吗？
all 用于所有设备 print 用于打印机和打印预览 screen用于电脑屏幕，平板电脑，智能手机等 speech 应
用于屏幕阅读器等发声设备
14.编写高效的 CSS 应该注意什么？
首先，浏览器从最右边的选择器，即关键选择器（key selector），向左依次匹配。根据关键选择器，
浏览器从 DOM 中筛选出元素，然后向上遍历被选元素的父元素，判断是否匹配。选择器匹配语句链越
短，浏览器的匹配速度越快。避免使用标签和通用选择器作为关键选择器，因为它们会匹配大量的元
素，浏览器必须要进行大量的工作，去判断这些元素的父元素们是否匹配。
BEM (Block Element Modifier)原则上建议为独立的 CSS 类命名，并且在需要层级关系时，将关系也体
现在命名中，这自然会使选择器高效且易于覆盖。
<rect x="10" y="10" width="100" height="100" stroke="blue"
fill="purple" fill-opacity="0.5" stroke-opacity="0.8"/>
搞清楚哪些 CSS 属性会触发重新布局（reflow）、重绘（repaint）和合成（compositing）。在写样式
时，避免触发重新布局的可能。
15.使用 CSS 预处理的优缺点分别是什么？
优点：
提高 CSS 可维护性。
易于编写嵌套选择器。
引入变量，增添主题功能。可以在不同的项目中共享主题文件。
通过混合（Mixins）生成重复的 CSS。
将代码分割成多个文件。不进行预处理的 CSS，虽然也可以分割成多个文件，但需要建立多个
HTTP 请求加载这些文件。
缺点：
需要预处理工具。
重新编译的时间可能会很慢。
16.如何实现一个使用非标准字体的网页设计？
使用@font-face 并为不同的font-weight 定义font-family 。
Webfonts (字体服务例如：Google Webfonts，Typekit 等等。)
17.请解释浏览器是如何判断元素是否匹配某个 CSS 选择器？
浏览器先产生一个元素集合，这个集合往往由最后一个部分的索引产生（如果没有索引就是所有元素的
集合）。然后向上匹配，如果不符合上一个部分，就把元素从集合中删除，直到真个选择器都匹配完，
还在集合中的元素就匹配这个选择器了。
18.描述伪元素及其用途
CSS 伪元素是添加到选择器的关键字，去选择元素的特定部分。它们可以用于装饰（ :firstline
， :first-letter ）或将元素添加到标记中（与 content:...组合），而不必修改标记
（ :before ， :after ）。
:first-line 和:first-letter 可以用来修饰文字。
上面提到的.clearfix 方法中，使用clear: both 来添加不占空间的元素。
使用:before 和after 展示提示中的三角箭头。鼓励关注点分离，因为三角被视为样式的一部
分，而不是真正的 DOM。如果不使用额外的 HTML 元素，只用 CSS 样式绘制三角形是不太可能
的。
19. display 的属性值都有哪些？
none , block , inline , inline-block , table , table-row , table-cell , list-item .
20. inline 和inline-block 有什么区别？
block inline-block inline
大小
填充其父容器
的宽度。
取决于
内容。
取决于内容。
定位
从新的一行开
始，并且不允
许旁边有
HTML 元素
（除非是
float ）
与其他
内容一
起流
动，并
允许旁
边有其
他元
素。
与其他内容一起流动，并允许旁边有
其他元素。
能否设置width 和
height
能能不能。 设置会被忽略。
可以使用verticalalign
对齐
不可以可以可以
边距（margin）和填
充（padding）
各个方向都存
在
各个方
向都存
在
只有水平方向存在。垂直方向会被忽
略。 尽管border 和padding 在
content 周围，但垂直方向上的空间
取决于'line-height'
浮动（float） - -
就像一个block 元素，可以设置垂直
边距和填充。
21.请解释 relative、fixed、absolute 和 static 元素的区别?
各个属性的值：
static：默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index
声明）。
relative：生成相对定位的元素，通过top,bottom,left,right的设置相对于其正常位置进行定位。可
通过z-index进行层次分级。
absolute：生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。元素的位置
通过 “left”, “top”, “right” 以及 “bottom” 属性进行规定。可通过z-index进行层次分级。
fixed：生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 “left”, “top”, “right”
以及 “bottom” 属性进行规定。可通过z-index进行层次分级。
relative和absolute进行对比分析：
relative。定位为relative的元素脱离正常的文本流中，但其在文本流中的位置依然存在。
absolute。定位为absolute的层脱离正常文本流，但与relative的区别是其在正常流中的位置不在
存在。
fixed:定位为绝对定位，脱离正常文本流，相对于浏览器窗口进行定位
relative和absolute与fixed进行对比分析：
relative定位的层总是相对于其最近的父元素，无论其父元素是何种定位方式。
absolute定位的层总是相对于其最近的定义为absolute或relative的父层，而这个父层并不一定是
其直接父层。如果其父层中都未定义absolute或relative，则其将相对body进行定位，
fixed：生成绝对定位的元素，相对于浏览器窗口进行定位。
22.什么是语义化的html标签？
语义化的HTML就是写出的HTML代码，符合内容的结构化（内容语义化），选择合适的标签（代码语
义化），能够便于开发者阅读和写出更优雅的代码的同时让浏览器的爬虫和机器很好地解析。
语义化有利于SEO，有利于搜索引擎爬虫更好的理解我们的网页，从而获取更多的有效信息，提升
网页的权重。
在没有CSS的时候能够清晰的看出网页的结构，增强可读性。
便于团队开发和维护，语义化的HTML可以让开发者更容易的看明白，从而提高团队的效率和协调
能力。
支持多终端设备的浏览器渲染。
23.CSS 有哪些选择器？权重计算及优先级？
id选择器、类选择器、元素选择器、全局选择器、组合选择器、继承选择器、伪类选择器
权重计算: 第一等级：代表内联样式，如style=""，权值为 1000 第二等级：代表id选择器，如
#content，权值为100 第三等级：代表类，伪类和属性选择器，如.content，权值为10 第四等级：代
表标签选择器和伪元素选择器，如div p，权值为1 Css 语句权重由选择器的权值相加可得。 样式优先
级:！important>行内样式>内部样式>外部样式
!important声明的样式优先级最高，如果冲突再进行计算。
如果优先级相同，则选择最后出现的样式。
继承得到的样式的优先级最低。
24.CSS 引入方式有哪些？link和 @important的区别？
CSS的引入方式共有三种：
行内样式（使用style属性引入CSS样式）
内部样式表（在style标签中书写CSS代码。style标签写在head标签中）
外部样式表（链接式、导入式）
链接：
导入：
链接和导入的区别： ：
@import：
区别1：link是XHTML标签，除了加载CSS外，还可以定义RSS等其他事务；@import属于CSS范畴，只
能加载CSS。
区别2：link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。
区别3：link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器不支持。
区别4：link支持使用Javascript控制DOM去改变样式；而@import不支持。
区别5：Link引入样式的权重大于@import的引用（@import是将引用的样式导入到当前的页面中）(在
link 标签引入的 CSS 文件中，使用@import 时需注意，如果已经存在相同样式， @import 引入的这
个样式将被该 CSS 文件本身的样式层叠掉，表现出link 标签引入的样式权重大于@import 引入的样式
这样的直观效果。)
<link type="text/css" rel="styleSheet" href="CSS文件路径" />
<style type="text/css">
@import url("css文件路径");
</style>
25.响应式设计与自适应设计有何不同？
响应式设计和自适应设计都以提高不同设备间的用户体验为目标，根据视窗大小、分辨率、使用环境和
控制方式等参数进行优化调整。
响应式设计的适应性原则：网站应该凭借一份代码，在各种设备上都有良好的显示和使用效果。响应式
网站通过使用媒体查询，自适应栅格和响应式图片，基于多种因素进行变化，创造出优良的用户体验。
就像一个球通过膨胀和收缩，来适应不同大小的篮圈。
自适应设计更像是渐进式增强的现代解释。与响应式设计单一地去适配不同，自适应设计通过检测设备
和其他特征，从早已定义好的一系列视窗大小和其他特性中，选出最恰当的功能和布局。与使用一个球
去穿过各种的篮筐不同，自适应设计允许使用多个球，然后根据不同的篮筐大小，去选择最合适的一
个。
26.你有没有使用过视网膜分辨率的图形？当中使用什么技术？
我倾向于使用更高分辨率的图形（显示尺寸的两倍）来处理视网膜显示。更好的方法是使用媒体查询，
像@media only screen and (min-device-pixel-ratio: 2) { ... } ，然后改变backgroundimage
。
对于图标类的图形，我会尽可能使用 svg 和图标字体，因为它们在任何分辨率下，都能被渲染得十分清
晰。
还有一种方法是，在检查了window.devicePixelRatio 的值后，利用 JavaScript 将<img> 的src 属性
修改，用更高分辨率的版本进行替换。
27.什么情况下，用translate() 而不用绝对定位？什么时候，情况相反。
translate() 是transform 的一个值。改变transform 或opacity 不会触发浏览器重新布局
（reflow）或重绘（repaint），只会触发复合（compositions）。而改变绝对定位会触发重新布局，
进而触发重绘和复合。transform 使浏览器为元素创建一个 GPU 图层，但改变绝对定位会使用到
CPU。 因此translate() 更高效，可以缩短平滑动画的绘制时间。
当使用translate() 时，元素仍然占据其原始空间（有点像position：relative ），这与改变绝对
定位不同。
28.CSS选择器有哪些？哪些属性可以继承？
CSS选择符：id选择器(#myid)、类选择器(.myclassname)、标签选择器(div, h1, p)、相邻选择器(h1 +
p)、子选择器（ul > li）、后代选择器（li a）、通配符选择器（*）、属性选择器（a[rel=”
external”]）、伪类选择器（a:hover, li:nth-child）
可继承的属性：font-size, font-family, color
不可继承的样式：border, padding, margin, width, height
优先级（就近原则）：!important > [ id > class > tag ] !important 比内联优先级高
29.CSS3有哪些新特性？
RGBA和透明度
background-image background-origin(content-box/padding-box/border-box) backgroundsize
background-repeat
word-wrap（对长的不可分割单词换行）word-wrap：break-word
文字阴影：text-shadow： 5px 5px 5px #FF0000;（水平阴影，垂直阴影，模糊距离，阴影颜
色）
font-face属性：定义自己的字体
圆角（边框半径）：border-radius 属性用于创建圆角
边框图片：border-image: url(border.png) 30 30 round
盒阴影：box-shadow: 10px 10px 5px #888888
媒体查询：定义两套css，当浏览器的尺寸变化时会采用不同的属性
30.请解释一下CSS3的flexbox（弹性盒布局模型）,以及适用场景？
CSS3 弹性盒（ Flexible Box 或 flexbox），是一种当页面需要适应不同的屏幕大小以及设备类型时确
保元素拥有恰当的行为的布局方式。
引入弹性盒布局模型的目的是提供一种更加有效的方式来对一个容器中的子元素进行排列、对齐和分配
空白空间。
适用场景：弹性布局适合于移动前端开发，在Android和ios上也完美支持。
31.常见的兼容性问题？
不同浏览器的标签默认的margin和padding不一样。*{margin:0;padding:0;}
IE6双边距bug：块属性标签float后，又有横行的margin情况下，在IE6显示margin比设置的大。
hack：display:inline;将其转化为行内属性。
设置较小高度标签（一般小于10px），在IE6，IE7中高度超出自己设置高度。hack：给超出高度
的标签设置overflow:hidden;或者设置行高line-height 小于你设置的高度。
IE下，可以使用获取常规属性的方法来获取自定义属性,也可以使用getAttribute()获取自定义属
性；Firefox下，只能使用getAttribute()获取自定义属性。解决方法:统一通过getAttribute()获取自
定义属性。
Chrome 中文界面下默认会将小于 12px 的文本强制按照 12px 显示,可通过加入 CSS 属性 -
webkit-text-size-adjust: none; 解决。
超链接访问过后hover样式就不出现了，被点击访问过的超链接样式不再具有hover和active了。
解决方法是改变CSS属性的排列顺序:L-V-H-A ( love hate ): a:link {} a:visited {} a:hover {} a:active
{}
32.为什么要初始化CSS样式？
因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏
览器之间的页面显示差异。
33.CSS里的visibility属性有个collapse属性值？在不同浏览器下以后什么区别？
当一个元素的visibility属性被设置成collapse值后，对于一般的元素，它的表现跟hidden是一样的。
chrome中，使用collapse值和使用hidden没有区别。
firefox，opera和IE，使用collapse值和使用display：none没有什么区别。
34.display:none与visibility：hidden的区别？
display：none 不显示对应的元素，在文档布局中不再分配空间（回流+重绘）
visibility：hidden 隐藏对应元素，在文档布局中仍保留原来的空间（重绘）
35.position跟display、overflow、float这些特性相互叠加后会怎么样？
display属性规定元素应该生成的框的类型；position属性规定元素的定位类型；float属性是一种布局方
式，定义元素在哪个方向浮动。
类似于优先级机制：position：absolute/fixed优先级最高，有他们在时，float不起作用，display值需
要调整。float 或者absolute定位的元素，只能是块元素或表格。
36.为什么会出现浮动和什么时候需要清除浮动？清除浮动的方式？
浮动元素碰到包含它的边框或者浮动元素的边框停留。由于浮动元素不在文档流中，所以文档流的块框
表现得就像浮动框不存在一样。浮动元素会漂浮在文档流的块框上。 浮动带来的问题：
父元素的高度无法被撑开，影响与父元素同级的元素
与浮动元素同级的非浮动元素（内联元素）会跟随其后
若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构。
清除浮动的方式：
父级div定义height
最后一个浮动元素后加空div标签 并添加样式clear:both。
包含浮动元素的父标签添加样式overflow为hidden或auto。
父级div定义zoom
37.设置元素浮动后，该元素的display值是多少？
自动变成display:block
38.移动端的布局用过媒体查询吗？
通过媒体查询可以为不同大小和尺寸的媒体定义不同的css，适应相应的设备的显示。
里边
CSS : @media only screen and (max-device-width:480px) {/css样式/}
39.CSS优化、提高性能的方法有哪些？
避免过度约束
避免后代选择符
避免链式选择符
使用紧凑的语法
避免不必要的命名空间
避免不必要的重复
最好使用表示语义的名字。一个好的类名应该是描述他是什么而不是像什么
避免！important，可以选择其他选择器
尽可能的精简规则，你可以合并不同类里的重复规则
40.浏览器是怎样解析CSS选择器的？
CSS选择器的解析是从右向左解析的。若从左向右的匹配，发现不符合规则，需要进行回溯，会损失很
多性能。若从右向左匹配，先找到所有的最右节点，对于每一个节点，向上寻找其父节点直到找到根元
素或满足条件的匹配规则，则结束这个分支的遍历。两种匹配规则的性能差别很大，是因为从右向左的
匹配在第一步就筛选掉了大量的不符合条件的最右节点（叶子节点），而从左向右的匹配规则的性能都
浪费在了失败的查找上面。
而在 CSS 解析完毕后，需要将解析的结果与 DOM Tree 的内容一起进行分析建立一棵 Render Tree，
最终用来进行绘图。在建立 Render Tree 时（WebKit 中的「Attachment」过程），浏览器就要为每个
DOM Tree 中的元素根据 CSS 的解析结果（Style Rules）来确定生成怎样的 Render Tree。
41.在网页中的应该使用奇数还是偶数的字体？为什么呢？
使用偶数字体。偶数字号相对更容易和 web 设计的其他部分构成比例关系。Windows 自带的点阵宋体
（中易宋体）从 Vista 开始只提供 12、14、16 px 这三个大小的点阵，而 13、15、17 px时用的是小一
号的点。（即每个字占的空间大了 1 px，但点阵没变），于是略显稀疏。
42.margin和padding分别适合什么场景使用？
何时使用margin：
需要在border外侧添加空白
空白处不需要背景色
上下相连的两个盒子之间的空白，需要相互抵消时。
何时使用padding：
需要在border内侧添加空白
空白处需要背景颜色
上下相连的两个盒子的空白，希望为两者之和。
兼容性的问题：在IE5 IE6中，为float的盒子指定margin时，左侧的margin可能会变成两倍的宽度。通
过改变padding或者指定盒子的display：inline解决。
43.元素竖向的百分比设定是相对于容器的高度吗？
当按百分比设定一个元素的宽度时，它是相对于父容器的宽度计算的，但是，对于一些表示竖向距离的
属性，例如 padding-top , padding-bottom , margin-top , margin-bottom 等，当按百分比设定它们
时，依据的也是父容器的宽度，而不是高度。
44.全屏滚动的原理是什么？用到了CSS的哪些属性？
原理：有点类似于轮播，整体的元素一直排列下去，假设有5个需要展示的全屏页面，那么高度是
500%，只是展示100%，剩下的可以通过transform进行y轴定位，也可以通过margin-top实现
45.什么是响应式设计？响应式设计的基本原理是什么？如何兼容低版本的IE？
响应式网站设计(Responsive Web design)是一个网站能够兼容多个终端，而不是为每一个终端做一个
特定的版本。 基本原理是通过媒体查询检测不同的设备屏幕尺寸做处理。 页面头部必须有meta声明的
viewport。
46.视差滚动效果？
视差滚动（Parallax Scrolling）通过在网页向下滚动的时候，控制背景的移动速度比前景的移动速度慢
来创建出令人惊叹的3D效果。
CSS3实现: 优点：开发时间短、性能和开发效率比较好，缺点是不能兼容到低版本的浏览器
jQuery实现: 通过控制不同层滚动速度，计算每一层的时间，控制滚动效果。 优点：能兼容到各个
版本的，效果可控性好 缺点：开发起来对制作者要求高
插件实现方式: 例如：parallax-scrolling，兼容性十分好
47.::before 和 :after中双冒号和单冒号有什么区别？解释一下这2个伪元素的作用
单冒号(:)用于CSS3伪类，双冒号(::)用于CSS3伪元素。
::before就是以一个子元素的存在，定义在元素主体内容之前的一个伪元素。并不存在于dom之
中，只存在在页面之中。
:before 和 :after 这两个伪元素，是在CSS2.1里新出现的。起初，伪元素的前缀使用的是单冒号语法，
但随着Web的进化，在CSS3的规范里，伪元素的语法被修改成使用双冒号，成为::before ::after
css引入伪类和伪元素概念是为了格式化文档树以外的信息。也就是说，伪类和伪元素是用来修饰不在
文档树中的部分，比如，一句话中的第一个字母，或者是列表中的第一个元素。下面分别对伪类和伪元
素进行解释。
伪类用于当已有元素处于的某个状态时，为其添加对应的样式，这个状态是根据用户行为而动态变化
的。比如说，当用户悬停在指定的元素时，我们可以通过:hover来描述这个元素的状态。虽然它和普通
的css类相似，可以为已有的元素添加样式，但是它只有处于dom树无法描述的状态下才能为元素添加
样式，所以将其称为伪类。
overflow：hidden；transition：all 1000ms ease；
<meta name="’viewport’" content="”width=device-width," initial-scale="1."
maximum-scale="1,user-scalable=no”"/>
伪元素用于创建一些不在文档树中的元素，并为其添加样式。比如说，我们可以通过:before来在一个
元素前增加一些文本，并为这些文本添加样式。虽然用户可以看到这些文本，但是这些文本实际上不在
文档树中。
48.你对line-height是如何理解的？
行高是指一行文字的高度，具体说是两行文字间基线的距离。CSS中起高度作用的是height和lineheight，
没有定义height属性，最终其表现作用一定是line-height。 单行文本垂直居中：把line-height
值设置为height一样大小的值可以实现单行文字的垂直居中，其实也可以把height删除。
多行文本垂直居中：需要设置display属性为inline-block。
49.怎么让Chrome支持小于12px 的文字？
50.如果需要手动写动画，你认为最小时间间隔是多久，为什么？
多数显示器默认频率是60Hz，即1秒刷新60次，所以理论上最小间隔为1/60＊1000ms ＝ 16.7ms
51.li与li之间有看不见的空白间隔是什么原因引起的？有什么解决办法？
行框的排列会受到中间空白（回车空格）等的影响，因为空格也属于字符,这些空白也会被应用样式，占
据空间，所以会有间隔，把字符大小设为0，就没有空格了。 解决方法：
可以将
代码全部写在一排
浮动li中float：left
在ul中用font-size：0（谷歌不支持）；可以使用letter-space：-3px
52.display:inline-block 什么时候会显示间隙？
有空格时候会有间隙 解决：移除空格
margin正值的时候 解决：margin使用负值
使用font-size时候 解决：font-size:0、letter-spacing、word-spacing
53.style标签写在body后与body前有什么区别？
页面加载自上而下 当然是先加载样式。
写在body标签后由于浏览器以逐行方式对HTML文档进行解析，当解析到写在尾部的样式表（外联或写
在style标签）会导致浏览器停止之前的渲染，等待加载且解析样式表完成之后重新渲染，在windows的
IE下可能会出现FOUC现象（即样式失效导致的页面闪烁问题）
54.png、jpg、gif 这些图片格式解释一下，分别什么时候用。有没有了解过webp？
png是便携式网络图片（Portable Network Graphics）是一种无损数据压缩位图文件格式.优点
是：压缩比高，色彩好。 大多数地方都可以用。
jpg是一种针对相片使用的一种失真压缩方法，是一种破坏性的压缩，在色调及颜色平滑变化做的
不错。在www上，被用来储存和传输照片的格式。
gif是一种位图文件格式，以8位色重现真色彩的图像。可以实现动画效果.
webp格式是谷歌在2010年推出的图片格式，压缩率只有jpg的2/3，大小比png小了45%。缺点是
压缩的时间更久了，兼容性不好，目前谷歌和opera支持。
55.CSS盒模型
伪类的操作对象是文档树中已有的元素，而伪元素则创建了一个文档数外的元素。因此，伪类与伪元素的区别
在于：有没有创建一个文档树之外的元素。
p{font-size:10px;-webkit-transform:scale(0.8);} //0.8是缩放比例
CSS盒模型分成标准盒模型和IE模型
标准模型和IE模型的区别：height 和 width 计算方式不同
标准模型：只包括 content
IE模型： content + padding + border
标准模型(默认)： box-sizing: content-box
IE模型： box-sizing: border-box
IE模型下，浏览器的 width 属性不是内容的宽度，而是内容、内边距和边框的宽度的总和；
即在怪异模式下的盒模型，盒子的（content）宽度+内边距padding+边框border宽度=我们设置的
width(height也是如此)，
盒子总宽度/高度=width/height + margin = 内容区宽度/高度 + padding + border + margin。
在标准模式下的盒模型，盒子实际内容（content）的width/height=我们设置的width/height;
盒子总宽度/高度=width/height+padding+border+margin。
56.纵向 margin 重叠
父子元素重叠：添加overflow:hidden 相当于给父元素添加了一个BFC
兄弟元素重叠：取两个属性中较大的值
空元素设置上下边距：把margin-top 和margin-bottom 取最大值设为边距
