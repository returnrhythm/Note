# vue-router
## 安装
`npm install vue-router@4`
## 跳转方法
1. `<router-link to=''></router-link>`
2. `router.push('')`
## 示例
index.js
```js
import { createRouter, createWebHistory } from 'vue-router'
const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
        path:'/',
        component: Home,
        children:[
            {
                path:'',
                component:First
            },
            {
                path:'person',
                component:Person,
                children:[{
                         path:'',
                         component:User
                         },
                         {
                         path:'login',
                         component:Login
                          },
                          {
                        path:'user',
                        component:User,
                        children:[{
                                   path:'',
                                   component:userPage
                                  },
                                  {
                                  path:'myHomework',
                                  component:myHomework
                                  },
                                  {
                                  path:'userManagement',
                                  component:userManagement
                                  },
                                  {
                                    path:'userPage',
                                    component:userPage
                                  },
                                  {
                                    path:'rolePage',
                                    component:roleManage
                                  },
                                  {
                                    path:'courseSearch',
                                    component:courseSearch
                                  }
                        ]
                          } 
            ]
            },
            {
                path:'data',
                component:Data,
                children:[
                    {
                        path:'GPA',
                        component:DataGPA,
                        children:[
                        {
                            path:'',
                            component:DataGPACourse
                        },
                        {
                            path:'course',
                            component:DataGPACourse
                        },
                        {
                            path:'method',
                            component:DataGPAMethod
                        },
                        {
                            path:'major',
                            component:DataGPAMajor
                        }]
                    },
                    {
                        path:'stuGPA',
                        component:DataStuGPA
                    },
                    {
                        path:'GPAtrend',
                        component:DataGPATrend
                    },
                    {
                        path:'stuquality',
                        component:DataStuQuality
                    },
                    {
                        path:'stumsg',
                        component:DataStuMsg
                    },
                    {
                        path:'tchstu',
                        component:DataTchStu
                    },
                    {
                        path:'workrate',
                        component:DataWorkrate
                    },
                    {
                        path:'workplace',
                        component:DataWorkplace
                    },
                    {
                        path:'mysql',
                        component:Mysql
                    },
                    {
                        path:'clickhouse',
                        component:Clickhouse
                    },
                    {
                        path:'',
                        component:Default
                    }
                ]
            }
        ]
    },
  ]
})
export default router
```
main.js
```js
import router from './router/index.js'
app.use(router)//全局挂载使用
```