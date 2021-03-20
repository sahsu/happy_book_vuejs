# 創建一個頁面

激動人心的時刻到來了！ 接下來我們要通過自己動手，開始下一步的學習！

請各位同學務必準備好電腦。 只有一邊學習一邊敲代碼，才能真正的看到效果。 調試代碼的過程是無法腦補出來的。切記切記！

在Vuejs中創建頁面需要兩步：

1. 新建路由
2. 新建vue頁面

## 新建路由

默認的路由文件是 `src/router/index.js`, 打開之後, 我們增加兩行:

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

上面的代碼中， 

```
import SayHi from '@/components/SayHi'
```

表示從 當前目錄下的 `components` ，引入 `SayHi` 文件。  `@` 表示當前目錄。

然後，用下面的代碼來定義一個路由：

```
routes: [
    {
      path: '/say_hi',   // 對應 一個url 
      name: 'SayHi',     // Vuejs 內部使用的名稱
      component: SayHi   // 對應的 .vue 頁面的名字
    },
]
```

也就說，每當用戶的瀏覽器訪問  `http://localhost:8080/#/say_hi` 時， 就會渲染 `./components/SayHi.vue` 文件。

`name: 'SayHi' ` 定義了該路由在Vuejs內部的名稱。 後面會對此有所解釋。



## 創建一個新的Component

由於上面我們在路由中引用了component： `src/components/SayHi.vue`, 接下來，我們要創建這個文件.

代碼如下:

```
<template>
  <div >
    Hi Vue!
  </div>
</template>

<script>
export default {
  data () {
    return { }
  }
}
</script>

<style>
</style>

```

在上面的代碼中： 

- `<template></template>` 代碼塊中,表示的是 HTML模板.  裏面寫的就是最普通的HTML 。 

- `<script/>`表示的是 js 代碼. 所有的js代碼都寫在這裏.  這裏使用的是 EM Script.

- `<style>`表示所有的 CSS/SCSS/SASS 文件都可以寫在這裏.

現在,我們可以直接訪問  http://localhost:8080/#/say_hi 這個鏈接了. 打開後，頁面如下圖所示：

![say_hi圖片](./images/vue_doc_say_hi_page.png)

## 爲頁面增加一些樣式

接下來，我們可以爲頁面加一些樣式， 讓它變得好看一些:

```
<template>
  <div class='hi'>
    Hi Vue!
  </div>
</template>

<script>
export default {
  data () {
    return { }
  }
}
</script>

<style>
.hi {
  color: red;
  font-size: 20px;
}
</style>
```

注意上面代碼中的　`<style>`標籤，裏面跟普通的CSS一樣, 定義了樣式。

```
.hi {
  color: red;
  font-size: 20px;
}
```

刷新瀏覽器，可以看到, 文字加了顏色, 如下圖所示:

![加了顏色的文字](./images/say_hi_with_style.png)

## 定義並顯示變量

如果要在vue頁面中定義一個變量,並把它顯示出來,就需要事先在 `data` 中定義:

```
export default {
  data () {
    return {
      message: '你好Vue! 本消息來自於變量'
    }
  }
}
```

可以看到，上面的代碼是通過 Webpack的項目來寫的。 回憶一下， 原始的Vuejs中是如何定義一個變量(property) 的？

答案是：
```
var app = new Vue({
  data () {
    return {
      message: '你好Vue! 本消息來自於變量'
    }
  }
})
```

所以，我們可以認爲， 之前用“原始的Vuejs”的代碼中，寫的 `new Vue({...})`中的代碼，在webpack的框架下，都要寫到 `export default { .. }` 中來。

完整的代碼（`src/components/SayHi.vue`）如下所示： 

```
<template>
  <div>
    <!--  步驟2: 在這裏顯示 message -->
    {{message}}
  </div>
</template>

<script>
export default {
  data () {
    return {
      // 步驟1: 這裏定義了變量 message 的初始值
      message: '你好Vue! 本消息來自於變量'
    }
  }
}
</script>

<style>
</style>
```

頁面打開如下圖所示:

![來自於變量的消息](./images/vue_page_show_variable.png)
