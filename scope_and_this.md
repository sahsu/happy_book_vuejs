# js的作用域 與 this

無論是 javascript, 還是 emscript, 變量的作用域都屬於高級知識。 我們想考察一個js程序員的水平如何，可以直接用作用域來提問。 

同時，我們在實際的開發中發現，很多js/emscript 的新人，對於作用域和 this 都很含混，所以這裏要單獨的提一下。

## 作用域

無論是 javascript, 還是 emscript, 對於作用域的使用基本是一樣的。 後者更加嚴密一些。 我們看幾個例子。

1.全局變量, 可以直接引用。 

```
//全局變量 a:
var a = 1;
function one() {
  console.info(a) 
}
```

打印結果是 1 

2.函數內的普通變量

```
function two(a ){
  console.info('a is' + a)
}
```

運行： 

`two(2)` 打印結果是 ：  `a is 2` 

3.普通函數可以對全局變量做賦值。 如下圖所示：

```
var a = 1;
function four(){
  console.info(' in four, before a=4: ' + a)
  if(true) a = 4;
  console.info(' in four, after a=4: ' + a)
}
```

運行： 

```
four(4)
```

結果： 

```
in four, before a=4: 1     ( 這個是符合正常的scope邏輯的。)
in four, after a=4: 4      ( 這個也是符合)
```

再運行： `console.info(a)`, 可以看到輸出： `4`  說明 全局變量 `a` 在 four()函數中已經被髮生了永久的變化。

4.通過元編程定義的函數

```
var six = ( function(){
  var foo = 6;
  return function(){
    return foo;
  }
}
)();
```

在上述代碼中， js解析器會先運行（忽略最後的 `()` )：  

```
var temp = function(){
  var foo = 6;
  return function(){
    return foo;
  }
}
```

然後再運行：  `var six = (temp)()`  ， 所以， `six` 就是：

```
function(){
	return foo;
}
```

上面的 `foo` 就是來自於方法最開始定義的 `var foo = 6`, 而這個變量的定義，是在一個 function() 中的。 所以它不是一個全局變量。  

所以，如果我們在console中輸入 `foo` , 會看到報錯消息： `Uncaught ReferenceError: foo is not defined`


5.通過元編程定義的函數中的變量，不會污染全局變量。

```
var foo = 1;

var six = ( function(){
  var foo = 6;
  return function(){
    console.info("in six, foo is: " + foo);
  }
}
)();
```

在上面的代碼中，我們先定義了一個 全局變量 `foo`, 再定義了一個方法 `six`, 裏面定義了一個臨時方法 `foo`.   並且進行來了一些操作。 

運行：

```
six()   // 返回：   in six, foo is: 6
foo     // 返回： 1
```



## this

對於 `this` 的使用， 在這裏 https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this  指出了對於javascript中this的詳細用法。  

在emscript中也基本是一樣的。

簡單的說，大家只要記得， `this` 指的是當前作用域的對象實例就好了。

```
var apple = {
  color: 'red',
  show_color: function() {
    return this.color
  }
}
```

我們輸入  `apple.show_color` 就可以看到輸出 `red`.   這裏的 `this` 指的就是 `apple` 變量。


## 實戰經驗

### 1.在Vue的方法定義中容易用錯。

當我們發現 代碼看起來沒問題， 但是console 總報錯說 xx undefined 時， 十有八九就是 忘記加 this 了。

例如： 

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
				message: 'this 是很重要的。 不要忘記它喲~'
			},
			methods:  {
				highlight: function() {
					// 報錯：  message is not defined. 
					message += '是的， 工資還會漲~!'

					// 正確的代碼應該是：
					// this.message += '是的， 工資還會漲~!'
				}
			}

		})
	</script>
</body>
</html>
```

使用瀏覽器加載上述代碼，我們會發現報錯了：

![this的錯誤用法](./images/scope_without_this.png)

上面的代碼中， `message += ...` 那一行中， `message` 是當前的vue的實例的一個"property"(屬性)， 而我們如果希望在 `methods` 中引用這個屬性的話，
就需要用 `this.message` 纔對。 這裏的 `this` 對應的就是  `var app = new Vue()` 中定義的 `app`. 

### 2.在發起http請求時容易用錯

我們看下面的例子，是一段代碼片段：

```
new Vue({
	data: {
		cities: [...]
	},
	methods: {
		my_http_request: function(){
			let that = this
			axios.get('http://mysite.com/my_api.do')
			.then(function(response){
	            // 這裏不能使用 this.cities 來賦值
			    that.cities = response.data.result
			})
		}	
	}
})

```

在上面的代碼中，我們定義了一個屬性： `cities`, 定義了一個方法： `my_http_request`.  該方法會向遠程發起一個請求， 
然後把返回的response中的值賦給 `cities`. 

可以在上面代碼中看到， 需要先在 `axios.get`之前，定義一個變量 `let that = this`.  這個時候， `this` 和 `that` 都處於 "Vue"的實例中。

但是在 `axios.get(..).then()` 函數中，就不能再使用`this` 了。 因爲在 `then(...)` 中，這是個function callback, 其中的 `this` 會代表
這個 http request event. 這是個事件。  

所以， 只能用這樣的方式。 

（如果使用了 emscript 的 `=>` 的話，就可以避免上述問題)

### 3.在event handler中容易用錯

道理同上。 


