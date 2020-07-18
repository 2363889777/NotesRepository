# vue-router的基本使用

## 基本使用步骤

### 1.引入相关的库文件

~~~java
<!-- 导入vue文件，为全局 Windows 对象挂载 Vue 构造函数 -->
<script src="vue.js"></script>

<!-- 导入vue-router文件，为全局 Windows 对象挂载 VueRouter 构造函数 -->
<script src="vue-router.js"></script>
~~~

### 2.添加路由链接

~~~java
<!-- router-link 是vue中提供的标签，默认会被渲染为 a 标签 -->
<!-- to 属性默认会被渲染为 href 属性 -->
<!-- to 属性的值默认会被渲染为 # 开头的 hash 地址 -->
<router-link to="/user">User</router-link>
<router-link to="/register">Register</router-link>
~~~

### 3.添加路由填充位

~~~java
<!-- 路由填充位(也叫做路由占位符) -->
<!-- 将来通过路由规则匹配到的组件，将会被渲染到 router-view 所在的位置 -->
<router-view></router-view>
~~~

### 4.定义路由组件

~~~java
var User = {
    template:'<div>User</div>'
}
var Register = {
    template:'<div>Register</div>'
}
~~~

### 5.配置路由规则并创建路由实例

~~~java
// 创建路由实例对象
var router = new VueRouter({
    // routes 是路由规则数组
    routes: {
        // 每个路由规则都是一个配置对象，其中至少包含 path 和 component 两个属性：
        // path 表示当前路由规则匹配的 hash 地址
        // component 表示当前路由规则对应要展示的组件
        {path:'/user',component:User},
		{path:'/register',component:Register},
        // 使用路由重定向
        // 其中 path 表示需要被重定向的原地址，redirect 表示将要被重定向到的新地址
        {path:'/',redirect:'/user'}
    }
})
~~~

### 6.把路由挂载到 Vue 根实例中

~~~java
new Vue({
    el: '#app',
    // 为了能够让路由规则生效，必须把路由对象挂载到 vue 实例对象上
    // router = router:router
    router
})
~~~



