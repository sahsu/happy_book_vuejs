# 如何閱讀官方文檔

我們在查看vue-js的文檔的時候，會發現它跟我們真正使用的項目的代碼完全不一樣。 例如，vuejs的官方文檔的講解，都是這樣：

（完全是把所有代碼都寫在了js中）

```
var Child = {
  template: '
A custom component!
'
}
new Vue({
  // ...
  components: {
    //  將只在父模板可用
    'my-component': Child
  }
})
```

而我們的實際代碼是這樣：

```
<template>
....
</template>
<script>
.....
</script>
<style>
</style>
```

原因就在於我們在實際項目中使用了 `vue-loader`, 它可以非常好的幫我們自動加載所有的內容，按照webpacke + vue的約定。

## webpack

webpack就是一種工具，可以把各種js/css/html代碼最後打包編譯到一起。

Vuejs中已經集成了這個工具， 我們在使用vue-cli的時候，就會根據命令來生成 `webpack`所要求的
文件結構，然後在打包的時候(`npm run build`) ，vuejs的源代碼就會
被webpack打包成正常的html代碼和文件目錄。

這個內容我們以後會講到。大家要知道在本書中所講的知識，都是"基於
webpack的vuejs" 。 否則光看vuejs的官方文檔，是看不懂的。

## 官方文檔地址

https://cn.vuejs.org/  (點擊右上角的 translation就可以看到入口）


