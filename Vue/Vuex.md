# Vuex

[TOC]

> **vuex简单来说就是处理组件与组件的数据共享**

官网: https://vuex.vuejs.org/zh/

概念: vuex是Vue配套的公共数据管理工具,它可以把一些共享的数据,保存到vuex中,方便整个程序的任何组件直接获取或修改我们的公共数据

作用: 统一管理路由组件,让vue中的组件进行通讯,在全局最高环境下保存数据,使所有子组件都可以访问到

### 基本使用

```javascript
// 导入Vuex模块
import Vuex from 'vuex'
// Vue实例使用Vuex模块
Vue.use(Vuex)
// 实例化Vuex的store仓库
const store = new Vuex.Store({
  // 数据设置到state中 里面的数据只能读取,不能手动修改,只能通过mutations提交
  state: {    //  this.$store.state  使用方法
    count: 0
  },
  // 修改数据的方式
  mutations: {  // 在方法中使用 this.$store.commit("increment");
    increment (state) {
      state.count++
    }
  },
   // getter 类似于Vue的计算属性(当属性用)  只负责对外提供数据,不负责修改数据
  getters: {   //  模板中调用改方法{{$store.getters.cartDataCount}}  
    cartDataCount(state){
      state.totalCount ++
      return state.totalCount;
    }
  }
})

// 实例化vue
new Vue({
  // 渲染主组件
  render: h => h(App),
  // 挂载路由
  router,
  // 挂载 Vuex
  store
}).$mount('#app') // 挂载到index页面#app的div上


<!-- 模板中取值vuex -->
<input type="button" value="我是App.vue的按钮" @click="add"> 

// 点击事件调用方法
methods: {
    add() {
        // 获取$store中的数据
        console.log(this.$store.state)  
        // 调用vuex的increment的方法  commit 触发状态变更
        this.$store.commit("increment");
    }
},
```

![1539498632254](E:\CodeSettle\notes\Vue\assets\1539498632254.png)





### Vuex提交载荷遇到的坑(Vue.set)

写购物车模块时,发现页面的数据没有同步,当时找了很久,后来通过查阅文档发现没有使用Vue.set

### Vuex删除对应的数据坑点(Vue.delete)

如果直接删除对象的属性 delete =>  数据改变页面不同步

要用Vue.delete



#### Mutation 需遵守 Vue 的响应规则

既然 Vuex 的 store 中的状态是响应式的，那么当我们变更状态时，监视状态的 Vue 组件也会自动更新。这也意味着 Vuex 中的 mutation 也需要与使用 Vue 一样遵守一些注意事项：

1. 最好提前在你的 store 中初始化好所有所需属性。
2. 当需要在对象上添加新属性时，你应该

- 使用 `Vue.set(obj, 'newProp', 123)`,

### 如何在vue中自己传递参数,并获取元素的原始参数($event)

如果我们要动态的往数据里添加字段数据	

**$event 目的是获取原始的参数**

![1539498603126](E:\CodeSettle\notes\Vue\assets\1539498603126.png)

vue动态添加属性必须要设置Vue.set属性

否则vue就不会帮你设置get和set属性

![1539498540371](E:\CodeSettle\notes\Vue\assets\1539498540371.png)



### mutations 、 actions的区别

涉及改变数据的，就使用 mutations，存在业务逻辑的，就用 actions。至于将业务 逻辑放在 action 里还是 Vue 组件里完成，就需要根据实际场景拿捏了    



### modules

作用: 将 store 分割到不同模块    

每个 module 拥有自己的 state、 getters 、 mutations 、 actions，而且可以 多层嵌套。    