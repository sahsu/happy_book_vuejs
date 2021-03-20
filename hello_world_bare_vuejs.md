# 急速入門

如果只從體驗的角度來看， Vuejs 的安裝非常簡單. 只需要引入一個第三方的js包即可。

```
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
```

下面是一個最簡單的例子： 

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		{{ show_my_text }}
	</div>
	<script>
		var app = new Vue({
			el: '#app', 
			data: {
				show_my_text: 'Vuejs is the best one page app!'
			}
		})
	</script>
</body>
</html>

```

上面的代碼非常簡單， 

1. 在head中引入 vuejs 包
2. 在<body> 中，定義了一個 <div id='app'></div>, 可以認爲，所有的頁面展示，都是在這個<div>中。  
	每次我們做任何點擊的時候， 整個頁面不會刷新， 都是vuejs框架操作代碼，對<div id='app'>中的內容進行局部刷新。
3. 後面的  `var app=new Vue...` 這裏就是真正的操作代碼： 
```
		var app = new Vue({    // 表示創建了一個Vue對象
			el: '#app',        // 指定 所有的 代碼操作，都是對於 <div id='app' > 的元素來操作的
			data: {            // data 表示爲 Vuejs 管理的變量賦值。 這個變量的名字是 show_my_text
				show_my_text: 'Vuejs is the best one page app!'
			}
		})
```

使用瀏覽器打開這個頁面後， 就可以看到： 

![第一個vuejs例子](./images/hello_world_bare_vuejs.png)


## 源代碼

可以在 code_example/hello_world_bare_vuejs 中看到。 並直接運行. 