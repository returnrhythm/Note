# Node.js
## fs模块
模块类似插件，封装了一些方法和属性  
封装了一些关于操作本机文件的方法![图 0](images/8043f51e9bf43e22effc1d2202c608b43c2efb6bca1c7e597d53ec1848247441.png)  
## path
在利用fs模块进行操作的时候，我们需要用路径，但是通过在终端里执行的时候是从根据终端的打开位置来执行的，如果没有从打开的文件那里打开可能就找不到，所以我们统一使用绝对路径进行表示，这样无论在哪里打开终端都可以找到，这里用到了path
```js
const path=require("fs")
//以后路径要利用__dirname(两个下划线)获取到组成目标文件的绝对路径，再拼接上到目标文件的相对路径
path.join(__dirname,'相对路径')
```
### 创建web服务
   ```js
   /**
    * 目标：基于 http 模块创建 Web 服务程序
    *  1.1 加载 http 模块，创建 Web 服务对象
    *  1.2 监听 request 请求事件，设置响应头和响应体
    *  1.3 配置端口号并启动 Web 服务
    *  1.4 浏览器请求（http://localhost:3000）测试
    */
   // 1.1 加载 http 模块，创建 Web 服务对象
   const http = require('http')
   const server = http.createServer()
   // 1.2 监听 request 请求事件，设置响应头和响应体
   server.on('request', (req, res) => {
     // 设置响应头-内容类型-普通文本以及中文编码格式
     res.setHeader('Content-Type', 'text/plain;charset=utf-8')
     // 设置响应体内容，结束本次请求与响应
     res.end('欢迎使用 Node.js 和 http 模块创建的 Web 服务')
   })
   // 1.3 配置端口号并启动 Web 服务
   server.listen(3000, () => {
     console.log('Web 服务启动成功了')
   })
   ```
## CommonJS标准导入和导出
```js
//导出
module.exports={
    对外属性名1:baseURL,
    ...
}
//导入
require('模块名或路径')
//内置模块,npm下载模块：写模块名，如fs、http、path
//自定义模块：模块文件路径
```
使用的时候，要先导出，然后去导入
### ECMAScript默认导出和导入
1. 导出语法：

   ```js
   export default {
     对外属性名: 模块内私有变量
   }
   ```
2. 导入语法：

   ```js
   import 变量名 from '模块名或路径'
   ```

   > 变量名的值接收的就是目标模块导出的对象，接收的是一个对象，调用其中的内容就通过.去访问就可以了


3. 注意：Node.js 默认只支持 CommonJS 标准语法，如果想要在当前项目环境下使用 ECMAScript 标准语法，请新建 package.json 文件设置 type: 'module' 来进行设置

   ```json
   { "type": "module" }
   ```
### ECMAScript命名导出和导入
1. 命名导出语法：

   ```js
   export {修饰定义语句}
   ```

2. 命名导入语法：

   ```js
   import { 同名变量 } from '模块名或路径'
   ```
```js
export const baseURL = 'http://hmajax.itheima.net'
export const getArraySum = arr => arr.reduce((sum, item) => sum += item, 0)
import {baseURL, getArraySum} from './utils.js'
//可以一下子引入多个
```
这种适合按需导入
## npm
它是软件包管理器
1. (可选)如果没有package.json,需要先初始化清单文件`npm init -y`
2. 下载软件包 `npm i 软件包名称`，下载完成后会出现package-lock.json这个用来固定软件包的格式
3. 使用软件包(无固定格式，要看软件包自己)
### npm 安装所有依赖
当package.json没有node_modules时(也就是包的源代码)，我们可以执行`npm i`来根据package.json里面的信息安装所有依赖软件包
### 全局软件包nodemon
当安装软件包的时候后面加上' -g'就代表这个包要安装在全局，称为全局软件包，一般会封装一些命令和工具，平时安装的软件包称为本地软件包，只在当前项目中使用，封装一些命令和工具  
这里我们下载了全局软件包nodemon，在要执行的文件的终端下打开，输入命令nodemon执行文件，可以在代码自动更新的时候自动执行文件
### npm run dev 或者serve
去看package.json的scripts字段，有一个属性是serve或者dev，选择对应的启动方式