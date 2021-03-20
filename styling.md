# 使用樣式

樣式用起來特別簡單.  直接寫到 `<style>` 段落裏面即可. 如下代碼所示：

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

用瀏覽器打開上述代碼，就可以看到 一個紅色的，字體大小爲20px 的 "Hi Vue!". 如下圖所示：

![帶有style的demo](./images/vue_style.png)

## 使用全局

```
<style >
td {
  border-bottom: 1px solid grey;
}
</style>
```

## 使用局部的css

```
<style scoped>
td {
  border-bottom: 1px solid grey;
}
</style>
```
這段CSS只對當前的 component 適用. 

也就是說，當我們有兩個不同的頁面： page1, page2, 如果兩個頁面中都定義了某個樣式（例如上面的 `td`）的話，是不會互相沖突的。 

因爲Vuejs 會這樣解析： 

```
page1 的DOM： 
<div data-v-7cfd41e ... ></div>

page2 的DOM: 
<div data-v-3389dfw ... ></div>
```

而我們使用的 "scoped style" ，就可以存在於不同的頁面（component)上了！

