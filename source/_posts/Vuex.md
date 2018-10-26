---
title: Vuex 状态管理插件学习 #文章页面上的显示名称，一般是中文
date: 2018-03-12 16:16:16 #文章生成时间，一般不改，当然也可以任意修改
categories: JavaScript #分类
tags: [vue, vuex, JS] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: Vuex 学习 #附加一段文章摘要，字数最好在 140 字以内，会出现在 meta 的 description 里面
author: 'Mark'
---

# Vue 状态管理插件学习

- vuex vue 提供的数据状态管理插件（俗称数据共享中心）
- state（数据商店也就是数据仓库）,mutations（定义更改数据的方法）

- 获取仓库中定义值的方法

```javascript
// {{$store.state.定义的属性}}
// 使用计算属性
computed:{
	count(){
		return this.$store.state.定义的属性
	}
}
```

- 3.使用 vuex 中的 mapState，也就是 vuex 中提供给我们的方法

```javascript
//es6写法
computed: mapState({
	count: state => state.count
})
```

- 等同于

```javascript
computed: mapState({
	count: state => {
		return state.count
	}
})
```

- 4.mapState 扩展使用

```javascript
computed: mapState(['在state中定义的属性'])
// 这个会根据你定义的属性名绑定到vue实例上
```

- 5.mutations 提交更改仓库中定义值的方法（修改状态）
- 使用$store.commit('调用定义在 mutations 中定义的方法名'，要传递给调用方法的参数)
- 获取状态管理器中定义的方法(mutations)

```javascript
const mutations = {
	// 定义一个加的方法
	add(state) {
		state.count++
	},
	// 定义一个减的方法
	reduce(state) {
		state.count--
	}
}
// 调用方法
// 在vue中使用import导入辅助函数
import { mapState, mapMutations } from 'vuex'

methods: mapMutations(['add', 'reduce'])
// 或
methods: mapMutations([(countAdd: 'add'), (countReauce: 'reduce')])
```

- 6.vuex 中的计算属性（过滤属性）getters

```javascript
// 定义方法
const getters = {
  count:function(state){
    return state.count += 100;
	}
	// 或者
	count: state => { return state.count += 100 }
}

// 调用方法
import { mapState,mapMutations,mapGetters } from 'vuex';

computed: mapGetters({
	count: (state) => { return state.count }
})
```

- 7.vuex 中的 actions，异步提交方式

```javascript
const actions = {
	// context：上下文对象，这里你可以理解称store本身。
	addAction(context) {
		context.commit('add', 10)
	},
	// {commit}：直接把commit对象传递过来，可以让方法体逻辑和代码更清晰明了。
	reduceAction({ commit }) {
		commit('reduce')
	}
}
```

- 8.module 模块组

```javascript
// 定义模块，和定义一个store实例一样只不过把封装store的全部方法和属性，又封装在了一个模块中
const moduleA={
  state,
  mutations,
  getters,
  actions
}
// 调用方法
modules: {
  //模块别名:模块名，记得要使用import引入模块
	a:moduleA
}

//使用模块值和方法
和以上的使用方法一样，只不过前边加一个模块别名
```
