# APIs
---
## DOM
* DOM:操作网页内容
* DOM对象：对于html里的属性，js把他们当作对象
* ducument是DOM里最大的对象
### 获取并操作DOM对象
1. 通过css选择器  
   1. 选择匹配的第一个元素  
   ```javascript
   ducument.querySelector('css选择器')
   ```
* ()里css怎么写就咋写，引号不能省略  
* 返回值为css选择器匹配的第一个元素(对象)  
   2. 选择匹配的多个元素   
   ```javascript
   ducument.querySelectorAll('css选择器')
* 返回值：css选择器匹配的所有对象集合，返回的是一个数组    
* *querySelector()只能选择一个元素，可以直接操作，querySelectorAll可以选择多个操作，**不能直接操作***  
* 另外得到的数组是一个伪数组(与长度无关)：有长度有索引号，没有操作方法，如pop(),push()
### 操作元素内容
*先获取元素 再修改元素*
1. innerText属性：显示纯文本，不解析标签
```javascript
const info=document.querySelector('.info')
info.innerText='abc'
```
2. innerHtml属性：比innerText多一个解析标签
```javascript
const info=document.querySelector('.info')
info.innerHtml='<strong>abc</strong>'
```
### 操作元素属性
1. 常用属性：对象.属性=值
2. 样式属性：对象.style.样式属性=值(有引号)，生成的是行内样式表，权重比较高，如果你想再在上面调整css是改不掉的
*出现-的用小驼峰命名法如backgronud-color变成backgroundColor* 
3. 样式属性：直接修改类名(为了解决修改多个元素通过style比较繁琐的问题)：元素.className='类名'(不用加.) 
```javascript
<style>
.nav{
   color:red;
   margin:0 auto;
}
.box{
   padding:10px 10px;
   margin:10px 10px;
}
</style>
<div class="nav">123</div>
<script>
const div=document.querySelector('.nav')
div.className='box'
</script>
//最后解析div的时候会变成<div class="box"></div>
//如果想保留原来的样式，那么div.className='nav box'
```
4. 样式属性：通过classList控制(为了解决className容易覆盖以前类名的问题)
```javascript
//追加一个类
元素.classList.add('类名')
//删除一个类
元素.classList.remove('类名')
//切换一个类
元素.classList.toggle('类名')
//切换就是有就删掉，没有就加上
```
### 操作表单属性
```javascript
<input type="text" value="电脑">
// value是用户输入的
<script>
//获取元素
const uname=document.querySelector('input')
//获取值,innerHtml得不到
console.log(uname.value)
//赋值
uname.value='我要电脑'
uname.type='password'
</script>
```
表单中添加就有效果，移除就没有效果，一律用布尔值表示，true表示添加该属性，false表示移除该属性，比如disabled(按钮禁用)checked(勾上)selected(下拉菜单)
```javascript
<input type="checkbox">
<script>
const ipt=document.querySelector('input')
ipt.checked=true
//用'true'也可以但不提倡，因为这是因为是非空字符串才隐式转换为true
</script>
```
```javascript
<input type="buuton">
<script>
const ipt=document.querySelector('input')
ipt.disabled=true
</script>
```
其实checked和disabled也是属性也是值，如果写在input里可以直接写checked，这相当于checked="checked"，像这样值和属性一样的，就可以只写一个。
* 表单里的button用innerHTML，一般单标签用value，双标签用innerHTML
### 操作自定义属性
* data-自定义属性
* 一律用data-开头
* dom对象上一律用dataset对象当时获取
```js
<div class="box" data-id="1"></div>
<script>
const box=document.querySelector('.box')
console.log(box.dataset.id)
</script>
```
### 定时器-间歇函数
1. 开启定时器
```js
setInterval(函数,间隔时间)
```
* 经过一段时间就调用函数，单位为毫秒1s=1000ms
* 函数要么是函数名(不加小括号)，要么是匿名函数，要么'fn()'
* 定时器的返回值为它的id，独一无二
2. 关闭定时器
```js
let n=setInterval(fn,1000)
clearInterval(n)
```
### 事件监听
* 事件：系统内发出的动作或者发生的事情
* 事件监听：有事件触发，立即调用一个函数做出响应
```js
元素对象.addEventListener('事件类型',要执行的函数)
//事件源：哪个dom元素被事件触发了，获取dom元素
//事件类型：用什么方式触发
//调用的函数：做什么事
```
### 事件类型
1. 鼠标事件(鼠标触发):click 鼠标点击   
   mouseenter 鼠标经过  
   mouseleave 鼠标离开
(没代码提示)
2. 焦点事件(表单获得光标)：focus获得焦点  
blur失去焦点
3. 键盘事件(键盘触发)：Keydown键盘按下触发  
Keyup键盘抬起触发
4. 文本事件(表单输入触发)：input
5. change事件，change 事件被<input>, <select>, 和<textarea> 元素触发
   1. <input type="radio"> 和 <input type="checkbox"> 的默认选项被修改时（通过点击或者键盘事件）。

   2. 当用户完成提交动作时 (例如：点击了 <select>中的一个选项，从 <input type="date">标签选择了一个日期，
通过 <input type="file">标签上传了一个文件等 )。

   3. 当标签的值被修改并且失去焦点后，但并未进行提交 
(例如：对<textarea> 或者<input type="text">的值进行编辑后)。
### 事件对象
* 是一个对象，存储事件触发时的信息
* 使用场景：判断点了哪个键/哪个元素
在事件绑定的回调函数里第一个参数就是事件对象
* 一般叫event、ev、e
```js
元素对象.addEventListener('click',function(e){})
//括号里的那个参数就是事件对象
```
#### 获取事件对象属性
1. type: 获取事件类型(如click)
2. clientX/clientY:获取光标相对于浏览器可见窗口左上角的位置
3. offsetX/offsetY:获取光标相对于dom元素左上角的位置
4. key 摁下键盘的值
```js
元素对象.addEventListener('click',function(e){
   if(e.key==='Enter'){
      console.log(11)
   }
})
```
### 环境对象
* 函数内部特殊的变量this，代表当前函数运行时所处的环境
* 普通函数里this指向window
* this指向他的对象(谁调用，this就是谁)
### 回调函数
```js
function fn(){
   console.log('1')
}
setInterval(fn,1000)
//fn就是一个回调函数
```
### 事件流
* 事件流指的是事件完整执行过程中的流动路径：分为捕获和冒泡，一般使用冒泡
* 事件捕获：从外到里查找，代码：在事件监听的括号的最后添加一个参数，true(基本不用)，L0事件只有冒泡没有捕获
* 事件冒泡：代码，把true删了(平时的样子)，一个元素被触发事件后，依次向上调用所有父级元素的同名事件，它是默认存在的
***捕获冒泡就是执行顺序不同，捕获从上往下，父到子，冒泡从下到上，子到父***
###  阻止冒泡
因为默认就有冒泡，容易导致事件影响到父级元素。
```js
事件对象.stopPropagation()
//事件对象就是e
//阻止流动传播，阻止冒泡和捕获
//写在子事件里面
```
### 解绑事件
addEventListener方式，必须使用removeEventListener(事件类型,事件处理函数,[捕获或冒泡阶段])[]里的可写可不写。
例如
```js
function fn(){
   alert('点击了')
}
btn.addEventListener('click',fn)
btn.removeEventListener('click',fn)
//匿名事件无法被解绑
```
* 鼠标经过区别
mouseover、mouseout会有冒泡效果  
mouseenter、mouseleave没有冒泡效果(推荐)
### 事件委托
利用冒泡解决的小技巧  
原理：给父元素注册事件，当我们触发子元素的时候，会冒泡到父元素身上，从而触发父元素的事件
```js
//需求：点击li变色，点击其他不变色
<ul>
<li>1</li>
<li>2</li>
<li>3</li>
<p>4</p>
</ul>
<script>
ul.addEventListener('click',function(e)){
   if(e.target.tagName==='LI'){
      e.target.style.color='red'
   }
}
</script>
//e.target指的是点击的那个元素
//原理是点击子元素，子元素把点击冒泡冒到ul上，因为之前给ul设置了一个事件监听，由ul触发事件再控制子元素
```
### 阻止默认行为
```js
e.preventDefault()
```
### 其他事件
#### 页面加载事件
改变代码从上到下的执行顺序,加载外部资源(图片，外联css和js)加载完毕后触发的事件
事件名:load
```js
window.addEventListener('load',function(){
   //执行操作
})
//window是最大的对象，是整个页面，也可以针对某一个元素设定加载事件
   img.addEventListener('')
```
事件名:DOMContentLoaded
当html文档被加载出来后，即可执行操作，不等待样式表、图像
```js
document.addEventListener('DOMContentLoaded',function(){
//这个加载速度快一点
})
```
#### 元素滚动事件
滚动条滚动时持续触发的事件
事件名:scroll
```js
window.addEventListener('scroll',function(){

})
//scrollTop和scrollLeft为属性，表示被卷去的头部(左侧)
//返回的是数字型，不带单位
//可读也可写，赋值不带单位
```
开发中常用页面滚动，首先要获取要页面(要给body单位)
```js
document.documentElement//返回对象为html
```
#### 页面尺寸事件
在媒体查询前，用js可以在窗口尺寸改变的时候触发事件
resize(在文本域那里的css设置为resize:none)
```js
window.addEventListener('resize',function(){
   //执行的代码，可以检测屏幕宽度，但后来有媒体查询了
})
```
js里还能获取可见的宽高  
属性名:clientWidth和clientHeight(无单位，不包括border)
它能获取一些不好获取的宽度，如行内及行内块的宽度
##### 元素尺寸
offsetWidth和offsetHeight获取自身宽高，包括border，得到的还是数字型，如果盒子被隐藏，获取的结果是0，是只读属性
##### 位置尺寸
获取元素离自己最近的父级元素的左上距离，如果没有已经定位的父级元素，那就去找浏览器(有一点像绝对定位)
### 日期对象
表示事件的对象，让网页显示日期
#### 实例化
加new的就是实例化
```js
const date=new Date()
console.log(date)
//想得到特定某一天写Date('2022-5-1 08:30:00')
```
#### 日期对象方法(对象里的方法)
由于日期对象返回的数据我们不能直接使用，需要转换格式
![图 1](../images/c67abe9e2656e90c4e1861a5bc980e0cbffa00bec1bcaaf00c2b22944a541272.png)
注意月份是0-11，星期是0-6    
关于星期，我们可以通过用数组的方式来返回
这个是方法，需要有一个对象
```js
const date=new Date()
let h=date.getHours()
```
其他方法
```js
const date=new Date()
const div=document.querySelector('div')
div.innerHTML=date.toLocaleString()//2020/1/1 24:00:00
div.innerHTML=date.toLocaleDateString()//2020/1/1
div.innerHTML=date.toLocaleTimeString()//24:00:00
```
#### 时间戳
是1970.01.01.00:00:00到现在的毫秒数，用于倒计时效果   
算法:将来的时间戳-现在的时间戳=剩余时间毫秒数，再将剩余时间毫秒数转换为剩余时间的年月日时分秒就是倒计时时间
获取时间戳:+new Date()
返回指定时间:+new Date('2022-4-1 18:30:00')
#### 倒计时计算
注意要从毫秒化成秒
天数:d=parseInt(总秒数/60/60/24)
小时:h=parseInt(总秒数/60/60%24)
分:m=parseInt(总秒数/60%60)
秒:s=parseInt(总秒数%60)
### 节点操作
#### DOM节点
dom树里每一个内容都称为节点   
节点类型:
1. 元素节点:body、div，其中html为根节点    
2. 属性节点:href、class、id
3. 文本节点
重点是元素节点，后面操作也指dom节点
#### 查找节点 都是属性别加括号
1. 父节点
```js
//子元素.parentNode 返回最近的一级父节点 找不到返回null
<div class="ye"></div>
<div class="fu"></div>
<div class="zi"></div>
const zi=document.querySelector('.zi')
console.log(zi)
console.log(zi.parentNode)
console.log(zi.parentNode.parentNode)
//返回的是dom对象
```
2. 子节点
```js
//childNodes获得所有子节点,包括文本节点,注释节点
//children(重点)仅仅获得所有元素节点,返回的是伪数组
父元素.children
```
3. 兄弟节点
```js
元素.nextElementSibling//上一个兄弟节点
元素.previousElementSibling//下一个兄弟节点
```
#### 增加节点
分两步:创建一个新的节点，之后把该节点放入指定元素内部
```js
document.createElement('标签名')
//假设现在body里有一个ul
//1.插入到父元素的最后一个子元素
父元素.appendChild(要插入的元素)
const li=document.createElement('li')
const ul=document.querySelector('ul')
ul.appendChild(li)
//2.插入到父元素某个子元素的前面
父元素.insertBefore(插入元素,在哪个元素前面)
```        
关于insertBefore插入到哪个元素前面，可以用属性children
如ul.children就代表了ul的子元素，它是一个伪数组，如果要放到最前面那就是ul.children[0]
#### 克隆节点
```js
元素.cloneNode(布尔值)
//若为true，代表克隆时会包含后代节点一起克隆，一般写true
//若为false，克隆时不代表后代节点，只克隆标签，不包含内容
//默认为false
```
#### 删除节点
```js
//只能通过父元素删除
父元素.removeChild(要删除的元素)
```
### js插件
1. 把css、js引入到目录下
2. 引入代码
3. 挑效果之后复制
## BOM
### window对象
浏览器对象模型，BOM包含DOM  
window对象是一个全局对象，是js里最顶级的对象
#### 延时函数(定时器)
```js
setTimeout(回调函数,等待的毫米数)
//把一段代码延迟执行一次
```
清除延时函数
```js
let timer=setTimeout(回调函数,等待的毫米数)
clearTimeout(timer)
```
#### js执行机制
单线程:同一个页面只能做一件事，但页面渲染可能挺慢的  
为了解决这个问题，出现了同步和异步  
同步:执行任务顺序和任务排列顺序一致   
异步:在做这件事的同时，还可以去处理其他事   
同步任务:同步任务在主线程上执行，形成一个执行栈
异步任务:通过回调函数解决，一般有三种类型  
1. 普通事件:click resize
2. 资源加载:load error
3. 定时器:包括setInterval setTimeout   
异步任务添加到任务队列(消息队列)中
***执行机制***   
1. 先执行执行栈里的同步任务(主车道)
2. 异步任务放到任务队列中(应急车道)
3. 执行栈里的所有同步任务执行完毕，系统按序读取任务队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行
***由于主线程不断地重复获取任务、执行任务、再获取任务、再执行，这种机制被称为事件循环(event loop)***
#### location对象
1. href(重要的)
```js
location.href//可以获取网址
location.href='http://www.baidu.com'//可以赋值，通过js来跳转页面
//实例:注册完成5s后跳回首页
```
2. search:search属性获取中携带的参数，?后面的部分(表单中输入的东西？我也不确定)
3. hash:获取#后的参数，后期使用，单页面变化
4. reload，这是方法要加()，前面的是属性
```js
location.reload(true)//强制刷新，等于CTRL+f5
//如果给了true表示强制刷新
```
#### 其他对象(navigator history)
1. navigator 记录了浏览器自身的相关信息
```js
// 检测 userAgent（浏览器信息）
!(function () {
const userAgent = navigator.userAgent
// 验证是否为Android或iPhone
const android = userAgent.match(/(Android);?[\s\/]+([\d.]+)?/)
const iphone = userAgent.match(/(iPhone\sOS)\s([\d_]+)/)
// 如果是Android或iPhone，则跳转至移动站点
if (android || iphone) {
location.href = 'http://m.itcast.cn' }
})()
```
2. history 管理历史记录，有前进后退等功能
常用方法：
   1. back()后退
   2. forward()前进
   3. go(参数)参数是1就前进一个页面，是-1就后退一个页面
### 本地存储
小型仓库，放在浏览器里面，可以存储数据
#### localStorage
作用:将数据永久存储在本地，除非手动删除，否则关闭页面也会存在   
特性:可以多窗口共享(统一浏览器共享)，以键值对形式存储   
语法:
1. 存储数据
```js
localStorage.setltem(key,value)
localStorage.setItem('uname','gh')
```
2. 获取
```js
localStorage.getItem('uname')
```
3. 删除
```js
localStorage.removeItem('uname')
```
4. 改
```js
localStorage.setItem('uname','ab')
```
***如果不是数字型加引号啊，不然被认成变量***
另外本地存储只能放字符串，数字型放进去就变成字符串了
#### sessionStorage
生命周期为关闭浏览器窗口，使用方法与local一致，使用的比较少
#### 存储复杂数据类型
```js
const obj={
   uname:'gh',
   age:18,
   gender:'男'
}
//存储复杂数据类型 无法直接使用(下一条)
localStorage.setItem('obj',obj)
//1. 复杂数据类型想要存储必须转换为JSON字符串存储
localStorage.setItem('obj',JSON.stringify(obj))
//JSON对象属性和值都有双引号，检测类型依然是字符串 {"uname":"gh","age":18,"gender":"男"}
//2. 把JSON字符串转换为对象，JSON.parse(localStorage.getItem())
const str=JSON.parse(localStorage.getItem(obj))
```
#### map和join方法
1. map(),最大特点是能修改数组并直接返回一个新数组
```js
const arr=['red','blue','pink']
const newArr=arr.map(function(ele,index){
   return ele+'颜色'
})
console.log(newArr)
//得到的是red颜色，blue颜色，pink颜色
```
2. join(),把数组所有元素转换一个字符串
```js
const arr=['red颜色','blue颜色','green颜色']
console.log(arr.join())
//得到的是red颜色,blue颜色,green元素
console.log(arr.join(''))
//red颜色blue颜色green元素
comsole.log(arr.join('|'))
//red颜色|blue颜色|green元素
//''里面放什么符号就用什么隔开
```
### 正则表达式
匹配字符串中字符组合的模式，用于查找
使用场景:验证表单，过滤/替换敏感词，获取字符串的特定部分
#### 语法
1. 简单的
```js
const str='学前端'
//1. 定义规则
const reg=/前端/
//2. 是否匹配
console.log(reg.test(str))//如果有前端就返回true，否则返回false
//3. 检索(了解)
console.log(reg.exec(str))//找到返回数组，找不到返回null
```
#### 元字符(特殊字符)
1. 边界符(表示位置,开头和结尾,必须用什么开头,用什么结尾)
^表示以什么开头，$表示以什么结尾，注意同时存在^$会精确匹配![图 2](../images/59c4c52c8cbf76cab3eb2fc10df70014dc9180c9d0b1107a5323e15cf99aa5db.png)  
2. 量词   
   1. *重复0次或更多次(>=0)
   2. +重复一次或更多次(>=1)
   3. ?重复零次或一次(0或1)
    ![图 3](../images/8aeec7136cca594c539be053d3fcb9de95a7ea30326c8b4bcfc1b3fe045f308b.png)  
   4. {n}重复n次
   5. {n,}重复n次或更多次
   6. {n,m}重复n到m次 逗号前后不能有空格
3. 字符类  
   1. [] 匹配字符集合，后面的字符串只要包含[]里的任意一个字符，即返回true， []里加-表示范围如 [a-z]再另外[]里加^表示反,如 [^a-z]表示除了26个字母之外的任何字符，另外还有[a-zA-Z0-9]表示字母和数字
   2. .匹配除换行符之外的任何单个字符
   3. 预定义:![图 4](../images/d71079a69b9427cbbb6088bea67e0c408be8b23176db3e9997f6d7ae4e77e7d3.png)  
#### 修饰符
约束正则表达式执行的某些细节行为，如是否区分大小写和多行匹配等 /表达式/修饰符
1. i是ignore的缩写，表示正则匹配时字母不区分大小写
```js
console.log(/^java$/.test('java'))//true
console.log(/^java$/i.test('JAVA'))//还是true
```
2. g 全局搜索
3. replace
```js
str.replace(/正则表达式，要查找的东西/,'用这个东西替换前面的东西')//如果要全替换要加g，返回值是替换之后的字符串
```