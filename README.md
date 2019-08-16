# Front-end-note
前端学习笔记
## 目录
* [HTML](#HTML)
    * [meta标签][meta标签]
* [CSS](#CSS)
    * [盒子水平、垂直居中](#盒子水平、垂直居中)
    * [弹性盒子](#弹性盒子)
    * [消除transition闪屏](#消除transition闪屏)
* [JavaScript](#JS)
    * [let&const&var](#let&const&var)
    * [箭头函数](#箭头函数)
    * [Cookie&Session][Cookie&Session]
    * [浏览器同源策略&跨域][同源策略&跨域]
    * [事件的三个阶段](#事件的三个阶段)
    * [数组乱序](#数组乱序)
    * [判断设备来源](#判断设备来源)
* [算法](#算法)
    * [常见排序算法][常见排序算法]

***
# HTML
# CSS
## 消除transition闪屏
```css
.css {
    -webkit-transform-style: preserve-3d;
    -webkit-backface-visibility: hidden;
    -webkit-perspective: 1000;
}
```
过渡动画（在没有启动硬件加速的情况下）会出现抖动的现象， 以上的解决方案只是改变视角来启动硬件加速的一种方式；启动硬件加速的另外一种方式：
```css
.css {
    -webkit-transform: translate3d(0,0,0);
    -moz-transform: translate3d(0,0,0);
    -ms-transform: translate3d(0,0,0);
    transform: translate3d(0,0,0);
}
```
##### 启动硬件加速
最常用的方式：translate3d、translateZ、transform<br>
opacity 属性/过渡动画（需要动画执行的过程中才会创建合成层，动画没有开始或结束后元素还会回到之前的状态）<br>
will-chang 属性（这个比较偏僻），一般配合opacity与translate使用（而且经测试，除了上述可以引发硬件加速的属性外，其它属性并不会变成复合层）。
>弊端：硬件加速会导致 CPU 性能占用量过大，电池电量消耗加大 ；因此尽量避免泛滥使用硬件加速。

## 盒子水平、垂直居中
1. 绝对定位  适用于盒子宽高已知， 
```css
position: absolute; 
left: 50%; 
top: 50%; 
margin-left:-自身一半宽度; 
margin-top: -自身一半高度;
```
2. table-cell布局 
```css
/* 父级  */
display: table-cell; 
vertical-align: middle; 
/* 子级  */
margin: 0 auto;
```
3. 绝对定位 + transform ; 适用于盒子宽高不定； 
```css
position: relative / absolute;
/*top和left偏移各为50%*/
top: 50%;
left: 50%;
/*translate(-50%,-50%) 偏移自身的宽和高的-50%*/
transform: translate(-50%, -50%); 
```
4. flex 弹性盒子
```css
/* 1.弹性父元素  */
display: flex;
/*实现垂直居中*/
align-items: center;
/*实现水平居中*/
justify-content: center;
```
```css
/* 2.弹性子元素两轴方向对齐 */
margin：auto；
```

## 弹性盒子
    * 弹性盒子是 CSS3 的一种新的布局模式。
    * CSS3 弹性盒（ Flexible Box 或 flexbox），是一种当页面需要适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式。
    * 引入弹性盒布局模型的目的是提供一种更加有效的方式来对一个容器中的子元素进行排列、对齐和分配空白空间。
    * 弹性盒子由弹性容器(Flex container)和弹性子元素(Flex item)组成。
    * 弹性容器通过设置 display 属性的值为 flex 或 inline-flex将其定义为弹性容器。
    * 弹性容器内包含了一个或多个弹性子元素。
### 弹性容器属性
1. flex-direction
flex-direction属性指定了弹性子元素在弹性容器中的位置。
    * __row__: 横向从左到右排列（左对齐），默认的排列方式。
    * __row-reverse__: 反转横向排列（右对齐），从后往前排，最后一项排在最前面。
    * __column__: 纵向排列。
    * __column-reverse__: 反转纵向排列，从后往前排，最后一项排在最上面。
2. justify-content
justify-content属性指定弹性子元素沿着弹性容器主轴线(水平)方向的对齐方式。
    * __flex-start__: 默认值。弹性项目位于容器开头。
    * __flex-end__: 弹性项目位于容器结尾。
    * __center__: 弹性项目居中紧挨着填充。（如果剩余的自由空间是负的，则弹性项目将在两个方向上同时溢出）。
    * __space-between__: 弹性项目平均分布在该行上。如果剩余空间为负或者只有一个弹性项，则该值等同于flex-start。否则，第1个弹性项的外边距和行的main-start边线对齐，而最后1个弹性项的外边距和行的main-end边线对齐，然后剩余的弹性项分布在该行上，相邻项目的间隔相等。
    * __space-around__: 弹性项目平均分布在该行上，两边留有一半的间隔空间。如果剩余空间为负或者只有一个弹性项，则该值等同于center。否则，弹性项目沿该行分布，且彼此间隔相等（比如是20px），同时首尾两边和弹性容器之间留有一半的间隔（1/2*20px=10px）。
3. align-items
align-items 设置或检索弹性盒子元素在侧轴（纵轴）方向上的对齐方式。
    * __flex-start__: 弹性项目位于容器开头。
    * __flex-end__: 弹性项目位于容器结尾。
    * __center__: 弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
    * __baseline__: 项目位于容器的基线上。如弹性盒子元素的行内轴与侧轴为同一条，则该值与`flex-start`等效。其它情况下，该值将参与基线对齐。
    * __stretch__: 默认值。如果指定侧轴大小的属性值为`auto`，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。
4. flex-wrap
用于指定弹性子元素的换行方式
    * __nowrap__: 默认， 弹性容器为单行。该情况下弹性子项可能会溢出容器。
    * __wrap__: 弹性容器为多行。该情况下弹性子项溢出的部分会被放置到新行，子项内部会发生断行
    * __wrap-reverse__: 反转 wrap 排列。
5. align-content
用于修改 flex-wrap 属性的行为。可以理解为垂直方向上的justify-content，但它不是设置弹性子元素的对齐，而是设置各个行的对齐。
    * __stretch__: 默认。各行将会伸展以占用剩余的空间。
    * __flex-start__: 各行向弹性盒容器的起始位置堆叠。
    * __flex-end__: 各行向弹性盒容器的结束位置堆叠。
    * __center__: 各行向弹性盒容器的中间位置堆叠。
    * __space-between__: 各行在弹性盒容器中平均分布。
    * __space-around__: 各行在弹性盒容器中平均分布，两端保留子元素与子元素之间间距大小的一半。
### 弹性子元素属性
1. order
用整数值来定义排列顺序，数值小的排在前面。可以为负值。
2. margin
设置"margin"值为"auto"值，自动获取弹性容器中剩余的空间。所以设置margin值为"auto"，可以使弹性子元素在弹性容器的两轴方向都完全居中
3. align-self
用于设置`弹性元素自身`在侧轴（纵轴）方向上的对齐方式。
    * __auto__：默认值。元素继承了它的父容器的 align-items 属性。如果没有父容器则为 "stretch"。
    * __stretch__：元素被拉伸以适应容器。
    * __flex-start__：元素位于容器的开头。
    * __flex-end__：元素位于容器的结尾。
    * __center__：弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
    * __baseline__：如弹性盒子元素的行内轴与侧轴为同一条，则该值与'flex-start'等效。其它情况下，该值将参与基线对齐。
4. flex-grow
一个数字，规定项目将相对于其他灵活的项目进行扩展的量。默认值是 0。
5. flex-shrink
一个数字，规定项目将相对于其他灵活的项目进行收缩的量。默认值是 1。
6. flex-basis
一个长度单位或者一个百分比，规定元素的初始长度。
auto：默认值。长度等于元素的长度。如果该项目未指定长度，则长度将根据内容决定。
7. flex
用于设置或检索弹性盒模型对象的子元素如何分配空间。<br>
>语法：
`flex: flex-grow flex-shrink flex-basis|auto|initial|inherit;`

##### flex 属性是 flex-grow、flex-shrink 和 flex-basis 属性的简写属性。
    * __flex-grow__: 一个数字，规定项目将相对于其他灵活的项目进行扩展的量。
    * __flex-shrink__: 一个数字，规定项目将相对于其他灵活的项目进行收缩的量。
    * __flex-basis__: 项目的长度。合法值："auto"、"inherit" 或一个后跟 "%"、"px"、"em" 或任何其他长度单位的数字。
    * __auto__: 与 1 1 auto 相同。
    * __none__: 与 0 0 auto 相同。
    * __initial__: 设置该属性为它的默认值，即为 0 1 auto。
    * __inherit__: 从父元素继承该属性。

# JS
## let&const&var
### 块级作用域
>let实际上为 JavaScript 新增了块级作用域。
1. ES6 的块级作用域必须有大括号，如果没有大括号，JavaScript 引擎就认为不存在块级作用域。
2. ES6 允许块级作用域的任意嵌套。
3. ES6 允许在块级作用域内声明函数，函数声明类似于var，即会提升到全局作用域或函数作用域的头部。
同时，函数声明还会提升到所在的块级作用域的头部。
### 三者区别
|变量声明|作用域|变量提升|暂时性死区|重复声明|初始化|
|:---:|:---:|:---:|:---:|:---:|:---:|
|let|块级作用域|不允许|存在|不允许|非必须|
|const|块级作用域|不允许|存在|不允许|必须|
|var|函数作用域|允许|不存在|允许|非必须|
### const
&emsp;const实际上保证的，并不是变量的值不得改动，而是变量指向的那个`内存地址所保存的数据`不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。
#### JS 常用类型
* 基本类型: Undefined、Null、Boolean、Number、String，保存在栈中；
* 复合类型： Array、Object ，保存在堆中；
* 基本数据当值发生改变时，那么其对应的指针也将发生改变，故造成 const申明基本数据类型时，再将其值改变时，将会造成报错， 例如 const a = 3 ; a = 5 时 将会报错；但是如果是复合类型时，如果只改变复合类型的其中某个Value项时， 将还是正常使用；


## 箭头函数
1. 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
2. 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
3. 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
4. 不可以使用yield命令，因此箭头函数不能用作 Generator 函数。

## 数组乱序
```js
function arrOutOfOrder(arr){
    for(let i = 0;i<arr.length;i++){
        let n = Math.floor(Math.random()*arr.length),
            j = arr.length-1;
        [arr[n],arr[j]] = [arr[j],arr[n]];
    }
    return arr;
}
```
## 事件的三个阶段
js中事件执行的整个过程称之为事件流，分为三个阶段：捕获阶段，目标阶段、冒泡阶段。
1. 当我们在 DOM 树的某个节点发生了一些操作（例如单击、鼠标移动上去），顶级对象window发出一个事件流，不断经过下级节点直到触发的目标节点。在到达目标节点之前的过程，就是`捕获阶段`（Capture Phase）。事件由页面元素接收，逐级向下，到具体的元素。`父级元素先触发，子元素后触发`。
2. 到达目标函数，便会执行绑定在此元素上的与事件相应的函数，即`目标阶段`。
3. 最后，从目标元素起，再依次往顶层元素对象传递，在此过程中便称之为`事件冒泡`。`子级元素先触发，父元素后触发`。
* 通常情况下，事件相应的函数是在`冒泡阶段`执行的。addEventListener()的第三个参数默认为false，表示冒泡阶段执行（为true的时候，表示捕获阶段执行）。
#### 事件传播的阻止方法:
* 在W3C中，使用stopPropagation()方法。
* 在IE下设置cancelBubble = true。
#### 阻止默认行为：
* 在W3c中，使用preventDefault()方法。
* 在IE下return false。

## 判断设备来源
```js
// 判断移动端设备
function deviceType(){
    var ua = navigator.userAgent;
    var agent = ["Android", "iPhone", "SymbianOS", "Windows Phone", "iPad", "iPod"];    
    for(var i=0; i<len,len = agent.length; i++){
        if(ua.indexOf(agent[i])>0){         
            break;
        }
    }
}
deviceType();
window.addEventListener('resize', function(){
    deviceType();
})
// 判断微信浏览器
function isWeixin(){
    var ua = navigator.userAgent.toLowerCase();
    if(ua.match(/MicroMessenger/i)=='micromessenger'){
        return true;
    }else{
        return false;
    }
}
```


[Cookie&Session]:https://github.com/Sulitude/Front-end-note/blob/master/Cookie.md?_blank "Cookie＆Session简介"
[同源策略&跨域]:https://github.com/Sulitude/Front-end-note/blob/master/同源策略-跨域.md?_blank "同源策略&跨域简介"
[meta标签]:https://github.com/Sulitude/Front-end-note/blob/master/meta%E6%A0%87%E7%AD%BE.md "meta标签简介"
[常见排序算法]:https://github.com/Sulitude/Front-end-note/blob/master/排序算法.md "常见排序算法"