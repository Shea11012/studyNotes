

### v-on 事件修饰符

```
事件修饰符可以支持 . 式语法
```

### v-model 双向绑定 只能运用在表单元素

### Vue 中使用 class 样式

```
<span :style="{color:'red'}"></span>
<span :class="{active:true,curr:false}"></span>
```

### v-for 遍历

```
<span v-for="(val,key,index) in users"> 循环出来的数据按照顺序放置
```

v-for 循环现在 key 属性是必须，用来标识唯一身份

### Vue watch computed  methods 区别

- watch 用来监听特定数据的变化，可以看做是 computed 和 methods 结合体
- computed 属性的结果会被缓存，主要当做属性使用
- methods 具体的操作，主要书写业务逻辑

### v-if 和 v-show

> v-if 有较高的切换性能消耗
>
> v-show 有较高的初始渲染消耗
>
> 如果元素涉及到频繁的切换，不建议使用 v-if

### Vue.filter 过滤器

```
// 过滤器调用时的格式
{{ name | 过滤器名称 }}

// 过滤器定义语法
// function 的第一个参数永远是管道符前的数据
Vue.filter('过滤器名称',function(){});
// 过滤器可以多次调用
{{ name | 过滤器名称 | 过滤器名称 }}
```

私有过滤器 `filters:{}`

### Vue.directive 自定义指令

```
自定义指令的名称，调用时前面需要加 v-
// 参数1 名称，参数2 对象
Vue.directive('focus',{})
// 钩子函数
bind 操作样式相关的东西，这步还再内存操作
inserted 操作 js 相关内容，这步已经渲染到 dom 中
```

私有指令 `directives:{}`

### Vue 生命周期

分类：

- 创建：
    -  beforeCreate  // 此时刚初始化一个空实例对象，这个对象上只有一些默认的生命周期函数和默认的事件
    - created  // data 和 methods 已经初始化完毕，初始化数据的 ajax 应该在这步发送
- 挂载：
    - beforeMount	// 模板在内存中编辑完成，还未挂载到页面中，此时已经可以操作 dom
    - mounted          //  数据也已经进行了替换，表示实例已经创建完毕
- 更新：
    - beforeUpdate  // 页面显示的数据是旧数据，data 数据是最新的
    - updated  // 此时页面数据和 data 数据都是同步
- 销毁：
    - beforeDestory
    - destoryed

![](assets/lifecycle.png)

### Vue 动画 transition

>动画6个时间点
>
>v-enter： 进入之前，元素的起始状态，动画未开始
>
>v-enter-to：
>
>v-leave：动画离开，离开的终止状态，动画已结束
>
>v-leave-to：
>
>v-enter-active：入场动画的时间段
>
>v-leave-active：离场动画时间段

#### 使用 v-on 绑定事件实现半场动画

```
每个事件绑定的第一个参数默认是 dom 对象
// 入场
@before-enter
@enter
@after-enter
// 离场
@before-leave
@leave
@after-leave
```

**v-for 渲染出来的数据实现动画 需要 trainsition-group 包裹**



### Vue 组件

##### Vue.extend 创建全局组件

```
// 创建全局组件
let template1 = Vue.extend({
    template:'<h1>Vue.extend 创建组件</h1>'
})
// 驼峰命名需要转化为 - 连接
// 第一个参数：组件名称，第二个：创建出来的组件模板对象
Vue.component('template1',template1)

// 另一种写法
// Vue 容器外创建一个 template 元素
<template id='temp1'></template>
Vue.component('temp1',{
    template:'#temp1'
})
```

##### 私有组件

`components:{}`

> 组件内的 data 必须是一个 function 且返回对象

Vue 提供了 component 标签来展示对应名称的组件

```
// is 属性可以用来指定要展示的组件名称
<component :is="tempName"></component> 
```

##### 父子组件传值与调用

> 父组件，在引用子组件时，通过 属性绑定 v-bind  把需要传递给子组件的数据，以属性绑定的形式，传递到子组件内部，给子组件使用

```
<div id='app'>
	<test :chuandi="msg"></test>
</div>
// 从父组件绑定给子组件的数据，需要先在 子组件的 props 选项数组中定义一下
components:{
    tests:{
        template:'<h1>{{chuandi}}</h1>',
        props:['chuandi'],
    }
}
```

**data 和 props 区别：**

- data
    - 子组件 data 可读可写，不是通过父组件传递过来的
- props
    - 只读，不可写
    - 从父组件传递过来的值

##### 子组件通过事件调用向父组件传值

```
// 子组件
// func 这个参数可以随意命，此参数是子组件调用父组件的参数
// show 是父组件内的方法或属性，两者的关系是引用
<test @func="show"></test>

// 此处是父组件的方法
methods:{
    show(data){
        
    }
}

<template id='temp1'>
	<input type='button' value='调用父组件的方法' @click="ziFunc">
<template>

// 在子组件内调用父组件的方法
let template = {
    template:'#temp1',
    data(){
        return {
             msg:'fsa' 
        }
    },
    methods:{
        ziFunc(){
            this.$emit('func'，this.msg);	// 如果父组件的方法允许传递参数，那么就可以传递参数,此时就可以实现子组件通过调用父组件的方法，向父组件传值
        }
    }
}
```

##### Vue $refs 获取 DOM 元素和组件引用

```
<span ref='span'></span> 	// 在普通的标签上就是 获取 dom 元素
<test ref='test'></test>	// 在组件上就是对组件的引用
```



### Vue-router

前端的路由主要依靠 hash 来实现

```
// 定义路由
let router = new VueRouter({
    // 这个选项内定义路由 URL
    routes:[
        {path:'/',component:test},
    ]
});

// 再将此 router 对象和 Vue 实例关联起来
let app = new Vue({
    router:router
})
// 或者使用
app.$mount(router);

// 作为 占位符，显示对应路由的组件
<router-view></router-view> 

// 自动生成一个 a 标签，tag 参数可以改变默认生成的标签
<router-link to="/login" tag="button"></router-link>
```

##### 路由器嵌套

```
children 属性 实现子路由，子路由的 path 不要带斜线
```

##### 命名视图

```
route:[
    {
        path:'/',
        components:{
			default:'index',
			left:'left',
			main:'main'
        }
    }
]

<router-view>默认显示 default</router-view>
<router-view name="left"></router-view>
<router-view name="main"></router-view>
```

### 组件中 style 标签 lang 属性和 scoped 属性

