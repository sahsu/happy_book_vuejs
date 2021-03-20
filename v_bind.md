# 雙向綁定

雙向綁定這個概念現在越來越普及。 

在Angular出現的時候,就作爲宣傳的王牌概念. 現在幾乎是個js前端框架,就有這個功能. 它的概念是:

某個變量，定義於 `<script/>`, 需要展現在 `<template/>`中的話:

1. 如果在代碼層面進行修改, 那麼頁面的值就會發生變化
2. 如果在頁面進行修改(例如在input標籤中), 那麼代碼的值就會發生變化.

## 一個演示例子.

在我們的項目中,增加一個 vue頁面:  `src/components/TwoWayBinding.vue`

```
<template>
  <div>
    <!-- 顯示　this.my_value 這個變量 -->
    <p>頁面上的值:  {{my_value}} </p>

    <p> 通過視圖層，修改my_value: </p>
    <input v-model="my_value" style='width: 400px'/>

    <hr/>
    <input type='button' @click="change_my_value_by_code()" value='通過控制代碼修改my_value'/>
    <hr/>
    <input type='button' @click="show_my_value()" value='顯示代碼中的my_value'/>
  </div>
</template>

<script>
export default {
  data () {
    return {
      my_value: '默認值',
    }
  },
  methods: {
    show_my_value: function(){
      alert('my_value: ' + this.my_value);
    },
    change_my_value_by_code: function(){
      this.my_value += ",  在代碼中做修改, 666."
    }
  }
}
</script>

```

上面的代碼中， 顯示定義了一個 變量 "my_value"， 這個變量可以在 `<script/>`中訪問和修改，也可以在 `<template/>`中訪問和修改。 

- 在代碼(`<script/>`) 中訪問的話，就是 `this.my_value`
- 在視圖(`<template/>`)中訪問的話，就是 `<input v-model=my_value />`

所以，這個就是雙向綁定的方法。

接下來,修改路由文件:  `src/router/index.js`:

```
import TwoWayBinding from '@/components/TwoWayBinding'

export default new Router({
  routes: [
    {
      path: '/two_way_binding',
      name: 'TwoWayBinding',
      component: TwoWayBinding
    }
  ]
})
```

然後, 就可以用瀏覽器訪問路徑:　`http://localhost:8080/#/two_way_binding`

效果１：通過頁面，修改js代碼的值，可以看到，代碼中的`my_value`一邊， 視圖中的`my_value` 就發生變化。 

如下圖所示： 

![雙向綁定的效果: 頁面修改代碼](./images/vuejs_雙向綁定_頁面的修改影響代碼中的變量.gif )

效果２：通過代碼層面的改動，影響頁面的值. 

如下圖所示：

![通過代碼層面的改動，影響頁面的值](./images/vuejs_雙向綁定_代碼層面的修改，影響頁面的值.gif)

所以，這個特性是Vuejs自帶的。我們不需要刻意學它，只需要知道它可以達到這個目的，具備這個特性，就可以了。

以後我們會發現，Vuejs 等前端框架中，這種思想和現象特別常用。
