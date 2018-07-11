# vue路由跳转，传递参数，接受参数

##**编程式导航** 

```
// 字符串
this.$router.push('/home/first')

// 对象
this.$router.push({ path: '/home/first' })

// 命名的路由
this.$router.push({ name: 'home', params: { userId: wise }})
```



##使用router-link映射路由

home.vue 中引入了 header.vue 组件，其中含有导航菜单

当点击导航菜单的时候，会切换 home.vue 中 <router-view> 中的内容

这种**只需要跳转页面，不需要添加验证方法**的情况，可以使用 **<router-link>** 来实现导航的功能：

![img](https://images2015.cnblogs.com/blog/1059788/201701/1059788-20170113085510791-1278034799.png)

在编译之后，<router-link> 会被渲染为 <a> 标签， to 会被渲染为 href，当 <router-link> 被点击的时候，url 会发生相应的改变

如果使用 v-bind 指令，还可以在 to 后面接变量，配合 v-for 指令可以渲染导航菜单

 

如果对于所有 ID 各不相同的用户，都要使用 home 组件来渲染，可以在 routers.js 中添加**动态参数**：

```
{ 
    path: '/home/:id',
    component: Home
}
```

这样 "/home/user01"、"/home/user02"、"/home/user03" 等路由，都会映射到 Home 组件

然后还可以使用 $route.params.id 来获取到对应的 id



## 传递参数和接受参数

```javascript
//传递参数组件
<li class="diarylist" v-for="diarylist in diarylists" :key="diarylist.id" @click="click1(diarylist.id)">

//传递参数方法
click1 (id) {
      this.$router.push({
        path: '/diarydetail',
        query: {
          id: id
        }
      })
    }
//跳转路由方法
click () {
      this.$router.push('/uploadingdiary')
    }

//接受参数组件
created () {
    let id = this.$route.query.id
  },
```



## 传递到router-view上,通过props接受 

```javascript
<router-view v-bind:age="age"></router-view>
export default{
    name:'word',
    props:['age'],
    ...
```

