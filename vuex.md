# Vuex

Vuex 是 狀態管理工具. 跟React中的Redux相似，但是更加簡潔直觀。 

簡單的說, Vuex 幫我們管理 "全局變量", 供任何頁面在任何時刻使用.

跟其他語言中的“全局變量”相比， 使用Vuex 的優點是：

1. Vuex中的變量的狀態是響應式的。 當某個組件讀取這個變量時，只要Vuex中的變量發生變化，那麼對應的組件就會發生變化（類似於雙向綁定）
2. 用戶或者程序無法直接改變Vuex中的變量。必須通過Vuex提供的接口來操作. 這個接口就是通過 "commit mutation"來實現的。

Vuex 非常重要. 不管是大項目還是小項目,都有用到它的時候.  我們必須會用.

完整官方文檔:  https://vuex.vuejs.org/zh-cn/getting-started.html   

Vuex 的內容很龐大，用到了比較燒腦的設計模式（這是由於javascript語言本身不夠嚴謹和成熟決定的），所以我不打算帶同學們把源代碼和實現機理
詳細的學一遍。 大家只要可以嫺熟的使用就行了。

下面，我們以一個例子來說明如何使用。

## 正常使用的順序

假設,我們有兩個頁面:  "頁面1" 和"頁面2" , 共同使用一個變量: counter.  頁面1對 "counter" + 1 後, 頁面2的值也會發生變化.

### 1.修改`package.json`

增加 `vuex` 的依賴聲明，如下：

```
  "dependencies": {
    "vuex": "^2.3.1"
  },
```

如果不確定你的 vuex 用什麼版本,就先手動安裝一下:

```
$ npm install vuex --verbose
```

然後看安裝過來的版本號就可以了.

### 2.新建store文件

文件名： `src/vuex/store.js`

這個文件的作用,是在整個Vuejs項目中聲明: 我們要使用Vuex進行狀態管理了.

它的內容如下： 

```
import Vue from 'vue'
import Vuex from 'vuex'

// 這個就是我們後續會用到的counter 狀態．
import counter from '@/vuex/modules/counter'

Vue.use(Vuex)

const debug = process.env.NODE_ENV !== 'production'
export default new Vuex.Store({
    modules: {
			counter    			// 所有要管理的module, 都要列在這裏.
    },
    strict: debug,
    middlewares: []
})
```

在上面代碼中,大部分是雞肋代碼. 有用的代碼只有:

```
import counter from '@/vuex/modules/counter'
...
	modules: {
		counter
	}
...
```
這裏定義了所有的 vuex module.

### 3.新建vuex/module文件

文件名： `src/vuex/modules/counter.js`

內容如下：

```
import { INCREASE } from '@/vuex/mutation_types'

const state = {
  points: 10
}

const getters = {
  get_points: state => {
    return state.points
  }
}

const mutations = {
  [INCREASE](state, data){
    state.points = data
  }

}

export default {
  state,
  mutations,
  getters
}
```

上面是一個最典型的  vuex module, 它的作用就是計數.

- state: 表示狀態. 可以認爲state是一個數據庫，保存了各種數據。我們無法直接訪問裏面的數據。
- mutations: 變化。  可以認爲所有的state都是由mutation來驅動變化的。 也可以認爲它是個setter.
- getter:  取值的方法。  就是getter( 跟setter相對）

我們如果希望"拿到"某個數據，就需要調用 vuex module的`getter` 方法。
我們如果希望"更改"某個數據，就需要調用 vuex module的`mutation` 方法。


### 4.新增文件: `src/vuex/mutation_types.js`

```
export const INCREASE = 'INCREASE'
```

大家做項目的時候, 要統一把 mutation type定義在這裏. 它類似於方法列表.

這個步驟不能省略。 Vuejs官方也建議這樣寫。 好處是維護的同學可以看到 某個mutation有多少種狀態。

### 5.新增路由: `src/routers/index.js`

```
import ShowCounter1 from '@/components/ShowCounter1'
import ShowCounter2 from '@/components/ShowCounter2'

export default new Router({
  routes: [
    {
      path: '/show_counter_1',
      name: 'ShowCounter1',
      component: ShowCounter1
    },
    {
      path: '/show_counter_2',
      name: 'ShowCounter2',
      component: ShowCounter2
    }
	]
})
```

6.新增兩個頁面: `src/components/ShowCounter1.vue`
和 `src/components/ShowCounter2.vue`

這兩個頁面基本一樣.

```
<template>
  <div>
    <h1> 這個頁面是 1號頁面 </h1>
    {{points}} <br/>
    <input type='button' @click='increase' value='點擊增加1'/><br/>
    <router-link :to="{name: 'ShowCounter2'}">
          計數頁面2
    </router-link>
  </div>
</template>

<script>
import store from '@/vuex/store'
import { INCREASE } from '@/vuex/mutation_types'
export default {
  computed: {
    points() {
      return store.getters.get_points
    }
  },
  store,
  methods: {
    increase() {
      store.commit(INCREASE, store.getters.get_points + 1)
    }
  }
}
</script>
```

可以看出， 我們可以在 `<script>`中調用 vuex的module的方法。 例如：

```
increase() {
	store.commit(INCREASE, store.getters.get_points + 1)
}
```

這裏， `store.getters.get_points` 就是通過`getter`獲取到 狀態“points"的方法。

`store.commit(INCREASE, .. )` 則是 通過 `INCREASE` 這個`action` 來改變 "points"的值。

## Computed屬性

Computed 代表的是某個組件(component)的屬性, 這個屬性是算出來的。 每當計算因子發生變化時，
這個結果也要及時的重新計算。

上面的代碼中：

```
<script>
export default {
  computed: {
    points() {
      return store.getters.get_points
    }
  },
</script>
```

就是定義了一個叫做 'points' 的 'computed'屬性。 然後，我們在頁面中顯示這個”計算屬性":

```
<template>
  <div>
    {{points}}
	</div>
</template>
```

就可以把 state中的數據顯示出來， 並且會自動的更新了。

重啓服務器( `$ npm run dev` ) , 之後運行, 可以看到如下圖所示:

![vuex演示 計數器](./images/vuejs_vuex演示.gif)

## Vuex 原理圖

爲了學會本章內容，大家務必親手敲一敲代碼。

下面是Vuex的原理圖. 

![Vuex](./images/vuex.png)

可以看到： 

1.總體分成 ： Action, Mutation, State 三個概念. State由Mutation來變化
2.Vuex 通過 Action 與後端API進行交互
3.Vuex 通過 State 來渲染前端頁面。 
4.前端頁面通過觸發 Vuex的Action，來提交mutation, 達到改變 "state"的目的。

