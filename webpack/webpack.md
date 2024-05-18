## Webpack
### 打包流程
`npx webpack`
拿到import的东西，分析执行逻辑
### 配置项
新建webpack.config.js文件，在里面进行配置
```js
module.exports={
	//mode:"production/development"（生产环境/开发环境）
	//entry:"入口文件路径"
	//output:{
		//filename:'打包后的文件名字'
		//path:"打包后的文件路径"
	}
}
```