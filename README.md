# less-study
# 前端面试题--之LESS
## 一、LESS是什么？
LESS类似于jquery，less css是一种动态格式语言，属于css预处理语言的一种，它使用类似css的语法，为css赋予了动态语言的特性，如变量，继承，运算，函数等，更方便css的编写和维护。
## 二、编译工具
1.koala
国人开发的less/sass编译工具  
下载地址：http://koala-app.com/index-zh.html  
2.node.js库  
3.浏览器端使用  
## 三、LESS里面的注释
```  
第一种：
/*第一种注释*/
第二种：
//第二种注释
```  
注:第一种注释koala会进行编译出来，而第二种注释koala不会编译出来
## 四、变量
想声明一个变量的话一定要用：@开头，如下面：
```
//变量 必须以@开头 格式: @变量名:值;

@white_color:white;
.box{
	background: @white_color;
	width: 200px;
	height: 200px;
}

```
## 五、混合
写好的样式名称，直接可以在其他的样式里面使用：点+样式名称
如下面：
#### 1.混合--正常
```
@white_color: white;
.box {
    background: @white_color;
    width: 200px;
    height: 200px;

    .box2;
}

//混合
.box2 {
    font-size: 15px;
}

.box1 {
    border: 1px solid #ccc;
    .box;
}
```
#### 2.混合--带参数
```
.border_02(@border_width) {
    border: @border_width solid green;
}

.test_hunhe_02 {
    .border_02(10px)
}
```
#### 3.混合--默认值
```
.border_03(@border_width: 10px) {
    border: @border_width solid blue;
}

.test_hunhe_03 {
    .border_03(50px)
}
```
#### 4.混合--应用场景（兼容多种浏览器的写法的时候）
```
.border_radius(@border_radius:5px){
	-webkit-border-radius:@border_radius;
	-moz-border-radius:@border_radius;
	border-radius:@border_radius;
}

.test_hunhe_04{
	width: 50px;
	height: 50px;
	background: #000;
	.border_radius();
}
```
## 六、匹配
#### 匹配--实例：三角
1.html部分:
```
<div class="sanjiao_01"></div>
```
2.1.css部分:(三角的正常样式写法如下)
```
.sanjiao_01 {
    width: 0;
    height: 0;
    overflow: hidden;
    border-width: 10px;
    //朝下的箭头
    border-color: red transparent transparent transparent; 
	//兼容ie6
    border-style: solid dashed dashed dashed; 
}
```
2.2.css部分:(匹配写法三角的样式写法如下)
```
.triangle(top, @w: 5px, @c: #ccc) {
    border-width: @w;
    border-color: transparent transparent @c transparent; //朝上的箭头
    border-style: dashed dashed solid dashed; //兼容ie6
}

.triangle(bottom, @w: 5px, @c: #ccc) {
    border-width: @w;
    border-color: @c transparent transparent transparent; //朝下的箭头
    border-style: solid dashed dashed dashed; //兼容ie6
}

.triangle(left, @w: 5px, @c: #ccc) {
    border-width: @w;
    border-color: transparent @c transparent transparent; //朝左的箭头
    border-style: dashed solid dashed dashed; //兼容ie6
}

.triangle(right, @w: 5px, @c: #ccc) {
    border-width: @w;
    border-color: transparent transparent transparent @c; //朝右的箭头
    border-style: dashed dashed dashed solid; //兼容ie6
}

.triangle(@_, @w: 5px, @c: #ccc) {
    width: 0;
    height: 0;
    overflow: hidden;
}

.sanjiao_02 {
    .triangle(bottom, 100px)
}
```
#### 匹配--实例：定位
1.html部分:
```
<div class="pipei_position"></div>
```
2.css部分：
```
.pos(r) {
    position: relative;
}

.pos(a) {
    position: absolute;
}

.pos(f) {
    position: fixed;
}

.pipei_position {
    width: 100px;
    height: 100px;
    background: blue;
    .pos(a);
}
```
## 七、运算
一般用于一些长宽高的计算等等
虽然颜色也可以用于加减法的运算，但是一般不常用
```
@test_width: 20px;
.yunsuan_01 {
    width: (@test_width - 10) * 4;
    color: #ccc-10;
}
```
## 八、嵌套
1.html部分：
```
<ul class="list">
	<li><a href=""><span>文章标题</span><em>文章日期</em></a></li>
	<li><a href=""><span>文章标题</span><em>文章日期</em></a></li>
	<li><a href=""><span>文章标题</span><em>文章日期</em></a></li>
	<li><a href=""><span>文章标题</span><em>文章日期</em></a></li>
	<li><a href=""><span>文章标题</span><em>文章日期</em></a></li>
</ul>
```
2.css部分:
```
//正常css写法
/*
ul
ul li
ul li a
ul li a span
ul li a em
 */
//嵌套写法：
.list{
	width: 600px;
	margin:30px auto;
	padding: 0;
	list-style: none;
	li{
		background:pink;
	}
	a{
		font-size:16px;
		color:red;
		padding: 10px;
		overflow: hidden;
		display: block;
		//&继承父集元素 这的&指代的是a
		&:hover{
			color:blue
		}
	}
	span{
		float: left;
	}
	em{
		float: right;
		font-style: normal;
	}
}
```
## 九、@arguments变量
1.html部分：
```
<div class="arguments"></div>
```
2.css部分：
```
//@arguments 变量
.box_border(@w:2px,@c:red,@xx:solid){
	border:@arguments;//等价于 border:@w @c @xx
}
.arguments{
	.box_border();
}


//css编译结果：
.arguments {
  border: 2px #ff0000 solid;
}
```
## 十、避免编译和important用法
#### 1.避免编译希望我写的代码原封不动的出来：就需要用到：~‘内容’
```
@test_width:20px;
.bimian{
	width: ~'@test_width - 10px';
}

编译的结果：
.bimian {
  width: @test_width - 10px;
}
```
2.important用法
```
@test_width:20px;
.bimian{
	width: (@test_width - 10px) !important;
}

编译的结果：
.bimian {
  width: 10px !important;
}


```
## 十一、页面应用其他的less或者css文件时
```
@import 'reset';
//或者 @import (less) 'reset';或者 @import (less) 'reset.less';
@import (less) 'a.css';
```
