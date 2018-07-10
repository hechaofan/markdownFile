#vue的一些问题总结

##Vue不是内部或外部命令如何解决

1. 在我的电脑中搜索vue.cmd，找到它的路径 

2. 将 vue.cmd的路径加入环境变量 

3. 重启命令窗口，执行正常，新项目创建成功。

    

## vue懒加载(官方推荐)

```javascript
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)
//r就是resolve
const login = r => require.ensure([], () => r(require('@/page/login')), 'login')
//路由也是正常的写法  这种是官方推荐的写的 按模块划分懒加载 
export default new Router({
  routes: [
    {
      path: '/',
      name: 'login',
      component: login
    }
  ]
})
```



## vue关闭eslint 编码规范验证

![](https://raw.githubusercontent.com/hechaofan/imgMarkdown/master/2018-07-04_161928.png)