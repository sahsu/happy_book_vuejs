# 使用Computed properties(計算得到的屬性)和watchers(監聽器) 

很多時候，我們在頁面上想要顯示某個變量的值時，都需要經過一些計算， 例如： 

```
<div id="example">
  {% raw %}{{{% endraw %} some_string.split(',').reverse().join('-') }}
</div>
```

越是複雜，到後期越容易出錯。 

這個時候，我們就需要一種機制，可以方便的創建這樣的通過計算得來的數據。  

所以， Computed Properties 就是我們的解決方案。

## 典型例子

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		<p> 原始字符串： {{my_text}} </p>
		<p> 通過運算後得到的字符串： {{my_computed_text}} </p>
	</div>
	<script>
		var app = new Vue({
			el: '#app',
			data: {
				my_text: 'good good study, day day up'
			},
			computed: {
				my_computed_text: function(){
					// 先去掉逗號，然後按照空格分割成數組，然後翻轉，並用'-'來連接
					return this.my_text.replace(',', '').split(' ').reverse().join('-')
				}
			}

		})
	</script>
</body>
</html>
```

可以看到，上面的關鍵代碼是，在 Vue的構造函數中， 傳入一個 `computed`的段落。 

使用瀏覽器運行後，可以看到結果如下圖所示：

![computed properties例子](./images/computed_properties.png)

我們也打開 console 來查看。 

輸入
```
> app.my_text    
```
會得到：  "good good study, day day up"

輸入

```
> app.my_computed_text
```
會得到轉換後的： "up-day-day-study-good-good"

## Computed Properties 與 普通方法的區別。

根據上面的例子，我們可以使用 普通方法來實現： 

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		<p> 原始字符串： {{my_text}} </p>
		<p> 通過運算後得到的字符串： {% raw %}{{{% endraw %}my_computed_text() }} </p>
	</div>
	<script>
		var app = new Vue({
			el: '#app',
			data: {
				my_text: 'good good study, day day up'
			},
			methods: {
				my_computed_text: function(){
					return this.my_text.replace(',', '').split(' ').reverse().join('-') + '，我來自於 function, 不是computed '
				}
			}
		})
	</script>
</body>
</html>
```

上面的代碼，運行後如下圖所示： 

![使用function代替computed properties](./images/computed_properties_use_function.png)

可以發現，他們達到的效果是一樣的。 

他們的區別在於：  使用computed properties的方式，會把結果“緩存”起來。  每次調用對應的computed properties時，只要對應的依賴數據沒有改動， 
那麼就不會變化。 

而使用 "function" 實現的版本，則不存在緩存問題。 每次都會重新計算對應的數值。 

所以， 我們需要按照實際情況，來選擇是使用 "computed properties" ， 還是使用普通function的形式。


## watched property

Vuejs 中的property(屬性)， 是可以要麼根據計算髮生變化（computed) , 要麼根據監聽（watch)其他的變量的變化而發生變化

我們看一下，如何根據監聽（watch) 其他的變量而自身發生變化的例子， 如下；


```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		<p> 
			我所在的城市： <input v-model='city' />（這是個watched property)
		</p>
		<p> 
			我所在的街道： <input v-model='district' />（這是個watched property)
		</p>
		<p> 我所在的詳細一些的地址： {{full_address}} （每次其他兩個發生變化，這裏就會跟着變化) </p>
	</div>
	<script>
		var app = new Vue({
			el: '#app',
			data: {
				city: '北京市',
				district: '朝陽區',
				full_address: "某市某區"
			},
			watch: {
				city: function(city_name){
					this.full_address = city_name + '-' + this.district
				},
				district: function(district_name){
					this.full_address = this.city + '-' + district_name
				}
			}

		})
	</script>
</body>
</html>
```

在上面的代碼中， 可以看到， `watch: { city: ..., district: ...}`, 表示， `city` 和 `district` 都已經被監聽了， 這兩個都是 `watched properties`.

只要`city` 和 `district`發生變化， `full_address` 就會跟着變化。 

我們用瀏覽器打開上面的代碼，如下圖所示，此時 由於 `city` 和`district` 還沒有發生變化，所以 `full_address`的值還是 "某市某區" ： 

![被監聽的屬性](./images/watched_property.png)

當我在 “街道” 的輸入框， 後面加上 “望京街道” 幾個字後，可以看到， 下面的“詳細地址”， 就發生了變化。 如下圖所示： 

![發生了變化的監聽屬性](./images/watched_property_2.png)

### 使用computed 會比watch 更加簡潔

上面的例子，我們可以使用 `computed` 來改寫， 如下圖所示： 

```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		<p> 
			我所在的城市： <input v-model='city' />
		</p>
		<p> 
			我所在的街道： <input v-model='district' />
		</p>
		<p> 我所在的詳細一些的地址： {{full_address}} （這是使用computed 實現的版本) </p>

	</div>
	<script>
		var app = new Vue({
			el: '#app',
			data: {
				city: '北京市',
				district: '朝陽區',
			},
			computed: {
				full_address: function(){
					return this.city + this.district;
				}
			}
		})
	</script>
</body>
</html>
```

可以看到， 方法少了一個 ， `data`中定義的屬性也少了一個，簡潔了不少。 代碼簡潔，維護起來就容易（代碼量越少，程序越好理解）

## 爲 computed property 的setter （賦值函數) 

原則上來說， computed property 是根據其他的值，經過計算得來的。 是不應該被修改的。

不過在開發中，確實有一些情況，需要對 "computed property" 做修改， 同時影響某些對應的屬性。 （過程跟上面是相反的） . 

我們看下面的代碼： 


```
<html>
<head>
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
	<div id='app'>
		<p> 
			我所在的城市： <input v-model='city' />
		</p>
		<p> 
			我所在的街道： <input v-model='district' />
		</p>
		<p> 我所在的詳細一些的地址：	<input v-model='full_address' /> </p>

	</div>
	<script>
		var app = new Vue({
			el: '#app',
			data: {
				city: '北京市',
				district: '朝陽區',
			},
			computed: {
				full_address: {
					get: function(){
						return this.city + "-" + this.district;
					},
					set: function(new_value){
						this.city = new_value.split('-')[0]
						this.district = new_value.split('-')[1]
					}
				}
			}
		})
	</script>
</body>
</html>
```

可以看出， 上面代碼中，有這樣一段：

```
computed: {
	full_address: {
		get: function(){
			return this.city + "-" + this.district;
		},
		set: function(new_value){
			this.city = new_value.split('-')[0]
			this.district = new_value.split('-')[1]
		}
	}
}
```			

可以看出， 上面的 `get` 代碼段，就是原來的代碼內容。 而 `set`端中，則定義了，如果 computed property (也就是 full_address) 發生變化的時候， 
`city` 和 `district` 的值應該如何變化。 

用瀏覽器打開後， 我們在 “最下方的輸入框” 中，後面輸入一些字，可以看到， 對應的 “街道”發生了變化： 

![根據setter影響其他變量的例子](./images/computed_setter.png)