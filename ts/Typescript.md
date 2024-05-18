# Typescript
## 下载
1. 本地 npm i typescript -g
2. 官网playground
## 基础使用
### 类型推断
typescript根据已经书写的代码判断类型
### 类型注解
```ts
let str:string='a'
```
### 类型断言
人工干预结果，在值后面加 as number系统就会认为这个值就是一个number
### 基本类型
string number boolean null undefined
### 类型联合
```ts
//我们可以通过 | 来联合多个数据
let str:string|number =1
//也可以写值
let num:1|2=1
```
### 复杂数据
#### 数组
```ts
let arr:number[]=[1,2,3]
let arr:Array<string>=['a','b','c']
```
#### 元组
限制个数类型
```ts
let t1:[number,string,number?]=[1,2]
```
?为可选
#### 枚举类型
```ts
enum Mynum{
	A,
	B,
	C,
}
//通过Mynum.A或者[0]来访问
```
### 函数
1. 可以对参数、返回值进行类型限定 ,可以用？表示可选
```ts
function MyFn(a?:string,b:string,...rest:number[]):string{
	return a+b
}
```
### 接口
规范对象的定义
```ts
interface Obj{
	name:string,
	age:number
}
const obj:Obj={
	name:'1=gh'
	age:18
}
```
### 类型别名
个性化类型
```ts
type username=string|number
const a:username='123'
```
### 泛型
设置一组可复用的函数
```ts
function myfn<T>(a:T,b:T):T[]{
	return [a,b]
}
myfn<number>(1,2)
//也可以不加，ts会自己类型推断
myfn('a','b')
```
### 函数重载
在js中不支持函数的重载，函数重载其实就是写几个一样的函数，但是参数不一样，实现不同的功能
### 接口继承
上面说到，我们可以利用接口来规范对象的定义，接口也可以继承，就像java一样
```ts
interfance Parent{
	prop1:string,
	prop2:string
}
interfance Child{
	prop3: number
}
```
### 类
ts真的不是js和java的私生子吗？  
```ts
class A{
	content:string
	title:stirng
	aa?:string
	bb=100//默认值
	private t?:string//子类可访问
	protected i:string//子类且其他继承的类可以访问
	static h:string//只能通过类名.属性来访问
	readonly j:string='hhhh'//类似final 不能修改
	private _password:string=''//设置有get set的私有变量的时候，前面加_区分变量和get
	get password():string{
		return '******'
	}
	set password(newPass:string):string{
		this._password=newPass
	}
	constructor(title:string,content:string){
		this.title=title
		this.content=content
	}
}
const a=new A("11",'22')
a._password//私有变量无法访问
a.password//得到的是****** 实际上是访问到get password()的这个方法
a.password='040816'//调用了set方法
```
#### 抽象类
最大的作用就是派生子类(被别人继承)(当别人的爸爸)
```ts
abstract class Animal{
	abstract name:string
	abstract makeSound():void//可以有抽象类和方法 强迫子类重写
	move(): void{
		console.log('移动')//也可以有正常方法
	}
}
```
#### 类实现接口
除了用抽象类当父类，我们也可以让子类去实现接口，这样的好处是，子类可以实现多个接口但只能继承一个父类
```ts
interface Animal{
	name:string
}
interface B{
	age:number
}
class Dong implements Animal,B{
	name:string='age'
	age:number='5'
}
```
#### 泛型类
```ts
class data<T>{
public value:T
constructor(value:T){
	this.value=value
}
do(input:T):T{
	return input
}
}
const str=data<string>('hello')
const num=data<number>(111)
```