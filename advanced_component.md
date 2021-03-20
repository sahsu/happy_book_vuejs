# Component (組件) 進階

Component 是非常常見的，在我們的Web開發中，只要是生產環境的項目，就一定會有Component. 

下面就是我們的一個實際項目中的例子， 這個項目我們只做了兩個月，裏面就發展到了32個component. 如下圖所示：

![實際項目中的component](./images/components_in_real_project.png)

很多時候，我們甚至會看到 一個 component 中嵌套着另一個， 這個component再嵌套另外5個.... 

例如： 

`popup-picker` 這個component中，看起來是這樣的: 

```
<template>
  <div>
      <popup
	      class="vux-popup-picker"
	      :id="`vux-popup-picker-${uuid}`"
	      @on-hide="onPopupHide"
	      @on-show="onPopupShow">

          <picker
	          v-model="tempValue"
	          @on-change="onPickerChange"
	          :columns="columns"
	          :fixed-columns="fixedColumns"
	          :container="'#vux-popup-picker-'+uuid"
	          :column-width="columnWidth">
          </picker>
      </popup>
  </div>
</template>
<script>
import Picker from '../picker'
import Popup from '../popup'
...
</script>
```

可以看到，這個component中，還包含了另外兩個，一個是`popup`, 一個是 `picker`.  

這個時候，新人往往會眼花繚亂。 如果看到 `this.$emit` ， 就更暈了。 

所以，要做好實際項目，同學們一定要學好本章。 

## Component 命名規則

每個component 的命名，官方建議使用 小寫字母 + 橫線的形式，例如：

```
Vue.component('my-component-name', { /* ... */ })
```

這個是符合W3C的規範的。

也可以定義爲： 

```
Vue.component('MyComponentName', { /* ... */ })
```

這個時候，可以使用 `<MyComponentName/>` 來調用，也可以使用 `<my-component-name/>` 來調用。

## Prop 命名規則

同 component， 建議使用 小寫字母 + '-' 連接。

## Prop 可以有多種類型。 

下面是一個例子，可以看出，一個component的 prop 可以有多種類型， 包括： 字符串，數字，bool, 數組，和 Object. 

```
props: {
  title: "Triple Body",
  likes: 38444,
  isPublished: true,
  commentIds: [30, 54, 118, 76],
  author: {
  	name: "Liu Cixin", 
  	sex: "male"
  }
}
```

## 可以動態爲 prop 賦值

例如，這是個靜態的賦值：

```
<blog-post title="Vuejs的學習筆記"></blog-post>
```

這是個動態的賦值：

```
// 1. 在script中定義
post = {
	title: 'Triple body',
	author: {
		name: "Big Liu",
		sex: 'male'
	}
}

// 2. 在模板中使用。 
<blog-post v-bind:title="post.title + 'by' + post.author.name"></blog-post>
```

賦值的時候，只要是符合標準的類型，都可以傳入（包括String, bool, array 等）. 

## 使用Object來爲Prop賦值

假設，我們定義有：

```
post = {
	author: {
		name: "Big Liu",
		sex: 'male'
	}
}
```

那麼，下面的代碼：

```
<blog-post v-bind:author></blog-post>
```

等價於：

```
<blog-post v-bind:name="author.name" v-bind:sex="author.sex"></blog-post>
```

## 單向的數據流

當“父頁面” 引用一個“子組件”時， 如果父頁面中的變量發生了變化，那麼對應的“子組件”也會發生頁面的更新。  

反之則不行。

## Prop的驗證

Vuejs 的組件的Prop , 是可以被驗證的。 如果驗證不匹配，瀏覽器的 console就會彈出警告（warning). 這個對於我們的開發非常有利。

我們下面的代碼： 

```
Vue.component('my-component', {
	props: {
		name: String,   				 
		sexandheight: [String, Number],
		weight: Number,
		sex: {
			type: String,
			default: 'male'
		}
	}
})
```

可以看得出，

name: 必須是字符串
sexandheight: 必須是個數組。 第一個元素是String, 第二個元素是Number
weight: 必須是Number
sex: 是個String, 默認值是 'male'

支持的類型有： 

- String
- Number
- Boolean
- Array
- Object
- Date
- Function
- Symbol

## Non Prop (非Prop) 的屬性

很多時候，component的作者無法預見到應該用哪些屬性， 所以Vuejs在設計的時候，就支持讓 component接受一些沒有預先定義的 prop.  例如：

```
Vue.component('my-component', {
	props: ['title']
})

<my-component title='三體' second-title='第二冊： 黑暗森林'></my-component>
```

上面的 `title` 就是預先定義的 "Prop",  `second-title` 就是“非Prop”

我們想傳遞一個 non-pro, 非常簡單， prop 怎麼傳， non-prop 就怎麼傳。

## 對於Attribute的合併和替換

如果component中定義了一個 attribute,  例如： 

```
<template>
	<div color="red">我的最終顏色是藍色</div>
</template>
```

如果在引用了這個“子組件”的“父頁面”中，也定義了同樣的attribute, 例如：

```
<div>
	<my-component color="blue"></my-component>
</div>
```

那麼，父頁面傳遞進來的 `color="blue"` 就會替換子組件中的 `color="red"`

但是，對於 `class` 和 `style` 是例外的。 對於上面的例子， 如果attribute換成 `class`, 那麼最終component中的class的值，就是 "red blue" (發生了合併)

## 避免 子組件的attribute 被父頁面 所影響

根據上面的小結， 我們知道了 “父頁面”的值總會“替換” “子組件”中的同名attribute .

如果不希望有這樣的情況的話，我們就可以在定義 component的時候， 這樣做：

```
Vue.component('my-component', {
  inheritAttrs: false,
  // ...
})
```


