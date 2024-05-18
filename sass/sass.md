# SASS
### 语法
#### 选择器嵌套
在写某些后代选择器时，父级有时候过于繁杂，SASS改变了选择器1 选择器2 选择器3这种方式，使用选择器嵌套的形式
```scss
// css
.container{width:1200px; margin: 0 auto;}
.container .header{height: 90px; line-height: 90px;}
.container .header .log{width:100px; height:60px;}
.container .center{height: 600px; background-color: #F00;}
.container .footer{font-size: 16px;text-align: center;} 
// sass
.container{
    width: 1200px;
    margin: 0 auto;
    .header{
        height: 90px; line-height: 90px;
        .log{
            width:100px; height:60px;
        }
    }
    .center{
        height: 600px; background-color: #F00;
    }
    .footer{
        font-size: 16px;text-align: center;
    }
}
```
##### 父选择器&
遇到a:hover或者.top-left这种复合的属性，允许用&代表父选择器(对于前面的就是a和top)
```scss
a{
	color:#333;
	&:hover{
	
	}
}
.top{
	color:#333;
	&-left:{
	
	}
}
```
##### 属性嵌套(font-size,font-weight)
```scss
a{
//注意font后面要加空格
	font: {
		size:
		weight:
	}
}
```
##### 注释
1. /* */多行，会编译到css中
2. //单行，只有scss自己能看到
#### 变量
`$color:#666`

#### 导入@import
scss文件里可以引入别的scss文件。格式如下
`@import 'public'`
在scss文件中，如果一个文件只被引入，那就没有必要监测他，可以在文件名的开头加一个`_`这样就不会被编译成css文件

#### 混合指令@mixin存储一组css声明
```scss
//基础定义
@mixin block{

}
//基础使用
.block{
	@include block;
}
//带选择器定义
@mixin block{
	.block-text{
	
	}
}
//带选择器使用
.block{
	@include block;
}//这里实际编译出的css为 .block .block-text{}
//使用变量
@mixin block($widths:10px){//默认值10px
	width:$widths;
}
//基础使用
.block{
	@include block(20px);//或者指定参数($width:20px)
}
//可变参数使用(目前还没看见过)参数不固定的情况
/** 
    *定义线性渐变
    *@param $direction  方向
    *@param $gradients  颜色过度的值列表
 */
@mixin linear-gradient($direction, $gradients...) {
    background-color: nth($gradients,1);
    background-image: linear-gradient($direction, $gradients);
}
//使用
.table-data{
    @include linear-gradient(to right,#F00, orange, yellow);
}
```
#### 继承@extend
在设计的时候我们可能会设计通用样式和特殊样式，例如对于一个警告按钮，我们通常会设计一个按钮类，再设计一个警告类，这时我们可以令警告类继承按钮类，这样在标签上只写一个类就行。
```scss
.btn{
	//通用样式
}
.alert{
	@extend .btn;
	//特殊样式
}
```
如果被继承的父类的作用只是被继承，我们可以在它的开头加上%(让它前面只有%)，这样不会被编译出css
```scss
%btn{
	//通用样式
}
```
#### 运算符
== and +(连接字符串)   
/ 这个符号，在三种情况下会被看成除号
1. 前后的值有一部分是变量或者函数返回值
2. 值被圆括号包裹
3. 除了/，整个表达式还存在其他的算术运算符
#### 插值表达式#{}
在需要变量的场合，如选择器，属性名，属性值，注释都可以用插值表达式，里面填写变量。
#### 函数
https://sass-lang.com/documentation/modules 自己看吧
#### @if
```scss
.block{
	@if(){
	
	}@else if(){
	
	}@else{
	
	}
}
```
#### @for生成一组css
重复输出
```scss
for $i from 1 through 5{
	//重复渲染css选择器，里面的属性值也可以跟i挂上钩
}
```
through是既包括前面又包括后面，可以改成to这样就不包含后面
#### @each遍历一个集合
```scss
@each $color in $colors{
//index方法返回位置(1 2 3 4)
	$index=index($colors,$color)
	//#{$index - 1}这里如果没有空格，会被认成变量
	.p#{$index - 1}{
		color:$colors;
	}
}
```
#### @while依然是循环
```scss
@while condition{
	
}
```
#### 函数(自定义)

```scss
@function function-name([$param1,$param2,...]){
	...
	@return $value;
}
```
#### if三元条件函数
```scss
if(condition,条件为真的值,条件为假的值)
```
#### @use @import的promax
首先@use拥有@import的基础功能，也就是引入
```scss
@use 'use/common'
```
每个被引入的都会被视为模块，默认文件名为模块名，我们也可以通过as语法改变，我们可以通过模块名.的方式引入变量，混合指令，函数等等
```scss
@use 'use/common as com'
//如果为*则提升为全局模块，不需要模块名. 就可以使用里面的内容，慎用
```
可以定义私有成员，这样别的文件引入也用不了`$-color:blue;`，加一个-
通过！default能变量定义默认值
```scss
$font-size:14px !default;
* {
    margin: 0;
    padding: 0;
    font-size: $font-size;
    color: #333;
}
```
@use引入时可通过with(...)修改默认值
```scss
@use 'use/common' with ( $font-size:16px, );
@use 'use/global' as *;
@use 'use/global' as g2;
common.$font-size:28px; // 也可能通过这种方式覆盖
body {
    font-size: common.$font-size;
    @include base('#FFF');
    @include g2.base('#000');
}
```
建一个_index.scss，把所有的import都放里面，外面用就只用引index
```scss
@use 'use/index';
//默认就引的是index
@use 'use'
```
#### @forward index的promax
在使用index引入的时候，index会引很多模块，之后引index的时候，发现使用变量还需要在index里面再次定义引入其他模块的变量才行，为了解决这个问题，@forword出现了，它可以把这些成员当作自己的成员暴露出去
```scss
@forward 'use/common'
```
我们可以控制转发的成员
`@forward "module" hide $var, mixinName, fnName` 禁止转发某些成员
`@forward "module" show $var, mixinName, fnName` 只转发某些成员
它可以转发多个，但如果里面有相同名字的成员就出问题了，于是出现了前缀
```scss
@forward 'uses/common' as com-*;
@forward 'uses/global' as glob-*;
//使用
@use 'bootstrap';
.body {
    font-size: bootstrap.$com-font-size;
    width: bootstrap.com-column-width(3, 12);
    @include bootstrap.com-bgColor('#F00');
    @include bootstrap.glob-base('#000');
}
```
但是注意，如果有前缀，那么在使用hide或者show的时候也要加上前缀，还有使用with改的时候，判断是在加前缀的时机前还是后，来推出是否要加前缀。  
定义变量都要加!default，不然不能用with改   
同一个文件，如果要同时使用use和forward，建议先写forward。