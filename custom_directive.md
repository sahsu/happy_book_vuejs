# 自定義 Directive

Vuejs除了自身提供的 `v-if`, `v-model` 等標準的Directive之外， 還提供了非常強大的自定義功能。 使用這個功能，我們就可以定義屬於我們自己的Directive.  

## 例子

我們看下面例子：

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		下面是使用了自定義的Directive的input, 可以自動聚焦（ 調用 focus() 方法) : <br/>
		<br/>
		<input v-myinput/>
	</div>
	<script>
		var app = new Vue({
			el: '#app',
			directives: {
				"myinput": {
					inserted: function(element){
						element.focus()
					}
				}
			}
		})
	</script>
</body>
</html>
```

上面的代碼中， 先是在 Vue中，定義了一個 `directives` 代碼段： 

```
directives: {
	myinput: {
		inserted: function(element){
			element.focus()
		}
	}
}
```

- `myinput` 就是自定義Directive的名字。  使用的時候就是 `v-myinput`. 
- `inserted`: 這是一個定義好的方法（鉤子方法），表示在頁面被Vuejs渲染的過程中， 在該DOM被 "insert"（插入）到頁面中的時候，被觸發。
觸發的內容是 `element.focus()`


用瀏覽器打開後，可以看到`<input/>` 標籤是會自動聚焦的。 這個時候用戶就可以直接輸入內容了。 如下圖所示：

![custom directive例子](./images/custom_directive.png)

如果在 "webpack" 等可以修改 “全局Vue實例”的時候，也可以使用這樣的方法： 

```
Vue.directive('myinput', {
  inserted: function (elemenet) {
    element.focus()
  }
})
```

## 自定義Directive的命名方法

如果您希望把 `v-myinput`的調用，寫成 `v-my-input`， 那麼在定義的時候，就應該： 

```
directives: {
	// 注意下面的寫法. 使用雙引號括起來 
	"my-input": {    
		inserted: function(element){
			element.focus()
		}
	}
}
```

那麼我們就可以在View中這樣使用了： 

```
<input v-my-input />
```

## 鉤子方法 (Hook Functions)

我們在上面的例子中，知道了 `inserted` 是一個鉤子方法。  下面是一個完整的列表： 

- `bind`  只運行一次。 當該元素首次被渲染的時候。（綁定到頁面的時候） 
- `inserted`  該元素被插入到父節點的時候（也可以認爲是該元素被Vue渲染的時候）
- `update`  該元素被更新的時候。
- `componentUpdated`  包含的component被更新的時候。
- `unbind`  只會運行一次。 當該元素被Vuejs從頁面解除綁定的時候。 

## 自定義Directive可以接收到的參數

Vuejs 爲自定義Directive 實現了強大了功能， 可以接收很多個參數。 下面是個例子：

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		下面是一個非常全面的自定義Directive的例子： <br/>
		<br/>
		<input v-my-input:foo.click="say_hi" />
	</div>
	<script>
		var a 
		var app = new Vue({
			el: '#app',
			data: {
				say_hi: '你好啊，我是個value'
			},
			directives: {
				"my-input": {
					inserted: function(element, binding, vnode){
						element.focus()
						console.info("binding.name: " + binding.name)
						console.info("binding.value: " + binding.value)
						console.info("binding.expression: " + binding.expression)
						console.info("binding.argument: " + binding.arg)
						console.info("binding.modifiers: ")
						console.info(binding.modifiers)
						console.info("vnode keys:")
						console.info(vnode)
					}
				}
			}
		})
	</script>
</body>
</html>
```

先看運行結果： 

![custom directive arguments](./images/custom_directive_arguments.png)

在上圖中，可以看到， 自定義Directive 在聲明的時候，接收了3個參數：  `function(element, binding, vnode)`

通過這三個參數，就可以看到很多對應的內容，包括 `binding.name`, `binding.value`, `binding.expression`. 他們的含義都是字面的意思。

藉助這些內容，我們可以實現我們自己想要的Directive.  

## 實戰經驗

1.優先考慮使用Component. 

考慮到維護成本。 它的作用跟 JSP 中的自定義標籤是一樣的。  與其使用 Directive, 不如使用 Component. 

2.如果一定要用，把它實現的儘量簡單。 

如果接手的新人水平不如你，那他很可能讀不懂這塊代碼。




