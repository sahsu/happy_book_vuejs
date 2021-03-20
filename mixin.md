# Mixin

Mixin是一種更好的複用代碼的模式.

我們知道 java , Object C 中的 interface , implements,  extends 等關鍵字的意義,就是爲了讓代碼可以複用、繼承.

但是這幾種方法, 都理解起來很不直觀, 給人一種拐彎抹角的感覺. 特別是像我這樣很不習慣 “設計模式”的人。 

在js, ruby等動態語言中, 我們如果要複用代碼的話,直接使用 mixin 就好了.

## Mixin 的 概念

Mixin 實際上是利用語言的特性（關鍵字），以更加簡潔易懂的方式，實現了 “設計模式”中的 “組合模式”。 可以定義一個公共的類，這個類就叫做"mixin". 
然後讓其他的類，通過“include” 這樣的語言特性，來包含mixin, 直接具備了 mixin 所具備的各種方法。

我們下面看一下在 Vuejs 中如何使用mixin 這種強大的工具。

## 建立一個Mixin文件

可以在 `src/mixin` 目錄下創建, 例如:

文件:  `src/mixin/common_hi.js`:

```
export default {
  methods: {
    hi: function(name){
      return "你好, " + name;
    }
  }
}
```

## 使用 

Mixin使用起來很簡單,在對應的 js文件, 或者 vue文件的`<script>`代碼中引用即可.

例如,新建一個vue文件:

`src/components/SayHiFromMixin.vue`, 內容如下:

```
<template>
  <div>
    {% raw %}{{{% endraw %}hi("from view")}}
  </div>
</template>

<script>
import CommonHi from '@/mixins/common_hi.js'
export default {
  mixins: [CommonHi],
  mounted() {
    alert( this.hi('from script code'))
  }
}
</script>
```

注意:

- 使用的時候, `mixins: [CommonHi]` 這裏的是中括號,表示是數組.
- 在js代碼中調用的話, 需要帶有this關鍵字,例如: `this.hi()`

路由如下:

```
import SayHiFromMixin from '@/components/SayHiFromMixin'

export default new Router({
  routes: [
    {
      path: '/say_hi_from_mixin',
      name: 'SayHiFromMixin',
      component: SayHiFromMixin
    }
  ]
} )

```

運行效果如下:

![mixin的效果](./images/vue_mixin.png)
