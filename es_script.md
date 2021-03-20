# Vuejs 中的 ECMAScript 

對於稍微有一定編程經驗的同學，會發現我們使用的不是"原生的javascript", 而是一種新的語言。  這個語言就是 ECMAScript.  

嚴格的說， ECMAScript是Javascript的規範，Javascript是ECMA的實現。

ECMAScript出了 javascript, 還有Jscript 和 ActionScript這樣的實現（也叫方言）

ECMAScript 也簡稱叫ES。 版本比較多， 有ES 2015, ES2016, ES2017 等。  很多時候我們用ES6來泛指這三個版本。  從這裏： http://kangax.github.io/compat-table/es6/ 
可以看到，ES6 的90%的特性都已經被各大瀏覽器實現了。 阮一峯先生的《ECMAScript入門》中對ES的歷史有着非常精彩的闡述。

具體的細節我們不去深究，我們就暫且認爲ECMAScript實現了很多 普通js無法實現的功能。 同時，在Vuejs項目中大量的使用了ES的語法。 

下面是一個極簡版的ES6的入門。 大家只要可以看懂這些代碼，就可以通讀本書。

## let, var, 常量 與全局變量

聲明本地變量,使用 let 或者 var .兩者的區別是:

- var: 有可能引起 變量提升,或者 塊級作用域的問題.

- let: 就是爲了解決這兩個問題存在的.

最佳實踐: 多用let, 少用var. 遇到詭異變量問題的時候,就查查是不是var的問題.

下面是三個對比:

```
var title = '標題';   // 沒問題
let title = '標題';   // 沒問題
title = '標題';       // 這樣做會報錯.
```

記得,在 webpack下的 vuejs中, 使用任何變量,都要使用 var 或者let來聲明.

常量:

```
const TITLE='標題';
```

對於全局變量, 直接在 index.html 中聲明即可. 例如:

```
window.title = '我的博客列表'
```

## 導入代碼： import

import 用來引入導入外部代碼. 如下所示： 

```
import Vue from 'vue'
import Router from 'vue-router'
```

上面的代碼, 目的是引入 `vue` 和 `vue-router` (由於他們是在`package.json` 中定義了, 所以可以直接 `import ... from <包名>`. 否則要加上路徑）

```
import SayHi from '@/components/SayHi'
```

上面這個,在from後面,有 `@`符號, 表示是在本地文件系統中,引入文件. `@`代表 源代碼目錄,一般是 src.

在 `@`出現之前,我們在編碼的時候也會這樣寫:

```
import Swiper from '../components/swiper'
import SwiperItem from '../components/swiper-item'
import XHeader from '../components/header/x-header'
import store from '../vuex/store'
```

大量使用了 `../..` 這樣的代碼,會引起代碼的混亂. 所以推薦使用 `@` 方法.

## 方便其他代碼來使用自己: export default {..}

我們會看到,在每個 vue文件中的 `<script>`中, 都會存在這個代碼.它的作用是方便其他代碼對這個代碼來引用.

對於Vuejs 程序員,我們就記住這個寫法就好了. 

在ES6之前js沒有一個統一的模塊定義方式，流行的定義方式有AMD, CommonJS等, 這些方式都是使用一種“打補丁”的形式來實現這個功能，總給人一種怪怪的感覺。 而ES6從語言層面對定義模塊的方式進行了統一。

假設有: `lib/math.js` 文件內容如下:

```
export function sum(x, y) {
  return x + y
}
export var pi = 3.141593
```

可以看到， `lib/math.js` 文件，可以導出兩個東西，一個是 `function sum`, 一個是 `var pi`. 

我們可以定義一個新的文件， `app.js`,  文件內容如下:

```
import * as math from "lib/math"
alert("2π = " + math.sum(math.pi, math.pi))
```

可以看到，在上面的代碼中， 可以直接調用 `math.sum` 和 `math.pi`方法。 

我們再看一個例子： 新建一個文件 `other_app.js` , 內容如下:

```
import {sum, pi} from "lib/math"
alert("2π = " + sum(pi, pi))
```

可以看到， 上面的代碼中，通過 `import {sum, pi} from "lib/math"`, 可以在後面直接調用 `sum()` 和 `pi` 

而 `export default { ... } ` 則是暴露出一段沒有名字的代碼, (不像 `export function sum(a,b){ .. }`這樣有個名字(叫sum) .  )

在webpack下的Vuejs ，會自動對這些代碼進行處理。 都屬於框架內的工作。各位同學只需要按照這個規則來寫代碼，就一定沒問題。

## ES中的簡寫

我們會發現,這樣的代碼:

```
<script>
export default {
  data () {
    return { }
  }
}
</script>
```

實際上,上面的代碼是一種簡寫形式,它等同於下面的代碼:

```
<script>
export default {
  data: function() {
    return { }
  }
}
</script>
```

## 箭頭函數 =>

跟coffeescript一樣, ES 也可以通過箭頭來表示函數.

```
  .then(response => ... );
```

等同於:

```
  .then(function (response) {
    // ...
  })
```

這樣寫法的好處，是強制定義了作用域。 使用了 `=>` 之後，可以避免很多由於作用域產生的問題。 建議大家多使用。

## hash中同名的 key, value的簡寫

```
let title = 'triple body'

return {
  title: title
}
```

等同於:

```
let title = 'triple body'

return {
  title
}
```

## 分號可以省略

例如： 

```
var a = 1
var b = 2
```

等同於：

```
var a = 1;
var b = 2;
```

## 解構賦值

我們先定義好一個hash:
```
let person = {
  firstname : "steve",
  lastname : "curry",
  age : 29,
  sex : "man"
};
```

然後，我們可以這樣做定義：

```
let {firstname, lastname} = person
```

上面一行代碼，等價於:

```
let firstname = person.firstname
let lastname = person.lastname
```

我們可以這樣定義函數:

```
function greet({firstname, lastname}) {
  console.log(`hello,${firstname}.${lastname}!`);
};

greet({
  firstname: 'steve',
  lastname: 'curry'
});
```

但是不建議大家這樣使用. 有一些奇淫技巧的感覺. 另外,瀏覽器和一些第三方支持的不是太好,
我們在實際項目中,曾經遇到過與之相關的很奇葩的問題.


## 學習資料

國內比較好的中文教材，是阮一峯編寫的 <<ECMAScript 6入門>>. 書中非常詳實的闡述了相關的內容，概念和用法。 

我們也可以在網上直接查看這本書的電子版： http://es6.ruanyifeng.com
