# 1. vuex是什么
    一个vue插件库
    作用: 对vue应用中组件的状态进行集中式的管理

# 2. vuex的核心概念
## 1). state
    vuex管理的状态对象
    它应该是唯一的
    const state = {
      xxx: initValue
    }

## 2). mutations
    包含多个直接更新state的方法(回调函数)的对象
    谁来触发: action中的commit('mutation名称')
    只能包含同步的代码, 不能写异步代码
    const mutations = {
      yyy (state, {data1}) {
        // 更新state的某个属性
      }
    }
## 3). actions
    包含多个事件回调函数的对象
    通过执行: commit()来触发mutation的调用, 间接更新state
    谁来触发: 组件中: $store.dispatch('action名称', data1)  // 'zzz'
    可以包含异步代码(定时器, ajax)
    const actions = {
      zzz ({commit, state}, data1) {
        commit('yyy', {data1})
      }
    }
    - {commit,state} 怎么来的？
    - store的action函数会接收一个对象，action函数可以接收一个与store实例具有相同方法的属性context，这个属性中包括下面几部分：
      ```
       context:{
		state,   等同于store.$state，若在模块中则为局部状态
		rootState,   等同于store.$state,只存在模块中
		commit,   等同于store.$commit
		dispatch,   等同于store.$dispatch
		getters   等同于store.$getters
        }
      ```

## 4). getters
    包含多个计算属性(getter)的对象
    谁来读取: 组件中: $store.getters.xxx
    const getters = {
      mmm (state) {
        return ...
      }
    }

## 5). modules
    包含多个module的对象
    一个module是一个包含state/mutations/actions/getters的对象
    是将一复杂应用的vuex代码进行多模块拆分的第2种方式

## 6). store
    vuex的核心管理对象, 是组件与vuex通信的中间人
    读取数据的属性
        state: 包含最新状态数据的对象
        getters: 包含getter计算属性的对象
    更新数据的方法
        dispatch(): 分发调用action
        commit(): 提交调用mutation

# 3. 使用vuex
## 0). vuex编码
    创建所有相关的模块： vuex/store|state|mutations|actions|getters|mutation-types
    设计state：从后台获取的数据
    实现actions：
        定义异步actions：async/await
        流程：发ajax获取数据，commit给mutation
    实现mutations：定义直接更新state数据的函数
    实现store.js: 创建store对象
    配置store：在入口文件mian.js导入store.js,配置store（使所有组件都可以看到 $store）
    组件中：
        分发：this.$store.dispatch('actionName', data)
        提交：this.$store.commit('mutationName', data)
        读取：mapState() / mapGetters()
## 1). 创建并向外暴露store对象
    export default new Vuex.Store({
      state,
      mutations,
      actions,
      getters,
      modules: {
        a,
        b
      }
    })

## 2). 注册store: main.js
    import store from './store'
    new Vue({
      store
    })


## 3). 组件中通过store与vuex通信
    import {mapState, mapGetters} from 'vuex'
    export default {
      computed: (
        ...mapState(['xxx']),  //等同于{xxx:function() { return 'xxx'}}
        ...mapGetters(['yyy'])
      )
      methods: {
            test () {
                this.$store.dispatch('zzz', data)
                this.$store.commit('zzz', data)
            }
      }
    }

# 4. Vuex结构图
![](D:\work\190719\video\2019-12-10\vuex结构图.png)

