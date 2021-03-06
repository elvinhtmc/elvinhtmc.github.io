---
layout: post
title: "JavaScript DOM-动画基础"
date: 2022-1-3
author: "何短短"
header-img: img/post-bg2-js.jpg
catalog: true
tags: 
  -JavaScript
---


JavaScript DOM-动画基础

#### 基础概念与方法

##### **position**

position属性值：static（默认）, fixed（相对于可视窗口）, relative（相对该元素自身）, absolute（相对父容器）

获取元素位置：`element.style.position`

设置元素位置：`element.style.position = "position属性值”；`（再设置left/right或top/bottom，为避免冲突，在x,y均只选一个方位，如left和top)

**time**

设置时间`setTimeout("function",interval)`（function是所要执行函数的名字；interval是数值，多少毫秒以后执行该函数）

在function中设置样式，`element.style.property = value` (value需要放在引号里，否则会解释为变量)

**！ 通过style获取样式有很大局限性，它不支持获取外部CSS设置的样式，只能返回内嵌样式。**

#### moveElement动画函数

``````js
function moveElement(elementID,final_x,final_y,interval) {//引号问题
    if (!document.getElementById) return false;
    if (!document.getElementById(elementID)) return false;
    var elem = document.getElementById(elementID);
    var ypos = parseInt(elem.style.top);//parseInt函数提取数值信息
    var xpos = parseInt(elem.style.left);//1.获取元素位置
    if (xpos == final_x && ypos == final_y) return true;
    //2.设置递归出口
    if (xpos < final_x) xpos++;//3.在未到达最终位置时让位置逐渐改变
    if (xpos > final_x) xpos--;
    if (ypos < final_y) ypos++;
    if (ypos > final_y) ypos--;
    elem.style.left = xpos + 'vw';//给变化后的数值添加单位
    elem.style.top = ypos + 'vh';
    var repeat="moveElement('"+elementID+"',"+final_x+","+final_y+","+interval+")";//将函数赋给repeat变量，组合字符串，方便后续调用函数
    movement = setTimeout(repeat, interval);
    //递归，调用函数自己，直到函数达成xy条件，通过return true结束
} 
``````

在调用`moveElement`函数时，要注意id名加引号。（在函数中`elementID`无需加引号）

``````js
function positionMessage() {
	 moveElement('image',20,40,50);
}
``````

#### 动画实例-mySlide

要求：当鼠标悬浮到文字上方，下方图片如幻灯片一般滑动，变更到对应的图片。

一开始想用css动画解决，但发现目前所学的css主要是元素自身响应（即一个元素触发某事件后，动画在该元素上发生，如a:hover{animation}），而我们需要的时让发生在某元素上的事件引起零一元素的变化，非自身响应，因此此处采用js动画。

**符合直觉的方法**：像之前的图片库项目一样，四张图片。获取文字选项对应的\<a>元素节点，为每个节点添加onmouseover事件，对应不同动画。但该方法每次更换选项都要重新加载新图片，在网络不好时，可能无法立即响应。

**更好的方法：**为所有选项生成一张集体照，设置一个用以显示部分图片的div容器，当鼠标悬停在某选项时，让图片对应部分移动到供显示的容器中。我们选用这一方法。









#### bug

##### 引号问题

在编写函数时，最好把需要带引号的参数（如id和tag）看成一个整体，即`elementID='idName'`，这样做，当参数出现在函数中时就无需重写引号，`document.getElementById(elementID)`。

``````js
function moveElement('elementID',final_x,final_y,interval) (×)
//形参带引号，后续使用elementID也要引号，这容易导致引号错误

function moveElement(elementID,final_x,final_y,interval) （√）
var elem = document.getElementById(elementID);
moveElement('image',20,40,50);//调用函数，名字带引号
``````

```js
//此处引号也易出错
repeat="moveElement('"+elementID+"',"+final_x+","+final_y+","+interval+")"
```







