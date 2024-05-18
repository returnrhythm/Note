# js进阶
### 作用域
1. 局部作用域  
   1. 函数作用域
   2. 块作用域，被{}包括的叫块作用域，{}外有可能可以访问，var声明的变量的外部可以访问，不产生块作用域，let和const声明的变量外部不可以被访问，产生块作用域
2. 全局作用域  
script标签和.js文件就是全局作用域，在此声明的变量在函数内部也可以被访问
注意：  
1. 为window对象动态添加的属性默认也是全局的，不推荐，如window.a=10     
2. num=10 未使用关键字声明的变量为全局变量，不推荐     
3. 减少全局变量
#### 作用域链
是底层的变量查找机制
1. 函数被执行时，优先当前函数作用域在查找变量
2. 如果当前查找不到，则会依次逐级查找父级作用域直到全局作用域
3. 子作用域可以访问父作用域，而父作用域无法访问子作用域
#### 垃圾回收机制 GC
js中内存的分配和回收都是自动完成的，内存不使用的时候被垃圾回收器自动回收
##### 内存的生命周期
1. 内存分配:声明变量、函数、对象时，系统自动分配内存
2. 内存使用:读写内存，使用变量、函数等
3. 内存回收:使用完毕，由垃圾回收器自动回收不再使用的内存
注意:  
1. 全局变量一般不会回收(关闭页面回收)
2. 一般局部变量的值，不用就会被自动回收
3. 内存泄漏:程序中分配的内存无法释放或者未释放叫做内存泄漏
##### 到底咋回收垃圾的
1. 栈:栈中的内存由操作系统自动分配内存自己释放，如函数的参数值、局部变量等基本数据类型要放到栈里
2. 堆:存放复杂数据类型，由程序员分配释放，如果程序员不释放则会由垃圾回收机制回收，所以回收垃圾一般指在堆里的
###### 两种算法
1. 引用计数法 ie浏览器在用，现在用的少
   1. 跟踪记录被引用的次数
   2. 被引用一次就++，减少引用就--
   3. 如果引用次数为0，就释放内存
   4. 缺陷:如果两个对象相互引用，尽管不再使用但垃圾回收器不会回收(引用次数永远不为0)导致内存泄漏
2. 标记清除法
从根部出发(js中就是全局对象)出发定时扫描内存中的对象，凡是能从根部到达的对象，都是需要使用的，那些不能从根部出发触及到的被标记为不再使用，稍后进行回收
![图 1](../images/eace9321c777e28d55ecc50908d436d2b7b89d01221b00f20702673d66800c80.png)  右面的形象描述了引用计数法的缺陷，在复杂数据类型相互引用的时候也可以回收
#### 闭包 把局部变量的作用域变大
一个函数对周围状态的引用捆绑在一起，内层函数中要访问外层函数的作用域 闭包=内层函数+外层函数的变量
```js
function outer(){
    const a=1
    function f(){
        console.log(a)
    }
    return f
}
outer()===f===function fn(){}
const fun=outer()
fun()//调用函数
```
我的理解:函数套函数，里面的函数要用到外层函数的变量
用处:外部要调用内部的函数   
我操闭包有点难懂
闭包可以实现数据的私有，看个例子,统计函数调用次数，函数调用一次就++
```js
//没闭包
let count=1
function fn(){
    count++
    console.log(count)
}
fn()//2
fn()//3
//但count是一个全局变量，容易被修改
```
```js
//有了闭包
function fn(){
    let count=1
    function fun(){
        count++
        console.log(count)
    }
    return fun
}
const res=fn()
res()//2
res()//3
//fn()()会执行let那句,但fn()的返回值是fun,所以res()不会执行let那句,能保存count的数据
```
#### 变量提升
在var声明的变量中，允许在变量声明之前即被访问
```js
console.log(str)
var str='hi'
```
变量提升是在当前作用域下搜索var声明的变量，把它放在当前作用域的最前面，但是只是声明，并没有赋值，相当于
```js
var str
console.log(str)
str='hi'
```
所以显示的是undidefined，但不会报错
***实际开发推荐使用let/const，先声明再访问***
### 函数进阶
#### 函数提升
函数声明之前就可以调用，代码执行之前会把函数声明提升到当前作用域的最前面，只提升声明，不提升调用   
但函数表达式有点特殊，必须先声明赋值再使用，如
```js
fun()
var fun=function (){
    console.log(1)
}
// 它提升完是这样的
var fun
fun()
fun=function (){
    console.log(1)
}
```
#### 函数参数
1. 动态参数:arguments是函数内部内置的伪数组变量、包含了调用函数传进来的所有实参，可以动态获取
```js
//做一个动态求和
function sum(){
    let s=0
    for(let i=0;i<arguments.length;i++){
        s=s+arguments[i]
    }
    console.log(s)
}
sum(5,10)
sum(1,2,3)
```
2. 剩余参数:...数组名(自己起的)，只存在于函数里面
```js
//还是求和
function sum(...other){
    let s=0
    for(let i=0;i<other.length;i++){
        s=s+other[i]
    }
    console.log(s)
}
```
事实上...数组名是置于最末函数形参之前的，用于获取多余的实参，这个数组是一个真数组
```js
//假设用户至少输入2个数
function sum(a,b,...other){
    let s=0
    for(let i=0;i<other.length;i++){
        s=s+other[i]
    }
    console.log(s)
}
sum(1,2,3,4)
//这样a是1，b是2，other[0]是3，other[1]是4
```
剩余参数会比动态参数更加灵活，且返回的数组是一个真数组，实际开发中建议使用剩余参数，后面箭头函数里面没arguments
##### 展开运算符
... 可以展开数组,不会修改原数组，应用:求数组最大值(小),合并数组
```js
const arr=[1,2,3]
const arr1=[4,5]
console.log(...arr)//1 2 3
console.log(Math.max(...arr))//等价于1,2,3
const arr2=[...arr,arr1]
```
#### 箭头函数(重要)
##### 基本写法
让函数更简短且不绑定this  
使用场景:需要匿名函数的地方
```js
const fn=function(x){
    console.log(x)
}
const fn=(x)=>{
    console.log(x)
}
//如果只有一个参数，可以省略小括号,没参数不能省略
const fn=x=>{
    console.log(x)
}
//只有一行代码，可以省略大括号
const fn=x=>console.log(x)
//如果只有一行代码 可以省略return
const fn=x=>x+x
//相当于const fn=x=>return x+x
//箭头函数还可以返回一个对象
const fn=uname=>({uname:uname})
console.log(fn('gh'))
//返回的是一个对象{uname:gh}
//注意，对象最外面用()包起来
```
##### 箭头函数参数
普通函数有arguments，但箭头函数没有，它只有剩余参数  
关于箭头函数的this 箭头函数没有this，需要去找上一级作用域的this指向谁
#### 解构赋值
##### 数组解构
数组解构是将数组的单元值快速批量给一系列变量的简洁语法(把数组元素赋值给变量)
```js
const arr=[100,60,80]
const [max,min,avg]=arr
//或者
const [max,min,avg]=[100,60,80]
//等价于const max=100，const min=60，const avg=80
//应用:交换两个数的值
let a=1
let b=2;
[b,a]=[a,b]
```
要加分号的场景
1. 对数组展开且它开头的时候进行操作，就像[a,b]而不是arr这样的，必须在[]的前面加一个;可以加在上一行末尾，不加的话，系统会合并这一行和他的上一行
2. 立即执行函数之间要用;隔开
###### 一些特殊场景
1. 值少变量多，多的那个参数是undefined
2. 变量少值多，就对应就行
3. 剩余参数解决2
```js
const [a,b,...c]=[1,2,3,4]
```
4. 防止undefined
```js
const [a=0,b=0]=[]
```
5. 可以忽略一些值
```js
const [a,b,,d]=[1,2,3,4]
//d是4
```
6. 支持多维数组解构
```js
const arr=[1,2,[3,4]]
console.log(arr[2][0])//3
const [a,b,c]=[1,2,[3,4]]//c是[3,4]
const[a,b,[c,d]]=[1,2,[3,4]]//c是3
```
##### 对象解构(重要)
把对象属性和方法快速批量赋值给一系列变量的简洁语法
###### 基本语法
```js
const obj={
    uname:'gh',
    age:18
}
const {uname,age}=obj
//或者
const {uname,age}={uname:'gh',age:18}
//等价于
const uname=obj.uname
```
***要求:解构时属性名和变量名相同，不能和外面的名字冲突***
如果真有一个变量和属性冲突了，还可以修改属性名
```js
const {uname:username,age}={uname:'gh',age:18}
```
这相当于把uname改成了username   
什么值:赋值给谁
###### 一些其他场景
1. 数组对象解构
```js
const pig=[
    {
        uname:'佩奇',
        age:18
    }
]
const [{uname,age}]=pig
```
2. 多级对象解构
```js
const pig={
    name:'a'
    family:{
        mother:'b'
    }
}
const {name,family:{mother}}=pig
const person=[
    {
     name:'a'
     family:{
         mother:'b'
     }
    }
]
const [{name,family:{mother}}]
```
后期在项目中，可以直接引用(见下)
```js
const msg={
    age:18
    data:{
        name:a
    }
}
//下面是之前的
function fn(arr){
   const {data}=arr//就等价代换了一下
   console.log(data)
}
fn(msg)
//改进后
function fn({data}){
    console.log(data)
}
fn(msg)
//如果要改名字
function fn({data:mydata}){
    console.log(data)
}
```
###### forEach只能遍历数组，不返回值，只起一个遍历
```js
arr.forEach(function(item,index){
    console.log(item)//数组元素，必须写
    console.log(index)//索引号，可不写
})
//map能返回数组，forEach不可以
```
遇到简单的没必要用forEach,但它适合遍历数组对象[{}]
### 深入对象
#### 创建对象(3种)
1. 字面量(推荐)
```js
const o={
    name:gh
}
```
2. new Object
```js
const obj=new Object({})
```
3. 构造函数
#### 构造函数
是特殊函数，用于初始化对象  
用字面量创建对象只能一次一个(属性一样值不同)，可以通过构造函数快速创建类似对象。
###### 语法
1. 函数的命名以大写字母开头
2. 只能由new来执行
```js
//this指向调用者
function Pig(uname,age){
    this.name=uname
    this.age=age
}
//前面的是属性，后面的是形参
new Pig('peiqi',6)
```
实例化:把东西变成了实在的东西(对象)
实例化过程(加new发生了啥子):  
1. 创建新的空对象
2. 让构造函数里的this指向新的对象
3. 执行构造函数里的代码
4. 返回新对象
#### 实例成员
用构造函数创建的对象称为实例对象，实例对象里面的属性和方法称为实例成员(实例属性、实例方法)  
```js
function Pig(name){
    this.name=name
}
const peiqi=new Pig('佩奇')
peiqi.name='小猪佩奇'//这是实例属性
```
说明:  
1. 虽然构造函数的创建的结构一样，但是他们是不同的对象
2. 创建的对象彼此独立互不影响
#### 静态成员
构造函数里的属性方法称为静态成员(静态属性和静态方法)
```js
function Person(name,age){

}
Person.eyes=2//静态属性
```
说明:
1. 静态函数只能构造函数来访问，如console.log(Person.eyes)
2. 静态函数里的this指向构造函数
之前学的什么Math.random都是构造函数
#### 内置构造函数(包装类型)
js中基本数据类型:字符串、数值、布尔、undefined、null，引用类型:对象  
明明是对象才有属性方法(str.length)，但事实上字符串、数值、布尔等基本数据类型都有专门都构造函数，这些称为包装类型
```js
const str='gh'
//事实上js底层是这样的
const str=new String('gh')
```
##### Object
三个静态方法(只有Object才可以用，实例不可用)
```js
const o={
    name:'gh',
    age:18
}
//1. 获得所有属性名
console.log(Object.keys(o))
//返回数组['name','age']
//2. 获得所有的属性值
console.log(Object.values(o))
//['gh',18]
//3. 对象的拷贝
const oo={}
Object.assign(oo,o)//把后面的拷贝给前面的
console.log(oo)
//使用:给对象添加属性
Object.assign(o,{gender:'man'})
```
##### Array
笑死了这个是对于方法了理解草哈哈![图 2](../images/23e7b08d9ab6a8773a395fb3b2491a3f5f78dd00f80c6a208dc0925488fb4f90.png)  
1. forEach 遍历数组
2. filter 过滤数组，返回符合筛选满足条件的数组元素
3. map 迭代数组，返回处理之后的数组
4. reduce 返回累计处理的结果(如:求和) 对于reduce下面介绍一下
```js
const arr=[1,2,3]
// arr.reduce(function(上一个值,现在的值){},初始值)//如果有初始值就写，没有就不写
const total=arr.reduce(function(pre,cur){
    return pre+cur
})//结果是6，第一次是1+2=3，第二次是3+3=6
//1.没初始值:上一次值为数组的第一个元素的值
//2.每一次循环，把返回值作为下一次循环的上一次值
const total=arr.reduce(function(pre,cur){
    return pre+cur
},10)//结果是16，第一次是10+1=11，第二次是11+2=13，第三次是13+3=16
//1.有初始值:则起始值为上一次值
//2.每一次循环，把返回值作为下一次循环的上一次值
```
在遇到对象数组时,[{}]，那个初始值要设置一下噢，不然pre会变成第一个对象
5. find,查找到满足函数条件的第一个元素，并返回这个元素
```js
arr.find(function(){})
const arr=[
    {
        name:'a',
        price:'100'
    }
    {
        name:'b',
        price:'200'
    }
]
const a=arr.find(item=>item.name==='a')
console.log(a)//结果为{name:'a',price:'100'}
```
![图 3](../images/6157205c32cd8f08387bb0ac784bb4118b5555af53bfeb2e297448c7b8bedc19.png)  
6. every 是不是每一个元素都满足函数条件，如果都满足就是true，有一个不满足就是false
```js
const arr1=[10,20,30]
const flag=arr1.every(item=>item>=10)
console.log(flag)//这是true
```
7. some 如果有一个满足函数条件那就是true，都不满足才是false
8. 把伪数组转化成真数组
```js
Array.from(数组名)//静态成员
<ul>
<li></li>
<li></li>
<li></li>
</ul>
const lis=document.querySelectALL('ul li')
const liss=Array.from(lis)
//liss是一个真数组，可以进行操作如pop
```
##### String
1. split 把字符串拆成数组
```js
const str='小明,小红'
const arr=str.split(',')//中间填写分隔符
console.log(arr)//['小明','小红']
```
2. substring(开始的索引号,结束的索引号)结束的可省略，省略了默认取到最后,截取一段字符串并返回它
```js
const str='abcd'
console.log(str.substring(1,2))//b
//注意开始的索引号的字符会被选上，结束的索引号不会被选上，如果要bc，就要(1,3)
```
3. startWith(字符,位置) 判断是否以某个字符串开头(返回布尔值)，位置可以省略，省略的时候默认从头开始观察
```js
const str='abcd'
console.log(str.startWith('ab'))//true
```
4. includes(字符,位置),检测字符串里是否含有这个方法里的字符串，返回布尔值，位置可以省略，省略后默认从头开始搜索
```js
const str='this is a cat'
console.log(str.includes('this'))//true
console.log(str.includes('this',1))//false
console.log(str.includes('is',6))//true
//注意空格标点也算一个位置
```
### 原型(为了解决浪费内存的问题)
1. 每一个构造函数都有一个prototype属性，指向另外一个对象，我们也称为原型对象，这个对象可以挂载函数
2. 可以把不变的方法，定义再prototype对象上，这样所有的实例就可以共享方法
3. prototype 属性允许您向对象添加属性和方法
```js
function peo(name,age){
    this.name=name
    this.age=age
    this.sing=function(){
        console.log('唱歌')
    }
}
const a=new peo('aa',10)
const b=new peo('bb',20)
console.log(a.sing===zxy.sing)
//这个结果是false，事实上，每次new出一个新的对象出来，都会重新创建一个方法，虽然方法看起来是一样的，但是检测出来是不一样的，这样会浪费内存，于是有了原型。
peo.prototype.sing=function(){
        console.log('唱歌')
    }
console.log(a.sing===zxy.sing)
//是true，共享了    
```
总结:公共属性写在构造函数里，公共方法写在原型里，构造函数和原型对象的this都指向实例化的对象
#### constructor属性
在每个构造函数的prototype里面，都有一个constructor属性，它指向了构造函数
```js
function peo(){}
console.log(peo.prototype.constructor===peo)
//结果为true
//在prototype里面加一些方法
peo.prototype.sing=function(){
    console.log('唱歌')
}
//如果需要大量添加方法，就直接给prototype赋值，但是赋值之后会导致prototype里面的属性constructor没了，我们需要让constructor重新指回构造函数
peo.prototype={
    constructor=Star,
    //其他方法
}
```
#### 对象原型 指向原型对象
1. 实例对象通过构造函数被创造，构造函数里有一个属性是prototype可以让里面的方法被所有实例对象共享，我们会发现实例对象和原型并没有直接联系，可实例对象是为什么能访问到原型里的方法呢?  
2. 对象都会有一个属性__proto__(一边是俩下划线)指向构造函数的prototype原型对象  
3. 由于__proto__是一个非标准属性，会有[[prototype]]的写法，意义相同,该属性只可读不可写
4. 在对象原型__proto__里面还有一个constructor属性指向构造函数![图 4](../images/a9a90dc1e2b734db592c4755e326870f315153878ee4bb45ef49be95db05e5a4.png)  
#### 原型继承
如果想写多个对象，但是它们有许多属性是相同的，我们可以采用原型继承(继承到原型对象里面)
```js
// 构造函数  new 出来的对象 结构一样，但是对象不一样
function Person() {
    this.eyes = 2
    this.head = 1
}
function Woman() {

}
// Woman 通过原型来继承 Person
Woman.prototype = new Person()
// 指回原来的构造函数
Woman.prototype.constructor = Woman
// 给女人添加一个方法  生孩子
Woman.prototype.baby = function () {
    console.log('宝贝')
}
const red = new Woman()
console.log(red)
// 男人 构造函数  继承  想要 继承 Person
function Man() {

}
// 通过 原型继承 Person
Man.prototype = new Person()
Man.prototype.constructor = Man
const pink = new Man()
console.log(pink)
```
注意:
1. 将父构造函数赋值给子构造函数的时候，需要将子构造函数的原型对象重新指回父构造函数
2. 为什么父级不是直接一个对象而是一个构造函数？那是因为在子构造函数继承的时候，它们继承的是一个东西，如果给某一子构造函数的prototype中追加属性或者方法时，会导致其他子构造函数也会出现改动，而使用了父构造函数正是应用了构造的函数结构一样，但实际上不一样是两个东西的特点。
#### 原型链-查找规则
1. 只要是对象就有__proto__
2. 只要是原型对象就有constructor
3. 好好看图片![图 5](../images/1127d4323df13ceb9fad14fb30ac8d6ddd9d21d6061e18186321243f9efdb6e6.png)  
##### instanceof
实例对象 instanceof 构造函数    
检测构造函数的prototype(原型对象)属性是否出现在某个实例对象的原型链上
```js
function Person(){

}
const a=new Person()
console.log(a instanceof Person)//t
console.log(Person instanceof Object)//t
//有点像亲子鉴定，到底有没有父子关系
console.log([1,2,3] instanceof Object)//t 这个要特别注意                     
console.log(Array instanceof Object)//t
```
### 技巧
#### 深浅拷贝
*直接等于*  ，拷贝的全是地址
```js
const obj={
    name:'a'
}
const o=obj
//修改o时obj也会被修改，原因图示在下面
```
![图 6](../images/7e7f49442b4cbd0c85063e703776a8b0db2a886a0d763d103311391850e73476.png)  
##### 浅拷贝(深浅只针对引用类型，因为简单类型可以直接赋值)
拷贝的是地址   
```js
const obj={
    name:'a'
}
const o={...obj}
//完成了浅拷贝，修改o不影响obj
const o={}
Object.assign(o,obj)
//也完成喽浅拷贝
```
上面是拷贝对象的，拷贝数组就用concat方法或者展开运算符([...arr])   
但是浅拷贝还是有点问题，浅拷贝只拷贝最外面一层，如果像对象里面还有对象，由于对象是复杂数据类型，在栈里面存的是地址而不是值(简单数据类型拷贝一个相同的值给自己，复杂数据类型拷贝的是地址，但地址虽然复制过来了，指向的东西却是一样的)![图 7](../images/1f90b47571fd5003b7fd2c36e1aad5156526feb2bb3406ba557f4ba7fed91423.png)  
##### 深拷贝
拷贝的是对象，不是地址
###### 递归实现
```js
const obj = {
      uname: 'pink',
      age: 18,
      hobby: ['乒乓球', '足球'],
      family: {
        baby: '小pink'
      }
    }
    const o = {}
    // 拷贝函数
    function deepCopy(newObj, oldObj) {
      debugger
      for (let k in oldObj) {
        // 处理数组的问题  一定先写数组 在写 对象 不能颠倒
        if (oldObj[k] instanceof Array) {
          newObj[k] = []
          //  newObj[k] 接收 []  hobby
          //  oldObj[k]   ['乒乓球', '足球']
          deepCopy(newObj[k], oldObj[k])
        } else if (oldObj[k] instanceof Object) {
          newObj[k] = {}
          deepCopy(newObj[k], oldObj[k])
        }
        else {
          //  k  属性名 uname age    oldObj[k]  属性值  18
          // newObj[k]  === o.uname  给新对象添加属性
          newObj[k] = oldObj[k]
        }
      }
    }
    deepCopy(o, obj) // 函数调用  两个参数 o 新对象  obj 旧对象
```
对于递归实现我自己的理解:
1. 事实上是先开一个浅拷贝，如果检测到复杂数据类型就特殊处理
2. 特殊处理就是根据其类型，再次重新把被复制的那个对象newobj[k]变成相应的复杂数据类型，再次复制的时候应用的是赋值而不是地址(浅拷贝)
3. 判断复杂数据类型的时候，先判断是否为数组，再判断是否为对象，因为数组也属于对象，如果先判断对象，那么数组可能也会按照处理对象的方式来处理
###### lodash _.cloneDeep()
```js
//先引用
<script src="./lodash.min.js"></script>
const obj={
    uname:'pink',
}
const o=_.cloneDeep(obj)
```
###### JSON
```js
const o=const JSON.parse(JSON.stringify(obj))
//转换后变成的对象和原来的不是一个
```
#### 异常处理
##### throw抛异常
```js
function sum(x,y){
    if(!x||!y){
        // throw '参数不能为空'
        throw new Error('参数不能为空')
    }
    return x+y
}
sum()
```
1. 在程序运行期间出现错误，throw将会抛出对应的错误信息，程序会终止运行。
2. Error对象配合throw使用，可以设置更加详细的错误信息(推荐使用)
##### try catch finally
```js
function fn(){
    try{
        //可能发生错误的代码
    }catch(err){
        //catch拦截错误,()里的是一个形参，写啥名字都可以，代表了错误
        console.log(err.message)
        //err.message,message是属性(名字不能写错噢)
        return
        //或throw
        //由于catch不会强制停止程序，所以需要一个return或者throw来强制停止下面所有的程序
    }
    finally{
        //不管程序对不对，一会都要执行的代码
    }
}
```
##### debugger
直接在代码的某一行打出debugger，代表打出一个断点
#### this
##### 指向谁？
1. 普通函数:谁调用我，我指向谁，在全局作用域和普通函数内部this都指向window，有一点我觉得要特别注意，构造函数，new一个后将指向新的对象，如果直接调用的话还是指向window的  
函数里面才有this  
其实还有一个严格模式，严格模式里this指向undefined
2. 箭头函数:向外层作用域中一层一层地找this，有定义且最近的就是
注意:
1. 事件回调中需要dom里的this，如点击按钮之类的，就不推荐使用箭头函数
2. 构造函数，原型函数不要用箭头函数
##### 改变this
1. call() 不咋用,调用函数同时改变this的指向
```js
函数.call(this的指向,剩余参数)
function fn(){
    console.log(this)
}
const obj={
    name:'gh'
}
fn.call(obj)
//这个时候fn里的this就不再指向window，而是指向obj了
```
2. apply()调用函数同时改变this的指向
```js
//apply特别的点是除了第一个参数是this的指向外，第二个参数必须是一个数组，但其实这个数组也是和原来函数的参数一一对应的
function fn(x,y){
    console.log(this)
    return x+y
}
fn.apply(obj,[x,y])
```
应用:数组求最大值(不遍历)  
```js
const arr=[1,2,3,4]
console.log(Math.max(...arr))
console.log(Math.max.apply(null,arr))//null也可以写math，但是不能省噢
```  
call和apply的返回值是函数值
3. bind()重点，不调用函数但能改变this
函数.bind(this指向,剩余参数)，返回值是一个this改变过的函数
### 提高性能
#### 防抖(debounce)
这是回城，只执行最后一次，正在回城我被打断了，然后再回城，执行最后一次，又要重新回城
![图 8](../images/1c983e09e864b08d4ca375413a70cbf659f4b9328a5ee5721b0c541fd83089f4.png)    
单位时间内，频繁触发事件，只执行最后一次  
使用场景:搜索输入、验证
实现方式：
1. lodash
_.debounce(fun,时间)，停一定时间后才执行前面的函数
2. 手写定时器+闭包
#### 节流(throttle)
节流是释放技能，释放完技能有cd，无法再次释放技能，使用场景:在高频触发场景使用，鼠标移动mousemove，页面尺寸缩放resize，滚动条滚动scroll
![图 9](../images/1f3a27cd5570a37a24612632a583e497f6953b283f789d303afae4debd00e061.png)  
单位时间内，频繁触发事件，只执行一次
1. lodash
_.throttle(fun,时间)在一定时间内最多执行一次fun
2. 手写定时器  
在开启定时器时，1s后它自己关掉，在timer=settimeout(fun,1000)里的fun函数里没有办法用cleartimeout清除，用timer=null清除
#### 总结
防抖是你一直触发，我等你不触发我过段时间再执行一次，节流是你一直触发你的，但是我不管，我自己隔一段时间才执行一次