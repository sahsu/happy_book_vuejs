# Event Handler 事件處理

Vuejs中的事件處理非常強大， 也非常重要。 我們一定要學好它。

Event Handler 之所以會被Vuejs放到很高的地位，是基於這樣的考慮： 

1. 把跟事件相關的代碼獨立的寫出來， 非常容易定位各種邏輯， 維護起來方便。 
2. event handler 被獨立出來之後， 頁面的DOM元素看起來就會很簡單。  容易理解。
3. 當一個頁面被關掉時，對應的ViewModel也會被回收。那麼該頁面定義的各種 event handler 也會被一併垃圾回收。 不會造成內存溢出。

## 支持的Event 

我們在前面曾經看到過 `v-on:click`, 那麼，都有哪些事件可以被`v-on`所支持呢？ 

只要是標準的HTML定義的Event, 都是被Vuejs支持的。 

- focus  （元素獲得焦點)
- blur    (元素失去焦點)
- click  （單擊 鼠標左鍵）
- dblclick (雙擊鼠標左鍵)
- contextmenu (單機鼠標右鍵)
- mouseover  (指針移到有事件監聽的元素或者它的子元素內)
- mouseout   (指針移出元素，或者移到它的子元素上)
- keydown   （鍵盤動作： 按下任意鍵) 
- keyup     (鍵盤動作： 釋放任意鍵)

可以來這裏查看 [所有HTML標準事件](https://developer.mozilla.org/zh-CN/docs/Web/Events)

總共定義了 162個標準事件， 和 幾十個非標準事件，以及 Mozilla的特定事件。  如下圖所示：

![mozilla定義的事件截圖](./images/events_defined_table.png)

我們不用全部都記住，通常在日常開發中，只有不到20個是最常見的event. 

## 使用 v-on 進行事件的綁定

我們可以認爲，幾乎所有的事件，都是由 `v-on` 這個 directive 來驅動的。 所以，本節會對 `v-on` 有更加詳盡的說明。

### 1. 在 v-on 中使用變量

如下面代碼所示，可以在 `v-on` 中引用變量： 

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		您點擊了： {% raw %}{{{% endraw %} count }} 次
		<br/>
		<button v-on:click='count += 1' style='margin-top: 50px'> + 1</button>
	</div>

	<script>
		var app = new Vue({
			el: '#app', 
			data: {
				count: 0
			}
		})
	</script>
</body>
</html>
```

上面的代碼，用瀏覽器打開後， 點擊 按鈕， 就可以看到 `count` 這個變量會隨之 +1. 如下圖所示： 

![event: 使用變量](./images/events_use_variable.png)

### 2. 在 v-on 中使用方法名

上面的例子，也可以按照下面的寫法來實現：

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		您點擊了：{% raw %}{{{% endraw %} count }} 次
		<br/>
		<button v-on:click='increase_count' style='margin-top: 50px'>  + 1 </button>
	</div>

	<script>
		var app = new Vue({
			el: '#app', 
			data: {
				count: 0
			}, 
			methods: {
				increase_count: function(){
					this.count += 1
				}
			}
		})
	</script>
</body>
</html>
```

可以看到，在 `v-on:click='increase_count'` 中， `increase_count` 就是一個方法名。 

### 3. 在v-on 中使用方法名 + 參數

我們也可以直接使用 `v-on:click='some_function("your_parameter")'` 這樣的寫法，如 下面的例子所示： 


```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		{% raw %}{{{% endraw %} message }}
		<br/>
		<button v-on:click='say_hi("明日的Vuejs大神")' style='margin-top: 50px'> 跟我打個招呼~ </button>
	</div>

	<script>
		var app = new Vue({
			el: '#app', 
			data: {
				message: "這是個 在click中調用 方法 + 參數的例子"
			},
			methods: {
				say_hi: function(name){
					this.message = "你好啊，" + name + "!"
				}
			}
		})
	</script>
</body>
</html>
```

使用瀏覽器打開後，點擊按鈕，就可以看到下圖所示：

![使用方法+參數的例子](./images/event_call_method_with_parameter.png)

### 4. 重新設計按鈕的邏輯

我們在實際開發中，往往會遇到這樣的情況： 點擊某個按鈕，或者觸發某個事件後，希望概念 按鈕的默認狀態。

最典型的例子： 提交表單(<form/>)的時候，我們希望先對該表單進行驗證。 如果驗證不通過，該表單就不要提交。 

這個時候，如果希望表單不要提交，我們就要讓 這個 submit 按鈕，不要有下一步的動作。 在所有的開發語言當中，都會有一個對應的方法，叫做： "preventDefault"
(停止默認動作)

我們看這個例子：


```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>

		請輸入您想打開的網址,   	<br/>
		判斷規則是： 				<br/>
		1. 務必以 "http://"開頭 	<br/>
		2. 不能是空字符串			<br/>
		<input v-model="url" placeholder="請輸入 http:// 開頭的字符串, 否則不會跳轉" /> <br/>
		<br/>
		<a v-bind:href="this.url" v-on:click='validate($event)'> 點我確定 </a>
	</div>

	<script>
		var app = new Vue({
			el: '#app', 
			data: {
				url: ''
			}, 
			methods: {
				validate: function(event){
					if(this.url.length == 0 || this.url.indexOf('http://') != 0){
						alert("您輸入的網址不符合規則。 無法跳轉")
						if(event){
							alert("event is: " + event)
							event.preventDefault()
						}   
					}
				}
			}
		})
	</script>
</body>
</html>
```

上面的代碼中，可以看到，我們定義了一個變量： `url`.  並且通過代碼： 

`<a v-bind:href="this.url" v-on:click='validate($event)'> 點我確定 </a>` 做了兩件事情： 

1. 把 `url` 綁定到了該元素上。
2. 該元素 在觸發 `click`事件時，會調用 `validate`方法。 該方法傳遞了一個特殊的參數： `$event`. 該參數是當前 事件的一個實例。（MouseEvent)

在 `validate`方法中，我們是這樣定義的：  先驗證是否符合規則。 如果符合，放行，會繼續觸發 `<a/>` 元素的默認動作（讓瀏覽器發生跳轉） 。 否則的
話，會彈出一個 "alert" 提示框。 


用瀏覽器打開這段代碼，可以看到下圖所示：

![preventdefault 的例子](./images/event_preventdefault.png)

我們先輸入一個合法的地址： http://baidu.com  ， 可以看到，點擊後，頁面發生了跳轉。 跳轉到了百度。 

我們再輸入一個 “不合法”的地址：  https://baidu.com  注意： 該地址不是以 "http://" 開頭，所以我們的vuejs 代碼不會讓它放行。 

如下圖所示：

![prevent-default例子2，不放行](./images/event_prevent_default2.png)

進一步觀察，頁面也不會跳轉（很好的解釋了 這個時候 `<a/>` 標籤點了也不起作用)

### 5. Event Modifiers  事件修飾語

我們很多時候，希望把代碼寫的優雅一些。 使用傳統的方式，可能會把代碼寫的很臃腫。  如果某個元素在不同的event下有不同的表現，那麼代碼看起來就會有
很多個 `if ...else ...` 這樣的分支。 

所以， Vuejs 提供了 "Event Modifiers"。

例如，我們可以把上面的例子略加修改：

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>

		請輸入您想打開的網址,   	<br/>
		判斷規則是： 				<br/>
		1. 務必以 "http://"開頭 	<br/>
		2. 不能是空字符串			<br/>
		<input v-model="url" placeholder="請輸入 http:// 開頭的字符串, 否則不會跳轉" /> <br/>
		<br/>
		<a v-bind:href="this.url" v-on:click='validate($event)' v-on:click.prevent='show_message'> 點我確定 </a>
	</div>

	<script>
		var app = new Vue({
			el: '#app', 
			data: {
				url: ''
			}, 
			methods: {
				validate: function(event){
					if(this.url.length == 0 || this.url.indexOf('http://') != 0){
						if(event){
							event.preventDefault()
						}   
					}
				},
				show_message: function(){
					alert("您輸入的網址不符合規則。 無法跳轉")
				}
			}
		})
	</script>
</body>
</html>
```

可以看出，上面的代碼的核心是：

```
<a v-bind:href="this.url" v-on:click='validate($event)' v-on:click.prevent='show_message'> 點我確定 </a>

methods: {
	validate: function(event){
		if(this.url.length == 0 || this.url.indexOf('http://') != 0){
			if(event){
				event.preventDefault()
			}   
		}
	},
	show_message: function(){
		alert("您輸入的網址不符合規則。 無法跳轉")
	}
}
```

先是在 `<a/>` 中定義了兩個 click 事件，一個是 `click`, 一個是 `click.prevent`.  後者表示，如果該元素的click 事件被 阻止了的話， 應該觸發什麼動作。 

然後，在 `methods` 代碼段中，專門定義了 `show_message` , 用來給 `click.prevent` 所使用。

上面的代碼運行起來，跟前一個例子是一模一樣的。 只是抽象分類的程度更高了一些。  在複雜的項目中有用處。

這樣的 "event modifier"，有這些： 

- stop  propagation 被停止後（ 也就是調用了 event.stopPropagation()方法後 )，被觸發 
- prevent  調用了  event.preventDefault() 後被觸發。
- capture  子元素中的事件可以在該元素中 被觸發。
- self   事件的 event.target 就是本元素時，被觸發。
- once   該事件最多被觸發一次。 
- passive  爲移動設備使用。 (在addEventListeners 定義時，增加passive選項。)

以上的 "event modifier" 也可以連接起來使用。  例如： `v-on:click.prevent.self`


### 6. Key Modifiers  按鍵修飾語

Vuejs 也很貼心的提供了 Key Modifiers, 也就是一種支持鍵盤事件的快捷方法。 我們看下面的例子：

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		輸入完畢後，按下回車鍵，就會<br/>
		觸發 "show_message" 事件~  <br/><br/>

		<input v-on:keyup.enter="show_message" v-model="message" />
	</div>

	<script>
		var app = new Vue({
			el: '#app', 
			data: {
				message: ''
			}, 
			methods: {
				show_message: function(){
					alert("您輸入了：" + this.message)
				}
			}
		})
	</script>
</body>
</html>
```

可以看到，在上面的代碼中， `v-on:keyup.enter="show_message"`  爲 `<a/>`  元素定義了事件，該事件對應了 "回車鍵"。 
（嚴格的說，是回車鍵被按下後，鬆開彈起來的那一刻）

我們用瀏覽器打開上面的代碼對應的文件，輸入一段文字，按回車，就可以看到事件已經被觸發了。 如下圖所示：

![event key modifier被觸發](./images/event_key_modifiers.png)

Vuejs 總共支持下面這些 Key modifiers: 

- enter    回車鍵
- tab      tab 鍵
- delete   同時對應了 backspace 和 del 鍵
- esc      ESC 鍵
- space    空格
- up       向上鍵
- down     向下鍵
- left     向左鍵
- right    向右鍵

隨着 Vuejs 版本的不斷迭代和更新，越來越多的 Key modifiers 被添加了進來， 例如 `page down`, `ctrl` 。對於這些鍵的用法，
大家可以查閱官方文檔。


