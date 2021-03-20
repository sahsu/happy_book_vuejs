# 視圖中的Directive (指令)

我們在學習java的時候，知道有jsp 頁面， 對於.net語言，有 .asp, aspx頁面， 對於ruby, 有erb這樣的頁面。

在Vuejs中，我們也有類似的編程能力。 

但是由於 Vuejs 是一種框架，所以它的稍微特殊一些，只能與標籤做結合使用。 叫做Directive. (指令)

我們之前看到的 `v-on`, `v-bind`, `v-if`, `v-for` 等等，只要是以`v`開頭的，都是Directive. 

原理： 

1. 用戶在瀏覽器中輸入網址，回車. 
2. 瀏覽器加載所有的資源（js, html內容。） 此時尚未解析。
3. 瀏覽器加載Vuejs  
4. Vuejs程序被執行， 發現 頁面中的 `Directive`, 進行相關的解析
5. HTML中的內容被替換。 展現給用戶。

所以，我們在開發一個Vuejs項目的時候，會看到大量的Directive。 這裏的基礎務必打好。

## 前提: 在directive中要使用表達式(Expression) 

- 表達式：  a > 1    （有效）
- 普通的語句：   a = 1  ; （這是個聲明，不會生效）
- 流控制語句：  return a;  （不會生效）

在所有的Directive中，都只能使用表達式。

正確的：

```

<div v-if="a > 100">
</div>
```

錯誤的： 

```
<div v-if="return false">
</div>
```

## 循環: v-for

完整的例子如下：

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		<p>Vuejs周邊的技術生態有： <p>
		<br/>
		<ul>
			<li v-for="tech in technologies">
				{% raw %}{{{% endraw %}tech}}
			</li>
		</ul>
	</div>
	<script>
		var app = new Vue({
			el: '#app', 
			data: {	
				technologies: [
					"nvm", "npm", "node", "webpack", "ecma_script"
				]
			}
		})
	</script>
</body>
</html>
```

可以注意到，上面代碼中的 `technologies`是在`data` 中被定義的： 

```
data: {	
	technologies: [
		"nvm", "npm", "node", "webpack", "ecma_script"
	]
}
```

然後在下面代碼中， 被循環顯示: 

```
<li v-for="tech in technologies">
	{{tech}}
</li>
```

用瀏覽器打開上面代碼後，如下圖所示： 

![v-for循環](./images/directive-v-for.png)

## 判斷: v-if

條件Directive, 是由 `v-if`, `v-else-if`, `v-else` 配合完成的。 下面是個完整的例子： 

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		<p>我們要使用的技術是： <p>

		<div v-if="name === 'vuejs'">
			Vuejs !
		</div>
		<div v-else-if="name === 'angular'">
			Angular
		</div>
		<div v-else>
			React
		</div>		
	</div>
	<script>
		var app = new Vue({
			el: '#app', 
			data: {	
				name: 'vuejs'
			}
		})
	</script>
</body>
</html>
```

注意，

上面的代碼中，  `v-if` 後面的引號中，是 `name === 'vuejs'`, `===` 是 Ecmascript 的語言， 表示嚴格的判斷。 
（由於js的 `==` 有先天的缺陷，所以我們在95%的情況下， 都是用 三個等號的形式) 


用瀏覽器打開上面代碼後，如下圖所示： 

![v-if 判斷](./images/directive-v-if.png)

## v-if 與 v-for 的優先級

當 `v-if` 與 `v-for` 一起使用時，`v-for` 具有比 `v-if` 更高的優先級。 也就是說，Vuejs 會先執行 `v-for`, 再執行 `v-if` . 

下面是個完整的例子：

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		<p> 全部的技術是： {{technologies}} </p>
		<p> v-for 與 v-if 的結合使用, 只打印出 以"n"開頭的技術： <p>
		<ul>
			<li v-for="tech in technologies" v-if="tech.indexOf('n') === 0">
				{% raw %}{{{% endraw %}tech}}
			</li>
		</ul>
	</div>
	<script>
		var app = new Vue({
			el: '#app', 
			data: {	
				technologies: [
					"nvm", "npm", "node", "webpack", "ecma_script"
				]
			}
		})
	</script>
</body>
</html>
```

可以看到， 在下面的代碼中， `v-if` 與 `v-for`結合使用了， 先是做了循環 `tech in technologies`，然後對當前的 循環對象 `tech` 做了判斷： 

```
<li v-for="tech in technologies" v-if="tech.indexOf('n') === 0">
	{% raw %}{{{% endraw %}tech}}
</li>
```

用瀏覽器打開上面代碼後，如下圖所示： 

![v-for與 v-if的結合使用](./images/directive-v-for-and-v-if.png)

## v-bind

`v-bind`指令用於把某個屬性綁定到對應的元素的屬性。 請看例子： 

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		<p v-bind:style="'color:' + my_color">Vuejs 學起來好開心~ </p>
	</div>
	<script>
		var app = new Vue({
			el: '#app',
			data: {
				my_color: 'green'
			}
		})
	</script>
</body>
</html>
```

在上面的代碼中，可以看到， 通過 `v-bind`, 把`<p>`元素的style的值，綁定成了 `'color:' + my_color ` 這個表達式。 
當 `my_color` 的值發生變化的時候， 對應`<p>`的顏色就會發生變化。 

例如： 默認頁面打開後， 是綠色的。 如下圖所示：

![v-bind的效果](./images/directive-v-bind-before.png)

如何知道變量`my_color` 已經綁定到了 `<p>` 上了呢？ 我們在 console中做修改， 讓 `app.my_color = "red"`, 就可以看到對應的文字的顏色，變成了紅色，
如下圖所示：

![v-bind修改後的效果](./images/directive-v-bind-after.png)


對於所有的屬性，都可以使用 `v-bind`, 例如： 

```
<div v-bind:style='...'>  
</div>
```

會生成： 
```
<div style='...'>  </div>
```

```
<div v-bind:class='...'> </div>
```

會生成： 
```
<div class='...'>  </div>
```

```
<div v-bind:id='...'> </div>  
```

會生成： 
```
<div id='...'>  </div>
```

## v-on

`v-on` 指令用於觸發事件。  例如： 

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		{{ message }}
		<br/>
		<button v-on:click='highlight' style='margin-top: 50px'>真的嗎</button>
	</div>

	<script>
		var app = new Vue({
			el: '#app', 
			data: {
				message: '學習Vuejs使我快樂~ '
			},
			methods:  {
				highlight: function() {
					this.message = this.message + '是的， 工資還會漲~!'
				}
			}

		})
	</script>
</body>
</html>
```

上面的代碼中， 通過`v-on:click` 的聲明，當 <button> 被點擊(click)後，就會觸發  `highlight` 這個方法。 

點擊前的頁面: 

![點擊前](./images/directive-v-on-click-before.png)

點擊後的頁面: 

![點擊後](./images/directive-v-on-click-after.png)


`v-on` 後面可以接HTML的標準事件， 例如： 

- click  （單擊 鼠標左鍵）
- dblclick (雙擊鼠標左鍵)
- contextmenu (單機鼠標右鍵)
- mouseover  (指針移到有事件監聽的元素或者它的子元素內)
- mouseout   (指針移出元素，或者移到它的子元素上)
- keydown   （鍵盤動作： 按下任意鍵) 
- keyup     (鍵盤動作： 釋放任意鍵)

對於 `v-on` 的更多說明，請看“Event” 的對應章節。

### v-on的簡寫

`v-on:click` 往往會寫成 `@click`,   `v-on:dblclick` 也會寫成 `@dblclick` , 所以同學們看代碼的時候要了解~

## v-model 與雙向綁定

`v-model` 往往用來做“雙向綁定” ( two way binding) ， 這個雙向綁定的含義是： 

1. 可以通過表單（用戶手動輸入的值） 來修改某個變量的值
2. 可以通過程序的運算 來修改某個變量的值， 並且影響頁面的展示。 

雙向綁定可以大大的方便我們的開發。 例如，我們做一個天氣預報的軟件時，需要在前臺展示“當前溫度”， 如果後臺代碼做了某些操作後，發現“當前溫度”發生了變化，

如果沒有雙向綁定這個技術，我們的代碼就會比較膨脹， 做各種條件判斷。 有了雙向綁定的話， 就可以做到後臺變量一變化，前臺的對於該變量的展示就會發生變化。 

下面是完整的頁面源代碼： 

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		<p> 你好， {% raw %}{{{% endraw %}name}} ! </p>
		<input type='text' v-model="name" />
	</div>
	<script>
		var app = new Vue({
			el: '#app',
			data: {
				// 下面一行代碼不能省略。 這裏聲明瞭 name 這個變量
				name: 'Vuejs（默認)'
			}
		})
	</script>
</body>
</html>
```

上面的代碼中， 可以看到，我們: 

1. 使用了  `<input type='text' v-model="name" />`  來把 變量 `name` 綁定在了 `<input>` 這個輸入框上（可以在這裏看到`name`的值)
2. 使用了 `<p> 你好， {% raw %}{{{% endraw %}name}} ! </p>` 來把變量 `name` 也顯示在 頁面上。
3. 在初始化中， 使用 `data: { name: '...' }` 的方式， 對變量 `name` 進行了初始化。

使用瀏覽器打開該html頁面， 可以看到，最初的狀態如下圖所示： 

![狀態1： 默認狀態](./images/directive-v-model-before.png)

可以看到， 上圖中的文字顯示的是：  “你好， Vuejs! ”

然後，我們在輸入框中，把內容改成：  “Vuejs 和 Webpack”, 於是頁面就發生了變化，如下圖所示：

![狀態2： 通過表單輸入來改變變量的值](./images/directive-v-model-after-1-by-input.png)

這樣就說明，我們通過輸入框來改變某個變量的值，是成功的。  

然後，我們打開瀏覽器的“開發者工具”  （建議用Chrome, Chrome下的操作方式是： 按F12) 。 在console中輸入： `app.name = "明日Vuejs高手"`, 
就會看到下圖所示： 

![狀態3： 通過運算改變變量的值](./images/directive-v-model-after-2-by-console.png)

這樣就說明，我們通過運算來改變某個變量的值，是成功的。  


