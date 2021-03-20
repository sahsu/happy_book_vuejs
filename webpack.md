# Webpack

隨着 SPA（Single Page App) 單頁應用的發展，大家發現，使用的js/css/png等文件特別多。難以管理。文件夾結構很容易混亂。

而且很多時候我們希望項目可以具備： 壓縮css, 壓縮js, 正確的處理各種js/css的import, 以及相關的模板html文件. 

在最初的一段時間裏，可以說每個SPA項目發展到一定規模，就會遇到這樣的瓶頸和痛點。

爲了解決這個問題，就出現了Webpack. 

官方網站： https://webpack.js.org/   github: https://github.com/webpack/webpack, 截止2018-6-25, 關注數是 41918. 

![webpack website](./images/webpack_official_logo.png)

它是一個打包工具，可以把  js, css, node module, coffeescript, scss/less , 圖片等等都打包在一起。 這個簡直是模塊化開發SPA福音！ 所以現在幾乎所有的SPA項目，
所有的JS項目，都會用到Webpack. 

我們在前面的入門中，看到了Vuejs只需要引入一個外部的js文件就可以工作。不過在我們的實際開發中，情況複雜了很多倍。 我們都是統一使用Webpack + Vuejs的方式來做項目的。
這樣纔可以做到 “視圖”， “路由”， “component”等等的分離，以及快速的打包，部署，項目上線。

## 功能

它的功能非常強大，對各種技術都提供了支持，彷彿是一個萬能膠水，把所有的技術都結合到了一起：

### 1.對文件的支持

- 支持普通文件
- 支持代碼文件
- 支持文件轉url （支持圖片）

### 2.對JSON的支持

- 支持普通JSON
- 支持JSON5
- 支持CSON

### 3.對JS 預處理器的支持

- 支持普通javascript
- 支持Babel  ( 來使用 ES2015+ )
- 支持Traceur ( 來使用 ES2015+ )
- 支持Typescript 
- 支持Coffeescript

### 4.對模板的支持

- 支持普通HTML
- 支持Pug 模板
- 支持JADE 模板
- 支持Markdown 模板
- 支持PostHTML
- 支持Handlebars

### 5.對Style的支持

- 支持普通style
- 支持import 
- 支持LESS
- 支持SASS/SCSS
- 支持Stylus
- 支持PostCSS

### 6.對各種框架的支持

- 支持Vuejs
- 支持Angular2
- 支持Riot

## 安裝方式

```
$ npm install --save-dev webpack
```

## 使用

由於Webpack自身是支持Vuejs的，Webpack 跟 Vuejs 已經結合到很難分清誰是誰，我們就索性不區分。 知道做什麼事兒需要運行什麼命令就可以了。 

所以我們不必專門來單獨學習Webpack，瞭解Webpack的概念就可以. 

在接下來的教程中，同學會看到Webpack + Vuejs 共同開發的方法和步驟。





