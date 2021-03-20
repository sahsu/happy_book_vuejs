# 與CSS預處理器結合使用

20年前的《程序員修煉之道》這本書，就提到了程序員的一個職業習慣： DIY。 (Don't Repeat Yourself), 不要做重複的事兒。

目前的編程語言，幾乎都具備了消滅重複代碼的能力。

除了CSS

CSS 是唯一不具備 支持變量的 編程語言。 因爲CSS 本身只是一個DSL（Domain Specific Language領域特定的語言）, 它不是一個“編程語言”。

這樣就決定了它的特點 ： 上手極快， 可以很好的表現HTML中某個元素的外觀。 缺點就是： 無法通過常見的重構手法（Extract Method, Extract Variable.. ) 來精簡代碼。

所以， SCSS, SASS, LESS 等一系列的 "CSS 預處理器" (precompiler) 應運而生。 

## SCSS 

全名叫 Sassy CSS, （時髦的CSS）  是 SASS 3 引入新的語法，其語法完全兼容 CSS3，並且繼承了 SASS 的強大功能。 

也就是說，任何標準的 CSS3 樣式表都是具有相同語義的有效的 SCSS 文件。

官方網站同 SASS。 

由於 SCSS 是 CSS 的擴展，因此，所有在 CSS 中正常工作的代碼也能在 SCSS 中正常工作。 也就是說，對於一個 SASS 用戶，只需要理解 SASS 擴展部分如何工作的，就能完全理解 SCSS。 

大部分的用法都跟SASS相同。 唯一不同的是，SCSS 需要使用分號和花括號而不是換行和縮進。 

SCSS可以說是全面取代了 SASS。

我們看下面的例子： 

```
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```
在上面的代碼中， 定義了兩個變量： `$font-stack` 和 `$primary-color` . 編譯後的CSS如下：

```
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}
```

更多內容，可以到官方網站來學習： https://sass-lang.com/guide 非常簡單，有一點兒CSS功底的人，往往可以瞬間上手。

## LESS 

LESS 也是一種 CSS 預處理器，它的自我介紹是： 只是多了"一丟丟"內容的CSS. ( It's CSS, with just a little more). 

官方網址： http://lesscss.org/ , Github: https://github.com/less/less.js  ， 截止到 2018-6-25 , github關注數是 15591 

它的作用跟SCSS一樣，也是爲了讓代碼更加精簡，去掉無意義的重複。 我們看下面的例子：

```
// Variables
@link-color:        #428bca; // sea blue
@link-color-hover:  darken(@link-color, 10%);

// Usage
a,
.link {
  color: @link-color;
}
a:hover {
  color: @link-color-hover;
}
.widget {
  color: #fff;
  background: @link-color;
}
```

可以看到，上面的例子定義了 兩個變量：  `@link-color` 和 `@link-color-hover`. 並且在下方進行了引用。 同時， 還有使用了換算功能 `darken(@link-color, 10%)`。 

上面的代碼會被編譯成下面的CSS： 

```
a,
.link {
  color: #428bca;
}
a:hover {
  color: #3071a9;   
}
.widget {
  color: #fff;
  background: #428bca;
}
```

可以看到， less的功能非常強大。 

## SASS

提到了SCSS, LESS， 就不得不提及SASS

官方網站： https://sass-lang.com/   , github: https://github.com/sass/sass , 截止到 2016-6-25, github關注數是 11376

特點是去掉了 花括號，分號，看起來特別簡單。 使用空格來標記不同的段落層次。 跟HAML基本是一樣的。 

我們看下面的例子： 

```
$font-stack:    Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

在上面的代碼中， 定義了兩個變量： `$font-stack` 和 `$primary-color` .  並且在下面對它們進行了引用。 

我們看編譯後的結果：

```
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}
```

不過在實際應用當中，卻很少使用這個語言。 因爲實際應用中，程序員最喜歡的事情，就是把“UI” 或者“美工” ， 或者“前端工程師”給過來的CSS文件直接使用。 如果使用SASS的話就很尷尬，
還要再動作做一遍轉換。 浪費時間。 而且“美工”同學可以看得懂CSS，卻無法看懂SASS。 

所以這個技術比較落沒。 慢慢的被SCSS (SASS 3.0) 所取代。

所以，同學在這個技術的故事上要學習到一個道理： 程序員認爲好的技術，在團隊配合的過程和實踐中是不一定適用的。 SASS 團隊也做的非常不做，立刻就在SASS 3.0的時候推出了SCSS。


## 在 Vuejs 中使用CSS預編譯器

使用的前提，是我們以 Webpack的形式使用Vuejs. 

使用方法非常簡單。 我們以SASS爲例： 

1.安裝依賴: "sass-loader" 和 "node-sass", 運行下面命令：

```
$ npm i sass-loader node-sass -D
```

2.在"webpack.base.conf.js"中添加相關配置：

```
{
    test: /\.s[a|c]ss$/,
    loader: 'style!css!sass'
}
```

3.在對應的 ".vue" 文件中，我們就可以這樣的定義某個樣式： 

```
<style lang='sass'>
td {
  border-bottom: 1px solid grey;
}
</style>
```

上面的代碼，會在運行的時候，被webpack編譯成對應的CSS文件。 

