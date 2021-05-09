---
title: vue使用
date: 2021-05-04 22:51:26
tags:
  - vue
---
# 复用子组件中的 slot /插槽

## 运用场景

1. 多个页面中具有一个类似的板块（多个父组件可复用同一个子组件）。
2. 该板块在各页面中大同小异。
3. **有差别的部分可以在该板块组件（即子组件）中通过slot/插槽占位**，然后在各个页面（即父组件）中自定义插槽。

## 使用

子组件结构：

```js
<header class="header">
  // 用name属性区分两个slot，插槽1
    <slot name="left"></slot>
	// 相似的部分
    <span class="header_title">
      <span class="header_title_text">{{title}}</span>
    </span>
	// 插槽2
    <slot name="right"></slot>
  </header>
```

```javascript
<script>
	export default {
	// 接收属性
  props: {
    title: String
  }
};
</script>
```

父组件：

```javascript
<script>
  // 1. 引入子组件
  import HeaderTop from '../../components/HeaderTop/HeaderTop.vue'

  export default {
    // 2. 映射成标签
    components: {
      HeaderTop
    }
  }
</script>
```

```javascript
<!-- 头部 -->
<!-- 3. 在父组件HTML中使用子组件 -->
  // title属性向子组件传递数据
  <HeaderTop title="昌平区北七家洪福科技园">
    // 在父组件中自定义两个slot的具体内容
    // 若不定义，则两个插槽的内容为空
  	<span class="header_search" slot="left">
      <i class="iconfont icon-sousuo"></i>
		</span>
		<span class="header_login" slot="right">
      <span class="header_login_text">登录|注册</span>
		</span>
  </HeaderTop>
```

# swiper 的使用

## npm 安装

```
npm install --save swiper
```

## 引入 swiper

```javascript
<script>
import Swiper from 'swiper'
// 具体文件名参考官网或查看 node_modules ➡️ swiper 文件夹
import 'swiper/swiper-bundle.min.css'

export default {};
</script>
```

## 使用(更多详见官网)

```javascript
<div class="swiper-container">
    <div class="swiper-wrapper">
      	<!-- 第一页内容 -->
        <div class="swiper-slide">Slide 1</div>
      	<!-- 第二页内容 -->
        <div class="swiper-slide">Slide 2</div>
    </div>
    <!-- 如果需要分页器 -->
    <div class="swiper-pagination"></div>
    
    <!-- 如果需要导航按钮 -->
    <div class="swiper-button-prev"></div>
    <div class="swiper-button-next"></div>
    
    <!-- 如果需要滚动条 -->
    <div class="swiper-scrollbar"></div>
</div>
导航等组件可以放在container之外
```

# Vuex

## 元素

- view - 页面，用户可以输入操作
- actions - 各种操作对应各种调用函数
- mutations -  各种变化方式。调用函数通过`commit`的方式选取一种变化方式
- getters - 可认为是`store`的计算属性，当依赖值发生变化时重新计算并返回值
- state - 数据源。函数计算后引起`state`的变化，数据将更新体现在`view`中

## 流程

1. 引入使用`vuex`。创建`store.js`文件：

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const state = {
  // 初始化数据，count为0
  count: 0
}
const mutations = {}
const actions = {}
const getters = {
  parity(state) {
    // 当state中的count变化时，重新计算并返回新的值
    return state.count%2 === 0 ? '偶数' : '奇数'
  }
}

export default new Vuex.Store({
  state,
  mutations,
  actions,
  getters
}
```

2. view -> dispatch -> actions：用户操作触发`store`中对应的`action`调用

```javascript
<div>
  	<!-- p标签绑定了state中count这一数据 -->
    <p>clicked: {{$store.state.count}} times, count is {{parity}}</p>
    <button @click="increment">+</button>
  </div>
<script>
	methods: {
    	// 点击按钮调用increment函数
    	// 该函数通过dispatch触发一个action: increment
      increment() {
        this.$store.dispatch('increment')
      } 
  },
	computed: {
    parity() {
      return this.$store.getters.parity
    }
  }
</script>
```

3. actions -> commit -> mutations：`action`中对应的函数发出`commit`触发状态变更

```javascript
// actions中定义各种行为
const actions = {
  increment({commit}) {
    // increment行为通过commit触发INCREMENT这种变化方式
    commit('INCREMENT')
  }
}
```

```javascript
// mutations中定义各种变化方式
const mutations = {
  // 该种变化方式改变state，即数据源的状态
  // 在这里是对count增加1
  INCREMENT (state) {
    state.count++
  }
}
```

4. `state`中的数据`count`发生变化（变为1），触发`getters`中的`parity`也被调用，二者皆更新在页面中。

   

## (辅助函数)优化

当组件中有多个state/getters需要管理，可以使用辅助函数进行简化：

在组件中引入辅助函数:

```javascript
<script>
  import {mapState, mapGetters, mapActions} from 'vuex'
</script>
```

使用对象展开运算符简化映射：

```javascript
computed: {
  		// 计算属性名称与state中子节点名称相同时，只需传一个字符串数组
  		// 映射 this.count 为 store.state.count
  		// mapState()返回值：
  		// {count(){return this.$store.state['count']}}
      ...mapState(['count']),
      ...mapGetters(['parity']),
      ...mapActions(['count']),
      parity() {
        return this.$store.getters.parity
      }
    },
methods: {
  // 有多个action则传多个字符串
  ...mapActions(['increment', 'decrement', 'parityIncrement','asyncIncrement'])
}
```

当计算属性名称与state中子节点名称不同时，则传入对象参数：

```javascript
computed: {
  ...mapGetters({parity: 'parity2'})
}
methods: {
  increment() { //
    // 通知 vuex 去增加
    this.$store.dispatch('increment') // 触发 store 中的 对应 action 调用
  }
}
```

# 组件间数据传递

## props

## vuex - store

# Vue Router

## 路由的 meta 属性

### 定义

`meta`元字段是每个路由所携带的信息。

定义路由及元字段

```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { showFooter: true }
        }
      ]
    }
  ]
})
```

### 运用场景

在不同情况下隐藏或显示子组件。

通过判定路由是否具有`meta`这个属性来筛选路由：

```javascript
<div id="app">
    <router-view/>
    <Footer v-show="$route.meta.showFooter"/>
  </div>
```



## $router.push()

## $router.replace()

<router-link replace></router-link>

## $router.back()

# Mock 

## 结构

数据类型：值为数值或字符串？格式为对象还是数组形式？

类型

## 值

# transition

## 伪类 

.fade-enter-active & .fade-leave-active

# 嵌套循环中数据 undefined

# 获取数据后创建对象再显示

watch -> This.$nextTick

```
Math.abs(y)
```

```
top += li.clientHeight
```

Vue.set()