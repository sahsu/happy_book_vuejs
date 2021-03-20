# Component 組件

組件是 Vuejs中最最重要的部分之一. 學號需要一定的時間投入。 

在 "webpack" 項目中，每一個頁面文件( .vue) 都可以認爲是一個組件.

在Vuejs 1.x中, 組件跟 視圖 是分別放到不同的文件夾下面的.

在Vuejs 2.8以後, 所有的視圖文件,都保存到 'components' 目錄下.

可見 "組件" 這個概念已經越來越重要和普及了.

## 說明

這裏說的內容, 跟 官方文檔的"[原始組件](https://cn.vuejs.org/v2/guide/single-file-components.html)" 不是一個東西.

本章的內容,僅使用於" webpack 項目"中的組件. 對應的文檔是: [單文件組件](https://cn.vuejs.org/v2/guide/single-file-components.html)

## 如何查看文檔

1. 先快速，粗糙的查看"原始組件": https://cn.vuejs.org/v2/guide/single-file-components.html

對所有的概念有所瞭解. (因爲這個原始組件的開發環境跟 webpack下項目的開發環境不太一樣, 所以很多以 webpack 作爲入門的同學(例如本書讀者) 會蒙圈)

2. 再看 "單文件組件" : https://cn.vuejs.org/v2/guide/single-file-components.html, 就可以對 webpack項目下的組件有清晰的認識.

3. 遇到問題之後,再看 "原始組件", 這裏包括很多API級別的概念和解釋.

## Component的重要作用: 重用代碼

我們可以想象一個場景: 有兩個頁面, 每個頁面的頭部都要有個LOGO圖片.

那麼我們每次都寫成原始HTML的話, 代碼會比較重複:

頁面1的代碼如下:

```
  <div class='logo'>
    <img src='http://siweitech.b0.upaiyun.com//image/569/upsz-fyfkzhs9232258.jpg'>
  </div>
  頁面1的其他代碼
```

頁面2的代碼如下:

```
  <div class='logo'>
    <img src='http://siweitech.b0.upaiyun.com//image/569/upsz-fyfkzhs9232258.jpg'>
  </div>
  頁面2的其他代碼
```

所以,我們應該把這段代碼抽取出來,成爲一個新的組件(component).

## 組件的創建

新建一個文件: `src/components/Logo.vue`

```
<template>
  <div class='logo'>
    <img src='http://siweitech.b0.upaiyun.com//image/570/siwei.me_header.png'/>
  </div>
</template>
```

這個文件定義了一個最最簡單的component.

然後,修改對應的頁面:

```
<template>
  <div >
    <my-logo>
    </my-logo>
		...
</template>

<script>
import MyLogo from '@/components/Logo'

export default {
	...
  components: {
    MyLogo
  }
}
```

注意1: 上面代碼中的`components: { MyLogo } ` , 必須是這個寫法,它等同於:

```
components: {
  MyLogo: MyLogo   // 前面的MyLogo 是template中的名字
                  // 後面的MyLogo 是import進來的代碼.
}
```

注意2: 上面代碼中定義的組件， 雖然名字叫做 `MyLogo`, 但是在 `<template/>` 中使用的時候，需要寫作 `<my-logo></my-logo>`. 


保存代碼，我們刷新一下，發現兩個頁面都發生了變化, 如下圖:

![兩個頁面都具備了logo](./images/vuejs_增加了logo_components.gif)


## 向組件中傳遞參數

如果我們希望兩個頁面, 每個頁面都有個title, 但是內容不同, 該怎麼辦? 就需要傳遞參數給Component了.

聲明組件的時候, 我們需要這樣:

修改文件: `src/components/Logo.vue`

```
<template>
  <div class='logo'>
    <h1>{{title}}</h1>
		...
  </div>
</template>

<script>
export default {
  props: ['title']    // 加上了這個聲明.
}
</script>
```

可以看到，上面的代碼中， 增加了這麼幾行代碼：  

```
export default {
  props: ['title']
}
```

上面的代碼表示， 爲該Component 增加了一個 "property(屬性)", 屬性的名字叫做 "title". 

### 組件接收字符串作爲參數

在調用的時候, 傳遞字符串:

```
<my-logo title="博客列表頁">
</my-logo>
```

就可以了.

### 組件接收變量作爲參數

如果要傳遞的參數是一個變量, 就要這麼寫:

```
<template>
    <my-logo :title="title">
    </my-logo>
    <input type='button' @click='change_title' value='點擊修改標題'/><br/>
</template>

<script>
export default {
  data: function() {
    return {
      title: '博客列表頁',
    }
  },
  methods: {
    change_title: function(){
      this.title = '好多文章啊(標題被代碼修改過了)'
    }
  },
</script>
```

如下圖:

![傳遞變量給組件](./images/vuejs_傳遞參數給組件.gif)

## 脫離Webpack , 在原生Vuejs中創建 Component

非常簡單。 如下面代碼所示：

```
<html>
<head>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
  <div id='app'>
    <study-process></study-process>
  </div>
  <script>
    Vue.component('study-process', {
      data: function () {
        return {
          count: 0
        }
      },
      template: '<button v-on:click="count++">我學習到了第 {{ count }} 章.</button>'
    })
    var app = new Vue({
      el: '#app', 
      data: {
      }
    })
  </script>
</body>
</html>
```

該代碼首先聲明瞭一個component: 

```
    Vue.component('study-process', {
      data: function () {
        return {
          count: 0
        }
      },
      template: '<button v-on:click="count++">我學習到了第 {{ count }} 章.</button>'
    })
```

可以看出，該component 中定義了一個 `data` 代碼段， 裏面有個變量 `count`.  

然後定義一個 `template`段落即可。



