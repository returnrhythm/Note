# pinia
## 安装
npm install pinia
## 使用
main.js
```js
import { createPinia } from 'pinia'
app.use(createPinia())
```
stores里的模块
```js
import { defineStore } from 'pinia'
export const useCounterStore=defineStore('counter',()=>{
	const count=ref(0)
	const doubleCount=computed(()=>count.value*2)
	function increment(){
		count.value++;
	}
	return [count,doubleCount,increment]
})
```
需要用stores的组件内
```js
import {useCounterStore} from ''
const countStore=useCounterStore()
//通过counterStore.count使用
```