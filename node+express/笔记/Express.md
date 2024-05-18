# Express
### 是什么
封装了node.js，可以很轻松写出接口
### 如何开始？
创建一个文件，之后运行npm init，然后npm install express就可以了，node-dev js文件 每次更新都自动启动服务器
### 最简单的示例
```js
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```
### 路由
#### 路由路径
?就是可选，+就是可重复，:id(也可以不是id哈如/ab/:id 代表匹配到/ab/xxxxxxx的路径
### 中间件(回调函数)
我们可以写多个中间件，控制下一个中间件的是next()，send后会强制结束
```js
app.get('/', (req, res,next) => {
  next()
  res.send('Hello World!')
},function(req,res,next){
	....
})
-------------------------------------
app.get('/',[func1,func2])//这样写也可以
//如果我想让中间件通信的话，可以给res设置属性，res.psd=1111，这样也可以访问到
```
#### 中间件的分类
##### 应用中间件
app.use(xxx)   
挂载到app上的都叫应用中间件，app.xxxx，   
应用：当很多个接口都要执行一个中间件的时候，我们把这个中间件提取出来，在他们之前使用app.use(中间件)，这样就可以让处于app.use()之后的接口执行之前全部执行一遍这个中间件   
注意：注意一下app.use()的位置，因为一旦写上了，后面所有的都要执行它    
补充：其实前面也可以加路径，但是这样为什么不直接写在具体的接口里面呢？
##### 路由中间件
app.use('/',路由模块)   
挂载到路由上面的都叫路由中间件
```js
//根的js
const IndexRouter = require('路径')
app.use('控制一级路由',IndexRouter)
//路由文件
const express = require('express')
const router = express.Router()
router.get()//就和以前写的一样
module.exports=router
```
##### 错误中间件
一般写在最后
```js
app.use((err, req, res, next) => {
  console.error(err.stack)
  res.status(404).send('Something broke!')//提交状态码
})
```
### 获取静态资源
#### get请求
req.query  
适合这种http://localhost:3000/form?num=8888
#### post请求
req.body  
但注意需要在请求之前配置中间件
```js
app.use(express.urlencoded({extended:false}))//能够接收key=value&key=value
app.use(express.json())//能够接收{name:"",age:100}这种
```
#### url获取参数
req.params  
适合这种http://localhost:3000/form/num 想获取num
### Express托管静态文件
尽量写前面
```js
app.use(express.static('public'))//填写放置静态资源的根目录
```
但是这样url后面会有后缀比如html等等
### 服务端渲染和前后端分离
服务端渲染：前端把静态页面返回后端，后端利用数据组装页面   
前后端分离：前端调用后端给的接口返回数据