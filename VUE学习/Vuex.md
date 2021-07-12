## Vuex

​												——[状态管理模式](https://vuex.vuejs.org/zh/)

### 核心概念：

state:存放基本数据

getters：从基本数据里派生出来的数据

mutations：`同步`提交基本数据的方法

actions：可以包裹mutation，使之异步

modules：模块化Vuex，可使不同模块有不同的state、getters、mutations、actions、modules

### 项目结构：

Vuex 并不限制你的代码结构。但是，它规定了一些需要遵守的规则：

1. 应用层级的状态应该集中到单个 store 对象中。
2. 提交 **mutation** 是更改状态的唯一方法，并且这个过程是同步的。
3. 异步逻辑都应该封装到 **action** 里面。

只要你遵守以上规则，如何组织代码随你便。如果你的 store 文件太大，只需将 action、mutation 和 getter 分割到单独的文件。

### 基本使用

```js
//文件：store/index.js
import axios from 'axios';
import { createStore } from 'vuex'

export default createStore({
  //声明变量
  state: { //此处的数据，只能存不能更改
    "name" : 'hx',
    "age" : null,
    "info" : {}
  },
  mutations: {
  //同步更改store状态的唯一方法就是mutation,在此可以修改state的内容
    getName(state,newValue){
        state.name = newValue;
    },
    getAge(state,newValue){
        state.age = newValue;
    },
    getInfo(state,newValue){
        state.info = newValue;
    },
  },
  actions: {
  //1、mutations的值可由actions设置
  //2、actions可异步修改state的值
    setName(context,value){
      context.commit('getName',value);
    },
    getAge(state,newValue){
      //返回一个promise函数
      return new Promise((resolve,reject) => {
        axios.get('Requesturl').then(res => {
          context.commit('getAge',res);
          resolve(rse);
        }).catch(res => { reject(res); })
      })
    },
  },
  modules: {}//
})

```

#### 如何在其它页面获取状态（获取store的值）

1. 使用computed计算属性，将数据全部写在computed里面，通过`this.$store.state.属性名` 获取

   ```js
   computed:{
       name(){
           return	this.$store.state.name;
       }
   }
   //或者在setup()中
   let name = computed(() => {return $store.state.name} );
   ```

2. 使用mapState：当一个组件需要获取多个状态时，将这些状态都声明为computed计算属性会有些重复。

   ```js
   computed:mapState({
       name:(state) => state.name
   })
   ```

3. mapState函数返回的是一个对象，通常我们需要使用一个工具函数将多个对象合并为一个，使我们可以将最终对象传给computed属性。另外使用对象展开运算符可以简化

   ```js
   computed:mapState(['name','age'])
   //或者简化如下
   computed:{...mapState(['name','age'])}
   ```

### 小结：

​		state是数据源，mutations同步修改数据，actions异步修改数据
