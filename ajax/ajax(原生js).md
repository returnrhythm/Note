### url地址
url中文叫做统一资源定位符，像身份证号一样标识了互联网上每个资源的唯一存放位置
url一般由三部分组成：
1. 客户端与服务器之间的通信协议，如http
2. 服务器名称，如www.baidu.com，域名
3. 资源在服务器具体存放的位置,后面的东西...html，资源路径
通信协议://服务器名称/具体地址
#### url查询参数
在url后加上?参数名1=值1&参数名2=值2
#### axios查询参数
axios({
    url:'目标资源地址',
    params:{
        参数名:值(拿回来)
    }
}).then(result=>{
    对返回的数据做处理
})
### 常用请求方式
axios({
    url:
    method:请求方式，默认为GET且不区分大小写，加引号
    data:{
        参数名:值(送出去)
    }
})
### axios错误处理
axios({
    url:'目标资源地址',
    params:{
        参数名:值(拿回来)
    }
}).then(result=>{
    对返回的数据做处理
}).catch(error=>{
    console.log(error)
})
catch可以拿到错误
### 响应码
2xx成功 4xx客户端错误 404不存在
### 图片上传
1. 获取图片文件对象
2. 使用FormData携带图片文件
```js
const fd=new FormData()
fd.append(参数名,值)
```
3. 提交表单数据到服务器，使用图片url网址
*** 去看图片上传哪个代码，还挺复杂的说不清楚 ***
### from-serialize插件快捷获取表单的值
之前的案例中，通过选择每个单独的标签再设置其value有点麻烦，该插件可以一下子获取相应区域的所有表单值。
1. 引入js文件
2. 告诉要获取哪个表单的值
```js
const form=document.querySelect('选择器')
```
1. 写serialize函数，该函数返回一个对象
```js
const data=serialize(form,{hash:true,empty:true})
```
* hash表示的是返回的数据结构，如果是true，就返回一个js对象(推荐)如果是false，那就返回一个查询字符串
* empty表示的是是否获取空值，如果是true，那就获取空值(推荐)，反之不获取获取空值的意思就是，如果表单是空的，依然获取它，会表示成passsword=''
* 该函数返回的对象，属性名为每个表单的name属性，name属性的值需要和接口文档一致
### ajax原理-XMLHttpRequest(与服务器通信)
```js
//1. 创建xhr对象
let xhr=new XMLHttpRequest()
//2. 调用open方法，根据接口文档补充
xhr.open('get','url');
//3. 利用loadend事件监听结果
xhr.addEventListener('loadend',()=>{
    console.log(xhr.response)
})
//4. 利用send方法发起请求
xhr.send()
```
#### 查询参数
在url后面加上?参数1=值，如果有参数二用&链接
#### 请求体参数
1. 在send之前设置请求头 告诉服务器内容类型（json字符串）
xhr.setRequestHead('Content-Type','application/json')
2. 准备提交数据
const obj={

}
const userStr=JSON.stringify(obj)
xhr.send(userStr)
### Promise管理异步任务
```js
const p=new Promise((resolve,reject)=>{
//执行异步代码
//成功就执行resolve，并且后面的信息会给then里面的参数
resolve('成功')
//失败执行reject，需要new 一个Error
reject(new Error('失败'))
})
p.then(result=>{
    console.log(result)
}).catch(error=>{
    console.log(error);
})
```
#### Promise的三种状态
当promise被创建的时候，状态为pending，执行代码后，成功就变为fullfilled状态，失败就为rejected状态，状态不能二次改变，改变后调用相应的then或catch
### 同步代码和异步代码
同步代码：逐条执行，需要原地等待结果后才向下执行，例如循环、判断。   
异步代码：调用后耗时，不会原地等待，在将来完成后触发一个回调函数返回结果，如settimeout、事件、ajax
### 回调函数地狱
回调函数里面嵌套回调函数，可读性差，牵一发动全身，捕获不到嵌套错误
#### Promise链式调用
then函数返回一个新的promise，一直串联下去
const p=p2.then(),返回的新的promise由p保存,解决了嵌套的问题
### async和await关键字简化promise链式调用
```js
//1. 定义async函数
//2. await(取代then)接收promise的返回值原地返回
async function fn(){
const obj1=await axios({})//不成功不执行下一步,await阻止异步
const a=obj1.data
}
```
#### 捕获await的错误.try catch代码
try{
可能出错的代码,错误就执行catch里的代码(不执行try剩下的)
}catch(error){

}
### 事件循环
执行代码和收集异步任务的方式,js为单线程,为了不阻塞js引擎设计出来
首先有一个调用栈,还有一个宿主环境(浏览器)这个是多线程的,还有一个任务队列,代码从上到下,同步代码直接在调用栈里面去执行,而异步代码先放在宿主环境执行,异步代码成功后放到任务队列里面排队,在调用栈空闲的时候放到调用栈去执行
#### 宏任务和微任务
宏任务是由浏览器环境执行的异步代码，包括：
1. js脚本执行事件(script)
2. setTimeout和setInterval
3. ajax请求
4. 用户的交互事件
微任务是由js引擎环境执行的异步代码：包括：
1. Promise对象.then() 
***Promise本身是同步代码***
分了宏任务和微任务后，事件循环补充完整，宏任务会被推入宿主环境之后进入宏任务队列，微任务会直接推入微任务队列，在执行完同步代码后优先执行微任务队列，之后执行宏任务队列。
#### Promise.all静态方法
合并多个Promise对象，返回一个大的Promise对象，当所有小的promise对象执行成功后then才会执行，否则有一个失败就执行catch
```js
const p=Promise.all([Promise对象1,对象2])
p.then(result=>{

}).catch(error=>{
    
})
```
### axios配置基地址
axios. defaults. baseURL = '基地址' 
### token
判断用户是否登陆过，前端只能判断token有无，后端才能判断token是否有效
### Header 参数和请求拦截器
header参数 有时候很多请求都需要token
```js
axios({
    url:
    headers:{
        Authorization:`Bearer ${localStorage.getItem('token')}`
    }
})
```
有个axios请求拦截器可以统一设置公共headers选项,在发起请求之前，触发的配置函数，对请求参数进行额外配置
```js
//格式 下面的是模板
axios.interceptors.request.use(function(config){
    //发送请求前做的
    return config
},function(error){
    return Promise.reject(error);
});//后面的function一般不写
```
```js
//发送请求前写的
const token=localStorage.getItem('token')
token&&(config.headers.Authorization=`Bearer ${token}`)
```
### axios响应拦截器
axios响应拦截器：是axios发送请求后在then/catch之前触发的一个函数叫做拦截函数，上面那个request改成response就是axios响应拦截器，事实上，这俩东西都是为了简化代码操作，如果统一设置了请求拦截器和响应拦截器，axios里面的then和catch就会少很多代码   
在请求拦截器返回的东西返回给then里面的result，响应拦截器返回的东西返回给catch的error
### 可选链操作符：?.
它的作用相当于.去访问更深的属性,但有时候如果一个null或者undefined用.的话会报错，所以出现了?.,如果你不确定是否是空，那就用?.
```js
//格式 下面的是模板
axios.interceptors.response.use(function(response){
    //2xx的响应码会触发该函数
    return response
},function(error){
    //超过2xx的会触犯这个函数
    return Promise.reject(error);
});//后面的function一般不写
```
### 在自己的项目里准备一个富文本编辑器
使用的插件：wangEditor插件  
步骤：
1. 引入css样式
2. 定义html结构‘
3. 引入js创建编辑器
4. 监听内容改变，保存在一个隐藏的文本域(便于后期收集)