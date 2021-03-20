# 路由

路由是所有前端框架中都必須具備的元素。 它定義了對於那個URL（頁面），應該由那個文件來處理。

在Vuejs中，路由專門獨立成爲了一個項目: vue-router. 

## 基本用法

每個vue頁面,都要對應一個路由.  例如, 我們要做一個"博客列表頁", 那麼,我們需要兩個東西:

1. vue文件,例如: `src/components/books.vue`   負責展示頁面

2. 路由代碼, 讓  `/#/books` 與 上面的vue文件對應.

下面的代碼就是一個完整的路由文件:

```
import Vue from 'vue'
import Router from 'vue-router'

// 增加這一行, 作用是引入 SayHi 這個component
import SayHi from '@/components/SayHi'

Vue.use(Router)
export default new Router({
  routes: [

    // 增加下面幾行, 表示定義了  /#/say_hi 這個url
    {
      path: '/say_hi',
      name: 'SayHi',
      component: SayHi
    },
  ]
})
```

寫法是固定的. 其中的:

- `path`: 定義了 鏈接地址, 例如:   `/#/say_hi`
- `name`: 爲這個路由加個名字, 方便以後引用,例如:   `this.$router.push({name: 'SayHi'})`
- `component`: 該路由由哪個component來處理.

## 跳轉到某個路由時,帶上參數

有路由，就會有參數，我們看一下路由是如何處理參數的。

### 1. 對於普通的參數

例如:

```
routes: [
  {
    path: '/blog',
    name: 'Blog'
  },
]
```

在視圖中, 我們這樣做:

```
<router-link :to="{name: 'Blog', query:{id: 3} }">User</router-link>
```

當用戶點擊這個代碼生成的 html 頁面時，就會觸發跳轉。

在`<script/>`中,也可以這樣做:

```
this.$router.push({name: 'Blog', query: {id: 3}})
```

都會跳轉到:  `/#/blog?id=3` 這個url.

### 對於在路由中聲明的參數

例如:

```
routes: [
  {
    path: '/blog/:id',
    name: 'Blog'
  },
]
```

在視圖中, 我們這樣做:

```
<router-link :to="{name: 'Blog', params: {id: 3} }">User</router-link>
```

在script中,也可以這樣做:

```
this.$router.push({name: 'Blog', params: {id: 3}})
```

都會跳轉到:  `/#/blog/3` 這個url.

## 根據路由獲取參數

Vue的路由中,參數獲取有兩種方式:  query 和 params


### 獲取普通參數

對於 `/#/blogs?id=3` 中的參數,這樣獲取:

```
this.$route.query.id  // 返回結果3
```

### 獲取路由中定義好的參數

對於 `/#/blogs/3` 這樣的參數, 對應的路由應該是:


```
  routes: [
    {
      path: '/blogs/:id',  // 注意這裏的 :id
      ...
    },
  ]
```

這個 named path, 就可以通過下面代碼來獲取id:

```
this.$route.params.id  // 返回結果3
```



