# Slot

作爲對Component的補充，Vuejs 增加了 Slot 這個功能. 

## 普通的Slot

我們從具體的例子來說明。

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		<study-process>
			我學習到了Slot 這個章節。
		</study-process>
	</div>
	<script>
		Vue.component('study-process', {
		  data: function () {
		    return {
		      count: 0
		    }
		  },
		  template: '<div><slot></slot></div>'
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

從上面的代碼從，我們可以看到，我們先是定義了一個 component: 

```
Vue.component('study-process', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<div><slot></slot></div>'
})
```

在這個component的template中，是這樣的：  

```
template: '<div><slot></slot></div>'
```

這裏就使我們定義的 slot. 

然後，我們在調用這個 component的時候，這樣： 

```
<study-process>
	我學習到了Slot 這個章節。
</study-process>
```		

所以，“我學習到了Slot 這個章節。”就好像一個參數那樣傳入到了 component中， component 發現自身已經定義了 slot, 就會把這個字符串放到slot的位置，然後顯示出來。 

如下圖所示： 

![slot](./images/slot.png)

## named slot

也就是帶有名字的slot . 

很多時候我們可能需要多個slot. 看下面的例子：


```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		<study-process>
			<p slot='slot_top'>
				Vuejs 比起別的框架真的簡潔好多！
			</p>

				我學習到了  Slot 這個章節。

			<h5 slot='slot_bottom'>
				再也不怕H5 項目了~ 
			</h5>
		</study-process>
	</div>
	<script>
		Vue.component('study-process', {
		  template: '<div>' + 
		  '<slot name="slot_top"></slot>' + 
		  '<slot></slot>' + 
		  '<slot name="slot_bottom"></slot>' + 
		  '</div>'
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

上面的代碼中， 我們定義了這樣的 component: 

```
Vue.component('study-process', {
  template: '<div>' + 
  '<slot name="slot_top"></slot>' + 
  '<slot></slot>' + 
  '<slot name="slot_bottom"></slot>' + 
  '</div>'
})
```

其中的 `<slot name="slot_top"></slot>`  就是一個named slot (具備名字的slot) ,這樣，在後面對於 component的調用中：

```
<p slot='slot_top'>
	Vuejs 比起別的框架真的簡潔好多！
</p>
```	
就會渲染在對應的位置了。		

## slot 的默認值

我們可以爲 slot 加上默認值。這樣當 “父頁面” 沒有指定某個slot的時候，就會顯示這個默認值了。

例如：

```
<slot name="slot_top">這裏 top slot的默認值 </slot>
```

