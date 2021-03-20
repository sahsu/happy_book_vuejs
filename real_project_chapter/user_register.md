# 用戶的註冊和微信授權

爲了追求快速上線，項目組決定，去掉傳統項目中的“用戶註冊”， “用戶登錄”， 直接使用微信的賬戶來覈對。

1. 用戶的微信瀏覽器帶着當前微信用戶的open_id，跳轉到“後臺服務器”。 
2. “後臺服務器” 給“微信服務器” 發送請求。 獲得當前微信用戶的信息
3. “後臺服務器” 爲該用戶生成一個用戶文件
4. “後臺服務器” 告知“H5端” 已經成功註冊該用戶。 
5. “H5端” 爲該用戶展示對應的頁面。

如下圖所示:

![用戶註冊過程](/images/real_project/user_register.png)

可以看出， 主要代碼都是在 服務器端。 

## 1. 讓微信用戶打開首頁後，直接跳轉到後臺服務器

1.1 修改對應的路由文件(src/router/index.js): 

```
Vue.use(Router)

export default new Router({
  routes: [
    { 
      path: '/wait_to_shouquan',
      name: 'wait_to_shouquan',
      component: require('../views/wait_to_shouquan.vue')
    },
  ]
})
```

1.2 增加對應的vue(src/views/wait_to_shouquan.vue) :

```
<template>
  <div style="padding: 50px;">
  <h3>正在跳轉到授權界面...</h3>
  </div>
</template>

<script>
  export default {
    created () {
      window.location.href = this.$store.state.web_share + "/auth/wechat"
    },
    components: {
    }
  }
</script>
```

可以看到， 上面的代碼中， 使用到了 Vuex 來保存系統變量（後臺服務器的地址） . 

1.3 增加核心模板文件(src/main.vue) ：

```
<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
import store from './vuex/store'
import { SET_BASEINFO, GET_BASEINFO } from './vuex/mutation_types'
export default {
  store,
  name: 'app',
  data () {
    return {
      user_info: {
        open_id: this.$route.query.open_id
      }
    }
  },
  mounted () {

    // TODO 開發環境下使用，　生產環境下注釋掉
    // store.dispatch(SET_BASEINFO, {open_id: 'opFELv6YkJkMaH-xFkokTWCs5AlQ'})

    if (this.user_info.open_id) {
      store.dispatch(SET_BASEINFO, this.user_info)
    } else {
      store.dispatch(SET_BASEINFO)
      if (store.state.userInfo.open_id === undefined) {
        console.info('用戶id和open_id不存在,  跳轉到授權等待頁面')
        this.$router.push({name: 'wait_to_shouquan'})
      } else {
        console.info('已經有了BASEINFO')
      }
    }
  },
  watch: {
    '$route' (val) {
    }
  },
  methods: {
  },
  components:{
  }
}
</script>
<!-- 下方的 CSS 略過 -->
```

可以看到， 上面代碼的 `mounted()` 方法中, 會對 當前用戶的open_id 進行判斷. 如果存在，調到首頁。 如果不存在，表示該用戶是新用戶。 需要跳轉到授權等待頁面。 
對應的代碼如下所示： 

```
    if (this.user_info.open_id) {
      store.dispatch(SET_BASEINFO, this.user_info)
    } else {
      store.dispatch(SET_BASEINFO)
      if (store.state.userInfo.open_id === undefined) {
        console.info('用戶id和open_id不存在,  跳轉到授權等待頁面')
        this.$router.push({name: 'wait_to_shouquan'})
      } else {
        console.info('已經有了BASEINFO')
      }
    }
```

1.4 增加對應的Vuex代碼

目前來看， Vuex需要保存2個信息：

- 用戶的open id 
- 遠程服務器的地址，端口等常量. 

1.4.1 增加 `src/vuex/store.js` . 這個是最最核心的文件。 完整如下所示：

```
import Vue from 'vue'
import Vuex from 'vuex'
import userInfo from './modules/user_info'
import tabbar from './modules/tabbar'
import toast from './modules/toast'
import countdown from './modules/countdown'
import products from './modules/products'
import shopping_car from './modules/shopping_car'

import * as actions from './actions'
import * as getters from './getters'

Vue.use(Vuex)
Vue.config.debug = true

const debug = process.env.NODE_ENV !== 'production'

export default new Vuex.Store({
  state: {
    web_share: 'http://shopweb.siwei.me',
    h5_share: 'http://shoph5.siwei.me/?#'
  },
  actions,
  getters,
  modules: {
    products,
    shopping_car,
    userInfo,
    tabbar,
    toast,
    countdown
  },
  strict: debug,
  middlewares: debug ? [] : []
})

```

上面的代碼中， 部分代碼是在後面會陸續用到的。 我們不用過多考慮。 只需要關注下面幾行： 

```
export default new Vuex.Store({
  // 這裏定義了若干系統常量
  state: {
    web_share: 'http://shopweb.siwei.me',
    h5_share: 'http://shoph5.siwei.me/?#'
  },

  modules: {
  	// 這裏定義了 當前用戶的各種信息， 我們把它封裝成爲一個js對象
    userInfo,
  },

})
```

1.4.2 增加`vuex/modules/user_info.js` 這個文件：

```
import {
  SET_BASEINFO,
  CLEAR_BASEINFO,
  GET_BASEINFO,
  COMMEN_ROLE,
  GET_BGCOLOR,
  GET_FONTCOLOR,
  GET_BORDERCOLOR,
  GET_ACTIVECOLOR,
  EXCHANGE_ROLE
} from '../mutation_types'

const state = {
  id: undefined, //用戶id
  open_id: undefined, // 用戶open_id
  role: undefined
}

const mutations = {
  //設置用戶個人信息
  [SET_BASEINFO] (state, data) {
    try {
      state.id = data.id
      state.open_id = data.open_id
      state.role = data.role
    } catch (err) {
      console.log(err)
    }
  },
  //註銷用戶操作
  [CLEAR_BASEINFO] (state) {
    console.info('清理緩存')
    window.localStorage.clear()
  },
}

const getters = {
  [GET_BASEINFO]: state => {
    console.info('進入到了getter中了')
    let localStorage = window.localStorage
    let user_info
    if (localStorage.getItem('SLLG_BASEINFO')) {
      console.info('有數據')
      user_info = JSON.parse(localStorage.getItem('SLLG_BASEINFO'))
    } else {
      console.info('沒有數據')
    }
    return user_info
  },
  [COMMEN_ROLE]: state => {
    if (state.role === 'yonghu') {
      return true
    } else {
      return false
    }
  },
}



const actions = {
  [SET_BASEINFO] ({ commit, state }, data) {
    //保存信息
    if (data !== undefined) {
      let localStorage = window.localStorage
      localStorage.setItem('BASEINFO', JSON.stringify(data))
      commit(SET_BASEINFO, data)
    } else {
      if (localStorage.getItem('BASEINFO')) {
        data = JSON.parse(localStorage.getItem('BASEINFO'))
        commit(SET_BASEINFO, data)
      } else {
      }
    }
  }
}

export default {
  state,
  mutations,
  actions,
  getters
}
```

該文件定義了用戶的信息的各種屬性。 

1.4.3 增加 `src/vuex/mutation_types.js` 

```
export const SET_BASEINFO = 'SET_BASEINFO'
export const GET_BASEINFO = 'GET_BASEINFO'
```

上面內容定義了該對象的兩個操作。 

1.4.4 增加 `src/vuex/modules/actions.js`

```
import * as types from './mutation_types'
```

可以看到上面的內容對於 mutation_types 進行了引用。

1.4.5 增加 `src/vuex/modules/getters.js`

```
// 先放成空內容好了
```

這個文件先這樣存在，目前階段不需要有任何內容。

## 1.5 與後臺的對接

後臺的同學， 爲我們提供了一個鏈接入口：

http://shopweb.siwei.me/auth/wechat

我們讓H5頁面直接跳轉過去就可以。 這裏不需要加任何參數， 直接由後端的同學搞定接下來的事情。

## 查看效果

在微信開發者工具中，打開我們的H5 頁面， 就會看到頁面會自動跳轉. 

觀察仔細的同學可以看到有兩次跳轉：

第一次跳轉到 http://shopweb.siwei.me/auth/wechat
第二次跳轉到 https://open.weixin.qq.com/connect/oauth2/authorize...

如下圖所示：

![微信授權頁面](/images/real_project/wechat_shou_quan.png)

點擊“確認登錄”按鈕，進行授權後，就會進入到H5的首頁

## 總結

本節中，爲了做一件事： 讓用戶跳轉到微信授權，並註冊， 我們做了如下的程序層面的內容： 

- 使用Vuex 記錄系統常量(遠程服務器的地址)
- 使用Vuex 記錄用戶的信息（新增了一個對象：user_info)
- 使用了一個獨立的頁面（等待微信授權頁面） 
- 每次打開首頁之前，都要判斷該用戶是否登錄。
- (後臺任務) 讓該用戶在微信端授權， 並且在本地生成一個新的用戶， 然後把相關數據返回給前端

Vuex 是Vuejs 最複雜最不好理解的地方。 同學們不要怕。 之所以這麼麻煩， 可以認爲是javascript的語言特性決定的。 就好像java, C語言中大量用到的設計模式， 在現代編程語言(Ruby , Python, Perl)中就用不到， 可以有更加簡單的辦法， 例如Mixin)

另外，本節對於後端的同學是個挑戰， 不但要在微信端做修改，還需要對微信返回的數據結構很熟悉。 由於本書內容所限，不再贅述。 