# Vue组件

[TOC]

### 作用

**组件是可复用的 Vue 实例** 

- 组件的出现,就是为了拆分Vue实例的代码量,能够让我们以不同的组件,来划分不同的功能模块,我们需要声明功能,就调用对应的组件即可

### 组件化和模块化的区别

- 组件化(前端): 从UI界面角度进行划分的 ,方便UI组件的重用
- 模块化: 从代码逻辑角度进行划分;方便代码分层开发,保证每个功能模块的职能单一



### 基本使用(组件切换)

```javascript
<body>
    <div id="app">
        <a href="#" @click.parent="flag=true">登录</a>
        <a href="#" @click.parent="flag=false">注册</a>
        <login-component v-if="flag"></login-component>
        <register-component v-else="flag"></register-component>
    </div>

    <script>
        // 组件
        Vue.component('login-component', {
            template: '<h3>登录组件</h3>'
        })
        Vue.component('register-component', {
            template: '<h3>注册组件</h3>'
        })

        let vm = new Vue({
            el: "#app",
            data: {
                flag: false
            }
        })
    </script>
</body>
```

### 父组件与子组件之间传递数据的方式

1. 父组件=>子组件 通过v-bind绑定传递数据 
2. 子组件=>父组件 通过 1. 通过v-on是事件绑定机制 $emit触发方发

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <!-- 导入vue -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>

<body>
    <div id="app">
        <!-- 组件传值的属性不能使用大写 -->
        <son :parentmsg="msg" @func='getson'></son>
        <input type="text" v-model="msg">
        <br>
        <input type="text" v-model="sonmsg">
    </div>
    <!-- 组件模板 -->
    <template id="son">
        <div>
            <h1>我是子组件 ---- {{parentmsg}}</h1>
            <input type="button" value="我是子组件的按钮,点击我传递数据给父组件" @click='sendMsg'>
        </div>
    </template>
    <script>
        // 子组件
        const son = {
            template: '#son',
            // 父组件=>子组件 通过v-bind绑定传递数据
            props: ['parentmsg'],
            data(){
                return{
                    son: '我是儿子,给你传个话'
                }
            },
            methods:{
                sendMsg(){
                    // 子组件=>父组件 通过 1. 通过v-on是事件绑定机制 $emit触发方法
                    this.$emit('func',this.son)
                }
            }
        }

        let vm = new Vue({
            el: "#app",
            data: {
               msg: "我是父组件的值",
               sonmsg: null
            },
            // 子组件
            components: {
                son
            },
            methods: {
                // 获取子组件的值
                getson(data){
                    this.sonmsg = data;
                }
            }
        })
    </script>
</body>
</html>
```

### 组件里的data为什么要返回一个对象

- 原因:
  - 如果你直接返回对象, 如 return {count: 1};  因为当前的对象是引用类型,如果使用多个同样的组件,数据会共享
  - 如果你直接返回对象的对象 如 return { {count: 1} }, 那么当前会开辟一个单独的内存空间,数据就不会共享了

