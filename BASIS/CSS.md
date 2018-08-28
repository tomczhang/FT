# 1. 盒子模型

> 盒子模型的定义

盒子模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框和实际内容。

> 标准盒子模型和怪异盒子模型的区别
* 标准盒子模型：盒子占位width = content + 2margin + 2padding + 2border
* 怪异盒子模型：盒子占位width = content + 2margin。content部分包含了border和pading。

通过border-sizing来控制盒子模型的种类：content-box/border-box

# 2. 弹性盒子模型
为了解决float不好用的问题，自动调整盒子的大小，创造了弹性盒子模型。

> 弹性盒子相关属性
* flex-direction：row/column; //方向
* flex-wrap：no-wrap/warp; //换行
* justify-content:space-between/flex-start/space-around; //调整空间的方式
* align-items:center/flex-start //垂直方向上如何防止元素
* flex-grow: 剩余空间扩展比例，0不扩展，同为1等分，2会占等分的两份
* flex-shrink: 空间不够时的收缩比例，0不收缩，同为1收缩同样的比例。
* flex:2 1 缩写代表flex-grow:2;flex-shrink:1

# 3. BFC

> BFC的定义

BFC: 块级格式上下文。BFC就是一个页面上独立的容器，规定了内部的块级元素如何布局，并且这些块级元素不会影响外部元素。BFC有以下约束规则：
* 内部的BOX会在垂直方向上一个接一个的放置
* 同一个BFC的两个相邻BOX的margin会发生重叠
* 每个元素的左外边距和包含块的左边界相接触
* BFC的区域不会和float的区域重叠
* 计算BFC高度时，浮动子元素也参与计算

> BFC的作用
* 自适应两栏布局
* 清除浮动
* 防止垂直margin重叠

> BFC的生成条件
* 根元素
* overflow不为visible
* float不为none
* position属性为absolute/fixed
* display为inline-block, table-cell, flex

> 清除浮动的方法
* 父元素BFC overflow:hidden
* 新增底部兄弟元素 clear:both 块级元素的左右都不能有浮动元素
* 添加自身伪元素 伪元素是元素的子元素，在元素的内容之前或者之后

```
.clearfix::after {
content: '';
display: block;
clear: both;
}
```

# 4. 伪类与伪元素
* 伪类：伪类用于当已有元素处于的某个状态时，为其添加对应的样式，这个状态是根据用户行为而动态变化的。
* 伪元素：伪元素用于创建一些不在文档树中的元素，并为其添加样式。

# 5. position的区别
* static: 默认值，元素正常出现在文档流中
* relative: 相对定位，对于元素正常位置进行定位，偏移后原本的位置仍占据空间，不会影响其他元素的位置，未脱离文档流。父节点设置为relative之后，子节点可以相对父节点绝对定位
* absolute: 绝对丁文，脱离文档流。生成绝对定位的元素，相对于最近一级的定位不是static的父元素来进行定位。
* fixed: 固定定位，脱离文档流。生成绝对定位的元素，只不过是相对于视口的。
* sticky: 粘性定位，相对定位与固定定位的结合。必须指定top/left/right/bottom四个阙值之一才能生效，兼容性不好。

# 6. 水平居中与垂直居中
> 水平居中方法
* text-align:center;
* margin: 0 auto;
* 有固定宽度，position:relative;left:50%;margin-left: -1/2宽度;
* CSS3，position:relative;left:50%;transform:translateX(-50%);
* display:flex;justify-content:space-around;
> 垂直居中方法
* vertical-align:middle;
* line-height和height同高
* display:flex;align-items:center;
* CSS3，position:relative;top:50%;transform:translateY(-50%);
* 有固定高度，postion:absolute/fixed;top:50%;margin-top: -1/2高度;
* display:table-cell;vertical-align:middle;
* parent:after {content:'';display:inlineblock;verticalalign:middle;height:100%;width:0}
son {display:inline-block;vertical-align:middle}

# 7. 移动适配
设置理想视口：
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">

> 移动设配的方法
* rem: fontSize = window.innerWidth / 7.5 + 'px'
* flexible: 为了解决小于1px的展示问题。将视口分成了10rem。不兼容响应式布局。
* vw: flexible是vw的hack写法。Viewport Units Buggyfill用来做android兼容。

# 8. 实现一个三角形/菱形
> 实现一个三角形
* 利用border实现
```
.triangle{
width: 0;
height: 0;
border-left: 50px solid transparent;
border-right: 50px solid transparent;
border-bottom: 100px solid red;
}
```
* 利用transform rotate: transform: rotate(45deg);

> 实现一个菱形
transform skew

# 6. CSS3动画
div {
  animation: funcName 5s;
}
@keyframes funcName {
  0% {css}
  100% {css}
}
# 7. 重绘/重排
浏览器下载完页面的内容后，会构建DOM树和渲染树，DOM树表示页面结构，渲染树表示节点如何显示。当DOM的改变影响了节点的几何属性，这时候浏览器就会计算重新计算节点和其他节点的几何属性和位置。浏览器会使渲染树受到影响的部分失效，并重新构建渲染树，这叫做重排。完成重排后，浏览器会重新绘制受到影响的部分到页面中，也叫做重绘。
* 引起重排的原因
  * 添加/删除可见的DOM元素
  * 元素的位置改变
  * 元素的尺寸改变
  * 元素的内容改变
  * 页面渲染初始化
  * 浏览器窗口改变
# 8. CSS3特性
* border-radius
* border-shadow
* CSS3 2D转换，translate, rotate
* CSS3 3D转换
* CSS3 过渡，transition
* CSS3 动画