# Vuejs 渲染頁面的過程和原理

只有知道了一個頁面是如何被渲染出來的，我們纔可以更好的理解框架和調試代碼。 下面我們就來仔細看一下這個過程。

## 渲染過程1. js入口文件

最初的最初，我們要知道  `./build/webpack.base.conf.js` 這個文件，是webpack打包的主要配置文件. 一個典型的代碼如下：

```
var path = require('path')
var utils = require('./utils')
var config = require('../config')
var vueLoaderConfig = require('./vue-loader.conf')

function resolve (dir) {
  return path.join(__dirname, '..', dir)
}

module.exports = {
  entry: {
    app: './src/main.js'
  },
  output: {
    path: config.build.assetsRoot,
    filename: '[name].js',
    publicPath: process.env.NODE_ENV === 'production'
      ? config.build.assetsPublicPath
      : config.dev.assetsPublicPath
  },
  resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src')
    }
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test')]
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      }
    ]
  }
}

```

在上面的代碼中，

```
module.exports = {
  entry : {
    app: './src/main.js'   // 這裏就定義了Vuejs的入口文件
  }
}
```

很重要， 其中的 `app: './src/main.js'` 就定義了Vuejs的入口文件

知道了這個打包文件，我們就可以知道接下來的事兒了。

## 渲染過程2. 靜態的HTML頁面： index.html

因爲我們每次打開的都是 'http://localhost/#/', 實際上打開的文件是 'http://localhost/#/index.html'

所以找到`index.html` ，可以看到它的內容如下；

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>演示Vue的demo</title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```

這裏的  `<div id="app"></div>`, 就是將來會動態變化的內容。特別類似於 Rails的 layout（模板文件）. 

這個index.html 文件是最外層的 模板。 

## 渲染過程3. main.js 中的 Vue定義

我們回頭看 `src/main.js`， 它的內容如下；

```
import Vue from 'vue'
import App from './App'
import router from './router'
import VueResource from 'vue-resource';
Vue.use(VueResource);

Vue.config.productionTip = false
Vue.http.options.emulateJSON = true;

new Vue({
  el: '#app',   // 這裏, #app 對應着 <div id=app></div>
  router,
  template: '<App/>',
  components: { App }  // 這裏, App就是 ./src/App.vue
})
```

熟悉 jquery, css selector的同學可以看到， `el: '#app'` 就是 `index.html` 中的 `<div id= app>`

注意上面的 `App.vue`,  它會被 `main.js`  加載， App.vue的內容如下：

```
<template>
  <div id="app">
		<router-view class="view"></router-view>
  </div>
</template>
<script>
</script>
<style>
</style>
```

上面代碼中的 `<template/>`,  這個就是第二層的模板。 可以認爲該頁面的內容，就是在這個位置被渲染出來的。

上面的代碼中，還有個 `<div id = 'app'>...</div>`, 這個元素跟`index.html`中的是同一個元素（暫且這麼理解）

所有的 `<router-view>`中的內容，都會被自動替換。

`<script>` 中的代碼則是腳本代碼。 至此，整個過程就出來了。

## 原理

Vuejs 就是最典型的Ajax 工作方式:  只渲染部分頁面.

瀏覽器的頁面從來不會整體刷新. 所有的頁面變化都限定在 `index.html` 中的`<div id="app"></div>`代碼中，如下圖所示:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>演示Vue的demo</title>
  </head>
  <body>
    <div id="app"></div>   <!-- 都在這一行 -->
  </body>
</html>
```

所有的動作,可以靠 url來觸發, 例如:

- `/#/books_list` 對應 某個列表頁

- `/#/books/3` 對應某個詳情頁

這個技術，就是靠 vuejs 的核心組件 `vue-router` 來實現的。  

## 不使用router的技術：QQ郵箱

QQ郵箱是屬於: url無法跟某個頁面一一對應的項目.

所有頁面的跳轉,都無法根據url 來判斷

最大的特點： 無法保存頁面的狀態， 難以調試， 無法根據url直接進入某個頁面。

